## Background

(Why is this problem important?) This problem is important, because before releasing a new album or EP, a music company would like or eager to know whether this album will be a hit or not. If there is a great chance that this album will be a great hit, then there is a higher possibility that a music company can make money and will be more willing to invest in creating new songs for this album. Therefore, a model that is capable of predicting whether a song will be a hit is very important in music industry. In our project, we are trying to build such a model by using the audio features of each songs we have collected. Furthermore, we want to know what kinds of musical features will more likely make a song be a hit.

## Method

### Data Collection
In this project, the data of top songs (y=1) is collected from the charts of Shazam Global Top 100 and Spotify Top 200 in 2017. On the other side, non-top songs (y=0) are randomly chosen across different genres from Spotify. In addition, each song or track contains 15 different types of audio features (See Table 1) that are provided by Spotify API AB, and we used the Python package, spotipy, to request these audio features automatically. At this point, our self-collected dataset contains 10809 observations (Top songs: 7700; non-top songs: 3109) and 15 features.

### Feature Generation
In addition to the 15 audio features extracted from Spotify Web API, we further generate 5 features for each song based on the song title. Basically, if there are some keywords appear in one song title, then we say this song has certain features. To be specific, 

## Experiment

### Exploratory Data Analysis (EDA)
As mentioned above, there are 20 features contained in our music dataset. To get a high-level understanding of our dataset, we conduct simple EDA in this section. Since the features *acousticness*, *danceability*, *energy*, *instrumentalness*, *liveness*, *speechiness* and *valence* have values ranged within 0 and 1, it is easier to analyze them together as a group. Shown in the boxplot below (add figure later), we can see that most of audio features, except for *liveness*, have quite different distribution between the top and the non-top. For example, *instrumentalness* for top songs has very densed distribution near value 0, this means that top songs have more vocal content.

Next for *available_markets* and *track_number* shown in Figure XXX (due to the simialr range of values). It seems that *track_number* is more distinctable between the top and non-top songs. On the other hand, *available_markets* for both top and non-top songs have quite similar distrubtion. 

After simple graphical analysis, we know that features like acousticness, danceability, energy, instrumentalness, speechiness, valence, track_number, duration_ms, loudness could be representative or distinctive and thus they could be a good feature for our binary classification problem. Alternatively, features such as liveness, available_markets, key and tempo are not that representative, and could be dropped once we want to redcue the model complexity. 
