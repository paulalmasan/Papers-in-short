# Summarized papers published during 2018

[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/)  

### *AuTO: Scaling Deep Reinforcement Learning for Datacenter-Scale Automatic Traffic Optimization*

L Chen, J Lingys, K Chen, F Liu. SIGCOMM 2018   
<a href="https://dl.acm.org/doi/abs/10.1145/3230543.3230551?casa_token=NxCL8PxJwosAAAAA:aL8sq6q2fCEqgX0BEKmj8T0ZBsCfHNxtZRpoRLpRwALSyzvHwflXsd-7TPL-VgLwJCL_hz9HnWc" target="_blank" rel="noopener noreferrer">Link to paper</a>  
Code available? <b style="color:red;">NO</b> 

#### Keywords
Deep Reinforcement Learning, Traffic Optimization, Datacenter networks

#### Problem addressed
This paper studies how to enable DRL-based solutions for Traffic Optimization (TO) in datacenter networks. Some examples of TO are load balancing, routing, flow scheduling and congestion control. Existing solutions are based on expert knowledge and handcrafted heuristics. Such solutions are not desirable because they suffer from performance degradation when they don’t match the network traffic. In addition, the process to design a solution is long (in the order of weeks) due to the many steps involved (e.g., collect statistics, analyze data, design heuristics, test, simulate, etc.)  
Datacenter traffic flows follow a long-tail distribution, which means that the majority of flows are short flows but most of the bytes come from long flows. The authors start by making a study to verify the effectiveness of DRL for TO. The results of the experiments indicate that existing DRL systems suffer from long processing delays, which means that short flows are gone before the DRL agent can take a decision. Thus, novel approaches should be explored to apply DRL in TO.

#### Background
The paper proposes AuTO, a DRL system that mimics the Peripheral System (PS) and Central Nervous System (CS) in animals. PS are executed on end-hosts and they make immediate decisions locally for short flows. In addition, PSs collect flow information which is later sent to the CS and processed. In Figure 3 we can see an overview.  

The PS is composed by a Monitoring Module and an Enforcement Module (EM). The former collects flow-related information (e.g., flow completion time, flow size) and sends it to the CS. The latter is used to make decisions locally for short flows. The EM is composed of a Multi-Level Feedback Queueing (MLFQ) which separates short flows (located in the first queues) from long flows (located in the last queue), based on some thresholds (see Figure 4). The flow priorities are changed according to the MLFQ thresholds (i.e., flows are tagged based on these thresholds). In other words, the longer the flow the lower the priority. Once the flows are tagged, the decisions for the short flows are taken locally in the PS (i.e., they use ECMP for routing and with no rate limit).  

The CS has two DRL agents: one in charge of optimizing the MLFQ thresholds (sRLA) and the other determines the priorities, routes and rate limit of long flows (lRLA). With the information received from the PSs, the sRLA agent takes global TO decisions and sends them to the PSs (in the form of MLFQ thresholds). Thus, using these queues the PS can take decisions locally for the short flows before they are gone. The actions for the long flows don’t need to be taken as fast as for the short flows but they are more important as the flow takes more time to finish. Therefore, the authors propose to process the long flows in the CS (by the lRLA) to properly determine the actions of the longer flows.  

#### Solution
The proposed solutions consist of two DRL agents located in the CS: sRLA and lRLA. The sRLA agent state space consists of all the finished flows in the entire network per time step. The action space consists of the MLFQ threshold values. Finally, the objective functions of two consecutive time steps are divided to obtain a reward for the DRL agent. In other words, the reward indicates if the previous action (i.e. MLFQ thresholds) led to a lower average Flow Completion Time or not. This first agent is trained using an actor-critic method implementing the DDPG learning algorithm.  
The lRLA agent state space consists of all active flows and all finished flows per time step. The DRL agent action space is composed of multiple values for each active flow per time step: a flow priority, a rate limit and the routing path. The average throughput of two consecutive time steps is divided to obtain the reward. To train this agent, they use a variant of the REINFORCE algorithm.  

#### Evaluation
The authors evaluate AuTO on experiments in a real testbed using realistic traffic workloads. They use two baselines for comparison: Quantized Shortest-Job-First (QSJF) and Quantized Least-Attained-Service-First (QLAS). They group the experiments in 4 groups:  
* First, they study AuTO’s performance with homogeneous traffic (flow size distribution and traffic load is fixed). The results from Figure 9 indicate that AuTO is capable of generating MLFQ thresholds that lead to a reduction in the average FCT up to ~48%.  
* The second experiment is using spatially heterogeneous traffic (different loads and flow size distributions). Results from Figure 10 indicate that AuTO is also capable of optimizing MLFQ’s weights in this more complex scenario.  
* In the third experiment, they use a varying traffic load and flow size distribution (i.e., they are changed hourly). Figure 11 and Figure 12 indicates that AuTO adapts well to these changes while the heuristics onyl have a low FCT when the traffic matches their configuration. In addition, AuTO’s average FCT doesn’t stop decreasing, which indicates that it’s continuously learning.  
* The final experiment is to study AuTO’s overhead. The results indicate that AuTO can respond in ~10ms on average to an environment state update.

<a href="https://dl.acm.org/doi/abs/10.1145/3230543.3230551?casa_token=NxCL8PxJwosAAAAA:aL8sq6q2fCEqgX0BEKmj8T0ZBsCfHNxtZRpoRLpRwALSyzvHwflXsd-7TPL-VgLwJCL_hz9HnWc" target="_blank" rel="noopener noreferrer">Images source</a>  

#### Take home ideas
* In a datacenter scenario, the majority of flows are short flows. Thus, DRL techniques must be carefully designed for solving datacenter optimization problems because they have a high latency. This is because short flows are already gone by the time the DRL agent decided the best action to perform.
* The paper proposes a method where actions for short flows are taken locally in the PS, while actions for long flows are taken centrally by a DRL agent. 
 
[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/) | [Go to top of the page](#summarized-papers-published-during-2018)


### *Experience-driven Networking: A Deep Reinforcement Learning based Approach*
Z Xu, J Tang, J Meng, W Zhang, Y Wang, et. al. IEEE INFOCOM 2018 - IEEE Conference on Computer Communications  
Link to paper: <a href="https://arxiv.org/abs/1801.05757" target="_blank"  rel="noopener noreferrer">https://arxiv.org/abs/1801.05757</a>  
Code available? <b style="color:red;">NO</b> 

#### Keywords
Deep Reinforcement Learning, Networking, Traffic Engineering  

#### Problem addressed
This paper tries to solve the traffic engineering (TE) problem. This problem consists of a set of network flows defined by src-dst node pairs (a.k.a., communication session), a set of candidate paths for each network flow and a traffic demand. Thus, the goal is to find the traffic rates (i.e., one rate for each of the candidate paths) to split the original traffic demands into such that it maximizes a utility function. This function is composed of the end-to-end throughput and the delay.  

#### Background
The authors propose a generic DRL-based control framework (DRL-TE) to solve the TE problem. The authors argue that existing solutions are based on simplifications and strong assumptions that might not hold in complex scenarios. The paper proposes DRL as a key technology to solve the TE problem because it’s capable of operating in time-variant environments and it can handle complex state spaces. The authors argue that to apply a DDPG framework to train the DRL agent is not enough to obtain good performance. This is due to the bad exploration for TE and the very simple uniform sampling used in the experience replay. The latter is motivated by the fact that a DRL can learn more from some transitions than others.  

#### Solution
In this framework, the state space consists of the throughput and delay of each communication session. The action space is a set of split ratios for all candidate paths for all communication sessions, and the reward is the total utility (based on the utility function) of all communication sessions. The authors discuss the limitations of vanilla DDPG and they propose two improvements: a TE-aware exploration strategy and a priority experience replay for the actor-critic method.  

#### Evaluation
The DRL’s agent performance evaluation is tested on 3 network topologies: NSFNET, ARPANET and a randomly generated topology. For each topology, there are a total of 20 communication sessions whose source and destination nodes were randomly chosen. The candidate paths for each communication session were chosen using the 3-shortest paths in terms of number of hops. The performance is compared with 4 other baselines: Shortest Path, Load Balancing, Network Utility Maximization by solving a convex problem and vanilla DDPG. The evaluation results show that the DRL-TE achieves better end-to-end delay and network utility (based on the utility function) than the baselines in all 3 topologies. The performance regarding the end-to-end throughput doesn't improve clearly. This could be caused by the complex optimization function which considers both delay and throughput.  

#### Take home ideas
* DRL enables experience-driven control in solving TE problems  
* Vanilla DRL methods might not be working well if we directly apply them to complex scenarios. Thus, it might be necessary to merge different DRL techniques to address complex problems  
  
[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/) | [Go to top of the page](#summarized-papers-published-during-2018)
