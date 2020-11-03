# A Data Science Project Using Spotify 
[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/built-with-love.svg)](https://forthebadge.com)

![Python Version](https://img.shields.io/badge/Python-3.7.4-red)
![Uses Json](https://img.shields.io/badge/uses-json-orange)
![Uses Numpy](https://img.shields.io/badge/uses-numpy-yellow)
![Uses Pandas](https://img.shields.io/badge/uses-Pandas-green)
![Machine Learning](https://img.shields.io/badge/Machine%20-Learning-blue)
![Uses Pytorch](https://img.shields.io/badge/uses-pytorch-magenta)
![Uses Matplotlib](https://img.shields.io/badge/uses-matplotlib-indigo)
![Ideas Welcome](https://img.shields.io/badge/ideas-welcome-purple)


## Introduction
This is data science side-project. Firstly, I gathered the information and track details of a few playlists from Spotify. The details are accessed using Spotipy which have wrapper functions for Spotify's RESTfuls API. The details are then used to analyze the difference between some of them. Prior to using Spotipy, remember to get the client id and client secret from Spotify Developer's Website.

After the initial analysis, I used machine learning techniques to train models for predicting if a user will like a song in a playlist using the dataset obtained via Spotify API. Click on `Machine Learning` section to find out more!

## Table of content

- [Getting keys for Spotipy](#getting-keys-for-spotipy)
- [Accessing the URI](#accessing-the-uri)
- [Accessing the features and other attributes](#accessing-the-features-and-other-attributes)
- [Comparing playlists](#comparing-playlists)
- [Further analysis](#further-analysis)
    - [Homogeneity of variance](#homogeneity-of-variance)
    - [Normality of variables](#normality-of-variables)
    - [Correlation between variables](#correlation-between-variables)
    - [Hypothesis testing](#hypothesis-testing)
- [Conclusion](#conclusion)
- [Machine Learning](#machine-learning)

## Getting keys for Spotify
[Back to top](#table-of-content)

As the codes use the Spotipy library, make sure you have Spotipy installed first. All methods in Spotipy requires authorization using `Client ID` and `Client Secret` which is available through [Spotify's Developer site](https://developer.spotify.com/dashboard/login). All you have to do is create an account, create a new app and then obtain the `Client ID` and `Client Secret`.

![screenshot of spotify developer site](https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/spotifydev.png)

Once you get both the `Client ID` and `Client Secret`, put them both in a json file. In my case, I placed them in `authorization.json`. This is how it should look like in the file:

```
{"client_id": "your_client_id",
"client_secret": "your_client_secret"}
```
## Accessing the URI
[Back to top](#table-of-content)

Spotify has URI(Unique Resource Identifier) for any track, album, playlist etc. For the purpose of analysing the playlist, you will need to get the URI for each playlist. The URI helps to communicate with Spotify API and also retrieving the information.

To get the URI:

 * Go to the three dots icon
 * Click on `Share`
 * Click `Copy Spotify URI` 
    
<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/uri.png" alt="Screenshot of getting the Spotify uri">
</p>

Once you get the URI, put them into json file again with other keys and values for that playlist. I've placed them in a file called `playlists_like_dislike.json`, which looks like this:

```
[{"uri":"spotify:playlist:37i9dQZF1DX1T2fEo0ROQ2",
"like":true, "purpose":"meditation"},
 {"uri":"spotify:playlist:4wibn1cPPP9m7WPiv7KF5Z",
 "like":true, "purpose":"feels"},
{"uri":"spotify:playlist:317O0e8iWJLClLGDKtieRe",
"like":false, "purpose":"house"},
{"uri":"spotify:playlist:04cqQXOsOibWJhmHTMTIrG",
 "like":true, "purpose":"workout"}]
```
For each playlist, other than the `uri`, I've also included `like`  for whether I like the playlist or not and `purpose` for what the occasion is for. Yes, I have a playlist for feels.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/feels.gif" alt="Gif of feels">
</p>

## Accessing the features and other attributes
[Back to top](#table-of-content)

This part is bit lengthy. It's an explanation about the different features for each of the track. Click on [(Skip)](#comparing-playlists) if you prefer to go to the next part. For each playlist, you could get the track details like the artists, ID's and the titles. Each track of have unique attributes called features. The examples of features for each track that are used for analysis later are loudness, livenes, energy, etc. For more information, check out [Spotify](https://developer.spotify.com/documentation/web-api/reference/tracks/get-several-audio-features/).

On top of that, Spotify also has a a number of audio attributes for each track, namely bars, beats, sections, tatum and segments. I didn't analyse them but here's the [link](https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-analysis/) if you want more information.

## Comparing playlists
[Back to top](#table-of-content)

I analysed two playlists that I listen in different circumstances. One is a playlist for meditation and the other one is for when I am working out.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/radarchart.png" alt="Radar chart comparing two playlists">
</p>

From the radar chart, it's clear to see that there are differences in different features between the two playlists. Meditation playlist scored quite high in acousticness and instrumentalness while workout playlist scored higher in energy, danceability, and valence. Workout playlist is slightly higher in speechiness, tempo, lineliness and loudness.

I was surprised by the small differences in speechiness. Meditation tracks barely have any voice in it, if at all but workout tracks are all just songs. So, it turns out that high score in speechiness means that a track is composed mostly of spoken words, e.g. talk show, audio books. If both speech and music are present simultaneously, it'll have lower score than those of purely just spoken words.

Let's have a look at them in the form of bar chart.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/stacked_bar_chart.png" alt="Stacked bar chart comparing two playlists">
</p>

The bar chart provides an alternative view at the differences between the meditation and workout playlist. Meditation differs a lot from Workout in Acousticness, Instrumentalness, Danceability, Energy and Valence. Maybe we should test the differences using statistical test? Before that, let's look at the another bar chart to highlight the differences.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/difference.png" alt="Bar chart to show the difference">
</p>

This barchart highlights the type of features that meditation and workout scored high in. Meditation scored highest in acousticness, instrumentalness and mode, whille workout tracks in everything else.

## Further analysis
[Back to top](#table-of-content)

I wanted to test the differences between the two playlists on a few variables. The following are my hypotheses:

```
Null hypothesis: There is no difference between the playlist in all variables
Alternative hypothesis: There is a difference between the playlist in all variables
```

Before selecting a statistical test for the hypotheses, it's common to check for assumptions to decide on which test to use. So, here I've decided to test homogeneity of variance, normality of variables and correlation between the variables.

### Homogeneity of variance
[Back to top](#table-of-content)

I used Levene's test for homogeneity of variance between variables of the two playlists. Results are:

```
Levene's stats for danceability is 2.01, p-value for is 0.15831
Levene's stats for energy is 31.25, p-value for is 0.0
Levene's stats for loudness_norm is 10.93, p-value for is 0.00113
Levene's stats for mode is 1.34, p-value for is 0.24831
Levene's stats for speechiness is 59.83, p-value for is 0.0
Levene's stats for acousticness is 0.09, p-value for is 0.76784
Levene's stats for instrumentalness is 15.91, p-value for is 9e-05
Levene's stats for liveness is 24.44, p-value for is 0.0
Levene's stats for valence is 84.96, p-value for is 0.0
Levene's stats for tempo_norm is 10.65, p-value for is 0.0013
```

From the results, it's clear that homogeneity of variance is violated.

### Correlation between variables
[Back to top](#table-of-content)

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/med_correlation.png" alt="Correlation matrix of meditation variables">
</p>
<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/workout_correlation.png" alt="Correlation matrix of workout variables">
</p>

From the results, we could see that the variables within each group are weakly correlated with one another. In other words, independence of variables could be assumed. Note that this is not the same as testing for correlation between "Energy" of meditation playlist and that of workout playlist. For that, we could asssume independence as the tracks are not related by any means.

### Normality of variables
[Back to top](#table-of-content)

Next, I used Shapiro-Wilk's test to test for the normality of the distribution. It turns out only `loudness` is normally distributed. 

```
Meditation's danceability p-value is 0.0; Workout's danceability p-value is 0.030599
Meditation's energy p-value is 7e-06; Workout's energy p-value is 0.008363
Meditation's loudness_norm p-value is 0.161003; Workout's loudness_norm p-value is 0.078042
Meditation's mode p-value is 0.0; Workout's mode p-value is 0.0
Meditation's speechiness p-value is 0.00011; Workout's speechiness p-value is 0.0
Meditation's acousticness p-value is 0.0; Workout's acousticness p-value is 0.0
Meditation's instrumentalness p-value is 0.0; Workout's instrumentalness p-value is 0.0
Meditation's liveness p-value is 0.0; Workout's liveness p-value is 0.0
Meditation's valence p-value is 0.0; Workout's valence p-value is 6.8e-05
Meditation's tempo_norm p-value is 0.0; Workout's tempo_norm p-value is 0.0
```
So, I decided to have a look at the distribution of the variables.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/distribution_pre.png">
</p>

The graphs show that the distribution for the data are far from normal. As per Shapiro-Wilk's test shown, loudness is probably the only variable that showed somewhat normal distribution. Mode has a bimodal distribution and is, therefore, not a continuous variable. Thus, mode should be dropped. The distributions of the variables are quite obviously skewed, however. Let's try to transform the data.

### Data transformation
[Back to top](#table-of-content)

When a dataset doesn't have normal disribution, it is common to tranform them so that you can use parametric tests on them. There are many ways to do this. For example, you could use log transformation, square root transformation, box cox transformation etc. Here, I tried MinMaxScaler from sklearn, log transformation, square and square root transformation (separately but I've overwritten them so they're no longer in the notebook), and box cox transformation. However, none has given any satisfactory results, as seen in the graphical distribution below. So, I decided to use non-parametric test instead.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/distribution_post.png" alt="Distribution for after log transformation">
</p>

### Hypothesis testing
[Back to top](#table-of-content)

Given that the normality of the dataset prior to transformation does is not normally distributed and there's no homogeneity of variance, this means that a parametric tests which normally has the assumption of normally distributed data could not be used. Even after different transformation, the distribution still isn't satisfactory. So, I've decided not to transform the data and try non-parametric tests instead. Wilcoxon's signed-rank test is a non-parametric test. However, it is sensitive to non-symmetrical data and so, it's not used. After doing some research, I decided to use sign test.

Sign test, short for paired-samples sign test, is a non-parametric test used to detect the difference in medians between paired or matched observations. It is normally used to test same cohort of participants in two conditions or time. However, two different samples are considered as "matched-pair". Furthermore, because I'm going to conduct multiple test, there's a possibility of inflating Type I error. In order to reduce it, I decided to use Bonferroni's correction. The alpha level is set to original alpha level (0.05) divided by number of test. [Here](https://statistics.laerd.com/spss-tutorials/sign-test-using-spss-statistics.php) are more information on sign test. More information on the [Bonferroni](https://www.statsandr.com/blog/how-to-do-a-t-test-or-anova-for-many-variables-at-once-in-r-and-communicate-the-results-in-a-better-way/) adjustment.

Sign test uses median. Therefore, the hypotheses are:

```
Null hypothesis: There is no difference in the median between the variables of the two playlists.
Alternative hypothesis: There is a difference in the median between the variables of the two playlists.
```

Results:
```
Difference in Danceability is significant (p=1.5777218104420236e-30).
Difference in Energy is significant (p=1.5777218104420236e-30).
Difference in Loudness_norm is significant (p=0.00239462935750601).
Difference in Speechiness is significant (p=1.911357536027706e-15).
Difference in Acousticness is significant (p=1.5777218104420236e-30).
Difference in Instrumentalness is significant (p=1.5934990285464443e-28).
Difference in Valence is significant (p=7.969072864542664e-27).
```
## Conclusion
[Back to top](#table-of-content)

This has been a fun project to do! A brief recap, I've selected a few playlist from Spotify. From these selections, I've picked Meditation and Workout playlist to analyze them. Spotify have different features for each track like energy, acousticness, etc. I analyze the differences between the two playlist using graphical means. Then, I tested some assumptions of the data in order to decide which statistical test to use in order to quantifiably test the differences. The datasets weren't normally distributed so I tried to transform the data using a few methods. None of them gave any satifying results. Therefore, I resorted to non-parametric test instead, specifically sign test. The test shows that the two playlist are different in terms of Danceability, Energy, Loudness, Speechiness, Acousticness, Instrumentalness and Valence. The data science-y part might be done for now but there's a few steps that I'm thinking of taking this project to. If you have any idea, let me know!

## Machine Learning
So, to train ml models to predict the likeness of a song, I've aggregated track data from 8 different playlists including a mix of playlist that I like and dislike. Since the data is labelled, I'll be using supervised machine learning. Fistly, I used Logistic Regression (LR) and K-nearest Neighbour (KNN) from sklearn. Then, I employed Pytorch as a classifier. 

### Data Distribution
First, let's have a look at the shape of the distribution for the variables that are used.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/Distribution_variables_500.png" alt="Distribution for larger dataset">
</p>

From the distribution of the different variables, it's fairly obvious that the songs I like have rather low scores in terms of danceability, energy, loudness and tempo. Songs that I'm into have rather high scores in instrumentalness and acousticness. I have a suspicion that this might caused by the meditation playlist. It looks like there are not much differences with the tracks that I dislike in terms of duration and sections. I'm hesitant to comment on the differences in tempo, bars and segments as there are quite a large amount of overlap. Normally, I would normalise the variables but we're not doing hypothesis testing atm so we'll leave them be, for now.

### Sklearn
Sklearn, also known as Scikit-learn, is a machine learning library in Python. There are many tools in there. Here's an ultra-brief explanation of the algorithms I used:

| Algorithms  | Description |
| ------------- | ------------- |
| Logistic Regression  | Predicts discrete values for a set of independent variables using logit function  |
| KNN  | Assumes that similar objects exist in close proximity and computes distances between objects to assign them into groups  |

Other than the two algorithms, I also used `classification_report` from sklearn which is a pretty neat tool as it tells you the accuracy, precision, recall, f1-score, etc. Pretty neat, huh?

One more thing, it wouldn't be good practice to train and test your model on the same set of data. Imagine building a classifier for food type. Then, train it on sausage only. What will happen to the model? It'll only ever 
be able to tell you if it's sausage or not. Not useful right? 

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/hotdog.gif" alt="Gif of hotdog Identifier">
</p>

So, to circumvent this pitfall, I used `train_test_split` from `sklearn.model_selection`.

```
from sklearn.model_selection import train_test_split
## x are the independent variables
## y are the dependent variables, ie what you're trying to predict
x_train, x_test, y_train, y_test = train_test_split(x,y, train_size=0.7, test_size=0.3, random_state=0)
```

#### Classification report results
Logistic regression:
|  | Precision | Recall | f1-score | support |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0 | 0.91 | 0.99 | 0.95 | 104 |
| 1 | 0.98 | 0.81 | 0.89 | 53 |
|  |  |  |  |  |
| accuracy |  |  | 0.93 | 157 |
| macro avg | 0.94 | 0.90 | 0.92 | 157 |
| weighted avg | 0.93 | 0.93 | 0.93 | 157 |

KNN:
|  | Precision | Recall | f1-score | support |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0 | 0.67 | 0.79 | 0.73 | 104 |
| 1 | 0.37 | 0.25 | 0.30 | 53 |
|   |   |   |   |   |
| accuracy |  |  | 0.61 | 157 |
| macro avg | 0.52 | 0.52 | 0.51 | 157 |
| weighted avg | 0.57 | 0.61 | 0.58 | 157 |

Accuracy is often used as a metric to judge a model. It's easy to see why: it's a measurement of how accurate your model is. However, the caveat is that it's not useful in imbalanced dataset. And it also depends on what you're testing. For example, if the model has high accuracy in testing Neg virus-infected people but what matters are the Pos virus-infected ones. People have used other measurments such as the precision, recall and [f1-score](https://en.wikipedia.org/wiki/F-score). All of them are useful but it depends on several factors. Here, instead of settling for one. We'll look at the overall. In general, Logistic Regression has better scores than KNN, so we'll use Logistic Regression.

#### Prediction
LR was selected to make the final prediction and the results is as below.
|  | Precision | Recall | f1-score | support |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0 | 0.91 | 0.98 | 0.94 | 341 |
| 1 | 0.95 | 0.81 | 0.88 | 181 |
|   |   |   |   |   |
| accuracy |  |  | 0.92 | 522 |
| macro avg | 0.93 | 0.90 | 0.91 | 522 |
| weighted avg | 0.92 | 0.92 | 0.92 | 522 |

The overall scores look pretty neat. The accuracy was 0.92 and the overall scores look pretty good to me. Let's try Pytorch!

### Pytorch
(readme in progress, codes in notebook)
