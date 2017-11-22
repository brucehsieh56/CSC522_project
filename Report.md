## Background
Music streaming services, such as Spotify, Google Music, Apple Music, and Pandora, have become a trend and changed the way of how people listen to musics. Thanks to their availability and convenience, nowadays people can play new songs anytime and anywhere. In fact, listeners are sensitive to the popular songs, and expect the service that can keep them up with the trends. In order to engage the audience and advertise new songs, those streaming services have started to analyze tremendous amount of data collected from their users, try to predict the songs that people will like based on music features, and recommend the predicted songs to the user. This new habit of listening music has inspired us to study the possible techniques for song prediction as well as recommendation.

In addition, before releasing a new album or EP, a music company would like or eager to know whether the songs in this album will be a hit or not. If there is a great chance that songs in an album will be a great hit, then there is a higher possibility that a music company can make profits and will be more willing to invest in this album. For these reasons, a model that is capable of predicting whether a song will be a hit is very important in music industry. In our project, we are trying to build such a model based on the audio features of the past top hit songs using supervised machine learning techniques. To be specific, we are interested in answering the question: whether a given song will be hit or not. In practice, this is a binary classification problem. Furthermore, we want to know what kinds of musical features will more likely make a song be a hit and make our model as simple as possible.

## Method
### Data Collection
In this project, the data of top songs (y=1) is collected from the charts of Shazam Global Top 100 and Spotify Top 200 in 2017. On the other side, non-top songs (y=0) are randomly chosen across different genres from Spotify. In addition, each song or track contains 15 different types of audio features (See Table 1) that are provided by Spotify API AB, and we used the Python package, spotipy, to request these audio features automatically. At this point, our self-collected dataset contains 10809 observations (Top songs: 7700; non-top songs: 3109) and 15 features.

### Feature Generation
In addition to the 15 audio features extracted from Spotify Web API, we further generate 5 features for each song based on the song title. Basically, if there are some keywords appear in one song title, then we say this song has certain features. To be specific, 

### Data Preprocessing
The first reasonable procedure of data cleaning is to remove the duplicates in our data, since most of the top songs will remain on the top chart for several weeks. After dropping duplicates, we have only 764 songs in the top set and 3015 songs in the non-top set. Secondly, instead of imputation, we dropped the observations whose features contain missing values, because only 11 observations containing missing values. Up to this point, we have 763 songs as top songs and 3005 as non-top songs (See Table 2).

### Classification Techniques
#### Logistic Regression
The first classification model we utilized is Logistic Regression, which is an adaption of Linear Regression, and there are several reasons to choose Logistic Regression to start with. For example, Logistic Regression is relatively fast in the process of training and prediction. More importantly, it can provide us with the information about how important our features are, and this would be helpful for future feature selection. Practically, Logistic Regression has different variations, and we chose the one with L2 regularization in this project. Logistic Regression has one hyperparameter, C, to control the model complexity. The lower the value of C, the higher the regularization of the model is.

## Experiment

### Exploratory Data Analysis (EDA)
As mentioned above, there are 20 features contained in our music dataset. To get a high-level understanding of our dataset, we conduct simple EDA in this section. Since the features *acousticness*, *danceability*, *energy*, *instrumentalness*, *liveness*, *speechiness* and *valence* have values ranged within 0 and 1, it is easier to analyze them together as a group. Shown in the boxplot below (**Figure 1**), we can see that most of audio features, except for *liveness*, have quite different distribution between the top and the non-top. For example, *instrumentalness* for top songs has very densed distribution near value 0, this means that top songs have more vocal content.

![](/figures/Fig01_boxplot.png)
Figure 1.

Next for *available_markets* and *track_number* shown in Figure 2 (due to the simialr range of values). It seems that *track_number* is more distinctable between the top and non-top songs. On the other hand, *available_markets* for both top and non-top songs have quite similar distrubtion.

![](/figures/Fig02_boxplot.png)
Figure 2.

After simple graphical analysis, we know that features such as acousticness, danceability, energy, instrumentalness, speechiness, valence, track_number, duration_ms, loudness could be representative or distinctive and thus they could be a good feature for our binary classification problem. Alternatively, features including liveness, available_markets, key and tempo are not that representative, and could be dropped once we want to redcue the model complexity.

### Result
The experiment results are summarized in Table 2 and Figure 3 below. 

Model | ROC-AUC
--- | --- 
baseline | 0.5000 
Logistic Regression | 0.8518
SVM - RBF kernel | 0.7372
Random Forest | 0.8627

![](/figures/ROCAUC_20features.png)
Figure 3.

Looking at AUC scores, it is not hard to find out that our three classifiers are much better than the random guessing (0.5). For Logistic Regression model, although its AUC is slightly the lower than the Random Forest, it does provide several advantages over the Random Forest. Firstly, as shown in Figure 4, it outputs the weight of each feature, which gives us an idea about the relationship between features and class prediction. To be specific, as a weight gets close to zero, this feature becomes less important in top song prediction. Using feature danceability as an example, when a given song has a higher value of danceability, this song has a higher probability to be listed in the Top Chart. This result is in agreement with what we have found earlier in Figure 1. Based on the value of weights, we might want to remove those features (e.g. key, mode etc.) that contributes little to the prediction.

![](/figures/Coeff_20features.png)
Figure 4.

SVM with RBF kernel has higher accuracy than Logistic Regression since SVM with RBF kernel is nonlinear while Logistic Regression is linear. However, kernel SVM has much lower AUC. To interpret this result, recall that SVM is not good at probability prediction, and AUC is basically constructed and related to the output probabilities of a model.

![](/figures/FeatureImportance_20features.png)
Figure 5.

Finally, Random Forest produces the highest test AUC and accuracy. Similar to Logistic Regression, Random Forest can also show us the importance of every feature (shown in Figure 5). However, unlike the weight of Logistic Regression, feature importance does not point out a feature will increase or decrease the possibility of a song to be hit, but only the importance when building a model. Perhaps the most significant point to note is that, in order to generate such a high AUC score, Random Forest must used as many as 140 decision trees to build a classifier. Compared to Logistic Regression, Random Forest not only takes longer time in model training, but also increases the complexity of a model. In fact, our Random Forest model has simply 0.0109 higher AUC than the Logistic Regression model, but has much higher model complexity and time complexity. Therefore, if time, memory, and interpretation are key concerns in an application, it might be more reasonable to adopt Logistic Regression instead.

As mentioned in background, we want to simplify the model and keep the similar model performance at the same time. We also
want to know which musical features will more likely make a song be a hit, because in that way, music companies can focus more on these representative musical features when writing or creating a song. 

According to Figure 4, we know that **available_markets**, **key**, **mode**, **tempo**, **remix** and **radio_edit** have weight close to zero. Basically, this shows that they contribute little to the top song predition. Therefore, we remove those 6 musical features to simplify the models. The results are shown in Table 3 and Figure 6. Compared to Table 2 above, all of the three classifiers produced similar results, which proves that those 6 features we have dropped are not that useful in our problem, and more importantly, we succeed in reducing the model complexity from the aspect of feature selection. However, in order to maintain the similar performance, our Random Forest model must use as many as 230 estimators (140 previously), which somewhat increases the model complexity.

Model | ROC-AUC
--- | --- 
baseline | 0.5000 
Logistic Regression | 0.8537
SVM - RBF kernel | 0.7578
Random Forest | 0.8567


![](/figures/ROCAUC_14features.png)
Figure 6.
