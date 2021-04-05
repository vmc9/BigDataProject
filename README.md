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
- The second objective is to run a clustering algorithm on the data, and compare using the data labels to see the acuracy of the clusters
- Here we plan to use kmneans to compare clustering with classification labels refering to the kind of data packet each data instance represents
- Once clustering is complete, we can inspect the clusters and see their composition to determine if clustering was done according to labels in any way
   
Successful classification in our first objective will help demonstrate that Big Data analysis techniques can be
 utilized alongside Machine Learning algorithms to detect incoming botnet attacks, and to label network traffic in an
  IoT context as dangerous or not. The generalizations of such findings could be applicable in many different
   scenarios considering the pervasiveness of botnet attacks and wide adoption of IoT devices.
   
 Successful clustering in our second objective will help demonstrate that Big Data analysis techniques can be
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

We will use Random Forest for 2-class classification. In particular, we would like to predict unlabelled datapoints that could be classified as either malign or benign.

As for data pre-processing, we will have to assign labels, either malign or benign, to some instances in our dataset. This can easily be done, since the data is nicely divided into malign and benign data sets.

Moreover, we intend to evaluate the 115 features to determine how much each is contributing to the prediction. Consequently, we may be able to reduce the number of features, which can improve the quality of our data, and further eradicate overfitting.

In the training process, we will test 2 purity metrics, Entropy and Gini. Also, we intend to do hyperparameter tuning. In random forest, hyperparameters are either used to increase predictive power, or to increase the model’s computation speed.

Some of the hyper-parameters that we will tune to increase predictive power are:
1. n_estimators: determines the number of trees that the algorithm uses to make a prediction.
2. max_features: represents the maximum number of features considered to split a node in a tree.
3. min_sample_leaf: determines the minimum number of leaves needed to split a node.

For computation speed, we will tune:
1. oob_score: it is a random forest cross-validation method.

During testing, we intend to use various evaluation metrics to compare results between different versions of the model. In particular, we will look at Accuracy, F1-score, and a confusion matrix.



##### K-means Clustering

We will use K-means Clustering to do multi-class classification. In particular, we expect to cluster the existing types of attacks in our dataset:
1. Network scanning for vulnerable devices
2. Junk: sending spam data
3. UDP flooding
4. TCP flooding
5. COMBO: sending spam and opening a connection
6. Ack flooding
7. Syn flooding
8. UDP flooding with fewer options.

The above could be grouped into fewer clusters, so we could have to adjust according to our initial results.

Since all our features have numerical values, we chose the Euclidean distance to find the nearest centroid of a particular data point. Moreover, to handle the different units of measurement among attributes, we will normalize the data points prior to computing the distance.

During training, we will run the algorithm using randomized initialization of centroids, and we will pick the version that satisfies 2 criteria: It generates lowest intra-cluster distance, and highest inter cluster distance. To do so, we will use the Inertia evaluation metric, and the Dunn index.

We will then use our optimal k-means algorithm to predict the clusters of a testing dataset, for which we will know the classes.

We intend to use various evaluation metrics to compare results between different versions of the model. In particular, we will look at Accuracy, F1-score, and a confusion matrix.


## Methods Results:

### Random Forest Results
- A naive attempt at random forest, with no data preprocessing, grid search, or hyperparameter tuning resulting in predictions which led us to believe that the model was overfitting the data

| Metric | Bashlite Result | Mirai Result |
| ------------- | ------------- |------------- |
| Accuracy | 99.94% | 99.99% |
| F1 | 99.94% | 99.99% |
| FP Rate for Benign | 0.07% | 0.0% |
| TP Rate for Bening | 99.96% | 99.98% |
| FP Rate for Malicious | 0.03% | 0.01% |
| TP Rate for Malicious | 99.92% | 99.99% |
| Precision | 99.94% | 99.99% |
| Recall | 99.94% | 99.99% |

- After this, we attempted to standardize the feature vectors, as well as run PCA to reduce the number of features from 115, to a smaller number that would be standardized to help prevent the model from overfitting.
- After running a PCA analysis, we determined that 13 features was enough to retain 99% of the variance, we proceded with 13 principal components.
- Unfortunately, even after maximizing the ammount of memory in Spark, we were unable to complete a single session of random forest using PCA without running into out of memory problems.
- We correctly diagnosed the overfiting, and planned accordingly to solve the problem, but our hands were tied regarding the capability of our machines to carry out and further experiment our with our solution.

### Kmeans Results
- We standardized our feature vectors to ensure that different features were scaled similarly for distance calculations in the clustering algorithm.
- We used a Silhouette analysis to optimize the number of clusters in a given range. Silhouette coefficients desrcibe the quality of clusters generated by kmeans on a range from 1 to -1. The closer the coefficient is to 1, the further that samples are from other clusters, and not close to the desicion boundary between clusters themselves.
- Using this analysis, we found the optimal k for clustering.

#### Mirai Results
- For Mirai, which has 5 kinds of attacks, alongside benign traffic, our analysis resulted in a local maximum of 6 clusters, which matched our labels.
  - All labels:  ACK, SCAN, SYN, UDP, UDP plain, Benign
  - K0 held 4% of all data clustered.
    - Cluster composition : 99.44% Benign, <1% for UDP, ACK, SYN, and UDP plain
  - K1 held 19.35% of all data clustered. 
    - Cluster composition: 50.47% UDP, 24.79% UDP plain, 24.68% ACK, 0.06% Benign
  - K2 held 29.75% of all data clustered.
    - Cluster composition: 64.84% UDP, 35.07% ACK, 0.05% Benign, 0.04% UDP plain
  - K3 held 0.75% of all data clustered.
    - Cluser composition: 100% Benign
  - K4  held 7.4% of all data clustered.
    - Cluster composition: 100% UDP plain
  - K5 held 38.6% of all data clustered.
    - Cluster composition: 45.13% SYN, 33.06% SCAN, 21.52% Bening, 0.29% ACK



## Discussion:
