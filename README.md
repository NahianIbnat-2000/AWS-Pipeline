# Group-11_Week5_Assignment

**Katharina Burtscher (2406294), Nahian Ibnat (2400062)**
    
**Date - 21.12.2025**
    
**Course - ECBS5146: Data Science 1, Fall 2025**

This repository contains code and csv files that we created for their Assignment 3.
The project looks at the reporting patterns on the BNP (Bangladesh Nationalist Party, a popular religious-conservative party) by two different news sites, [The Daily Star](https://www.thedailystar.net), and [Prothom Alo](https://www.prothomalo.com), a newspaper that publishes both in English and Bangla. The study utilized a curated list of news articles covering the political developments
surrounding the 2024 government resignation and subsequent election process in Bangladesh.

In our project we tried to figure out if the sentiment of the articles differ from each other, depending on the news site and the language. In order to do this, we scraped 10 articles from The Daily Star, 10 articles from the English Prothom Ali, as well as 10 articles from the Bengali Prothom Ali website. We translated all of the Bangla articles to English and saved the articles with their urls and both the original and translated version in "articles_final.csv".


## Repository Structure

```text
.
├── data/
│   ├── articles_final.csv
│   └── articles_raw.csv
│
├── outputs/
│   ├── web_scraping.ipynb
│   ├── table1_overall_sentiment_distribution.csv
│   └── table2_sentiment_by_source.csv
│
├── serverless/
│   └── articles.csv
│
├── requirements.txt
├── LICENSE
├── requirements.txt
└── README.md

```

## Configuration

### AWS Credentials

This project uses AWS managed services via the **boto3** Python SDK:

- Amazon S3  
- AWS Translate  
- AWS Comprehend  

To run the code locally, AWS credentials must be configured.

---

### 1. Install the AWS CLI
---

### 2. Configure AWS Credentials

After installing the AWS CLI, configure your credentials by running:

```bash
aws configure
```

### install packages if needed
```bash
pip install -r requirements.txt
```

## Problem Statement
#### Research Question
Do news sources exhibit different sentiment patterns when covering the same political entity (BNP) based on their language and editorial stance?
#### Background
Following the 2024 government resignation and election process in Bangladesh, media coverage of the Bangladesh Nationalist Party (BNP) provides a unique opportunity to analyze:

- Cross-language sentiment differences: How does sentiment differ between Bangla and English reporting?
- Source bias detection: Do different news outlets show systematic sentiment variations?
- Translation accuracy: How well does AWS Translate preserve sentiment across languages?

#### Why This Matters
Understanding media sentiment patterns is crucial for:

- Identifying potential media bias
- Assessing the impact of language on political reporting
- Evaluating AI translation quality in political contexts
- Media literacy and critical news consumption

## Data Sources

The list of news sites analyzed can be found in "data/articles_final.csv", alongside with the full text and translation of the articles. The article urls were hand-picked and used to programmatically scrape the articles, translate and comprehend them. The following table gives and overview of the number of articles per news source:

| Source | Article Count | Language | Editorial Stance |
|---------|---------------|----------------|----------------|
| The Daily Star | 10 | English (en) | Center-left, secular |
| Prothom Alo (English) | 10 | English (en) | Mainstream |
| Prothom Alo (Bangla)ো | 10 | Bangla (bn) | Mainstream |
| **Total** | **30** | — | — | 

#### Data Collection Process

1. Manual URL Selection: Articles were hand-picked based on:

- Relevance to BNP political activities
- Publication during August-December 2025
- Coverage of major events (rallies, statements, policy announcements)


2. Web Scraping: Programmatic extraction of article content

- Tool: Beautiful Soup / Selenium
- Extracted: Title, content, publication date, URL


3. Data Storage: Raw articles saved in data/articles_raw.csv

## Methodology

#### Architecture Overview

Data Collection → Translation → Sentiment Analysis → Visualization
     ↓               ↓                ↓                    ↓
  Web Scraping   AWS Translate   AWS Comprehend      Python Plots
     ↓               ↓                ↓                    ↓
  CSV Storage      S3 Storage      Results CSV        PNG/Charts

#### AWS Services Implementation

1. Amazon S3 - Data Storage
   
import boto3

s3_client = boto3.client('s3')

# Upload articles to S3
def upload_to_s3(file_path, bucket_name, object_key):
    s3_client.upload_file(file_path, bucket_name, object_key)
    print(f"Uploaded {file_path} to s3://{bucket_name}/{object_key}")

# Store raw and processed data
upload_to_s3('data/articles_raw.csv', 'bnp-analysis', 'articles_raw.csv')

##### Usage:

- Store raw scraped articles
- Maintain versioning for analysis reproducibility
- Enable collaborative access to datasets

2. AWS Translate - Language Translation

translate_client = boto3.client('translate')

def translate_article(text, source_lang='bn', target_lang='en'):
    """Translate Bangla articles to English"""
    response = translate_client.translate_text(
        Text=text[:5000],  # AWS Translate limit
        SourceLanguageCode=source_lang,
        TargetLanguageCode=target_lang
    )
    return response['TranslatedText']

# Process Bangla articles
df['translated_text'] = df[df['language'] == 'bn']['content'].apply(
    lambda x: translate_article(x, 'bn', 'en')
)

##### Challenges:

- 5,000 character limit per request → implemented chunking
- Automatic language detection for mixed-content articles
- Preserving political terminology and proper nouns

3. AWS Comprehend - Sentiment Analysis

comprehend_client = boto3.client('comprehend')

def analyze_sentiment(text):
    """Detect sentiment with confidence scores"""
    response = comprehend_client.detect_sentiment(
        Text=text[:5000],
        LanguageCode='en'
    )
    return {
        'sentiment': response['Sentiment'],
        'positive': response['SentimentScore']['Positive'],
        'negative': response['SentimentScore']['Negative'],
        'neutral': response['SentimentScore']['Neutral'],
        'mixed': response['SentimentScore']['Mixed']
    }

# Analyze all articles
df['sentiment_analysis'] = df['text_english'].apply(analyze_sentiment)

##### Analysis Features:

- Sentiment classification: Positive, Negative, Neutral, Mixed
- Confidence scores for each sentiment category
- Entity recognition for key political figures




## Analysis Results



## AWS Service Cost Breakdown

The table below presents an estimated cost breakdown of AWS services used in this project,
based on a single execution of the analysis pipeline.

| Service | Calculation / Details | Estimated Cost |
|--------|----------------------|----------------|
| **Amazon S3** | Storage of one CSV file (<1 MB) with a small number of upload and list requests | ~$0.00 |
| **AWS Translate** | Translation of ~30 articles (English & Bangla), automatic language detection and chunking (~60,000 characters total) at $15 per 1M characters | ~$0.90 |
| **AWS Comprehend** | Sentiment analysis on ~30 English-translated articles (~45,000 characters total) at $0.0001 per 100 characters | ~$0.05 |
| **Total Estimated Cost per Run** | — | **~€0.95** |

Both Nahian and Katharina ran the code multiple times independent from each other. Assuming that both ran all of the notebooks/services about three times each, the total estimated cost amounts to **0.95*6 = 5.70€**.

### Notes
- Actual costs may be lower if covered by the AWS Free Tier.
- Amazon S3 costs are negligible due to minimal storage and request volume.
- AWS Translate represents the largest cost component due to character-based pricing.


