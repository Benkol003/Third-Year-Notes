### Types of games

#### Nim-N Game
Starting with N matches, each player takes turns to take 1-3 matches. Player who takes the last match loses.
This is a *zero-sum* game.

- **Zero sum game**: The total gains/losses, or scores, for all players add up to zero i.e. one player's win is another's loss. There is no net benefit or loss to the entire game after completion. This does not concern the mechanics of the game but the actual outcome - there must be a winner and a loser.
- **Non zero sum game**: It is possible for all players to win. An example is the *prisoner's dilemma*. If a prisoner snitches they will get less jail time at the expense of the other. However if they both remain silent then they will get the least accumulated jail time.

#### Prisoner's Dilemma: A non-zero sum game

| (P1,P2) score | P1 Snitches | P1 Quiet |
| ------------- | ----------- | -------- |
| **P2 Snitches**   | (3,3)       | (5,0)    |
| **P2 Quiet**      | (0,5)       | (1,1)    |

If a prisoner snitches it is not always the case that their jail time is reduced.

**Game of perfect information**: A player knows what node of the game tree it is in. i.e. we know the other player's moves. Poker is a game of **imperfect information** as we cannot see other player's hands before they are played.

**Information set**: Set of possible nodes a player could be at given their current (imperfect) information. Taking an action is seen as having the same possible effect on the game even it is better or worse at different nodes in the information set.

**Game of chance**: Such as poker, there is an element of randomness. All games of chance are also games of imperfect information.


### Game Representations

#### Game in Normal Form
This represents the final scores of players for a given sequence of moves in a form of a matrix, where each entry denotes the final score. This is used for games of simultaneous play; Or for sequential games the sequence of moves is chosen in advance and we do not consider any kind of in-game strategy.

Normal form games are of *imperfect information* - we dont know what move the opponent will make. Both players have to choose a move at the same time. (however is not a game of chance).

Rock-Paper-Scissors game in normal form:

![](misc/Pasted%20image%2020231005121933.png)

Normal form is mathematically convenient, (as we have all possible moves), but computationally inefficient - it is impossible to do so for chess.

#### Game Tree
This is also known as *extensive form*.
Maps all possible choices players can make in a game from start to finish, with a resulting scored outcome.

Rock-Paper-Scissors game tree:

![](misc/Pasted%20image%2020231005121239.png)



### Optimal Path for graph traversal

#### Dijsktra's algorithm
Finds the shortest/most optimal path in a **Graph** (not just trees). Will visit every node in a graph and calculate the optimal path to all nodes from the start node.

Given a start node:
1. Mark all nodes as having $\infty$ distance from start node, except for start node, which =0, and not having a node to precede it in the optimal path.
2. Put start node in visited set, and all other nodes in unvisited set Q.
3. Given the current node, u, update the distances of all of it's *unvisited* neighbours, $\forall w_i$ Calculate their distance from the 
	start node, through the current node u, as $D(u)+W(w_i)$ (distance + weight at edge). 
	If this calculated distance $D_u(w_i)$ is smaller than it's previously recorded distance, then update it, and record u as the node to precede it in the optimal path.
	
	Place the current node in the visited set.
4. From the set of unvisited nodes, choose the node with the lowest distance as current node u. Goto step 3 and repeat until all nodes are visited.
5. To find the optimal path to a node, start there and traverse the recorded predecessor nodes to the start node for the optimal path.


#### A* Algorithm
Adapted from Dijkstra, uses a *heuristic* such that we don't have to visit every node. Only finds the optimal path between a start and end node (& not to all all nodes).

**Heuristic**: An estimate (typically as a mathematical function) to some sort of variable or score in a situation.

- The A* Heuristic: **Must** be an estimate of the *minimum distance* from the current node to the end node, i.e. the actual distance must be *at least* the value of the heuristic.
- If we cannot guarantee that the A* heuristic is **always** an underestimate, then the A* algorithm will fail to find the optimal path.
- Example of A* heuristic: For map traversal (e.g. google maps) we can use the direct '*as the crow flies*' distance (in a straight line) to the end node. All paths are guaranteed to be at least this length.
- A* Algorithm is almost identical to Dijkstra except:
	1. Must find a path (but not optimal) from start to end node
	2. For the current node u, we calculate its minimum path distance to the end node as $D(u)+H(u)$ - distance to u + heuristic from u to end node. If this is greater than the distance found for the previous best path then we can guarantee that no path through u will be optimal and can discard it to the visited set (don't consider it's neighbours).

### Game Solving

**Perfect Play**: A strategy that can guarantee the best possible outcome for a player from a position, irrespective of any moves that the opponent chooses. Not necessarily a win, may only e.g. guarantee a draw or a certain score, but guarantee's obtaining that nonetheless.

**Winning Strategy** & **Draw-ensuring strategy**: A strategy that guarantees those outcomes. More Formally they guarantee a positive score / score of at least 0 respectively.

**Zermelo's theorem**: For a finite two-person game of perfect information (& $\therefore$ no chance), we can guarantee one of the players has a winning strategy, or both players have a draw-ensuring strategy.

#### Solved Game
A game whose outcome (of win, loss, draw) can be determined from any position in the game, *assuming* both players play perfectly

Levels of game solving:
- **Ultra-weak**: Can prove the outcome of a game for a player given their starting position, assuming perfect play. Do not necessarily need to consider all moves of play.
- **Weak**: Have an algorithm to secure a win or draw for a player from any initial position, given any possible moves by their opponent.
- **Strong**: Can Provide an algorithm that can secure a win/draw from *any* point in the game, even if the player has previously made mistakes.

#### Hex 
This is the game that will be developing an AI to play. It has a very large state space: For 11×11 Hex, the state space complexity is approximately $2.4×10^{56}$.
Hex is *ultra-weak solved:* The first player is guaranteed a win if they perfectly play. (Draws cannot occur).

Safely Connected Patterns: Two groups of filled hex's arranged such that we can always connect them.
Hex strategy usually involves building blocks of safely-connected patterns to connect together into a full path to win the game.

### Generative Adversarial Networks
***(Note: this is only briefly mentioned in the lectures)***

A *G.A.N.* uses a *generative network* and a *discriminative network* that compete against each other.

- **Generative Network**: Takes real data as input; Attempts to generate 'false' data and mix it in with the provided data on the output.
	- The generative network takes random noise as an input; this may be accompanied by some input data (e.g. text prompt) for context in the case of *Conditional generative networks.*
- **Discriminative Network**: Takes the data provided by the generative network, and attempts to distinguish between what is *real data* and what is *false data*.
	- Classification error is backpropagated through the network. Again it may be a conditional network and take some context data input.

These two networks compete and penalize against each other as they train simultaneously. Ideally the two networks will reach an equilibrium point where they cannot be trained any further.

- **Mode collapse**: An issue in GAN's where it fails to generalize properly. E.g. for a MNIST dataset the generative network may only learn to produce zeroes.

Once trained, the generative network can be used as generative AI (e.g. generating an image from a text prompt). The discriminative network can be used for usual classification tasks.






 















