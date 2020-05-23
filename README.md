# DGN

Written by Jiechuan Jiang and Zongqing Lu

Reproduced by Sunoh Kim

### Table of Contents
1. [Introduction](#introduction)
2. [Dependencies](#dependencies)
3. [Citation](#citation)

### Introduction

DGN is graph convolutional reinforcement learning, where the multi-agent environment is modeled as a graph, each agent is a node, and the encoding of local observation of agent is the feature of node. We apply convolution to the graph of agents. By employing multi-head attention as the convolution kernel, graph convolution is able to extract the relation representation between nodes and convolve the features from neighboring nodes just like a neuron in a convolutional neural network (CNN). Latent features extracted from gradually increased receptive fields are exploited to learn cooperative policies. Moreover, the relation representation is temporally regularized to help the agent develop consistent cooperative policy.

The codes are the implementations of DGN in the scenario, Routing, presented in the paper
[Graph Convolution Reinforcement Learning](https://arxiv.org/abs/1810.09202)

In DGN, all agents share weights for the modules. The main reason is that agents use relation kernels to extract their relations based the encodings of their observations. If the encoders are different (agents encodes the observation in different ways), the relation kernels can hardly learn to extract their relations since the graph of agents is highly dynamic. 


### Dependencies

Install dependencies using pip.

```shell
python -m pip install -r requirements.txt
```

### Citation

If you are using the codes, please cite the paper.

[Jiechuan Jiang, Chen Dun, Tiejun Huang, and Zongqing Lu. *Graph convolutional reinforcement learning*. ICLR'20.](https://arxiv.org/abs/1810.09202)

	@inproceedings{jiang2020graph,
        	title={Graph Convolutional Reinforcement Learning},
        	author={Jiang, Jiechuan and Dun, Chen and Huang, Tiejun and Lu, Zongqing},
        	booktitle={ICLR},
        	year={2020}
	}

