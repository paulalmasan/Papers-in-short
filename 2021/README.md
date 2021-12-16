# Summarized papers published during 2021

[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/)  

### *Network Planning with Deep Reinforcement Learning*
H Zhu, V Gupta, SS Ahuja, et. al. SIGCOMM 2021   
Link to paper: <a href="https://dl.acm.org/doi/10.1145/3452296.3472902" target="_blank" rel="noopener noreferrer">https://dl.acm.org/doi/10.1145/3452296.3472902</a>  
Code available? <b style="color:green;">YES</b>  
<a href="https://github.com/netx-repo/neuroplan" target="_blank" rel="noopener noreferrer">Link to code</a>  

#### Keywords
Network Planning, Deep Reinforcement Learning, Graph Neural Network

#### Problem addressed
The paper is trying to solve the Network Planning combinatorial optimization problem. This problem consists of dimensioning the backbone network (both in link features and link connections) and at the same time to satisfy some performance requirements (e.g., sufficient bandwidth to absorb the traffic matrix). For example, whose link capacities need to be increased given some future traffic prediction or where should we add a link to ensure connectivity in case of link failures.  

#### Background
Several properties define the challenges of real-world network planning. First, it is a cross-layer process as it considers both IP and physical (optical) layers. Second, it’s an iterative process where some steps must be planned some time in advance (e.g., the physical deployment of a new fiber path can take several months or even years). Finally, to ensure network reliability, other factors such as link failures or hardware upgrades must be taken into account when dimensioning the network.  

Network planning can be formulated as a combinatorial optimization problem where the objective function is to minimize the network cost. The authors simplify the cost function by representing it as the sum of link costs for the optical and IP layers (see Equation 1). This problem can be solved using state-of-the-art solvers (e.g., CPLEX, Gurobi) but they suffer from scalability issues. This means that they can find the optimal solution fast in small problems, but when the problem size grows (i.e., larger topologies with hundreds of nodes), the ILP solver can take weeks or months to find the optimal solution. This forces network operators to use hand-tuned heuristics to solve the problem in a reasonable amount of time.  

#### Solution
The authors propose NeuroPlan, a two-stage solution to solve the network planning problem. In the first stage, they use DRL to find a good solution and in the second stage they use Integer Linear Programming (ILP) to improve the solution found by the DRL agent. The authors propose to use a tunable parameter alpha to indicate to the ILP solver how far from the DRL’s solution he can explore the solution space. In other words, a high alpha value will let ILP explore a larger space, which means higher chances to find close-to-optimal solutions, but it might take too much time. On the opposite, low alphas restrict the search space but the ILP will find a good solution in a shorter period of time.  

Figure 3 shows the workflow of the proposed solution. NeuroPlan takes five inputs and outputs the new network plan. From these inputs, only the network topology is used by the DRL agent, while the other ones are used in the plan evaluator. Notice that the network plan evaluator corresponds to the environment in a DRL setup, and is in charge of generating rewards for the DRL agent. Following an iterative process, the DRL agent performs an action (e.g., increase link capacity) and the plan evaluator generates a reward to indicate how good this action was. After some steps (until the end of the episode), the first stage outputs a network plan. Then, the solution is translated to some constraints that will be used in the ILP solver. The intuition is to make the ILP solver find better network plans than the one found by the DRL agent. The following summarizes the DRL setup:
* State: Node topology and node features. The solution makes a link to node transformation, where links are represented as nodes and the link features become node features (i.e., link capacity).  
* Action: The action is to select the link whose capacity will be increased, together with the capacity value to add to the existing link capacity.  
* Reward: The reward is divided into 2 values. First there is the final reward (at the end of the episode) which indicates the network plan cost. Then, to help the DRL agent learn and avoid sparse rewards, the authors designed an intermediate reward that corresponds to the cost of the new added link capacity.  

#### Evaluation
One of the first experiments the authors execute is to test NeuroPlan’s performance and compare it with the optimum solution for small problems. Figure 8 shows the results of solely the DRL part (First-stage), NeuroPlan and an ILP solver. The results indicate that NeuroPlan achieves close-to-optimal solutions for the same topology with varying link capacities. Then they perform similar experiments in larger topologies. As the other topologies are larger, ILP doesn’t finish in a reasonable amount of time. Finally, the authors study the impact in performance of different parameters from the GNN.  

#### Take home ideas
* NeuroPlan uses a DRL agent that integrates a GNN. Their solution has a first-stage where they use DRL to find an initial solution to the problem and then they use a second stage based on ILP to improve the solution  
* The authors introduce a parameter alpha that is used to trade optimality by execution cost (in time)  
* The experimental results show that NeuroPlan achieves close-to-optimal results  

[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/) | [Go to top of the page](#summarized-papers-published-during-2021)

### *Graph neural networks for scalable radio resource management: Architecture design and theoretical analysis*
Y Shen, Y Shi, J Zhang, et. al. IEEE Journal on Selected Areas in Communications 2021   
Link to paper: <a href="https://arxiv.org/abs/2007.07632" target="_blank" rel="noopener noreferrer">https://arxiv.org/abs/2007.07632</a>  
Code available? <b style="color:green;">YES</b>  
<a href="https://github.com/yshenaw/GNN-Resource-Management" target="_blank" rel="noopener noreferrer">Link to code</a>  

#### Keywords
Graph Neural Networks, Radio Resource Management  

#### Problem addressed
The problems addressed in this paper are radio resource management problems. Some examples of such problems are beamforming and power control in wireless networks.  

#### Background
Radio resource management problems are complex problems. This is because such problems require to be solved in real-time, the problem’s wireless channels change during time (i.e., dynamic topology) and they require low latency to enable mobile applications. Existing solutions scale poorly as the problem complexity grows (e.g., larger wireless topologies). In the last few years, the research community has been studying the use of Deep Learning (DL) techniques to solve radio resource management problems. The DL-based solutions were shown to achieve high optimization performance but they lack generalization capabilities or they have issues to scale to larger problems.  

The authors propose the use of Graph Neural Networks (GNN) to model wireless networks and to solve resource management problems. First, they demonstrate that such problems can be formulated as graph problems and that they satisfy the permutation equivariance property. The authors argue that this property can be used for the design of the GNN architecture. Then, they prove the equivalence between their DL-based solution and traditional optimizers. This is to make the analysis of the performance and generalization results more tractable. In other words, they use this equivalence to build a theoretical framework to obtain generalization performance guarantees. Finally, several experiments on three radio resource management problems are performed to study the performance of the GNN-based solution. Among these experiments, there is the sum rate maximization, the weighted sum rate maximization and beamformer design.  

#### Solution
The authors provide a detailed theoretical analysis of radio resource management problems. Then, they propose a GNN architecture based on the Message Passing Neural Networks (MPNN), called wireless channel graph convolution network (WCGCN). They also review the key properties of MPNN architectures (e.g., permutation equivariance, generalization, training complexity). Then, they make a theoretical analysis of the MPNN architecture for radio resource management. To do this, they prove the equivalence between MPNN and traditional distributed optimization algorithms and they analyze the performance obtained by the MPNN architecture.  

#### Evaluation
The authors test the MPNN architecture performance on three different radio resource management problems. The first one is the sum rate maximization problem. The results from Table I indicates that WCGCN achieves near-optimal performance. The second scenario is the weighted sum rate maximization. In this scenario, they run two kinds of experiments: performance comparison and generalization to larger problem instances or higher densities. The results indicate that WCGCN has a competitive performance to other baselines but it’s more stable when generalizing to larger problem sizes. The last experiment is the beamformer design. Similarly to the previous experiment, they test the performance and generalization performances, but they also make a study of the computation time. The results are in the same line like the previous experiments. The computation time shows that WCGCN is almost constant when executed on GPUs and inear when is executed on CPUs for different problem sizes.  

#### Take home ideas
* GNN enables the design of architecture to solve radio resource management problems that achieve competitive results but with strong generalization capabilities to larger problem instances  

[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/) | [Go to top of the page](#summarized-papers-published-during-2021)