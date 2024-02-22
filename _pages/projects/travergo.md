---
title: TraverGo
author_profile: true
permalink: /projects/travergo
---
{% include base_path %}

TraverGo won 1st place at the Georiga Tech Hacklytics at the Traversaal AI Challenge with over 1000 participants and 200 projects.

## Introduction
TraverGo is a hotel searching platform to provide a seamless for users to explore and find hotels for their vacation planning. To achieve this goal our platform consists of the following features,

1. Neural Search: We give the ability to match user queries to different hotels based on the hotel description and reviews.
2. Sentiment Analysis: Analyzes reviews and generates comprehensive ratings based on sentiment analysis, allowing travelers to make informed decisions.
3. Decoder Model: A decoder is used to understand why a particular hotel is the best match for the traveler's needs, providing insight tailored for each traveler.
4. Traversaal AI API Integration: With the Traversaal AI API, real-time information is fetched about nearby locations, allowing recommendations for nearby attractions, restaurants, and transportation options.
5. QnA Chatbot: Added a feature for users to ask for further questions and clarifications on details of the hotel. Also integrated with Ares API to address real-time information.

## Features
### Neural Search
Using our 6000 entry long dataset of hotels along with their description and reviews we first take in the hotel description and reviews and using sentence transformers model from hugging face we get a embedding vector for this. We then store all our hotel data into a qdrant vector database.

On the user side we first take in the user queries about their specific hotel needs, this can be in any format in terms of how it is structured. 
- eg. "Cheap hotel in new york with gym and wifi"

We convert this query into a vector and search through our vector database and shortlist 3 hotels based on the user requirement.

### Sentiment Analysis
We used sentiment analysis using DistilBert on the 40 reviews for each hotel and give them a score from -1 to 1. Then we average the individual review scores and give each hotel a singular score.

Once we get the hotels from our vector database we then use this score to rank the hotels and give them to the user. Hence ensuring that our hotel suggestions are ranked based on real world reviews by customers.

### Decoder Model
In order to specify why we choose the three hotels and show how the user requirements are satisfied, we use a decoder model to generate an explanation for why the three hotels were chosen. This gives the user a general idea of what each hotel can provide to the user based on the query and requirements they put forward.

### Chatbot + Ares API Integration
The user also gets the opportunity to converse with a chatbot which is given information about the hotel description and reviews on one of the three hotels they chose. The user is able to ask simple questions about the hotel such as availability of amenities such as gym, wifi, food etc.

In addition we leveraged the Ares API to make sure we are able to answer questions out of the domain of our chatbot. Real time questions such as restaurants near by or closest public transportation are questions that our model cannot answer as it does not have access to real time information. However we use the Ares API to retrieve this information and we append this information to the chatbots message history.

Hence using this method our chatbot is able to converse about the hotel description and reviews as well as answer real time questions surrounding the hotel.

This provides the user a seamless experience in shortlisting and booking a hotel.





