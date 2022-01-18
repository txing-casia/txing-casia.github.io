---
layout:     post
title:      "Gym框架下的CoppeliaSim小车倒立摆实现"
subtitle:   ""
date:       2022-01-17
author:     "Pcon"
header-img: "img/post-bg-py.jpg"
tags:
    - CoppeliaSim
    - V-rep
---

# Gym框架下的CoppeliaSim小车倒立摆实现

### 1 任务环境

cart_pole 是一个经典的强化学习任务。小车上连着一根杆，小车通过在横轴上左右移动，实现杆保持竖直向上，但小车不能运动得太远（有运动边界限制）。

![cart_pole.ttt](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220117-2.png)

### 2 环境配置

这个工程中，有点不一样的是.ttt文件中**不用补充**那句

> simRemoteApi.start(19997)

否则会收到报错空端口号，无法进行仿真：

> 3: Invalid port number. (in function 'simRemoteApi.start@simExtRemoteApi')
>   stack traceback:
>     [C]: in function 'simExtRemoteApiStart'
>     [string "mainScript"]:3: in function 'sim_code_function_to_run'

### 3 Gym环境封装

主要包含两个文件：

- 模型文件（MyArm.py）：主要包括模型的API接口；
- 环境文件（MyArmEnv.py）：主要按照gym标准的几个函数封装环境；

（我感觉这两个文件完全可以合成一个，放在MyArmEnv.py里面）

MyArm.py：

```python
# MyArm.py
import sys
sys.path.append('../VREP_RemoteAPIs')
import sim as vrep_sim

# CartPole simulation model for VREP
class CartPoleSimModel():
    def __init__(self, name='CartPole'):
        """
        :param: name: string
            name of objective
        """
        super(self.__class__, self).__init__()
        self.name = name
        self.client_ID = None

        self.prismatic_joint_handle = None
        self.revolute_joint_handle = None

    def initializeSimModel(self, client_ID):
        try:
            print ('Connected to remote API server')
            client_ID != -1
        except:
            print ('Failed connecting to remote API server')

        self.client_ID = client_ID

        return_code, self.prismatic_joint_handle = vrep_sim.simxGetObjectHandle(client_ID, 'prismatic_joint', vrep_sim.simx_opmode_blocking)
        if (return_code == vrep_sim.simx_return_ok):
            print('get object prismatic joint ok.')

        return_code, self.revolute_joint_handle = vrep_sim.simxGetObjectHandle(client_ID, 'revolute_joint', vrep_sim.simx_opmode_blocking)
        if (return_code == vrep_sim.simx_return_ok):
            print('get object revolute joint ok.')

        # Get the joint position
        return_code, q = vrep_sim.simxGetJointPosition(self.client_ID, self.prismatic_joint_handle, vrep_sim.simx_opmode_streaming)
        return_code, q = vrep_sim.simxGetJointPosition(self.client_ID, self.revolute_joint_handle, vrep_sim.simx_opmode_streaming)

        # Set the initialized position for each joint
        self.setJointTorque(0)
    
    def getJointPosition(self, joint_name):
        """
        :param: joint_name: string
        """
        q = 0
        if joint_name == 'prismatic_joint':
            return_code, q = vrep_sim.simxGetJointPosition(self.client_ID, self.prismatic_joint_handle, vrep_sim.simx_opmode_buffer)
        elif joint_name == 'revolute_joint':
            return_code, q = vrep_sim.simxGetJointPosition(self.client_ID, self.revolute_joint_handle, vrep_sim.simx_opmode_buffer)
        else:
            print('Error: joint name: \' ' + joint_name + '\' can not be recognized.')

        return q

    def setJointTorque(self, torque):
        if torque >= 0:
            vrep_sim.simxSetJointTargetVelocity(self.client_ID, self.prismatic_joint_handle, 1000, vrep_sim.simx_opmode_oneshot)
        else:
            vrep_sim.simxSetJointTargetVelocity(self.client_ID, self.prismatic_joint_handle, -1000, vrep_sim.simx_opmode_oneshot)

        vrep_sim.simxSetJointMaxForce(self.client_ID, self.prismatic_joint_handle, abs(torque), vrep_sim.simx_opmode_oneshot)
        
```

然后是 MyArmEnv.py：

```python
# MyArmEnv.py
import numpy as np
import gym
from gym.utils import seeding
from gym import spaces, logger
import time

import sys
sys.path.append('../VREP_RemoteAPIs')
import sim as vrep_sim

from MyArm import CartPoleSimModel

class CartPoleEnv(gym.Env):
    """Custom Environment that follows gym interface"""
    metadata = {'render.modes': ['human']}

    def __init__(self, action_type='descrete'):
        super(CartPoleEnv, self).__init__()
        self.action_type = action_type
        self.push_force = 0
        self.q = [0.0, 0.0]
        self.q_last = [0.0, 0.0]

        self.theta_max = 40*np.pi / 360
        self.cart_pos_max = 0.8

        high = np.array(
            [
                self.cart_pos_max,
                self.theta_max,
                1000000.0,
                1000000.0
            ],
            dtype=np.float32,
        )

        if self.action_type == 'discrete':
            self.action_space = spaces.Discrete(3)
        elif self.action_type == 'continuous':
            self.action_space = spaces.Box(low=-1.0, high=1.0, shape=(1,), dtype=np.float32)
        else:
            assert 0, "The action type \'" + self.action_type + "\' can not be recognized"

        self.observation_space = spaces.Box(low=-high, high=high, dtype=np.float32)

        self.seed()
        self.state = self.np_random.uniform(low=-0.05, high=0.05, size=(4,))
        self.counts = 0
        self.steps_beyond_done = None

        # Connect to VREP (CoppeliaSim)
        vrep_sim.simxFinish(-1) # close all opened connections
        while True:
            client_ID = vrep_sim.simxStart('127.0.0.1', 19997, True, False, 5000, 5) # Connect to CoppeliaSim
            if client_ID > -1: # connected
                print('Connect to remote API server.')
                break
            else:
                print('Failed connecting to remote API server! Try it again ...')

        # Open synchronous mode
        vrep_sim.simxSynchronous(client_ID, True) 
        vrep_sim.simxStartSimulation(client_ID, vrep_sim.simx_opmode_oneshot)
        vrep_sim.simxSynchronousTrigger(client_ID)

        self.cart_pole_sim_model = CartPoleSimModel()
        self.cart_pole_sim_model.initializeSimModel(client_ID)
        vrep_sim.simxSynchronousTrigger(client_ID)

    def seed(self, seed=None):
        self.np_random, seed = seeding.np_random(seed)
        return [seed]

    def step(self, action):
        if self.action_type == 'discrete':
            assert self.action_space.contains(action), "%r (%s) invalid"%(action, type(action))

        q = [0.0, 0.0]
        q[0] = self.cart_pole_sim_model.getJointPosition('prismatic_joint')
        q[1] = self.cart_pole_sim_model.getJointPosition('revolute_joint')
        self.q_last = self.q
        self.q = q

        if self.action_type == 'discrete':
            if action == 0:
                self.push_force = 0
            elif action == 1:
                self.push_force = 1.0
            elif action == 2:
                self.push_force = -1.0
        elif self.action_type == 'continuous':
            self.push_force = action*2.0 # The action is in [-1.0, 1.0], therefore the force is in [-2.0, 2.0]
        else:
            assert 0, "The action type \'" + self.action_type + "\' can not be recognized"

        # set action
        self.cart_pole_sim_model.setJointTorque(self.push_force)
        
        done = (q[0] <= -self.cart_pos_max) or (q[0] >= self.cart_pos_max) or (q[1] < -self.theta_max) or (q[1] > self.theta_max)
        done = bool(done)

        if not done:
            reward = 1.0
        elif self.steps_beyond_done is None:
            # Pole just fell!
            self.steps_beyond_done = 0
            reward = 1.0
        else:
            if self.steps_beyond_done == 0:
                logger.warn(
                    "You are calling 'step()' even though this "
                    "environment has already returned done = True. You "
                    "should always call 'reset()' once you receive 'done = "
                    "True' -- any further steps are undefined behavior."
                )
            self.steps_beyond_done += 1
            reward = 0.0

        dt = 0.005
        self.v = [(self.q[0] - self.q_last[0])/dt, (self.q[1] - self.q_last[1])/dt]
        self.state = (self.q[0], self.q[1], self.v[0], self.v[1])
        self.counts += 1

        vrep_sim.simxSynchronousTrigger(self.cart_pole_sim_model.client_ID)
        vrep_sim.simxGetPingTime(self.cart_pole_sim_model.client_ID)

        return np.array(self.state), reward, done, {}
    
    def reset(self):
        # print('Reset the environment after {} counts'.format(self.counts))
        self.counts = 0
        self.push_force = 0
        self.state = self.np_random.uniform(low=-0.05, high=0.05, size=(4,))
        self.steps_beyond_done = None

        vrep_sim.simxStopSimulation(self.cart_pole_sim_model.client_ID, vrep_sim.simx_opmode_blocking) # stop the simulation
        vrep_sim.simxGetPingTime(self.cart_pole_sim_model.client_ID)
        time.sleep(0.01) # ensure the coppeliasim is stopped
        vrep_sim.simxStartSimulation(self.cart_pole_sim_model.client_ID, vrep_sim.simx_opmode_oneshot)
        self.cart_pole_sim_model.setJointTorque(0)
        vrep_sim.simxSynchronousTrigger(self.cart_pole_sim_model.client_ID)
        vrep_sim.simxGetPingTime(self.cart_pole_sim_model.client_ID)

        return np.array(self.state)
    
    def render(self):
        return None

    def close(self):
        vrep_sim.simxStopSimulation(self.cart_pole_sim_model.client_ID, vrep_sim.simx_opmode_blocking) # stop the simulation
        vrep_sim.simxFinish(-1)  # Close the connection
        print('Close the environment')
        return None

if __name__ == "__main__":
    env = CartPoleEnv()
    env.reset()

    for _ in range(500):
        action = env.action_space.sample() # random action
        env.step(action)
        print(env.state)

    env.close()
```

最后补上主程序文件 main.py即可

```python
import sys
sys.path.append('../VREP_RemoteAPIs')
import sim as vrep_sim
from numpy import random

from MyArm import CartPoleSimModel


print ('Program started')

#%% ------------------------------- Connect to VREP (CoppeliaSim) ------------------------------- 
vrep_sim.simxFinish(-1) # just in case, close all opened connections
while True:
    client_ID = vrep_sim.simxStart('127.0.0.1', 19997, True, False, 5000, 5) # Connect to CoppeliaSim
    if client_ID > -1: # connected
        print('Connect to remote API server.')
        break
    else:
        print('Failed connecting to remote API server! Try it again ...')

# Open synchronous mode
vrep_sim.simxSynchronous(client_ID, True) 
# Start simulation
vrep_sim.simxStartSimulation(client_ID, vrep_sim.simx_opmode_oneshot)
vrep_sim.simxSynchronousTrigger(client_ID)  # trigger one simulation step, takes about 11 ms on Windows 10

cart_pole_sim_model = CartPoleSimModel('CartPole')
cart_pole_sim_model.initializeSimModel(client_ID)

q = [0.0, 0.0]
for i in range(1000):
    q[0] = cart_pole_sim_model.getJointPosition('prismatic_joint')
    q[1] = cart_pole_sim_model.getJointPosition('revolute_joint')
    print('q={}'.format(q))

    action = random.uniform(-1.0, 1.0)
    cart_pole_sim_model.setJointTorque(action)

    vrep_sim.simxSynchronousTrigger(client_ID)
    _, ping_time = vrep_sim.simxGetPingTime(client_ID) # make sure the last simulation step is finished

vrep_sim.simxStopSimulation(client_ID, vrep_sim.simx_opmode_blocking) # stop the simulation
vrep_sim.simxFinish(-1)  # Close the connection
print('Program terminated')

```
