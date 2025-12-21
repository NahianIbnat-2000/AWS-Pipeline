# Group-11_Week5_Assignment

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

