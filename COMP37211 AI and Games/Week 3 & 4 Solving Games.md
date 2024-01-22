This covers more mathematical notation and algorithms on week 3 topics (includes some of the week 3 maths).
- If **Move sequence** is mentioned, this may be considered a type of **strategy** (to avoid confusion). 

- **Strategy set:** (self explanatory) - we try to pick the best move sequence out of the set based on the one we *think* has the best payoff.
- **Strategy profile:** Is a complete plan for playing a game; It's a set of move sequences, such that for every possible information set i.e. every possible in-game situation, we have a calculated move sequence.

- We denote pure strategies/move sequences $S_1,S_2$ etc.
- $U_1(S_1,S_2)$ is the payoff for player 1, given move sequences $S_1,S_2$; (we assume the player acts to pick the best one at each move).
Typically this is a heuristic estimate, unless we know our opponent's move sequence.

- $E[U(S_1,S_2)]$ - the **expected** payoff strategy set $(S_1,S_2); $S_1$ & $S_2$ may be mixed or pure strategies (if pure, then may select from strategies probabilistically). It is still useful for pure strategies, as choosing e.g. $S_1$ over $S_2$ is dependent on the opponents moves, which may be unknown/probabilistic.

#### Mixed Strategy Normal Form
given $i$ possible pure strategies/move sequences:
Assign a probability $q_i$ to choose move $i$, such that $0\le q_i\le 1$ & $\sum\limits_{\forall i}q_i =1$.
We get payoff $U_i$ with likelihood $Q_i$; expected payoff $=\sum\limits_{\forall i}q_iU_i$ 
### Calculating Mixed Nash Equilibrium
TODO mathematical definition
#### Example 1
We have a game:
![](misc/Pasted%20image%2020231028143636.png)

They choose their café probabilistically:
![](misc/Pasted%20image%2020231028143719.png)

We want to optimise donald's strategy - i.e. optimise $x$.

Let us assume eric has a *pure strategy* - chooses either red or blue café.

![](misc/Pasted%20image%2020231028144023.png)

We can see the ideal probability for each individual strategy for eric. However optimal strategy that cover's all of eric's possible (pure) strategies lies at the intersection point - $x=0.5$.
The mixed nash equilibrium lies here as the expected payoff for donald is the same no matter eric's move.
We have reduced this to a linear programming problem. Note we are assuming there is an equal likelihood for which strategy (red/blue) eric picks.

Mathematically:

![](misc/Pasted%20image%2020231028144401.png)

$\implies 1-2x = 2x - 1 \implies 4x=2 \implies x=0.5$.

#### Example 2: Rock-Paper-Scissors
If we modify the payoff of RPS:

![](misc/Pasted%20image%2020231028153604.png)

Let us have:

![](misc/Pasted%20image%2020231028153629.png)

- We want to calculate the probabilities for player 2 at Nash equilibrium. 

The expected payoff's for player 1 for each move ${R,P,S}$ are thus:

![](misc/Pasted%20image%2020231028153708.png)

player 2 needs to adjust $y_R,y_P,y_S$ such that player 1's payoff is always the same no matter their move:

![](misc/Pasted%20image%2020231028154243.png)

Otherwise, player 1 would adjust to always play the strategy that has the highest payoff.

After solving the system of linear equations we obtain:

![](misc/Pasted%20image%2020231028154441.png)

as the probabilities where $U^{(1)}=K$.
(and the same for $x_R,x_P,x_S$ due to the symmetry of the game.)


TODO epsilon closure -tends to actual value towards infinity

(ok, doing the problem sets & looking at the answers gives a better demonstration of finding equilibrium from normal form tables)