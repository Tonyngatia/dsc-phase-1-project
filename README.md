# Phase 1 Project 

## Project Overview

For this project I used exploratory data analysis to generate insights for a business stakeholder. I was able to use importing, cleaning and data analysis tools such as pandas and also generate visualizations using scatter plots, histograms and bar graphs.

## Business Problem

Microsoft sees all the big companies creating original video content and they want to get in on the fun. They have decided to create a new movie studio, but they don’t know anything about creating movies. I was charged with exploring what types of films are currently doing the best at the box office. I then translated those findings into actionable insights that the head of Microsoft's new movie studio can use to help decide what type of films to create.

## Objectives
- To determine the highest and lowest grossing movies and their ratings based on the available datasets.
- To determine the most and least produced genres of films.
- To determine the highest and lowest grossing movie genres.
- To determine the average runtime and period to produce a movie.
- To determine the relationship between the relationship between the average rating of customers based on several factors such as: the average production time, the release year of the movie and the runtime.

## Data Understanding
The main sources used to extract and analyse data was from the following data files that were provided:

* imdb.title.basics
* imdb.title.ratings
* bom.movie_gross

Each record represents a movie and the column in these datasets contain features such as: title, start_year, foreign gross and domestic gross, average rating, runtime in minutes and the number of votes.

## Requirements
- In this project, I used the data munging and visualization skills to conduct an exploratory analysis of the dataset.

- I also imported datasets with pandas to import csv files and also to plot the different visualisation tools.

- I was required to clean the data to make it more useable for actionable analysis, by using the various methods of cleaning data such as dropping and replacing with the median/mean.

- I was required to find the features that have the strongest and negative correlations and produce plots representing these relationships.

- I was required to create a new column generations to represent the different classifiations of movies. i.e whether old generation or new generation.


1. Loading the required Libraries

import pandas as pd
import numpy as np
import seaborn as sns
from matplotlib import pyplot as plt
%matplotlib inline
import datetime
import requests

2. Loading the datasets

dfratings = pd.read_csv('zippedData/imdb.title.ratings.csv.gz')
dfgross = pd.read_csv('zippedData/bom.movie_gross.csv.gz')
dfbasics = pd.read_csv('zippedData/imdb.title.basics.csv.gz')

3. Inspecting the contents of the datasets

dfratings.head()
dfgross.head()
dfbasics.head()
print(dfgross.shape)
print(dfratings.shape)
print(dfbasics.shape)
print(dfgross.info())
print(dfratings.info())
print(dfbasics.info())

4. Setting the primary key and joining the indiviual datasets to one dataset using the common columns

dfbasics_indexed = dfbasics.set_index('tconst')
dfbasics_indexed = dfbasics.drop('original_title',axis=1)
joined_df1 = dfbasics_indexed.join(dfratings_indexed)

5. Identifying the missing values and also the percentage to determine which method to use for filling in missing data.

joined_df1.isna().sum()
def missing_values(data):

6. Replacing the missing data

joined_df1['runtime_minutes'] = joined_df1['runtime_minutes'].fillna(joined_df1['runtime_minutes'].median())
joined_df1['averagerating'] = joined_df1['averagerating'].fillna(joined_df1['averagerating'].median())
#replacing the missing values in numvotes with an int value of 0
joined_df1['numvotes'] = joined_df1['numvotes'].fillna(int(0))
#replacing the missing values in genres with the definition 'missing'
joined_df1['genres'] = joined_df1['genres'].fillna('Missing')

dfgross['foreign_gross'] = dfgross['foreign_gross'].fillna(dfgross['foreign_gross'].median())
dfgross['domestic_gross'] = dfgross['domestic_gross'].fillna(dfgross['domestic_gross'].median())
dfgross['studio'] = dfgross['studio'].fillna('Missing')

7. Obtaining the mean,median,min and max values and standard deviation of the dataset

final_joined_df.describe()

8. Plotting a boxplot to visualise the different gross incomes and identify outliers if any

fig,ax = plt.subplots()
ax.set_ylim(-1.0e+08,1.0e+08)
final_joined_df.boxplot(column=['domestic_gross','foreign_gross'], figsize = (20,10));

9. Creating a column generations to separate between movies that were produced post 2017 inclusive and pre 2017.

joined_df.loc[joined_df['year']>=2017,'generation'] = 'new_movie'
joined_df.loc[joined_df['year']<2017,'generation'] = 'old_movie'
joined_df.head()

10. Grouping the dataset by genres 

generation_mean = joined_df.groupby(['generation']).mean()

11. Plotting a histogram and scatter plots to analyse the data further

columns1 = joined_df.columns
joined_df[columns1].hist(figsize=(16,16));
pd.plotting.scatter_matrix(joined_df, figsize = (20,20));

columns2 =  generation_mean.columns
generation_mean[columns2].hist(figsize=(16,10)
    
12. Introducing placeholders to the dataset

for col in ['primary_title','start_year','runtime_minutes','genres','averagerating','numvotes','studio','domestic_gross','foreign_gross','year']:
    print('Values for {}:\n{}\n\n'.format(col, final_joined_df[col].unique()))
    
13. Grouping the dataset with reference to the genres

grouped_genres = joined_df.groupby('genres').mean()
grouped_genres.sort


## Observations

Average rating: - Has a minimal positive correlation between the numvotes, and both domestic and foreign gross gains.
Runtime minutes: - Has a minimal positive correlation with foreign and domestic gross gains.
Domestic_gross: - Has a strong positive correlation witht foreign gross.
Year: - Has almost zero correlation with the domestic and foreign gross.

The top grossing movie is Black Panther and the least grossing movie is Storage 24.

Most of the highest grossing films were movies that were produced post 2017 inclusive. Whereas, the least grossing films were movies that were produced pre-2017.

The new movies have an average production time of approximately 1 year.

The runtimes of both new and old movies is approximately the same at 104~105 minutes.

New movies have a fairly longer production time of at least one year as compared to older movies, which begin and complete production within the same year.

## Summary

The highest and lowest grossing films from the data were; Black Panther and Storage 24 respectively.

The highest and lowest grossing genres from the data were; Adventure-Drama-Sport and Action-Horror-Mystery respectively.

The highest produced genre of films are; Drama, Documentaries, Comedy and Romance films.
The least produced films were Action- Romance, Action-Comedy-Sport, Action-Biography-History and Sport.






