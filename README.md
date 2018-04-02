# [Video4U](https://www.yuanhuang.club/news)

### How it works

![alt text](./img/concept.png)

 In this section, I’ll explain how I build the recommendation engine from the ground up.

* Step 1: [Finding users with similar interests](#finding-readers-with-similar-interests)

* Step 2: [Topic modeling](#topic-modeling)

* Step 3: [Making recommendations](#making-recommendations)

* Step 4: [Evaluation of the recommender](#evaluation-of-the-recommender)

#### Evaluation of the recommender

Okay, that was cool. But how do I know whether the recommendation engine is working well or just spitting out random selections? How much will the users like the recommendations that they get? In fact, the evaluation of a recommending system can be quite tricky. The golden metric for a recommending system is how much the system will add value to the user and business. Ultimately, you want to perform A/B testing to see whether recommendations will increase usage, subscriptions, clicks, etc.

However, in practice there are other common metrics to evaluate a recommender, which can still help us gain some insights of the performance before actually putting the system into use. Most of these offline methods need us to hold out a subset of the items from the training data set (in our case, holding out a subset of previously retweeted news posts), pretending the users haven’t seen these items and trying to recommend them back to the users. Since the reader groups’ history about this test set already exists, we can leverage on this information to validate the performance of recommender.

One natural goal of recommender systems is to distinguish good recommendations from bad ones. In the binary case, this is very natural — a “1” is a good recommendation, while “0” means a bad recommendation. However, since the data that we have (number of retweets of an article by users from a user group) is non-binary, a threshold must be chosen such that all ratings above the threshold are good and called “1”, while the rest are bad with label “0”. A natural way to set the threshold is to choose the median value of the number of retweets in a user group, leaving all the articles above median “good” recommendations and all the rest “bad”. The predicted score from the recommending system is the cosine similarity between the topics of an article and the topics in a user group, which ranges from 0 to 1.

This good/bad, positive/negative framework is the same as binary classification in other machine learning settings. Therefore, standard classification metrics could be useful here. Two basic metrics are precision and recall. In our project, precision is fraction of good recommendations we got correct, out of all the articles got recommended by the system. Recall is the fraction good recommendations we got correct, out of all the “good” articles in the test set. 

All these metrics are clear once we have defined the threshold of good/bad in our predictions. For instance, in a binary situation our labels are 0, 1 while our predictions are continuous from 0–1. To compare the predictions, we select a threshold (here we choose the median value of all the predictions), above which we call predictions 1 and below 0. From 10 rounds of validations with 1000 hold-out articles, the average precision score of our recommending system is 65.9%, while the average recall is 66.5%.

The choice of the threshold is left to the user, and can be varied depending on desired tradeoffs. Therefore to summarize classification performance generally, we need metrics that can provide summaries over this threshold. One tool for generating such a metric is the Receiver Operator Characteristic (ROC) curve, which plots the True Positive Rate (TPR) versus the False Positive Rate (FPR) at different threshold levels. The area under the curve (often referred to as the AUC) indicates the probability that a classifier will rank a randomly chosen positive instance higher than a randomly chosen negative one. The average AUC of our recommender is 0.77, which is a fair score that can provide some insight on the predictive power of our model.

![alt text](./img/roc.png)

### Summary & What’s Next

The framework of this recommendation engine is quite versatile: it can be adapted to combine data from different platforms (Twitter, Facebook, news websites) to construct the user network; if combined with more personal information from the users, the system can also be developed to analyze user behavior and help making better marketing and advertising decisions.
