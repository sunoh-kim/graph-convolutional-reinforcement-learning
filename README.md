# Graph Convolutional Reinforcement Learning


Based on [DGN](https://github.com/PKU-AI-Edge/DGN/), I reproduced the results in the scenario, Routing, presented in the paper
[Graph Convolution Reinforcement Learning](https://arxiv.org/abs/1810.09202).

The algorithm for the assignment is implemented using TensorFlow in Python.

### DGN

DGN is graph convolutional reinforcement learning, where the multi-agent environment is modeled as a graph, each agent is a node, and the encoding of local observation of agent is the feature of node. We apply convolution to the graph of agents. By employing multi-head attention as the convolution kernel, graph convolution is able to extract the relation representation between nodes and convolve the features from neighboring nodes just like a neuron in a convolutional neural network (CNN). Latent features extracted from gradually increased receptive fields are exploited to learn cooperative policies. Moreover, the relation representation is temporally regularized to help the agent develop consistent cooperative policy.

In DGN, all agents share weights for the modules. The main reason is that agents use relation kernels to extract their relations based the encodings of their observations. If the encoders are different (agents encodes the observation in different ways), the relation kernels can hardly learn to extract their relations since the graph of agents is highly dynamic. 

## Requirement
 - Ubuntu 16.04
 - Anaconda 3
 - Python 3.6
 - Cuda 9.2
 - Cudnn 7.6.3
 
## Setting RL Environment
First, we have to install [MAgent](https://github.com/geek-ai/MAgent) which is a research platform for multi-agent reinforcement learning. MAgent aims at supporting reinforcement learning research that scales up from hundreds to millions of agents.

 - Installing MAgent on Linux:
```
git clone https://github.com/geek-ai/MAgent.git
cd MAgent
sudo apt-get install cmake libboost-system-dev libjsoncpp-dev libwebsocketpp-dev
bash build.sh
export PYTHONPATH=$(pwd)/python:$PYTHONPATH
```

## Installing Dependencies
Before implementing DGN, we need to create a new conda environment (If you want) and install dependencies using pip.

 - Creating new conda environment
```
conda create -n <Your Environment Name> python=3.6
conda activate <Your Environment Name>
```

 - Cloning DGN codes and installing dependencies

```
git clone https://github.com/suno8386/DGN-reproduced.git
cd DGN-reproduced
pip install -r requirements.txt
```
	(The authors of DGN used tensorflow 2.1.0 and Keras 2.3.1. But some functions are deprecated in them. So, I used tensorflow 1.14.0 and Keras 2.0.8 and modified the codes accordingly.)

## Implementing DGN
Now, we will reproduce the result of the DGN in the Routing scenario.
The result will be saved in the log file. (e.g. /home/DGN-reproduced/log_router_gqn.txt)
 - Reproducing DGN without regularization 
```
python routers.py
```

 - Reproducing DGN with regularization 
```
python routers_regularization.py
```

## Code Outline
The main code is composed of 1) build the graph, 2) build the model, 3) playing, 4) training.

 - class Router: define an object ‘Router’ that is randomly connected to a constant number of other routers and become destination for the data packet (agent) to reach goal.
 - class Edge: define an object ‘Edge’ where the Routers are randomly connected .
 - class Data: define an object ‘Data’ (agent). There are 𝑁 data packets with a random size, and each packet is randomly assigned a source and destination router. If there are multiple packets with the sum size larger than the bandwidth of a link, they cannot go through the link simultaneously. They aim to quickly reach the destination while avoiding congestion. 

 - def observation: define a method which makes observation of the data packet. At each time step, the observation of a packet is its own attributes (i.e., current location, destination, and data size), the attributes of cables connected to its current location (i.e., load, length), and neighboring data packets (on the connected cable or routers).
 - def set_action: define a method which sets action of the data packet. It takes some time steps for the data packet to go through a cable, a linear function of the cable length. The action space of a packet is the choices of next hop. Once the data packet arrives at the destination, it leaves the system and another data packet enters the system with random initialization.
 - def Adjacency: define a method which makes adjacency matrix of the graph.
 - def MLP: define a method which models a multi-layer perception.
 - def MultiHeadsAttModel: define a method which models a multi heads attention.
 - def Q_Net: define a method which models a Q-network.


## Reference


	@inproceedings{jiang2020graph,
        	title={Graph Convolutional Reinforcement Learning},
        	author={Jiang, Jiechuan and Dun, Chen and Huang, Tiejun and Lu, Zongqing},
        	booktitle={ICLR},
        	year={2020}
	}

