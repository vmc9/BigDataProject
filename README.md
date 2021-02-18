# BigDataProject
### A dataset analysis project leveraging Big Data techniques and Machine Learning algorithms.

We plan on performing a dataset analysis on cyber attack data in the context of  IoT device botnet attacks with data
 compiled from the UCI repository. 

## Introduction

This project will encompass an analysis of network packet data generated by IoT devices that have been compromised
 and ensnared by a botnet that can control and command devices remotely to launch different kinds of attacks. 
 
The data we will consider comes from research published by Yair Meidan, Michael Bohadana, Yael Mathov, Yisroel Mirsky,
Dominik Breitenbacher, Asaf Shabtai, and Yuval Elovici. The dataset was donated by Yair Meidan in 2018 to the UCI
 Machine Learning repository.
 
The dataset was originally used to construct a "centralized, automated method that is highly effective and
 accurate in detecting compromised IoT devices which have been added to a botnet and have been used to launch attacks
 " using deep learning techniques (autoencoders) in this [paper](https://arxiv.org/abs/1805.03409).

Our objective is to perform two kinds of classification. 
##### Binary Classification
- The first is to classify any instance of data as
malign or benign network traffic. The dataset includes benign / malign data
 instances already allowing binary classification without modifying the dataset.
- Here we plan to use decision trees or random forrest to perform the classification task.
##### Multi-Class Classification
- The second objective is to classify any instance of data as originating from a type of network attack. The dataset
 does not include attack type / botnet type / device type as labels, but these can be
  easily added and we can splice and bind the data into one or more dataset(s) that can be used for multi-class
   classification.
- Here we plan to use k-means clustering to perform the classification task.
   
Successful classification in our first objective will help demonstrate that Big Data analysis techniques can be
 utilized alongside Machine Learning algorithms to detect incoming botnet attacks, and to label network traffic in an
  IoT context as dangerous or not. The generalizations of such findings could be applicable in many different
   scenarios considering the pervasiveness of botnet attacks and wide adoption of IoT devices.
   
 Successful classification in our second objective will help demonstrate that Big Data analysis techniques can be
  utilized alongside Machine Learning algorithms to identify network data as associated with a known attack pattern
  , or a known botnet. The generalization of such findings could be applicable in cyber forensics, threat detection
  , and threat mitigation.

## Meterials and Methods

### [detection_of_IoT_botnet_attacks_N_BaIoT Data Set](http://archive.ics.uci.edu/ml/datasets/detection_of_IoT_botnet_attacks_N_BaIoT#)

### Dataset Summary:
This dataset is comprised of data instances generated from actual traffic data that was originated in compromised IoT
 devices from a set of 9 different kinds of IoT device. Most of them have some kind of wireless camera component and
  range from doorbell cams, to baby monitors. The data was gathered from compromised devices infected with either the
   Mirai, or the Bashlite botnet. Regular device network data was also gathered for training on.

The data instances were generated by creating snapshots described in the research paper as follows:

"Whenever a packet arrives, we take a behavioral snapshot of the hosts and protocols that communicated this packet
 .The snapshot obtains the packet’s context by extracting 115 traffic statistics over several temporal windows to
  summarize all of the traffic that has (1) originated from the same IP in general, (2) originated from both the same
   source MAC and the same IP address, (3) been sent between the source and destination IPs (channel), and (4) been
    sent between the source to destination TCP/UDP sockets (socket).

We extract the same set of 23 features (capturing the above, see Table 2) from five time windows of the most recent
 100ms, 500ms, 1.5sec, 10sec, and 1min."

### Details:

In total, there are 7062606 data instances, defined by 115 attributes computed from 23 different features, across 5
 different time windows. All specifications come from the data set description available at the UCI repo, relayed
  here for convenience.

##### Features

The 23 features aggregated from the raw data correspond to the following "traffic statistics" :

- MI: Stats summarizing the recent traffic from this packet's host (IP + MAC)
  - Mean, Variance, Weight
  
- H: Stats summarizing the recent traffic from this packet's host (IP)
  -  Mean, Variance, Weight

- HH: Stats summarizing the recent traffic going from this packet's host (IP) to the
 packet's destination host.
  - Mean, Weight, Magnitude, Radius, Pearson Correlation Coefficient, Covariance, Standard Deviation
 
- HH_jit: Stats summarizing the jitter of the traffic going from this packet's
 host (IP) to the packet's destination host.
  - Mean, Variance, Weight
 
- HpHp: Stats summarizing the recent traffic going from this packet's host+port (IP) to
 the packet's destination host+port. Example 192.168.4.2:1242 -> 192.168.4.12:80
  - Mean, Weight, Magnitude, Radius, Pearson Correlation Coefficient, Covariance, Standard Deviation

##### Statistic Definitions
- Weight: The weight of the stream (can be viewed as the number of items observed in recent history)
- Radius: The root squared sum of the two streams' variances
- Magnitude: The root squared sum of the two streams' means 
- Covariance: An approximated covariance between two streams
- PCC: An approximated correlation coefficient between two streams

##### Time Windows
Each of the 23 features is computes within a specific time window: 100ms, 500ms, 1.5sec, 10sec, and 1min.
Each time-frame is how much recent history of the stream is capture in these statistics.
- L5, L3, L1, L0.1 and L0.01

### Methods Summary:

##### Random Forrest / Decision Tree

##### K-means Clustering