# Mastodon-Youtube-Sentiment-Trend-Analyzer
A Lambda Architecture-based social media analytics system for tracking public sentiment and trending topics across YouTube and Mastodon in real time, powered by Hugging Face, Apache Spark, MongoDB, and Streamlit.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Architecture Overview](#architecture-overview)
3. [Step-by-Step Guide](#step-by-step-guide)
    1. [Data Collection for Batch Layer](#data-collection-for-batch-layer)
    2. [Data Collection for Speed Layer](#data-collection-for-speed-layer)
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

The project is based on the Lambda Architecture, which consists of:
1. **Batch Layer**: Responsible for storing and computing large-scale historical data.
2. **Speed Layer**: Handles real-time data processing.
3. **Serving Layer**: Provides a unified view of processed data using a combination of batch and speed layer results.

---

## Step-by-Step Guide

### 1. Data Collection for Batch Layer
