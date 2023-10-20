TODO add more maths examples if needed.

#### Best Response
Given a **fixed opponent strategy**, is the strategy that rewards the player with the best outcome.

For a *finite* strategy space, we can simply check the outcome of all strategies. For an *infinite* (or very large) strategy space this is not possible.

#### Game Dominance Strategy
A game strategy that always ensures a better outcome of other players regardless of other players strategy, though this is different to nash equilibrium & having a solved game.

Distinctions:
- Dominant strategy - ensures you win, independent of opponent strategy. But is not the *best* strategy.
- Strategy at nash equilibrium - We can get the best possible outcome (possibly higher than a dominant strategy.), **dependent** on the opponent's strategy which wont change.
- Solved game strategy - Best possible outcome for **any** opponent's strategy.

(IMHO should be concerned more with game search algorithms)

#### Nash equilibrium
Each player makes the best decision possible, knowing their opponent's strategies; and no player has an incentive to change their strategy (for a better outcome) given their opponents strategies (this is the equilibrium). **Is not** having a best strategy/solved game - an opponent may have a non-optimal strategy.

**Solving a zero-sum game of perfect information & no chance** means finding the Nash equilibrium of that game.

#### Mixed strategy
Used for games that don't have a *pure (deterministic) strategy* nash equilibrium.

A mixed strategy uses a randomised & probabilistic combination of strategies; Ideally assign probabilities to maximise expected payoff.

#### Mixed-strategy Nash equilibrium
At least one player has a mixed strategy, and no player will change their strategy to increase their *expected* payoff in this case. Are useful from preventing opponents from being able to predict your moves.

#### Nash's existence theorem 
For any game, of finite no. of players and strategies, then there is at least one nash equilibrium, for either a pure or mixed (probabilistic) strategy.

#### General-sum games
The total payoff summed across all players can vary by outcome. Different to a non-zero game; the total payoff for a non-zero sum game is not zero, but may still be the same value for all outcomes.

#### Minimax algorithm
Algorithm for choosing good strategy for a *two player* game, by choosing the path that *maximises* the *minimum* payoff: it assumes the other player always takes the best strategy.

**Alpha-beta pruning**: Method applied to minimax to remove nodes in the minimax search tree that are worse than previously examined moves.
We can improve pruning with heuristics, e.g. **killer heuristic** - test for pruning using moves that cause cutoff at other parts of the tree, as they are likely to cause more cutoff.

#### Alternative game search algorithms:
- SSS* algorithm
- negaScout/Principal variation search
- Monte carlo search algorithm - used widely in board game/chess solvers such as *AlphaGo*.

(Go research the algorithm details yourself, have already done minimax/alpha-beta in AI & games.)













