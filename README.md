# Reinforcement_Learning_Atari_Game
The repo contains the training of Atari Game by Reinforcement Learning tech such as DQN

The code I used here is based on the reference: https://github.com/plopd/deep-reinforcement-learning.

## Algorithm

Q-learning learns the action-value function Q(s,a): how good to take an action at a particular state.In Q-learning, we build a memory table Q[s, a] to store Q-values for all possible combinations of s and a. We sample an action from the current state. We find out the reward R (if any) and the new state s’ (the new board position). From the memory table, we determine the next action a’ to take which has the maximum Q(s’,a’).

We want to determine an action a’ to maximize the Q function, which would be an approximation of the true Q function. Though we could not find the true Q function, we could always use the deep nerual network as an approximation to such kind of function. So that it becomes we are going to train a neural network model that take in the past state to predict a new action. That’s why it is called Deep Q Network.

There two most important design of this algorithm:

1. Experience replay: Numbers of the transitions or the video frames are put into a buffer and we will sample a mini-batch of samples from the buffer to train the deep network. It gives a very stable input for training. And the process of randomly sampling from the replay buffer would give us a more independent dataset which would be close to i.i.d.

2. The policy network and the target network: Two networks are built here. The policy network is used to retrieve Q values and get training for each game. The target network is used to include all updates in the training. There is a reason we want to use a temporary frozen target network that we don’t want to have a moving target to chase. It will give us a stable training progress.

## Implementation

### The implemented functions

1. Q network: The Q network here contains 5 fully connected layers. The dimension of each hidden layer is 1028 always. Between two fully connected layers, a layer of RELU activation is used. The input dim is 128 and the output dim is 4.

2. The agent: Used to do the trainig of the policy network, updating of the target network and also playing the game by retrieving the memory from the Beplay Buffer.

3. The ReplayBuffer: Used to store the memory of playing the game and also containa a function to do the random sampling of the memory.

### Training Stages and The parameters using

There are several parmeter to tune for the training, the most important parameters are listed below:

Buffer size: The replay buffer size. I choose buffer size to be 1000000 to same more memory in the replay buffer.

Learning Rate: The learning rate used for the Adam optimization algorithm. For the first stage I used 0.0002, for the second stage I used 0.0001 and for the final stage I used 0.00005.

Update Every: How often we want to update the target network. The higher this number, more stable increase we would have. Here I used 8, which gives nice result.

Max T: The maximum number of timesteps per episode for the game. Just make sure this number big enough for the game to play. I set 5000 here which is really big enough.

## Sample Game and Results

### Scores by epochs

According to my implementation design, there are three stages of training. The plots of scores for each stage of traninig will be listed below.

<div align="center">
        <img src="https://github.com/nji3/Reinforcement_Learning_Atari_Game/blob/master/readme_pics/stage_1.png" width="250px"</img>
        <img src="https://github.com/nji3/Reinforcement_Learning_Atari_Game/blob/master/readme_pics/stage_2.png" width="250px"</img>
        <img src="https://github.com/nji3/Reinforcement_Learning_Atari_Game/blob/master/readme_pics/stage_3.png" width="250px"</img>
</div>

From the left one to the right, it can be clearly seen that based on the preivious memory and a smaller learning rate, the new stage would have a faster rate to have better scores. It seems that my strategy here works.

### Sample Games

I stopped when I have the averaged score reached 32. And I used my final model (the policy network) to generate some sample games. The results are relatively good and have a average scores beyond 50. I generate 200 samples and below are the screen shots of several games with scores above 80.

<div align="center">
        <img src="https://github.com/nji3/Reinforcement_Learning_Atari_Game/blob/master/readme_pics/81_gif.gif" width="200px"</img>
        <img src="https://github.com/nji3/Reinforcement_Learning_Atari_Game/blob/master/readme_pics/82_gif.gif" width="200px"</img>
        <img src="https://github.com/nji3/Reinforcement_Learning_Atari_Game/blob/master/readme_pics/87_gif.gif" width="200px"</img>
        <img src="https://github.com/nji3/Reinforcement_Learning_Atari_Game/blob/master/readme_pics/96_gif.gif" width="200px"</img>
</div>
