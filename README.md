# How to Run Training and Test
- These steps were run and verified on Linux system. For anyone on another OS, good luck! Ensure your system matches the dependencies specified below
- If you're running arm, expect **significant** code migration

## System Requirements
- Linux (specific specs to be added later)

## Step 1: Install MuJoCo
```bash
sudo apt install unzip
mkdir ~/.mujoco && cd ~/.mujoco
wget https://www.roboti.us/download/mujoco200_linux.zip
unzip mujoco200_linux.zip && mv mujoco200_linux mujoco200
wget https://www.roboti.us/file/mjkey.txt
```

## Step 2: Update Environment Variables

```bash
nano ~/.bashrc
```
Add the following lines to the bottom of the file, and save.
```bash
export LD_LIBRARY_PATH=/home/<YOUR_USER_NAME>/.mujoco/mujoco200/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/nvidia
export PATH="$LD_LIBRARY_PATH:$PATH"
export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libGLEW.so
```

## Step 3: Install MuJoCo Dependencies
```bash
sudo apt-get install patchelf
sudo apt-get install python3-dev build-essential libssl-dev libffi-dev libxml2-dev  
sudo apt-get install libxslt1-dev zlib1g-dev libglew-dev python3-pip
sudo apt install libosmesa6-dev libgl1-mesa-glx libglfw3
```
Then run
```bash
source ~/.bashrc
```
## Step 4: Clone Repo and Install Dependencies
```bash
cd ~ 
sudo apt install git-all
git clone https://github.com/aliameur/pytorch-maml-rl
cd pytorch-maml-rl
sudo apt install python3.11-venv
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```
**Happy Training!**


# Reinforcement Learning with Model-Agnostic Meta-Learning (MAML)

![HalfCheetahDir](https://raw.githubusercontent.com/tristandeleu/pytorch-maml-rl/master/_assets/halfcheetahdir.gif)

Implementation of Model-Agnostic Meta-Learning (MAML) applied on Reinforcement Learning problems in Pytorch. This repository includes environments introduced in ([Duan et al., 2016](https://arxiv.org/abs/1611.02779), [Finn et al., 2017](https://arxiv.org/abs/1703.03400)): multi-armed bandits, tabular MDPs, continuous control with MuJoCo, and 2D navigation task.

## Getting started
To avoid any conflict with your existing Python setup, and to keep this project self-contained, it is suggested to work in a virtual environment with [`virtualenv`](http://docs.python-guide.org/en/latest/dev/virtualenvs/). To install `virtualenv`:
```
pip install --upgrade virtualenv
```
Create a virtual environment, activate it and install the requirements in [`requirements.txt`](requirements.txt).
```
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
```

#### Requirements
 - Python 3.5 or above
 - PyTorch 1.3
 - Gym 0.15

## Usage

#### Training
You can use the [`train.py`](train.py) script in order to run reinforcement learning experiments with MAML. Note that by default, logs are available in [`train.py`](train.py) but **are not** saved (eg. the returns during meta-training). For example, to run the script on HalfCheetah-Vel:
```
python train.py --config configs/maml/halfcheetah-vel.yaml --output-folder maml-halfcheetah-vel --seed 1 --num-workers 8
```

#### Testing
Once you have meta-trained the policy, you can test it on the same environment using [`test.py`](test.py):
```
python test.py --config maml-halfcheetah-vel/config.json --policy maml-halfcheetah-vel/policy.th --output maml-halfcheetah-vel/results.npz --meta-batch-size 20 --num-batches 10  --num-workers 8
```

## References
This project is, for the most part, a reproduction of the original implementation [cbfinn/maml_rl](https://github.com/cbfinn/maml_rl/) in Pytorch. These experiments are based on the paper
> Chelsea Finn, Pieter Abbeel, and Sergey Levine. Model-Agnostic Meta-Learning for Fast Adaptation of Deep
Networks. _International Conference on Machine Learning (ICML)_, 2017 [[ArXiv](https://arxiv.org/abs/1703.03400)]

If you want to cite this paper
```
@article{finn17maml,
  author    = {Chelsea Finn and Pieter Abbeel and Sergey Levine},
  title     = {{Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks}},
  journal   = {International Conference on Machine Learning (ICML)},
  year      = {2017},
  url       = {http://arxiv.org/abs/1703.03400}
}
```

If you want to cite this implementation:
```
@misc{deleu2018mamlrl,
  author = {Tristan Deleu},
  title  = {{Model-Agnostic Meta-Learning for Reinforcement Learning in PyTorch}},
  note   = {Available at: https://github.com/tristandeleu/pytorch-maml-rl},
  year   = {2018}
}
```
