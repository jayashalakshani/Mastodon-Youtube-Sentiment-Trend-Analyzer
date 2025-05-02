# Mastodon-Youtube-Sentiment-Trend-Analyzer
A Lambda Architecture-based social media analytics system for tracking public sentiment and trending topics across YouTube and Mastodon in real time, powered by Hugging Face, Apache Spark, MongoDB, and Streamlit.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Architecture Overview](#architecture-overview)
3. [Step-by-Step Guide](#step-by-step-guide)
    1. [Data Collection & Preprocessing for Batch Layer and Speed Layer](#data-collection-for-batch-layer-and-speed-layer)
    2. [Batch Layer](#batch-layer)
    3. [Data Preprocessing](#data-preprocessing)
    4. [Sentiment Analysis](#sentiment-analysis)
    5. [Trending Topic Detection](#trending-topic-detection)
4. [Installation](#installation)
5. [Usage](#usage)
6. [Project Structure](#project-structure)
7. [Contributing](#contributing)
8. [License](#license)

---

## Introduction

The Mastodon-Youtube-Sentiment-Trend-Analyzer is a powerful tool designed to analyze social media data in real time. It employs a Lambda Architecture to combine batch and real-time processing for efficient sentiment analysis and trending topic detection. This project integrates technologies such as Hugging Face for NLP, Apache Spark for distributed computation, MongoDB for data storage, and Streamlit for interactive visualizations.

---

## Architecture Overview

![lambda drawio](https://github.com/user-attachments/assets/4fc47161-3abc-479f-b6f3-3222720a1b0f)


The project is based on the Lambda Architecture, which consists of:
1. **Batch Layer**: Responsible for storing and computing large-scale historical data.
2. **Speed Layer**: Handles real-time data processing.
3. **Serving Layer**: Provides a unified view of processed data using a combination of batch and speed layer results.

---

## Step-by-Step Guide

### 1. Data Collection & Preprocessing for Batch Layer and Speed Layer

#### Sources

**YouTube**  
- Pulled US-region trending videos and top-level comments.  
- Filtered for English language.  
- **Collected Data**:  
  - Video ID  
  - Snippet  
  - Tags  
  - Title  
  - Published Date (`published_at`)  
- **Storage**:  
  - Stored selected features in the MongoDB `social_media_analytics` database, under the `youtube_tags_data` collection.  
  - Unique tags are stored in the `youtube_unique_tag` collection.
- **Next Steps**:  
  - Fetch a minimum of 10 comments related to the unique trending tags.
  - Preprocessed each comment.
  - Store them in the `youtube_sentiment_collection` collection.
 
**Mastodon**   
- Filtered for English language tags.
- Clean the tag. 
- **Collected Data**:  
  - tag_raw  
  - tag_clean  
  - url  
  - score  
  - Fetched Date (`fetched_at`)  
- **Storage**:  
  - Stored selected features in the MongoDB `social_media_analytics` database, under the `mastodon_tags_data` collection.  
  - Unique tags are stored in the `mastodon_unique_tag` collection. 
- **Next Steps**:  
  - Fetch 10 comments related to the unique trending tags.
  - Preprocessed each comment.
  - Store them in the `mastodon_sentiment_data` collection.

### 2. Batch Layer

The batch layer performs historical processing on previously collected data stored in MongoDB to extract trends and sentiment patterns over time.

#### 1. Initialize Spark Session & Connect to MongoDB ####
- Connect to the `social_media_analytics` database using pymongo or Spark's MongoDB connector.
- Read collections like `youtube_sentiment_collection` & `mastodon_sentiment_data` which store tagged comments.

#### 2. Load Historical Comment Data ####
- Load the full dataset of previously saved YouTube comments from the `youtube_sentiment_collection` collection & Manstodon comments from the `mastodon_sentiment_data` collection.
- Convert to a Spark DataFrame for distributed processing.

#### 3. Data Cleaning & Preprocessing ####
- Remove null or irrelevant entries.
- Ensure each comment is associated with a valid tag and preprocessed text.

#### 4. Sentiment Analysis ####
- Apply a pretrained Hugging Face sentiment model (cardiffnlp/twitter-roberta-base-sentiment) on each comment.
- Predict sentiment labels (Positive/Negative/Neutral).
- This is often done using pandas UDFs (or PySpark UDFs) for batch inference in parallel.

#### 5. Aggregation and Trend Analysis ####
- Group by tags and calculate:
    - Count of comments.
    - Number of positive vs negative sentiments.
    - Sentiment ratio or sentiment score.
    - Extract most frequently appearing tags with high positive/negative sentiment over time.

#### 6. Save Results #####
- Store the aggregated results in a new MongoDB collection (`batch_tag_sentiment` & `batch_platform_sentiment`) for visualization.
  ![image](https://github.com/user-attachments/assets/822f6d34-4f7c-4041-926c-0f25a627bb73)
  ![image](https://github.com/user-attachments/assets/43752de8-c7f7-416b-bea5-0ae0924196b4)
  ![image](https://github.com/user-attachments/assets/debc76dc-87c4-4343-bd9e-1ef2e6a76f2f)
  ![image](https://github.com/user-attachments/assets/f75a132d-eee6-41dc-9c15-0bd22284798c)

- Store the comments with sentiment localy

#### 7. Compare MapReduce vs. Spark Performance ####
- Implement a simple MapReduce-style sentiment analysis.






  
  
  
