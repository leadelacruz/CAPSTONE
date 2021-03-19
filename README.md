# Capstone Project: A Guide to Optimize Airbnb Listings for the Best Guest Reviews


## Table of Contents:
* [Problem Statement](#Problem-Statement)
* [Data](#Data)
* [Summary of Analysis](#Summary-of-Analysis)
* [Conclusions and Recommendations](#Conclusions-and-Recommendations)


### Problem Statement
Airbnb has become immensely popular and makes for a very competative market with there being thousands and sometimes tens of thousands of Airbnb listings available in most major cities in the world and the numbers are continuously growing. So even after going through all the filters of selecting the number of guests, the property types, specific amenities, and the dates of check-in and check-out, we’re still left with an abundance of listings to choose from.

In this project, I use sentiment analysis and topic modeling to determine what the strongest correlations are for a good review in order to provide recommendations to Airbnb hosts on what they should focus on for their guests in order to acheive optimal reviews to attract potential future guests.

### Data

The datasets were found on insideairbnb.com as CSV files. I chose to use the listings datasets and the review datasets for the cities: Chicago, Asheville, and Santa Cruz. The listings dataset has all the details of each Airbnb listing for that city, including the amenities they provide, what the property consists of, what the neighborhood is like, and much more. The reviews dataset has the written reviews from guests along with the date they were posted and the correlated listing.

### Summary of Analysis

#### Data Cleaning 

Starting with the listings, those datasets originally had 74 columns, which many were either only null values, all the same values or containing redundant information so I was able to reduce it to 32 columns. I also had up to 42,000 null values total between the 3 datasets to handle, some rows I had to remove altogether like those that did not have any reviews or rating scores since those would not provide any valuable information. And many others I had to either impute the average or the most frequent value, or use values from other related columns. Several of the remaining columns contained categorical values so I created dummy columns for those.

Looking at the reviews dataset, there were almost 550,000 reviews in total so I worked on some EDA first to see what I was working with and noticed many reviews were short and simple such saying things like “great stay”, “loved the place”, “just what we needed” so I removed all reviews that were less than 5 words long and then I noticed many listings were in foreign languages so I removed all those as well and kept only the reviews that were in English. I also discovered that Airbnb automatically posts reviews on listings when the host cancells a booking so I made sure to remove all of those reviews. Since I was still left with about 400,000 reviews in total, I narrowed down the dates of the review and chose to use the reviews from 2018-2019.


#### Topic Modeling
For the topic modeling I used an algorithm called Latent Dirichlet Allocation, otherwise known as LDA. For this algorithm the texts of the reviews had to be reduced to it’s base or dictionary form so all words in third person were changed to first person and verbs in past tense were changed to present tense, in order to be able to analyze each word as a single item. 

Once the text was reduced to it’s dictionary form, I removed various words such as articles, basic adjectives, and general verbs that had no substantial meaning, as well as the cities names since those appeared frequently in the reviews.

The reduced text was then vectorized using a TF-IDF vectorizer which essentially calculates the frequency of each word and gives each word a score based off both its frequency in the specific review and its frequency throughout all the reviews. So the higher the TF-IDF score for a word means a higher level of importance.

Once I have the TF-IDF score for all words in the reviews, I need to convert the format into a Gensim corpus data type and create a dictionary with the vocabulary and index of each term and then I can apply the LDA model to the corpus and dictionary. 

The LDA model is used to extract hidden topics from large volumes of text and does this by grouping words together and ranking the topics by importance according to the volume and percentage contribution.

I attempted several variations of tuning parameters with different  number of topics and the model showed the best results for 4-5 topics.



### Recommendations

Based off of my findings, location holds a heavy weight when it comes to good reviews, but location isn’t really something you can just change about your listing but you can be aware that people like to stay in places where they would be walking distance from things to do. With this in mind, I would recommend mentioning on your listing what local options are available to your guests to walk to, such as restaurants and bars, local attractions, and/or local markets. Also to have that printed out somewhere on property for the guest as a reminder while they are there.

Another big topic was communication, “quick respond” popped up a lot during my eda process which tells me that people are really receptive and appreciative to quick responses. Most of the values for average response time by listing is “within an hour” so I would recommend to respond to potential and current guests within an hour or so. 

The last topic I wanted to discuss is the property, which had “bed” and “comfortable” as key words which is hard since a comfortable bed is very subjective, but at least be aware of the condition of the mattress and the quality of bedding used. “Kitchen” was also another keyword for property and from my EDA, there were many kitchen amenities that were correlated to positive reviews such as dishes and silverware, coffee maker, and microwave. So I’d recommend, depending on space limitiations, to at minimum offer these kitchen ammenities.   
