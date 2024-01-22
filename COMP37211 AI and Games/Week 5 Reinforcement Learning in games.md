When to use machine learning over search algorithms like minimax:
- When we have a very large game tree to search
- We can't come up with a good evaluation function
- Hard to find Nash equilibrium - in:
	- Games of imperfect information
	- General Sum games
	- Games with large no. of players

Machine learning is preferred for games such as poker, chess, go...

### Reinforcement Learning
We use the outcome of the game to provide training feedback to our ML model.

- State - current position of play of the RL agent e.g. current board
- Actions - which change the state, e.g. moving pieces
- Rewards - Incentives to train /reinforce our model e.g. taking 'best moves', taking pieces, game score; have to evaluate these carefully as they affect the training quality of our agent. We can give a reward after every action (*immediate reward*) e.g. taking a piece; or *reward delay*, e.g. winning a game (& use a reward of 0 after intermediate actions).

![](misc/Pasted%20image%2020240121200046.png)

Typical RL agent setup:

![](misc/Pasted%20image%2020240121200126.png)

- Self play / *adversarial learning*: using two agents playing against each other such that they learn from each other;

![](misc/Pasted%20image%2020240121190108.png)


- *Epsilon-greedy policy*: Balance of using previously  'good' moves (i.e. exploitation) vs performing a random move (exploration in case we find a better move). Our $\epsilon$ value determines the probability of choosing between these two actions.

![](misc/Pasted%20image%2020240121190511.png)

Typically we decrease epsilon over time as our model becomes better trained and it is less likely to find better moves on random action.

![](misc/Pasted%20image%2020240121200221.png)

### RL Multi-armed bandit problem
(Our slot machines are our bandit's 'arms'.)

![](misc/Pasted%20image%2020240121190918.png)

(we have $n$ machines)

 Maintain a value table V:
![](misc/Pasted%20image%2020240121200359.png)

Epsilon-greedy choice of playing best-value machine vs exploration:

![](misc/Pasted%20image%2020240121200425.png)

#### Updating value table

- Method 1:

![](misc/Pasted%20image%2020240121200521.png)

(similar to gradient descent)

The positive rate machine will provide a positive $r_t$ value, whilst the rest will provide a negative value.
Machine can randomly give higher/lower rewards but on average i.e. over time we will tend to see the value match the machine win rate.

- Method 2:

![](misc/Pasted%20image%2020240121200812.png)

#### Mean Rewards proof (Method 2)

- $V[i]$ = (sum of $t_i-1$ elements) / $(t_i-1)$
- new mean should be (sum of $t_i$ elements) / $t_i$; which is 

$$\implies \frac{V[i](t_i-1)+r_t}{t_i} \implies \frac{V[i]t_i - V[i] + r_t}{t_i} \implies V[i] + (r_t-V[i])/t_i$$



Anyway from the example, we would see the following results in our value table:

![](misc/Pasted%20image%2020240121201907.png)

so 'arm 2' machine is the positive rate one.


### Tabular Learning
- State values: Table of $V(s_t)$ values - for each state e.g. a game board we would either evaluate the current board score, or look at all possible actions & choose the one with highest payoff. Used in *state-value learning, or V-learning*. Typically we use this if we want to estimate the value of being in a current state, e.g. in chess.

- State-actions pairs: $Q(s_t,a_t)$ - payoff for performing specific action with current state; used in *Q-learning*

V-learning is better when we have sparse rewards, and there is a large action space e.g. chess.

Q-learning is better when individual actions have clear unique rewards i.e. would be bad practice to combine rewards of all actions into a singular state; & if we have a smaller action space that is computationally manageable; applicable to *immediate reward RL* i.e. each action has an immediately observable reward.

#### Q-learning algorithm

![](misc/Pasted%20image%2020240121210518.png)

- We would have to look up all possible actions for current state in the table to perform epsilon-greedy
- update equation (1) uses time-decreasing learning rate to calculate mean payoff, whilst (3) uses fixed learning rate.

After training we can use the Q-table

![](misc/Pasted%20image%2020240121214115.png)

i.e. epsilon value of 0. Can also reduce table size after training by removing all non-optimal actions for a given state i.e. only one action per state.

#### Q-function approximation

Consider a game of poker: 5 card hand, and we can choose to replace or not replace any card. We have a space of:

- $2^5=32$ actions
- $\binom{52}{5}=2,598,960$ states, all equally as likely to occur!

It is unfeasible to have this many states (or actions if it was the case); (also V-learning will have the same problem here).

Therefore we need to approximate our input to a smaller dimension / table.

- use a hand-coded representation; card hand type e.g. flush, straight, which cards are not part of a 'hand'. We have 'equivalent' hands represented by the same table entries.

- Use supervised learning e.g. neural network, SVM to replace our Q-table. We can then use this to produce an expected reward i.e. we have a regression problem to train - this is our *Q-function approximation*.

![](misc/Pasted%20image%2020240121214301.png)

with the following training algorithm:

![](misc/Pasted%20image%2020240121214352.png)

i.e. we use the estimated reward from model in place of the $Q(s,a)$ value.


### Temporal-Difference (TD) Learning

Allows us to use Q-learning in cases where we don't have immediate reward e.g. a board game.

Now we change our definition of the value of $V(s)$ or $Q(s,a)$ is now changed to be a combination of the current/immediate reward, + *future reward*:

![](misc/Pasted%20image%2020240121222930.png)

How to calculate 'future reward'?

![](misc/Pasted%20image%2020240121223019.png)

#### Horizons
- Finite horizon: we only consider a finite number of steps (& rewards) into the future, e.g. tic-tac-toe has max 9 moves; or we may limit look-ahead (e.g. chess)
- Infinite horizon - there is not theoretical game end; e.g. using RL for investment portfolio's

Discounted rewards:
1. Encourage finite-horizon behaviour i.e. prioritises more short-term gains / immediate rewards - its a good idea to choose moves to win a board game sooner.
2.  We are guaranteed convergence in the infinite horizon case i.e. our RL model wont choose to aim for a reward in the infinite future (e.g. invest but never sell stocks)

#### Learning from future reward

The value at a given state is given by

$V(s_t) = r_t + \sum\limits_{t' = t+1}^{\text{game end}}r_{t'}\gamma^{t'-t}$
which is equivalent to
$V(s_t) = r_t + \gamma V(s_{t+1})$.

(btw this **isn't** the equation to update V-learning table values, just he equation to calculate the immediate + future reward).

In Q-learning, we assume a greedy policy - *off policy* approach: Always choose/exploit the best action/move, when choosing the next action to fetch future reward for.


Therefore our update equation for our Q-table with TD learning is:

![](misc/Pasted%20image%2020240121224850.png)

With this learning formula, $Q(s,a)$ converges to the expected future reward(s) (assuming best actions are taken to maximise reward), discounted by time to it (& parameter $\gamma$).

For the following game, if we perform RL for infinite time; for all actions from states their Q values will converge to values:

![](misc/Pasted%20image%2020240121225446.png)

We can guarantee Q-learning converges to the correct answer if:
- We visit every possible state
- Run for infinite time

However, we don't always want to visit every state, as some states can be seen as useless pretty quickly


#### Applications to games

Typically we only update $V(s_t)$ after an opponent has made a move, as they influence the immediate reward $r_t$ (However we still assume best moves after this).

### TD($\lambda$) Learning

- TD($\lambda$) learning, when a reward is received, we don't just update the current state / state-action pair, but previous ones before that aswell; but with reduced updated reward the further we go back, parameterised with $\lambda$ value.
- Q-learning is an example of TD(0) Learning.
- Algorithm:
- 
![](misc/Pasted%20image%2020240121231925.png)

TD($\lambda$) learning improves learning speed (over Q-learning) as we update more values at once

#### Eligibility traces
[Useful site](http://www.incompleteideas.net/book/ebook/node72.html)

In $TD(\lambda)$ learning, our elegibility trace parameter is $\lambda$. another way to view the TD($\lambda$) algorithm is to calculate 'elegibility' at each time step:

![](misc/Pasted%20image%2020240121233613.png)

The value of which we then use in this algorithm which is equivalent to the one above:

![](misc/Pasted%20image%2020240121233745.png)

The concept of eligibility traces are used elsewhere such as other algorithms e.g. SARSA or neuroscience for synaptic plasticity.

#### TD-Gammon
[paper](https://www.csd.uwo.ca/~xling//cs346a/extra/tdgammon.pdf)

![](misc/Pasted%20image%2020240121231947.png)

TD gammon has very good performance - 

![](misc/Pasted%20image%2020240121233955.png)

![](misc/Pasted%20image%2020240121234430.png)

(for point 1., TD-gammon was greedy / epsilon = 0)