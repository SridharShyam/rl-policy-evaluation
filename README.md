# POLICY EVALUATION

### NAME: SHYAM S
### REG.NO: 212223240156
## AIM
To develop a Python program to evaluate the given policy by maximizing its cumulative reward while dealing with slippery terrain.

## PROBLEM STATEMENT
We are assigned with the task of creating an RL agent to solve the "Bandit Slippery Walk" problem. The environment consists of Seven states representing discrete positions the agent can occupy. The agent must learn to navigate this environment while dealing with the challenge of slippery terrain. Slippery terrain introduces stochasticity in the agent's actions, making it difficult to predict the outcomes of its actions accurately.

## POLICY EVALUATION FUNCTION
## FORMULA:
![image](https://github.com/user-attachments/assets/02a1e9f1-ee4f-4669-96b9-35103b0a3808)
## PROGRAM:
```
!pip install gym
!pip install git+https://github.com/mimoralea/gym-walk
```
```
import warnings ; warnings.filterwarnings('ignore')

import gym, gym_walk
import numpy as np

import random
import warnings

warnings.filterwarnings('ignore', category=DeprecationWarning)
np.set_printoptions(suppress=True)
random.seed(123); np.random.seed(123)



def print_policy(pi, P, action_symbols=('<', 'v', '>', '^'), n_cols=4, title='Policy:'):
    print(title)
    arrs = {k:v for k,v in enumerate(action_symbols)}
    for s in range(len(P)):
        a = pi(s)
        print("| ", end="")
        if np.all([done for action in P[s].values() for _, _, _, done in action]):
            print("".rjust(9), end=" ")
        else:
            print(str(s).zfill(2), arrs[a].rjust(6), end=" ")
        if (s + 1) % n_cols == 0: print("|")


def print_state_value_function(V, P, n_cols=4, prec=3, title='State-value function:'):
    print(title)
    for s in range(len(P)):
        v = V[s]
        print("| ", end="")
        if np.all([done for action in P[s].values() for _, _, _, done in action]):
            print("".rjust(9), end=" ")
        else:
            print(str(s).zfill(2), '{}'.format(np.round(v, prec)).rjust(6), end=" ")
        if (s + 1) % n_cols == 0: print("|")


def probability_success(env, pi, goal_state, n_episodes=100, max_steps=200):
    random.seed(123); np.random.seed(123) ; env.seed(123)
    results = []
    for _ in range(n_episodes):
        state, done, steps = env.reset(), False, 0
        while not done and steps < max_steps:
            state, _, done, h = env.step(pi(state))
            steps += 1
        results.append(state == goal_state)
    return np.sum(results)/len(results)


def mean_return(env, pi, n_episodes=100, max_steps=200):
    random.seed(123); np.random.seed(123) ; env.seed(123)
    results = []
    for _ in range(n_episodes):
        state, done, steps = env.reset(), False, 0
        results.append(0.0)
        while not done and steps < max_steps:
            state, reward, done, _ = env.step(pi(state))
            results[-1] += reward
            steps += 1
    return np.mean(results)



env = gym.make('SlipperyWalkFive-v0')
P = env.env.P
init_state = env.reset()
goal_state = 6
LEFT, RIGHT = range(2)


P


init_state


state, reward, done, info = env.step(RIGHT)
print("state:{0} - reward:{1} - done:{2} - info:{3}".format(state, reward, done, info))


# First Policy
pi_1 = lambda s: {
    0:LEFT, 1:LEFT, 2:LEFT, 3:LEFT, 4:LEFT, 5:LEFT, 6:LEFT
}[s]
print_policy(pi_1, P, action_symbols=('<', '>'), n_cols=7)


# Find the probability of success and the mean return of the first policy
print('Reaches goal {:.2f}%. Obtains an average undiscounted return of {:.4f}.'.format(
    probability_success(env, pi_1, goal_state=goal_state)*100,
    mean_return(env, pi_1)))

# Second Policy
pi_2 = lambda s: {
    0:RIGHT, 1:RIGHT, 2:LEFT, 3:RIGHT, 4:RIGHT, 5:LEFT, 6:RIGHT
}[s]
print_policy(pi_2, P, action_symbols=('<', '>'), n_cols=7)

# Find the probability of success and the mean return of you your policy
print('Reaches goal {:.2f}%. Obtains an average undiscounted return of {:.4f}.'.format(
    probability_success(env, pi_2, goal_state=goal_state)*100,
    mean_return(env, pi_2)))



# Compare your policy with the first policy


def policy_evaluation(pi,P,gamma=0.1,theta=1e-10):
  prev_v=np.zeros(len(P))
  while True:
    V=np.zeros(len(P))
    for s in range(len(P)):
      for prob, next_state,reward,done in P[s][pi(s)]:
        V[s]+=prob*(reward+gamma*prev_v[next_state]*(not done))
    if np.max(np.abs(prev_v-V))<theta:
      break
    prev_v=V.copy()
  return V

# Code to evaluate the first policy
V1 = policy_evaluation(pi_1, P)
print_state_value_function(V1, P, n_cols=7, prec=5)

V2 = policy_evaluation(pi_2, P)
print_state_value_function(V2, P, n_cols=7, prec=5)


if(np.sum(V1>=V2)==7):
  print("The first policy is the better policy")
elif(np.sum(V2>=V1)==7):
  print("The second policy is the better policy")
else:
  print("Both policies have their merits.")
```

## OUTPUT:
![image](https://github.com/user-attachments/assets/3bf1a2b2-df51-454c-a16a-c87871dbb595)

![image](https://github.com/user-attachments/assets/5f5439a6-79aa-429e-a092-659f80fc5c33)

![image](https://github.com/user-attachments/assets/9c04b489-e9a3-447d-b702-a834bcaca085)

## RESULT:

Thus, the Given Policy has been Evaluated and Optimal Policy has been Computed using Python Programming and execcuted successfully.
