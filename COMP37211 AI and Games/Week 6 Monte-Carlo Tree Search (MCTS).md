https://www.youtube.com/watch?v=UXW2yZndl7U&t=310s

Tree overview: with a root node at the current game state, each node contains two values:
- The accumulated scores or wins count, for all nodes we have rolled out, $Q$; 
- The number of times the node has been visited / no. of times accumulated score has been updated, $N$.

For a node, $\frac{Q}{N}$ is the average score or win ratio for that node. 

MCTS with UCT tree policy & random rollout policy:
1.  Node selection
From the root node (which is the current game state), choose a leaf node to expand; We typically use the *UCT strategy*. A leaf node is one that has child nodes.

- UCB1 Formula for a node $v$: $\frac{Q_v}{N_v} + c\sqrt{\frac{\ln N_p}{N_v}}$, where $N_p$ is the visit count of the parent node. The algorithm balances exploitation moves (win ratio term) with exploration (second term, a child node that has been visited less will provide a higher term value). $c$ controls the balance between these; *theoretically equal* to $\sqrt{2}$ (according to the original [paper](https://arxiv.org/pdf/1402.6028.pdf)), & this value is commonly used. 
- UCT strategy: Choose the child node with the highest UCB1 value.

2. Expansion
(The definition here is shady)
We add all possible child nodes (for all possible moves). Note that when a node is initialised, its UCB1 = $\infty$ (as $N_v=0$). This means that during the selection process we will always select unvisited children first. We then select (first or randomly) one of the created child nodes for the next step:

3. Simulation
Calculate the estimated payoff for a node via Rollout (/Playout): From a node in a game tree, play the game to the end to obtain an estimated score. Choosing which moves to take may be *random*. May also use multiple random playouts to calculate an average payoff. You can also use other heuristics for choosing playout moves; AlphaGo uses a neural network to evaluate the score of a node.

4. Backpropagation
Work up the tree and visit all nodes on the way to the root node; if the child node $v$ has a rollout payoff $P_v$, then for a visited node $i$ add $P_v$ to its accumulated score $Q'_i \leftarrow Q_i + P_v$, and increment visit count $N'_i\leftarrow N_i+1$.

MCTS is commonly modified - *pure* MCTS uses pure randomness for its policies (but not very good).
There are multiple variations on the expansion method (*Tree Policy*), such as [flat UCB](https://arxiv.org/ftp/arxiv/papers/1408/1408.2028.pdf),epsilon-greedy; and calculating payoff (*Rollout Policy*) with [AMAF](https://users.soe.ucsc.edu/~dph/mypubs/AMAFpaperWithRef.pdf), [RAVE](https://www.cs.utexas.edu/~pstone/Courses/394Rspring13/resources/mcrave.pdf), or neural network like AlphaGo. You may also only partially expand a node with N child nodes (or a heuristic) if the search tree is very wide/for deeper searches.

MTCS can be easily parallelised in code:
![](misc/Pasted%20image%2020231109235045.png)

