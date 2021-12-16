# Summarized papers published during 2016

[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/)  

### *Resource management with deep reinforcement learning*
H Mao, M Alizadeh, I Menache, S Kandula. HotNets 2016: Proceedings of the 15th ACM Workshop on Hot Topics in Networks  
<a href="https://dl.acm.org/doi/abs/10.1145/3005745.3005750?casa_token=X81PmxfQfLEAAAAA:oe-fWxAyDKqsbsU-fRJ-Pt48sNbBjhU6yx38U68d93wIgvXmtqFNcfh3xi4xFm0n3kauCPp_pkI" target="_blank" rel="noopener noreferrer">Link to paper</a>  
Code available? <b style="color:green;">YES</b>  
<a href="https://github.com/hongzimao/deeprm" target="_blank" rel="noopener noreferrer">Link to code</a>  

#### Keywords
Deep Reinforcement Learning, Resource Management, Job Scheduling 

#### Problem addressed
The paper addresses the resource management problem. The authors are trying to investigate if machine learning-based systems can learn to manage resources by themselves.    

#### Background
Existing solutions for resource management problems (e.g., job scheduling, congestion control) are based on heuristics and expert knowledge. Such solutions typically make assumptions to simplify the problem and they don’t adapt to dynamic scenarios (e.g., when system workload changes with time). Consequently, the heuristics will perform well when the resource management scenario meets the assumptions made during the design process.  

The authors argue that Deep Reinforcement Learning (DRL) is a technology that suits well for solving the resource management problem. The latest works in DRL indicate that this technology enables learning of complex decision-making policies in challenging environments to optimize without having previous knowledge of the problem. Therefore, the authors propose DeepRM, a DRL-based solution for solving a resource management problem.  

Their resource management problem consists of several parts. The first one is the cluster with the set of resource types (e.g., memory, CPU). Each of the cluster resources are represented by a grid where allocated jobs occupy positions of that grid. Figure 2 illustrates the state representation. When it’s filled, this means the cluster has no more free resources available. The second component is the jobs queue which has a fixed length (i.e., it can only store up to N jobs). Each job is represented by as many grids as resources there are in the cluster, and each grid is partially filled. This is to indicate, for each job, how much CPU and memory resources is going to occupy. The scheduling problem can be seen as a tetris game where the jobs are the incoming blocks and the cluster resources status (i.e., CPU and memory) are the main panels where to allocate the incoming jobs. The last part is the backlog which counts how many jobs there are, beyond the ones in the queue.   

#### Solution
The DRL agent is trained using the REINFORCE algorithm. To reduce variance, they use a baseline value (i.e., average of the return values). Each job comes with the demand already specified and some duration of the job. The objective function to minimize is the average job slowdown (i.e., the completion time of the job divided by the duration time).
The following summarizes the DRL setup:  
* State: The state is represented by the current cluster allocation configuration, the job characteristics of the jobs waiting in the queue to be allocated in the cluster and the backlog status.  
* Action: The action space consists of allocating all the jobs (or part of them) that are waiting in the queue. This means that if there are 4 Jobs in the queue, the action is to allocate each of them or until the agent can not allocate more jobs.  
* Reward: The reward at each timestep is a negative inverse sum of the ideal job time of all the jobs in the system (including the ones from the backlog).  

#### Evaluation
The authors use several baselines to compare with their DRL agent performance. There is Shortest Job First (SJF), Packer and Tetris, where the last two are solutions based on previous work. Several experiments are performed to study the DRL’s performance. Figure 4 from the paper shows the results of DeepRM against the baselines. The results indicate that DeepRM obtains better performance than the other baselines as the average cluster load increases. Additional experiments are executed to study the convergence properties of the DRL agent. Specifically, Figure 6 shows the evolution of the agent performance during training. In addition, the experiments from Figure 7 are trying to indicate why DeepRM’s performance is better than the other baselines. The results indicate that DeepRM freezes the large jobs in the waiting queue and processes the small jobs.  

<a href="https://dl.acm.org/doi/abs/10.1145/3005745.3005750?casa_token=X81PmxfQfLEAAAAA:oe-fWxAyDKqsbsU-fRJ-Pt48sNbBjhU6yx38U68d93wIgvXmtqFNcfh3xi4xFm0n3kauCPp_pkI" target="_blank" rel="noopener noreferrer">Images source</a>  

#### Take home ideas
* DRL is a technology that showed to have a good performance in resource management problems  
* DRL can outperform existing heuristics for the scheduling problem  
  
[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/) | [Go to top of the page](#summarized-papers-published-during-2016)
