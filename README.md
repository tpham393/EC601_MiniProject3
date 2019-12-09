# EC601_MiniProject3
Disclaimer: Algorithm code was modified/updated from previous code I had already written. Code for rendering and testing the OpenAi Gym environment is newly written.

## Technology Requirements
To run the code contained in this repo, you will need to install the following:
- Open Ai Gym:
```
pip install gym
```
- Python 3.5+
- [JupyterNotebook](https://www.anaconda.com/distribution/)

## Purpose of Mini Project
To explore the differences between the Q-Learning and Value Iteration algorithms for Reinforcement Learning as applied to an OpenAi Gym environment, the TaxiCab game (v.3). Pseudocode for both algorithms can be found [here](https://towardsdatascience.com/introduction-to-various-reinforcement-learning-algorithms-i-q-learning-sarsa-dqn-ddpg-72a5e0cb6287).

## Description of TaxiCab Environment
The TaxiCab game takes place on a simple grid. There are four locations on the grid, each denoted by a different letter. When the game is initialized, the passenger's location, destination, and the taxi's location are randomly selected. The taxi's/agent's goal is to pick up and drop off passengers in as few time steps as possible. A complete description of the environment information can be found [here](https://github.com/openai/gym/blob/master/gym/envs/toy_text/taxi.py). The complete description breaks down the state space, the action space, and the rewards.

## Goal
The goal of both algorithms is to determine a(n optimal) policy for the agent (i.e. the taxi) to follow. The policy = the action that the taxi should take for every state; in other words, given a random starting state, what path should the taxi take to maximize the total score.

## Value Iteration Algorithm
The Value Iteration algorithm answers the question of how to find the optimal policy when the agent has information about the environment, i.e. the state transition probability matrix and the reward function. In this case, the problem of learning the optimal policy is reduced to a "planning problem." The Value Iteration algorithm uses the Bellman Equation to approximate the optimal Value Function, V(s), i.e. the maximum values for each state. The value can be interpreted as the "probability of winning the game," and is the expected total return given that the game starts in a specific state. Once the optimal Value Function is approximated, the optimal policy can be extracted by choosing the action that maximizes the value for each state.

## Q-learning Algorithm
Unlike in the previous case, the Q-learning Algorithm addresses the problem of "how do you learn the optimal policy when you do not have all the information about the environment?" In this case, the state transition probability matrix is unknown, but the rewards are known. The Q-learning algorithm can be interpreted as a stochastic version of the Value Iteration algorithm. The algorithm proceeds by first sampling the state space. Then the agent plays the game until completion. 
Every time it takes an action, it determines whether a random action should be taken (exploration) or whether to choose the action that maximizes the value based on its current knowledge of the value function (exploitation). It takes note of the reward received and uses this information to update the value function. Since the state is randomly sampled, the more episodes (i.e. times) the agent plays the game, the better it can estimate the value function.
An additional consideration of the algorithm is the hyperparameter alpha, which indicates the learning rate. The learning rate impacts the weight assigned to the "old" (or current) value for a given state (i.e. what the agent has already learned about this state, action pair, if anything) vs the weight assigned to the "new" value for a given state (i.e. what the agent just learned by taking an action in this state).

## Comparison
For theta = 1e-5, the Value Iteration algorithm took 74 steps to converge. From a small sample of 5 simulations, the agent's average total reward was -0.86, and the average # of penalties the agent incurred was 0.
For alpha = 0.9 and a constant epsilon = 0.1, data for the Q-learning algorithm was as follows:
Metric | Episodes = 500 | Episodes = 5000 
------------ | ------------ | ------------- 
Avg # steps per episode | 49.56 | 18.23
Avg score | -1.64 | -1.99 
Avg # penalties | 0 | 0 
I also tried running the simulations with # episodes = 5 and 50. In these cases, the policy that the agent learned was terrible. It resulted in an infinite loop, where the taxi would keep choosing to essentially remain stagnant. For example, the taxi would move left and then right over and over again, or the taxi would keep moving up (staying in the same position) at the top of the grid.
