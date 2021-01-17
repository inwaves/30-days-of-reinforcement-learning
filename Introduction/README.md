## Introduction

### Learning Goals

- Understand the Reinforcement Learning problem and how it differs from Supervised Learning


### Summary

- Reinforcement Learning (RL) is concerned with goal-directed learning and decision-making.
- In RL an agent learns from experiences it gains by interacting with the environment. In Supervised Learning we cannot affect the environment.
- In RL rewards are often delayed in time and the agent tries to maximize a long-term goal. For example, one may need to make seemingly suboptimal moves to reach a winning position in a game.
- An agent interacts with the environment via states, actions and rewards.


### Lectures & Readings

**Required:**

- [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/RLbook2018.pdf) - Chapter 1: The Reinforcement Learning Problem
- David Silver's RL Course Lecture 1 - Introduction to Reinforcement Learning ([video](https://www.youtube.com/watch?v=2pWv7GOvuf0), [slides](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching_files/intro_RL.pdf))
- [OpenAI Gym Tutorial](https://gym.openai.com/docs)

**Optional:**

N/A


### Exercises

Exercises from Chapter 1:

- Suppose, instead of playing against a random opponent, the reinforcement learning algorithm described above played against itself, with both sides learning. What do you think would happen in this case? Would it learn a different policy for selecting moves?
  - I don't feel as though I have a good intuition. The rewards of the states would change, because the opponent would be anticipating your moves. If the rewards change, your policy changes. You would learn to plan differently.
- Many tic-tac-toe positions appear different but are really the same because of symmetries. How might we amend the learning process described above to take advantage of this? In what ways would this change improve the learning process? Now think again. Suppose the opponent did not take advantage of symmetries. In that case, should we? Is it true, then, that symmetrically equivalent positions should necessarily have the same value?
  - Maybe observing one state as having a certain reward value gives us information about 3 other, symmetrical states' reward values. That means we wouldn't "trade places" with them, since they're equivalent. It reduces the size of the action space. If you have fewer actions to consider, you run faster, but that doesn't mean you do better in the long run. If the opponent doesn't take advantage of symmetries—they don't understand that there are really "duplicate" states—then they will play differently when they should have played the same. This opens them up to exploitation, so we shouldn't model symmetries either, so we can take advantage of the opponent's mistakes.
- Suppose the reinforcement learning player was __greedy__, that is, it always played the move that brought it to the position that it rated best. Might it learn to play better, or worse, than a nongreedy player? What problems might occur?
  - The problem of local optima: maybe to get to the absolute best position you have to take a temporarily worse position. A greedy player would not be able to do this, which means it might not win in that case. If the layout of the game is such that you never need to do this, a greedy player is at least as good as a sophisticated nongreedy player, possibly better. Because noughts-and-crosses is a simple game, and there are no trade-offs to make, being greedy pays off.
- Suppose learning updates occurred after all moves, including exploratory moves. If the step-size parameter is appropriately reduced over time (but not the tendency to explore), then the state values would converge to a different set of probabilities. What (conceptually) are the two sets of probabilities computed when we do, and when we do not, learn from exploratory moves? Assuming we continue to make exploratory moves, which set of probabilities might be better to learn? Which would result in more wins?
  - So if I take my exploratory actions into account -> one set of probabilities of states
  - If I don't take xplo actions -> another set of probabilities
  - My intuition here is that if I do learn from those moves, I realise the value of exploration, i.e. as in the previous question on the greedy agent I learn that it's possible to take a suboptimal action __now__ and still have higher expected value than if I greedily maximised the reward from each action at each timestep. If we continue to explore, we would rather know when/how exploration pays off, so I think we should compute the probabilities including explo moves. That should result in more wins across an average of plays—though maybe sometimes, depending on the environment, it would lead to losses/less than the maximum reward.
- Can you think of other ways to improve the RL player? Can you think of any better way to solve the tic-tac-toe problem as posed?
  - Because TTT has a relatively small action space, we could use a planning algorithm like [[Monte Carlo Tree Search]] to find the answer. We could even do something like [[A* search]]. 
  - I really don't have good intuitions about how to improve the player other than hand-engineering features and heuristics for it to learn, which is probably not the point here. You could set a very large negative reward for states where the opponent has 2-in-a-row, so that you always block.

- [Work through the OpenAI Gym Tutorial](https://gym.openai.com/docs)
