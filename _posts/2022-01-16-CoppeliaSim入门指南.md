---
layout:     post
title:      "CoppeliaSim入门指南"
subtitle:   ""
date:       2022-01-16
author:     "Pcon"
header-img: "img/post-bg-py.jpg"
tags:
    - CoppeliaSim
    - V-rep
---

# CoppeliaSim入门指南

### 1 软件安装

CoppeliaSim 以前叫做 V-rep，是一款广泛使用的机器人仿真软件环境。在本指南中，尝试在 python 中实现对 CoppeliaSim 的调用。

> CoppeliaSim下载地址：https://www.coppeliarobotics.com/

下载完成后按照默认引导完成安装。

### 2 环境配置

然后新建 python 工程环境，在工程文件夹内需要添加三个 CoppeliaSim 相关的文件，分别为：

- sim.py（原vrep.py）
  
  > 位置：C:\Program Files\CoppeliaRobotics\CoppeliaSimEdu\programming\remoteApiBindings\python\python
  
- simConst.py（原vrepConst.py）
  
  > 位置：C:\Program Files\CoppeliaRobotics\CoppeliaSimEdu\programming\remoteApiBindings\python\python
  
- remoteApi.dll 
  
  > 位置：C:\Program Files\CoppeliaRobotics\CoppeliaSimEdu\programming\remoteApiBindings\lib\lib\Windows

在 CoppeliaSim 窗口中，双击 main 文件，添加如下所示第三行代码。帮助实现与 python 的远程连接

![main文件位置](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220117-1.png)

```
-- The main script is not supposed to be modified, except in special cases.
require('defaultMainScript')
simRemoteApi.start(19999)
```

### 3 程序代码

在 python 窗口中，添加如下代码：

```python
# Make sure to have the server side running in CoppeliaSim: 
# in a child script of a CoppeliaSim scene, add following command
# to be executed just once, at simulation start:
#
# simRemoteApi.start(19997)
#
# then start simulation, and run this program.
#
# IMPORTANT: for each successful call to simxStart, there
# should be a corresponding call to simxFinish at the end!

try:
    import sim
except:
    print ('--------------------------------------------------------------')
    print ('"sim.py" could not be imported. This means very probably that')
    print ('either "sim.py" or the remoteApi library could not be found.')
    print ('Make sure both are in the same folder as this file,')
    print ('or appropriately adjust the file "sim.py"')
    print ('--------------------------------------------------------------')
    print ('')

import time

print ('Program started')
sim.simxFinish(-1) # just in case, close all opened connections
clientID=sim.simxStart('127.0.0.1',19999,True,True,5000,5) # Connect to CoppeliaSim
if clientID!=-1:
    print ('Connected to remote API server')

    # Now try to retrieve data in a blocking fashion (i.e. a service call):
    res,objs=sim.simxGetObjects(clientID,sim.sim_handle_all,sim.simx_opmode_blocking)
    if res==sim.simx_return_ok:
        print ('Number of objects in the scene: ',len(objs))
    else:
        print ('Remote API function call returned with error code: ',res)

    time.sleep(2)

    # Now retrieve streaming data (i.e. in a non-blocking fashion):
    startTime=time.time()
    sim.simxGetIntegerParameter(clientID,sim.sim_intparam_mouse_x,sim.simx_opmode_streaming) # Initialize streaming
    while time.time()-startTime < 5:
        returnCode,data=sim.simxGetIntegerParameter(clientID,sim.sim_intparam_mouse_x,sim.simx_opmode_buffer) # Try to retrieve the streamed data
        if returnCode==sim.simx_return_ok: # After initialization of streaming, it will take a few ms before the first value arrives, so check the return code
            print ('Mouse position x: ',data) # Mouse position x is actualized when the cursor is over CoppeliaSim's window
        time.sleep(0.005)

    # Now send some data to CoppeliaSim in a non-blocking fashion:
    sim.simxAddStatusbarMessage(clientID,'Hello CoppeliaSim!',sim.simx_opmode_oneshot)

    # Before closing the connection to CoppeliaSim, make sure that the last command sent out had time to arrive. You can guarantee this with (for example):
    sim.simxGetPingTime(clientID)

    # Now close the connection to CoppeliaSim:
    sim.simxFinish(clientID)
else:
    print ('Failed connecting to remote API server')
print ('Program ended')

```

代码来自于样例程序simpleTest.py (C:\Program Files\CoppeliaRobotics\CoppeliaSimEdu\programming\remoteApiBindings\python\python)

### 4 功能实现

功能大致是：获取 CoppeliaSim 窗口中鼠标的 x 坐标，并返回到 python 窗口输出。

运行方式为：先点击 CoppeliaSim 的 start simulation 按键运行仿真，再运行 python 程序，会自动实现通信。
