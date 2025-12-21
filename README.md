# Group-11_Week5_Assignment

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

