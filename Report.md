## Background

## Experiment

### Feature Generation
In addition to the 15 audio features extracted from Spotify Web API, we further generate 5 features for each song based on the song title. Basically, if there are some keywords appear in one song title, then we say this song has certain features. To be specific, 

### Exploratory Data Analysis (EDA)
As mentioned above, there are 20 features contained in our music dataset. To get a high-level understanding of our dataset, we conduct simple EDA in this section. Since the features *acousticness*, *danceability*, *energy*, *instrumentalness*, *liveness*, *speechiness* and *valence* have values ranged within 0 and 1, it is easier to analyze them together as a group. Shown in the boxplot below (add figure later), we can see that most of audio features, except for *liveness*, have quite different distribution between the top and the non-top. For example, *instrumentalness* for top songs has very densed distribution near value 0, this means that top songs have more vocal content.

Next for *available_markets* and *track_number* shown in Figure XXX (due to the simialr range of values). It seems that *track_number* is more distinctable between the top and non-top songs. On the other hand, *available_markets* for both top and non-top songs have quite similar distrubtion. 

After simple graphical analysis, we know that features like acousticness, danceability, energy, instrumentalness, speechiness, valence, track_number, duration_ms, loudness could be representative or distinctive and thus they could be a good feature for our binary classification problem. Alternatively, features such as liveness, available_markets, key and tempo are not that representative, and could be dropped once we want to redcue the model complexity. 
