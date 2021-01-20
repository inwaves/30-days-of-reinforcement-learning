## MDPs and Bellman Equations

### Learning Goals

- Understand the Agent-Environment interface
- Understand what MDPs (Markov Decision Processes) are and how to interpret transition diagrams
- Understand Value Functions, Action-Value Functions, and Policy Functions
- Understand the Bellman Equations and Bellman Optimality Equations for value functions and action-value functions


### Summary

- Agent & Environment Interface: At each step `t` the agent receives a state `S_t`, performs an action `A_t` and receives a reward `R_{t+1}`. The action is chosen according to a policy function `pi`.
- The total return `G_t` is the sum of all rewards starting from time t . Future rewards are discounted at a discount rate `gamma^k`.
- Markov property: The environment's response at time `t+1` depends only on the state and action representations at time `t`. The future is independent of the past given the present. Even if an environment doesn't fully satisfy the Markov property we still treat it as if it is and try to construct the state representation to be approximately Markov.
- Markov Decision Process (MDP): Defined by a state set S, action set A and one-step dynamics `p(s',r | s,a)`. If we have complete knowledge of the environment we know the transition dynamic. In practice, we often don't know the full MDP (but we know that it's some MDP).
- The Value Function `v(s)` estimates how "good" it is for an agent to be in a particular state. More formally, it's the expected return `G_t` given that the agent is in state `s`. `v(s) = Ex[G_t | S_t = s]`. Note that the value function is specific to a given policy `pi`.
- Action Value function: q(s, a) estimates how "good" it is for an agent to be in states and take action a. Similar to the value function, but also considers the action.
- The Bellman equation expresses the relationship between the value of a state and the values of its successor states. It can be expressed using a "backup" diagram. Bellman equations exist for both the value function and the action value function.
- Value functions define an ordering over policies. A policy `p1` is better than `p2` if `v_p1(s) >= v_p2(s)` for all states s. For MDPs, there exist one or more optimal policies that are better than or equal to all other policies.
- The optimal state value function `v*(s)` is the value function for the optimal policy. Same for `q*(s, a)`. The Bellman Optimality Equation defines how the optimal value of a state is related to the optimal value of successor states. It has a "max" instead of an average.


### Lectures & Readings

**Required:**

- [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/RLbook2018.pdf) - Chapter 3: Finite Markov Decision Processes
- David Silver's RL Course Lecture 2 - Markov Decision Processes ([video](https://www.youtube.com/watch?v=lfHX2hHRMVQ), [slides](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching_files/MDP.pdf))


### Exercises

- Devise three example tasks of your own that fit into the MDP framework, identifying for each its states, actions, and rewards. Make the three examples as different from each other as possible. The framework is abstract and flesible and can be applied in many different ways. Stretch its limits in some way in at least one of your examples.
  - A merchant in a local bazaar has the goal of maximising his profit given his material and time investment. Each state could encompass the size of his inventory, and measurements about the frequency of customers, or how much they purchase. His actions could be increasing/decreasing the stock in certain items, moving location, finding a way of advertising, etc. I think in this case an infinite-horizon discounted return can be used, because money now is worth more than money in the future ("the time value of money"). I'm not sure whether you __can__ say "infinite", though, since the merchant's life itself is limited and it's likely that he will not be in charge of the store throughout it.
  - A horse in a single episode of a race has the "sole" goal of winning the race. The abstraction in this case is interesting, because we can't know what the horse's instrumental goals are, how it perceives its reward, or how it "thinks" about its actions. The reward might be how far ahead of other horses it is, or how close to the finish line it is, or it may decrease per timestep, penalising the horse the longer it takes to finish the race. The actions may be increasing or shortening their stride, taking a deeper breath, digging in their hooves more. An internal state of the horse's may take into account how it's doing in the race as well as measurements of their energy levels: how much longer they can keep going, or whether they can speed up or not. Fascinatingly, it may also encompass some sort of emotion about the race itself. Does the horse feel anxiety? Resolve? Is it disappointed when it loses? 
  - A smart car of the future—and perhaps some EVs today—may want to maximise the range it can reach on a single fuel charge. It may tweak controls of the various systems in the car, like torque, how much fuel passes through the engine, whether it should coast or accelerate etc. It could also "understand" trade-offs, where it may be important for the passengers to reach a certain destination quickly, at the expense of fuel—an ambulance, for example. The states might contain a satellite position and many data points regarding the car itself. They should also probably contain visual information about the environment, if it is self-driving. The reward may be in terms of fuel left at destination, or time elapsed per trip.
- Is the MDP framework adequate to usefully represent __all__ goal-directed learning tasks? Can you think of any clear exceptions?
  - Intuitively, no, and there rarely exist formalisations that don't have corner cases. Finite MDPs are not applicable in continuous, infinite environments, and you could posit that the Universe is such an environment. The reason they work is that they're probably the next-best thing—as faithful an approximation of the __real__ formalisation/mechanism of the Universe as we can get.
  - One thing that still puzzles me is the Markov property for states, and how it is possible to encode all the useful information from the history in the current state. If the agent state and the environment's state are the same, i.e. the agent knows __everything__ about the environment, down to atomic layouts, it may in a sense capture all the useful information. But even so, what about cyclic events? Would an agent be able to notice those if it only paid attention to the way the world is now? Beyond that, agent states and environment states are rarely one and the same, in practice, and specifically in complex environments. The agent can't really know all atomic layouts, so by definition the state no longer has the Markov property. Some information is never noticed!
- Consider the problem of driving. You could define the actions in terms of the accelerator, steering wheel, and brake, that is, where your body meets the machine. Or you could define them farther out—say, where the rubber meets the road, considering your actions to be tire torques. Or you could define them farther in—say, where your brain meets your body, the actions being muscle twitches to control your limbs. Or you could go to a really high level and say that your actions are your choices of __where__ to drive. What is the right level, the right place to draw the line between agent and environment? On what basis is one location of the line to be preferred over another? Is there any fundamental reason for preferring one location over another, or is it a free choice?
  - If we take this very example and look at it deeply, it reaches into big questions around free will, and what it means to be a person. Is the person the human body? Is the person the thoughts inside their mind, and is their mind the brain? Without veering off track, those are questions where there is still no consensus. If we take the environment to be anything that the agent cannot changed arbitrarily,  
        - we should not take the tyres as part of the agent, since there are other factors, that do not depend on the agent which can influence how the tyres work. (Think road condition, tyre condition)
        - we possibly should not take the car controls as part of the agent, either, because even though we usually consider our bodies within our control, it is possible to lose control of them temporarily or permanently. (For example, having a seizure, falling asleep, having a leg twitch or cramp that makes you accelerate inadvertently).
        - we could draw a reasonable line at the person's brain, because it encompasses both the high-level decisions around where to go and the automatisms, the engrained habits that comprise "driving" (and which they presumably learned some time before).
- Give a table analogous to that in example 3.3, but for $$p(s', r |s, a)$$. It should have columns for $$s, a, s', r$$ and $$p(s', r | s, a)$$, and a row for every 4-tuple for which $$p(s', r | s, a) > 0$$
  - Table is [here](https://docs.google.com/spreadsheets/d/1VCYbBD0rx2u_6In7mBVsrFuuTsbQtyhrlxNqZtcJMPQ/edit?usp=sharing).
