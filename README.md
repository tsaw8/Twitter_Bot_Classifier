# Twitter Bot Classifier
### Background
Over the past ten plus years, Twitter has explosively evolved into a major communication hub. "Twitter can be a very helpful platform for growing a following and providing your audience with valuable content before they even become customers" [Hubspot](https://blog.hubspot.com/marketing/what-is-twitter). However, not all accounts are genuine users. According to a Twitter SEC filing in 2017, Twitter estimated 8.5% of all users to be bots. To validate the credibility of communication exchanged on the platform, efforts in identifying spam bots will help improve the user's experience on twitter. 

* Part I: Use several supervised learning algorithms to design a text classification model to detect Twitter bot accounts
* Part II: Utilize clustering models to identify tweet behavioral patterns

### Files
* Twitter Bot Classifier - Assemble Datasets: Create and format dataset 
* Twitter Bot Classifier - Preprocessing, Modeling, and Interpretation: Preform modeling 

### Overview of Data
We will be using [The Fake Project](https://botometer.iuni.iu.edu/bot-repository/datasets.html) collected by S. Cresci, R. Di Pietro, M. Petrocchi, A. Spognardi, M. Tesconi in 2017. 

Tweets and users tables were concatenated together to form one dataset for modeling. 

| Genuine Tweets  | Bot Tweets   |
| :--------------------- | :-----------------|
| 2,826,718             | 3,456,718      |

## Part I: Building a Bot Classifier 

#### Text Preprocessing:
- [x]  Subsampling: use only 0.05% of tweets due to hardware and memory limitations
- [x]  Filter out none English tweets and strip irrelevant information(hyperlinks, mentions, etc.)
- [x]  Create bag of words with CountVectorization 
- [x]  Word Frequencies with Tfidf-Transformation 
- [x]  Dimensionality Reduction with TruncatedSVD

### Metric:
We will evaluate our models based on how well it can classify genuine and bot tweets with the __ROC AUC score__.
We’ll also focus on the tradeoff between:
* false positive rate
* false negative rate

### Models:
* Logistic Regression
* Random Forest Classifier 
* Multiple-Perceptron-Classifier

### Results 
#### Model Performance

![](images/sl_results.png?raw=true)<br>

__Observation__: All the models performed fairly well in across all metrics. However, the average scores of the random forest model with a 100% precision after hyperparameter tuning. Logistic regression might not been able to capture non-linear relationships in the data. 


#### Random Forest: Feature Importance 

![](images/feat_imp.png?raw=true)<br>

__Observation__: User profile features play an important role in classifying tweets based on the user's favorite, status, and friend count. There might be bias in the data since there is importance on the tweet year and tweet month. Twitter might have made updates to their bot detection and thus providing new classification results.



## Part II: Clustering Accounts

#### Clustering Preprocessing:
- [x]  Normalize data
- [x]  Reduce Dimensions: PCA vs. TSNE <br>

![](images/tsne.png?raw=true)<br>

__Observation__: Although PCA processed quickly, T-SNE was better at forming distinct tweet groups. 

### Metric:
Homogeneity: measures how many members of a single class is within a cluster 
Silhouette Coefficient: means distance between a sample and all other points in the same class

### Models:
* KMeans
* DBSCAN
* Mean-shift

### Clustering Analysis
Clustering techniques were not effective in separating genuine and bot tweets. Instead, KMeans was used to create 5 clusters to reflect tweet behavior patterns. 

### Conclusion
From supervised learning, we compared the performance of three different models (logistic regression, random forest, and neural network) to classifying a given Twitter user as bot or human.  We found that the random forest classifier had the best ROC AUC and precision on predictions. Textual features created from CountVectorization and TF-IDF transformation was helpful in classifying the user. Profile features such as favorite, status, and friends count played an important role in differentiating between the tweet types.

We continued to explore user patterns by clustering similar tweets. We used three clustering methods (KMeans, DBSCAN, and Mean-shift) to see which model would return sensible groups.  DBSCAN produced over a thousand groups and Mean-shift recommended one cluster. Thus, we used KMeans to partition the data into segments. Using the elbow method, 4 to 5 clusters was optimal. Clusters that consist of most genuine tweets had a larger following and a higher friend count. Groups with more bot tweets had a lower average of favorite and retweet counts. 

In future work, I would like to collect my own dataset to limit mismatch in the training and testing set. I would also like to expand on the English-only tweet corpus by incorporating features like tweet sentiment analysis and topic extraction. Learning about the evolution of bot accounts would also be helpful in classifying the different types of bot. The insights from this project can be used to build a web application that can classify Twitter users in real-time.

