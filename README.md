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
{"uri":"spotify:playlist:6W0sKut7XZL7V6wTjR52R0",
"like":false, "purpose":"metal"},
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

## Comparing playlists

## Further analysis

### Homogeneity of variance

### Normality of variables

### Correlation between variables

### Hypothesis testing

## Conclusion
