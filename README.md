# A Data Science Project Using Spotify 
[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/built-with-love.svg)](https://forthebadge.com)

![Python Version](https://img.shields.io/badge/Python-3.7.4-brightgreen)
![Uses Json](https://img.shields.io/badge/uses-json-success)
![Uses Numpy](https://img.shields.io/badge/uses-numpy-informational)
![Uses Pandas](https://img.shields.io/badge/uses-Pandas-yellow)
![Uses Matplotlib](https://img.shields.io/badge/uses-matplotlib-purple)
![Ideas Welcome](https://img.shields.io/badge/ideas-welcome-orange)


## Introduction
This is data science side-project. Firstly, I gathered the information and track details of a few playlists from Spotify. The details are accessed using Spotipy which have wrapper functions for Spotify's RESTfuls API. The details are then used to analyze the difference between some of them. Prior to using Spotipy, remember to get the client id and client secret from Spotify Developer's Website.

## Table of content

- [Getting keys for Spotipy](getting-keys-for-spotipy)
- [Accessing the URI](accessing-the-uri)
- [Accessing the features and other attributes](accessing-the-features-and-other-attributes)
- [Comparing playlists](comparing-playlists)
- [Further analysis](further-analysis)
    - [Homogeneity of variance](homogeneity-of-variance)
    - [Normality of variables](normality-of-variables)
    - [Correlation between variables](correlation-between-variables)
    - [Hypothesis testing](hypothesis-testing)
- [Conclusion](conclusion)

## Getting keys for Spotify
As the codes use the Spotipy library, make sure you have Spotipy installed first. All methods in Spotipy requires authorization using `Client ID` and `Client Secret` which is available through [Spotify's Developer site](https://developer.spotify.com/dashboard/login). All you have to do is create an account, create a new app and then obtain the `Client ID` and `Client Secret`.

![screenshot of spotify developer site](https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/spotifydev.png)

Once you get both the `Client ID` and `Client Secret`, put them both in a json file. In my case, I placed them in `authorization.json`. This is how it should look like in the file:

```
{"client_id": "your_client_id",
"client_secret": "your_client_secret"}
```
## Accessing the URI
Spotify has URI(Unique Resource Identifier) for any track, album, playlist etc. For the purpose of analysing the playlist, you will need to get the URI for each playlist. The URI helps to communicate with Spotify API and also retrieving the information.

To get the URI:

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

![Gif of Feels](https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/feels.gif)

## Accessing the features and other attributes
This part is bit lengthy. It's an explanation about the different features for each of the track. Click on [(Skip)](#comparing-playlists) if you prefer to go to the next part. For each playlist, you could get the track details like the artists, ID's and the titles. Each track of have unique attributes called features. The features and description (from [Spotify](https://developer.spotify.com/documentation/web-api/reference/tracks/get-several-audio-features/)) for each track that are used for analysis later are:

- Danceability: 
    - Danceability describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength, and overall regularity. A value of 0.0 is least danceable and 1.0 is most danceable.
- Energy: 
    - Energy is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy. For example, death metal has high energy, while a Bach prelude scores low on the scale. Perceptual features contributing to this attribute include dynamic range, perceived loudness, timbre, onset rate, and general entropy.  
- Loudness: 
    - The overall loudness of a track in decibels (dB). Loudness values are averaged across the entire track and are useful for comparing relative loudness of tracks. Loudness is the quality of a sound that is the primary psychological correlate of physical strength (amplitude). Values typical range between -60 and 0 db.
- Mode: 
    - Mode indicates the modality (major or minor) of a track, the type of scale from which its melodic content is derived. Major is represented by 1 and minor is 0.
- Speechiness: 
    - Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value. Values above 0.66 describe tracks that are probably made entirely of spoken words. Values between 0.33 and 0.66 describe tracks that may contain both music and speech, either in sections or layered, including such cases as rap music. Values below 0.33 most likely represent music and other non-speech-like tracks.
- Acousticness: 
    - A confidence measure from 0.0 to 1.0 of whether the track is acoustic. 1.0 represents high confidence the track is acoustic.
- Instrumentalness: 
    - Predicts whether a track contains no vocals. “Ooh” and “aah” sounds are treated as instrumental in this context. Rap or spoken word tracks are clearly “vocal”. The closer the instrumentalness value is to 1.0, the greater likelihood the track contains no vocal content. Values above 0.5 are intended to represent instrumental tracks, but confidence is higher as the value approaches 1.0.
- Liveness: 
    - Detects the presence of an audience in the recording. Higher liveness values represent an increased probability that the track was performed live. A value above 0.8 provides strong likelihood that the track is live.
- Valence: 
    - A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).
- Tempo: 
    - The overall estimated tempo of a track in beats per minute (BPM). In musical terminology, tempo is the speed or pace of a given piece and derives directly from the average beat duration.
    
On top of that, Spotify also has a a number of audio attributes for each track, namely bars, beats, sections, tatum and segments. I didn't analyse them but here's the [link](https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-analysis/) if you want more information.

## Comparing playlists
I analysed two playlists that I listen in different circumstances. One is a playlist for meditation and the other one is for when I am working out.

<p align="center">
    <img src="https://github.com/hannz88/Spotify_data_science/blob/main/Graphs/radarchart.png" alt="Radar chart comparing two playlists">
</p>

From the radar chart, it's clear to see that there are differences in different features between the two playlists. Meditation playlist scored quite high in acousticness and instrumentalness while workout playlist scored higher in energy, danceability, and valence. Workout playlist is slightly higher in speechiness, tempo, lineliness and loudness.

I was surprised by the small differences in speechiness. Meditation tracks barely have any voice in it, if at all but workout tracks are all just songs. So, it turns out that high score in speechiness means that a track is composed mostly of spoken words, e.g. talk show, audio books. If both speech and music are present simultaneously, it'll have lower score than those of purely just spoken words.

Let's have a look at them in the form of bar chart.


## Further analysis

### Homogeneity of variance

### Normality of variables

### Correlation between variables

### Hypothesis testing

## Conclusion
