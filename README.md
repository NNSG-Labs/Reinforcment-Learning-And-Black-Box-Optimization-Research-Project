# Reinforcment-Learning-And-Black-Box-Optimization-Research-Project

## What is Reinforcement Learning?

You've probably all seen the agent-environment interaction loop when being introduced to RL.
It's basically a problem formulated with two moving parts: the agent, and the environment.
The agent can take an action affecting the environment, then the environment changes and gives the agent a new state/observation and a reward, and the agent in turn takes a new action, ad infinitum.
The agent's goal is to find a policy (mapping from state to actions) that maximizes the amount of reward obtained from the environment.
Any problem that can be formulated as a state transition and reward function (i.e. a function mapping from a starting state and action to a new state and reward) can be treated as a reinforcement learning problem.

![RL Loop](https://raw.githubusercontent.com/NNSG-Labs/Reinforcment-Learning-And-Black-Box-Optimization-Research-Project/master/figs/rl_loop.png)

One of the simple algorithms you'll be introduced to when starting RL is SARSA. 
The figure below is the algorithm as described in Richard Sutton's RL textbook.

![SARSA](https://raw.githubusercontent.com/NNSG-Labs/Reinforcment-Learning-And-Black-Box-Optimization-Research-Project/master/figs/sarsa.png)

We see that SARSA as described in this figure has two tunable hyperparameters: the exploration randomness (epsilon), and the learning rate (alpha).
Finding the values for these hyperparameters which lead to the best performance is what we call hyperparameter optimization.

## Evauating RL Algorithms

Given an environment and a reinforcement learning algorithm, how do we determine how good the algorithm is?
When we run the algorithm on the environment, we get back a sequence of rewards, which we can plot out to obtain a figure like the one below.

![SARSA](https://raw.githubusercontent.com/NNSG-Labs/Reinforcment-Learning-And-Black-Box-Optimization-Research-Project/master/figs/perf_vs_time.png)

Given two of these curves, it's not immediately obvious which algorithm is better uness one algorithm is better than the other at every time step.
We can compute several different values, all of which are valid performance measures.
Some examples are:
* Average reward
  * Averaged over iterations
  * Averaged over wall clock time
  * Averaged over episodes
* Total reward
* Total discounted reward
* Reward after a fixed number of training iterations

## Hyperparameter Optimization

Hyperparameter optimization is a very expensive process.
It typically involves evaluating the algorithm on many different hyperparameters, then choosing the one with the best performance.
If we plot out the performance with respect to one of the hyperparameters, we will probably see like this:

![SARSA](https://raw.githubusercontent.com/NNSG-Labs/Reinforcment-Learning-And-Black-Box-Optimization-Research-Project/master/figs/perf_vs_param.png)

One thing we can probably safely assume about the performance vs hyperparameter curve is that it is continuous or smooth.
That is, if we change the hyperparameter a little, we expect a small change in the performance.
With that assumption, we can then ask, how smooth is it?
I'm hypothesizing that properties like smoothness are related to properties of the MDP that can be calculated.
Knowing these values ahead of time will allow for a much more efficient hyperparameter search.

Bayesian optimization is a popular way of doing hyperparameter optimization.
The essential idea is that we evaluate the algorithm on a small set of hyperparameters, then the performance vs hyperparameter curve is approximated using a Gaussian process.
An example of a Gaussian process with two points is shown in the figure below.

![SARSA](https://raw.githubusercontent.com/NNSG-Labs/Reinforcment-Learning-And-Black-Box-Optimization-Research-Project/master/figs/gp.png)

The nice thing about Gaussian processes is that they make use of a smoothness assumption to give us an uncertainty estimate.
So given an arbitrary set of hyperparameters, we can use this Gaussian process to compute an upper and lower bound on the performance.
This allows us to search the space more efficiently, because we can focus on hyperparameters that are likely to give us better results.

The smoothness assumptions of a Gaussian process is baked into choice of kernel function.
This is usually a Matérn covariance function, because they seem to work well enough for everything.
This Matérn kernel is a parametric function, so that's another parameter we have to choose.

## Questions

* What setting for the Matérn kernel works best for RL?
* Does it vary depending on the problem?
  * If so, by how much? How big is the range?
* What about other parameteric covariance functions?
* Can we design another covariance function that work better for RL?
* Do any of these parameters correlate with properties of the MDP? Properties such as connectedness, mixing time, eigenvalue, etc.
