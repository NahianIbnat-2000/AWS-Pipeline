# Group-11_Week5_Assignment

Katharina Burtscher (), Nahian Ibnat (2400062)
DAte - 21.12.2025
Course - ECBS5146: Data Science 1, Fall 2025

## Configuration

### AWS Credentials

This project uses AWS managed services (**Amazon S3, AWS Translate, AWS Comprehend**) via the `boto3` Python SDK.

To run the code locally, AWS credentials must be configured.

#### 1. Install the AWS CLI
Install the AWS CLI following the official AWS documentation:
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

#### 2. Configure AWS credentials
After installing the AWS CLI, configure your credentials by running:

```bash
aws configure


    Explain in your document/blog post explicitly:
    
    The Problem - What are you researching/trying to solve and how
    The Dataset / Data Source
    What you did to arrive at your results
    Your findings/results
    Add a section at the end about the breakdown of the costs of the services used (your trial and failure included). It doesn't need to     be an amount down to a penny, but give approximations, like "Using AWS Comprehend on ~1200 words 10 times: $0.12"... Provide a total     cost estimate at the end.


This repository contains code and csv files that Katharina Burtscher and Nahian Ibnat created for their Assignment 3.
Their project tries to look at the reporting on the BNP (Bangladesh Nationalist Party, a popular religious-conservative party) by two different news sites, [The Daily Star](https://www.thedailystar.net), and [Prothom Alo](https://www.prothomalo.com), a newspaper that publishes both in English and Bengali.

In our project we tried to figure out if the sentiment of the articles differ from each other, depending on the news site and the language. In order to do this, we scraped 10 articles from The Daily Star, 10 articles from the English, as well as 10 articles from the Bengali Prothom Ali website. We translated all of the Bengali articles to English and saved the articles with their urls and both the original and translated version in "articles_final.csv".

## Repository Structure

```text
.
├── images/
│   ├── histogram.png
│   ├── scatter.png
│   └── table.png
│
├── serverless/
│   ├── web_scraping.ipynb
│   ├── translate.ipynb
│   └── comprehend.ipynb
│
├── data/
│   └── articles.csv
│
├── requirements.txt
├── LICENSE
└── README.md



## AWS Cost Estimation

| AWS Service | Usage Description | Pricing Basis | Estimated Cost |
|------------|------------------|---------------|----------------|
| Amazon S3 | Storage of one CSV file (<1 MB) and a small number of upload/list requests | $0.023 per GB per month; request costs negligible | ~$0.00 |
| AWS Translate | Translation of ~30 news articles (English & Bangla), with automatic language detection and chunking for long texts (~60,000 characters total) | $15 per 1 million characters | ~$0.90 |
| AWS Comprehend | Sentiment analysis on ~30 English-translated articles (~45,000 characters total) | $0.0001 per 100 characters | ~$0.05 |
| **Total Estimated Cost** | — | — | **~$0.95** |


### Notes
- Actual costs may be lower if covered by the AWS Free Tier.
- Amazon S3 costs are negligible due to minimal storage and request volume.
- AWS Translate represents the largest cost component due to character-based pricing.


