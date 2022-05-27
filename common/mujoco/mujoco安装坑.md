# gym[all]安装的一些坑

|      系统      | 是否安装桌面 | 修改时间 |
| :------------: | :----------: | :------: |
| Ubuntu22(WSL2) |      无      | 20220527 |

本文默认使用anaconda虚拟环境，本意为跑一些meta-RL的代码，第一次发现安装一个库可以有那么多坑...

## 安装mujoco

安装前置

```bash
sudo apt update
sudo apt install -y build-essential libxcursor1 libglew-dev
```

### 不需要gym环境 -> github最新版

```bash
cd ~
mkdir .mujoco
cd .mujoco
wget https://github.com/deepmind/mujoco/releases/download/2.2.0/mujoco-2.2.0-linux-aarch64.tar.gz
tar -xzvf mujoco-2.2.0-linux-x86_64.tar.gz 
mv mujoco-2.2.0 mujoco220
```

下载mjkey.txt

```bash
wget https://roboti.us/file/mjkey.txt
```

放到合适位置

```bash
cp mjkey.txt mujoco220/bin/mjkey.txt
# cp mjkey.txt mujoco220/mjkey.txt 有部分教程说要
```

增加环境变量

```bash
nano ~/.zshrc
```

在最后增加这一句

```bash
export MUJOCO_KEY_PATH=~/.mujoco${MUJOCO_KEY_PATH}
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/zy6/.mujoco/mujoco220/bin${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

刷新配置

```bash
source ~/.zshrc
```

测试 (我的总在说xml文件error，不理它)

```bash
cd ~/.mujoco/mujoco220/
./bin/simulate ./model/humanoid
```

### 需要gym环境 -> mjpro150

```bash
cd ~
mkdir .mujoco
cd .mujoco
wget https://www.roboti.us/download/mjpro150_linux.zip
sudo apt install -y unzip
unzip mjpro150_linux.zip
```

其余操作类似上述

测试出现问题：

```bash
./bin/simulate ./model/humanoid.xml
./bin/simulate: error while loading shared libraries: libXcursor.so.1: cannot open shared object file: No such file or directory
```

解决方法，安装缺失libxcursor1库

```bash
sudo apt-get install libxcursor1
```

其他部分问题解决方法可见[Mujoco&Mujoco-py安装教程以及常见报错解决方法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/352304615)，大部分问题百度基本可解决，需要Google的部分问题基本覆盖

## mujoco-py安装问题

安装gym[all]

```bash
pip install gym[all]
```

出现问题：

```bash
warning: mujoco_py/generated/../pxd/mjmodel.pxd:99:4: 'mjtDisableBit' redeclared
  warning: mujoco_py/generated/../pxd/mjmodel.pxd:114:4: 'mjtEnableBit' redeclared
...
  warning: mujoco_py/generated/../pxd/mjmodel.pxd:239:8: 'mjGAIN_USER' redeclared
...
    File "/tmp/pip-install-le5ipkw0/mujoco-py_0a9f1c124bb7490698a7f20c5c7cacea/mujoco_py/builder.py", line 124, in load_dynamic_ext
      return loader.load_module()
  ImportError: /home/zy6/anaconda3/envs/rl/bin/../lib/libstdc++.so.6: version `GLIBCXX_3.4.30' not found (required by /lib/x86_64-linux-gnu/libLLVM-13.so.1)
  ERROR: Failed building wheel for mujoco-py
  Running setup.py clean for mujoco-py
Failed to build mujoco-py  
----------------------------------------
  ERROR: Failed building wheel for mujoco-py
  Running setup.py clean for mujoco-py
Failed to build mujoco-py
Installing collected packages: mujoco-py, lz4, box2d-py, ale-py
```

缺少GLIBCXX_3.4.30

原因是ubuntu22 gcc版本与anaconda版本不一样的问题，cpp std库缺少一点东西

```bash
ImportError: /home/zy6/anaconda3/envs/rl/bin/../lib/libstdc++.so.6: version `GLIBCXX_3.4.30' not found (required by /lib/x86_64-linux-gnu/libLLVM-13.so.1)
```

寻找以下这个GLIBCXX_3.4.30

```bash
strings /home/zy6/anaconda3/envs/rl/bin/../lib/libstdc++.so.6 | grep GLIBCXX_3.4.3
```

发现是没有的

```bash
$ strings /home/zy6/anaconda3/envs/rl/bin/../lib/libstdc++.so.6 | grep GLIBCXX_3.4.3   (0s)[13:52:40]
GLIBCXX_3.4.3
GLIBCXX_3.4.3
```

在系统gcc标准库中寻找一下

```bash
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX_3.4.3
```

发现是有的

```bash
$ strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX_3.4.3                    (0s)[14:00:54]
GLIBCXX_3.4.3
GLIBCXX_3.4.30
```

解决方法：

```bash
cp /home/zy6/anaconda3/envs/rl/bin/../lib/libstdc++.so.6 /home/zy6/anaconda3/envs/rl/bin/../lib/libstdc++.so.6.bak
cp ~/libstdc++.so.6 /home/zy6/anaconda3/envs/rl/bin/../lib/libstdc++.so.6 
```

## mujoco版本问题

如果你要使用gym，不能用最新的mujoco[写的时候是220]，请下载mjpro150，否则会出现下面的问题(当时的路径指向2.1.5版本)

```bash
Missing path to your environment variable. 
Current values LD_LIBRARY_PATH=:/home/zy6/.mujoco/mujoco215/bin
Please add following line to .bashrc:
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/zy6/.mujoco/mjpro150/bin
```

因此得修改xxxrc文件(看你是bash还是zsh\tmux什么的)

打开配置文件

```bash
nano ~/.zshrc
```

在最后加上一句

```bash
export MUJOCO_KEY_PATH=~/.mujoco${MUJOCO_KEY_PATH}
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/zy6/.mujoco/mjpro150/bin${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

刷新配置

```bash
source ~/.zshrc && conda activate rl 
```

基本上能踩的坑都踩完了

## 测试

测试代码1：

源于[ubantu20安装mujoco_py（mjpro150）教程-vmware_Jeremy Hua的博客-CSDN博客](https://blog.csdn.net/weixin_45992833/article/details/121132236)

```python
import mujoco_py
import os

mj_path, _ = mujoco_py.utils.discover_mujoco()
xml_path = os.path.join(mj_path, 'model', 'humanoid.xml')
model = mujoco_py.load_model_from_path(xml_path)
sim = mujoco_py.MjSim(model)

print(sim.data.qpos)

sim.step()
print(sim.data.qpos) 
```

测试代码2：

源于[Mujoco&Mujoco-py安装教程以及常见报错解决方法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/352304615)

```python
import gym
env = gym.make('Humanoid-v2')

from gym import envs
print(envs.registry.all())    # print the available environments

print(env.action_space)
print(env.observation_space)
print(env.observation_space.high)
print(env.observation_space.low)

for i_episode in range(200):
    observation = env.reset()
    for t in range(100):
        env.render()
        print(observation)
        action = env.action_space.sample()    # take a random action
        observation, reward, done, info = env.step(action)
        if done:
            print("Episode finished after {} timesteps".format(t+1))
            break
env.close()
```

