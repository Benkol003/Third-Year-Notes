In the minimax search algorithm, we assume that each player chooses the move with the best payoff; & that they do not act probabilistically.
If you can fully search the game tree i.e. calculate payoff exactly, then minimax will always find the optimal strategy; however more often we estimate payoff with a heuristic (e.g. chess), & therefore an opponent may make better moves (as the algorithm hasn't searched deep enough to discover them).

### Assumptions of applying minimax
For a general-sum game of two player & no chance, minimax can find the pure Nash equilibrium. WE can apply minimax to other games to still find a good strategy, however we lose the ability to find the Nash equilibrium, as the game assumptions do not hold.
#### More than two players
If there are 3 or more players, assume we have one *MAX* player (that we are searching a strategy for); assume all other opponents are *MIN* players; Not true as opponents will also play against each other. However still considers the worst case for the *MAX* player.

#### Game of chance
What if the opponent plays with chance, rather than taking the best move? 
**Expectimax**: For *MIN* nodes, we calculate the expected value of the children nodes i.e. assume the opponent will take any move randomly. For a *MAX* node we choose the highest value as normal.
The expected value could be calculated using probabilities/weights of equal value, or a model, e.g. calculate from the payoff / payoff heuristic.

#### Game of imperfect Information
Use **Counterfactual Regret Minimization**. (Out of course scope).

### Win-Loss Trees
Instead of a payoff score we just have win/loss. Allows for faster evaluation as we don't need to evaluate all nodes:

![](misc/Pasted%20image%2020231028183913.png)

![](misc/Pasted%20image%2020231028183934.png)

![](misc/Pasted%20image%2020231028183952.png)
### Minimax Algorithm
For two players: player 1 as the *MAX* player, player 2 as the *MIN* player - trying to maximise/minimise their final score.

![](misc/Pasted%20image%2020231028172810.png)

For minimax the best move at a node is the one that produces the best payoff for the player given the child nodes; Minimax does depth-first search, backtracking to calculate the best move for nodes starting at the bottom of the tree.

### Alpha-Beta pruning
We start with $[\alpha,\beta]=[-\infty,\infty]$ ; When $\beta\le\alpha$ prune the node/tree we are searching.
In this context, score is evaluated for the current game state/for both players - e.g. the total number of points, where one player has negative points.
- $\alpha$: Min. score the *MAX* player is guaranteed i.e. we update with the max score found so far
- $\beta$: Max score the *MIN* player is guaranteed i.e. we update with the min score found so far

If $\beta\le\alpha$, then we have a move that is 'too good' to be chosen; for example:
if we are at a max node $X^{\max}$, & we find a higher scoring move, and update $\alpha$ to then have $\beta\le\alpha$; The previous parent *MIN* node, ($Y^{\min}$) already has a best move of value $\beta$; $X^{\max}$ will never be chosen, as we can guarantee that $X^{\max}$ will always be a worse choice for $Y^{\min}$; as due to that $\alpha$ *can only increase* and $\beta$ *can only decrease* as we search the tree more. Therefore discard $X^{\max}$ & backtrack to continue evaluating $Y^{\min}$.

### Evaluation Functions
For complex games, it's computationally impossible to search the entire game tree (e.g. chess); therefore we only search to a certain depth; This makes it imposssible to calculate actual payoff; therefore we use a heuristic or *evaluation function*.

#### Example Heuristics
- Tic Tac Toe:
![](misc/Pasted%20image%2020231028185155.png)

- Connect 4:
![](misc/Pasted%20image%2020231028185227.png)

- Othello:
  ![](misc/Pasted%20image%2020231028185255.png)
  Hard to choose a good heuristic.
  - use trial-and-error - pit them against each other and use the one that wins.
  - Combining heuristics or ideas. E.g. for chess we can have aggressive & defensive heuristics:
  ![](misc/Pasted%20image%2020231028185533.png)
### Stochastic Hillclimber
Algorithm to find optimal weights for our heuristics. A hillclimber algorithm locally searches the parameter space & moves in the direction with the best improvement; *stochastic hillclimber*is the most basic as it searches local parameter space randomly.

For a set of $n$ heuristics $\{e_1,e_2,\dots e_n\}$, & a training rate $\epsilon$:

1. Initialise & normalise the weights for the combined heuristic $H=\sum\limits_{i=1}^nw_ie_i$ .
For N iterations / given time duration:
	1. Generate $n$ zero-mean weights: generate $n$ random values $\{r_1,\dots r_n\}$; calculate the mean $\bar{r}$ and remove from weights: $\hat{r}=\{r_1-\bar{r},\dots r_n-\bar{r}\}$
	2. New test heuristic $H'=\sum\limits_{i=1}^n(w_i+\epsilon r_i)e_i$ 
	3.  If $H'$ performs better than $H$ then $H\leftarrow H'$. 

However, sometimes finding a good heuristic is impossible - this is the case for the game Go; In this case then will use reinforcement learning.