# Report

## Implementation
The environment was solved using a deep reinforcement learning agent. The implementation can be found in the `continuous_control`-directory.
`agent.py` contains the rl-agent and `model.py` contains the neural networks used as the estimators. A lot of the code was taken
from the Udacity ddpg-pendulum exercise and adapted to the needs of this problem.

### Learning algorithm
[DDPG](https://arxiv.org/abs/1509.02971) which is an actor-critic approach was used as the learning algorithm for the agent.
This algorithm is quite similar to DQN, but also manages to solve tasks with continuous action spaces. As an off-policy algorithm
DDPG utilizes four neural networks: a local actor, a target actor, a local critic and a target critic
Each training step the experience (state, action, reward, next state) the 20 agents gained was stored.
Then every second training step the agent learned from a random sample from the stored experience. The actor tries to estimate the
optimal policy by using the estimated state-action values from the critic while critic tries to estimate the optimal q-value function
and learns by using a normal q-learning approach. Using this approach one gains the benefits of value based and policy based
methods at the same time.

### hyperparameters
The following hyperparameters were used:
* replay buffer size: 1e6
* max timesteps: 3000 (all episodes get shutdown after 3000 timesteps)
* minibatch size: 256
* discount factor: 0.99
* tau (soft update for target networks factor): 1e-3
* learning rate: 1e-4 (actor) and 1e-3 (critic)
* update interval (how often to learn): 2
* beta start (factor for the noise added to the actions selected by the actor): 0.1
* beta decay factor: 0.995
* min beta: 0.01

### Neural networks
The actor model is a simple feedforward network:
* Batch normalization
* Input layer: 33 (input) neurons (the state size)
* 1st hidden layer: 128 neurons (leaky relu)
* 2nd hidden layer: 128 neurons (leaky relu)
* output layer: 4 neurons (1 for each action) (tanh)

The critic model:
* Batch normalization
* Input layer: 33 (input) neurons (the state size)
* 1st hidden layer: 132 neurons (action with action_size 4 added) (leaky relu)
* 2nd hidden layer: 128 neurons (leaky relu)
* output layer: 1 neuron

## Results
The agent was able to solve the environment after 133 episodes achieving an average score of 30.11 over the last 100 episodes
of the training.

The average scores of the 20 agents during the training process:
![scores](https://user-images.githubusercontent.com/9535190/78456465-2bd03180-76a4-11ea-8cb9-bedcb75827bd.png)

## possible future improvements
The algorithm could be improved in many ways. For example one could implement some DQN improvements, for example Prioritized Experience Replays
which would improve the learning effect gained from the saved experience. Also true parallel algorithms like A3C could be tried out.