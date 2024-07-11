# Retrosynthesis prediction using an end-to-end graph generative architecture for molecular graph editing

Zhong, W., Yang, Z. & Chen, C.YC. Retrosynthesis prediction using an end-to-end graph generative architecture for molecular graph editing. Nat Commun 14, 3009 (2023). 

DOI: https://doi.org/10.1038/s41467-023-38851-5

GitHub: https://github.com/Jamson-Zhong/Graph2Edits (MIT License)

---

## Contributions
1. **Graph2Edits**: an end-to-end architecture that generates arbitrary length graph edits in an auto-regressive way.
2. **D-MPNN**: encodes the atom/bond and LG to predict atom/bond edits and a termination, respectively.
3. **Edit label**: introduces chirality and cis-trans isomerism in predefined atom and bond edits.


## Data Preparation

This paper builds an edits vocabulary in the training set. These edits cover 99.9% of the reactions in the test set, including 6 bond edits, 152 atom edits, and a termination symbol. For example, 

* ('Change Bond', (2, 0)),
* ('Attaching LG', '*O'),
* ('Attaching LG', '*C(=O)CCCCBr').

They include 4 types of edits and a termination symbol:

* **Delete bond**
* **Change bond**
* **Change atom**, e.g., ('Change Atom', (0, 0)) the two numbers in brackets represent the number of hydrogen and the chiral type to be changed, respectively.
* **Attach leaving group (LG)**: e.g., ('Attaching LG', `*C(=O)c1ccccc1C(*)=O`) represents the functional group is added on the specified atom.
* **Terminate**

## Learning & Inference

The message passing neural network (MPNN), particularly the directed MPNN (D-MPNN),  operates on an undirected graph $\mathcal{G}=(\mathcal{V}, \mathcal{E})$ to build the atom representations of molecules. 

Graph2Edits learns an autoregressive model and predicts the next edit. The model is learned by cross-entropy loss over possible edits (bond edits, atom edits, termination).

## Code Snippet
```python
class MPNEncoder(nn.Module):
    """Class: 'MPNEncoder' is a message passing neural network for encoding molecules."""

    def __init__(self, atom_fdim: int, bond_fdim: int, hidden_size: int,
                 depth: int, dropout: float = 0.15, atom_message: bool = False):
        """
        Parameters
        ----------
        atom_fdim: Atom feature vector dimension.
        bond_fdim: Bond feature vector dimension.
        hidden_size: Hidden layers dimension
        depth: Number of message passing steps
        droupout: the droupout rate
        atom_message: 'D-MPNN' or 'MPNN', centers messages on bonds or atoms.
       """
        super(MPNEncoder, self).__init__()
        self.atom_fdim = atom_fdim
        self.bond_fdim = bond_fdim
        self.hidden_size = hidden_size
        self.depth = depth
        self.dropout = dropout
        self.atom_message = atom_message

        # Input
        input_dim = self.atom_fdim if self.atom_message else self.bond_fdim
        self.w_i = nn.Linear(input_dim, self.hidden_size, bias=False)

        # Update message
        if self.atom_message:
            self.w_h = nn.Linear(
                self.bond_fdim + self.hidden_size, self.hidden_size)

        self.gru = nn.GRUCell(self.hidden_size, self.hidden_size)

        # Dropout
        self.dropout_layer = nn.Dropout(p=self.dropout)
        # Output
        self.W_o = nn.Sequential(
            nn.Linear(self.atom_fdim + self.hidden_size, self.hidden_size), nn.ReLU())

    def forward(self, graph_tensors: Tuple[torch.Tensor], mask: torch.Tensor) -> torch.FloatTensor:
        """
        Forward pass of the graph encoder. Encodes a batch of molecular graphs.

        Parameters
        ----------
        graph_tensors: Tuple[torch.Tensor],
            Tuple of graph tensors - Contains atom features, message vector details, the incoming bond indices of atoms
            the index of the atom the bond is coming from, the index of the reverse bond and the undirected bond index 
            to the beginindex and endindex of the atoms.
        mask: torch.Tensor,
            Masks on nodes
        """
        f_atoms, f_bonds, a2b, b2a, b2revb, undirected_b2a = graph_tensors
        # Input
        if self.atom_message:
            a2a = b2a[a2b]  # num_atoms x max_num_bonds
            f_bonds = f_bonds[:, -self.bond_fdim:]
            input = self.w_i(f_atoms)  # num_atoms x hidden
        else:
            input = self.w_i(f_bonds)  # num_bonds x hidden

        # Message passing
        # message = torch.zeros(input.size(0), self.hidden_size, device=input.device)
        message = input
        message_mask = torch.ones(message.size(0), 1, device=message.device)
        message_mask[0, 0] = 0  # first message is padding

        for depth in range(self.depth - 1):
            if self.atom_message:
                # num_atoms x max_num_bonds x hidden
                nei_a_message = index_select_ND(message, a2a)
                # num_atoms x max_num_bonds x bond_fdim
                nei_f_bonds = index_select_ND(f_bonds, a2b)
                # num_atoms x max_num_bonds x hidden + bond_fdim
                nei_message = torch.cat((nei_a_message, nei_f_bonds), dim=2)
                # num_atoms x hidden + bond_fdim
                message = nei_message.sum(dim=1)
                message = self.w_h(message)  # num_bonds x hidden
            else:
                # num_atoms x max_num_bonds x hidden
                nei_a_message = index_select_ND(message, a2b)
                a_message = nei_a_message.sum(dim=1)  # num_atoms x hidden
                rev_message = message[b2revb]  # num_bonds x hidden
                message = a_message[b2a] - rev_message  # num_bonds x hidden

            message = self.gru(input, message)  # num_bonds x hidden_size
            message = message * message_mask
            message = self.dropout_layer(message)  # num_bonds x hidden

        if self.atom_message:
            # num_atoms x max_num_bonds x hidden
            nei_a_message = index_select_ND(message, a2a)
        else:
            # num_atoms x max_num_bonds x hidden
            nei_a_message = index_select_ND(message, a2b)
        a_message = nei_a_message.sum(dim=1)  # num_atoms x hidden
        # num_atoms x (atom_fdim + hidden)
        a_input = torch.cat([f_atoms, a_message], dim=1)
        atom_hiddens = self.W_o(a_input)  # num_atoms x hidden

        if mask is None:
            mask = torch.ones(atom_hiddens.size(0), 1, device=f_atoms.device)
            mask[0, 0] = 0  # first node is padding

        return atom_hiddens * mask
```
