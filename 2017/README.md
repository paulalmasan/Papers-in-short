# Summarized papers published during 2017

[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/)  

### *Neural Adaptive Video Streaming with Pensieve*
H Mao, R Netravali, M Alizadeh. SIGCOMM 2017  
<a href="https://dl.acm.org/doi/10.1145/3098822.3098843" target="_blank" rel="noopener noreferrer">Link to paper</a>  
Code available? <b style="color:green;">YES</b>  
<a href="https://github.com/hongzimao/pensieve" target="_blank" rel="noopener noreferrer">Link to code</a>  

#### Keywords
Reinforcement Learning, Adaptive Video Streaming  

#### Problem addressed
In a video streaming context, the videos are stored on servers as multiple chunks. Each of these chunks are encoded at different bitrates. A high bitrate translates to a higher video quality and consequently a larger chunk size. When a client wants to reproduce a video on his device, he uses an adaptive bitrate (ABR) algorithm to request video chunks one at a time. These ABR algorithms use several network related information to select the bitrate at which to download the next video chunks. The goal is to dynamically select the bitrates that maximize some Quality of Experience (QoE) metrics.  

#### Background
ABR algorithms are used by content providers to optimize the video quality delivered to the clients. These algorithms make decisions about the bitrate used during the download of video chunks with the goal of maximizing the user’s QoE. The decision of selecting the proper bitrate is a difficult task due to several reasons. For example, the network conditions fluctuate with time and the different QoE metrics that want to be maximized are usually in conflict between each other (e.g., maximize video quality and minimize the events when the client’s buffer is empty). This makes it necessary for the ABR algorithms to be able to dynamically adapt to the network status and the user’s requirements.  

Existing ABR algorithms typically are designed using estimations of the network throughput. This means that existing ABR algorithms include fixed rules, obtained from network estimations, which makes them rigid and not able to adapt to evolving network states. The authors propose Pensieve, an ABR algorithm based on Deep Reinforcement Learning (DRL) that is able to dynamically choose the proper chunk bitrate by taking into account the QoE and the current network status. The particularity of the proposed solution is that Pensieve runs on an ABR server instead of the client’s device. When the client wants to request a video chunk, it first queries the ABR server to obtain the bitrate for the next chunk to download. When making the query, the client includes observations about the network state (or environment) in each request sent to the ABR server.  

#### Solution
The authors propose to use a DRL agent to select the bitrate at which the video chunks will be downloaded. Figure 2 (see the paper) shows an overview of the proposed solution. The DRL agent receives some network metrics (i.e., playback buffer occupation, past bitrate decisions, network throughput measurements) and outputs the action (i.e., the bitrate value to use to download the next video chunk). Consequently, some QoE metrics will be obtained after downloading the video chunk. These metrics are sent to the DRL agent as a reward signal, which is then used to train the Neural Network (NN). The authors use A3C to train the DRL agent. The following summarizes the DRL setup:  
* State: The environment state is defined by the network throughput measurements of the last K chunks, the downloading time of the last K chunks, the current buffer occupancy, the available sizes of the next chunk, the remaining number of chunks and the bitrate used to download the last chunk.  
* Action: The DRL agent outputs a probability distribution over the different bitrates available to download the next chunk. The action is to select the bitrate that corresponds to the highest probability.  
* Reward: The authors train a DRL agent for each of the QoE metrics used in their work. This means that the reward corresponds to the QoE metric used in the experiment.  

#### Evaluation
The authors offer an extensive evaluation of the proposed ABR algorithm. The authors are trying to compare Pensieve’s performance against state-of-the-art algorithms, they want to test the generalization capabilities and finally they want to study how sensitive Pensieve is to different parameters (see Section 5.4 to learn more about the parameters). In each experiment, Pensieve was trained to optimize for the corresponding QoE metrics. Table 1 shows the different QoE metrics considered in the evaluation experiments. Figure 7 shows the results of Pensieve compared against 5 baselines for each of the QoE metrics. The results indicate an outstanding performance from Pensieve in all metrics and for both datasets used (FCC and HSDPA). Figure 8 shows the CDF of the results for the FCC dataset and they also plot the optimal (computed offline with perfect information of future network throughput). To test the generalization, the authors train a DRL agent using only synthetic data and they evaluate it on real-world data. Figure 12 shows the experimental results.  

#### Take home ideas
* The solution proposes to run the DRL agent in an ABR server. Then, when the client makes the query about the next video chunk, the ABR server answers with the bitrate of the next chunk
* Pensieve showed an outstanding capability to optimize for different QoE metrics. The average performance gap between Pensieve and the online optimal is 0.2%
* Pensieve outperforms state-of-the-art ABR algorithms by 12%-25%
  
[Go to full paper list](https://paulalmasan.github.io/Papers-in-short/) | [Go to top of the page](#summarized-papers-published-during-2017)
