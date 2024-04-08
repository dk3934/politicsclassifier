# Algorithmically Distinguishing Political vs. Non-Political Articles

## Motivation

TDM Studio provides access to U.S. Newsstream: a large database of digitized news articles written for U.S.-based publications. For this study on the portrayal of female politicians in the American media, we were interested solely in the *political* articles in this database—particularly those concerning elections. In order to filter out the political articles from U.S. Newsstream, we used the following keywords:

```
election OR elections OR candidate OR primaries OR gubernatorial or incumbent 
```

While this filtering process does indeed eliminate many of the non-political articles loaded by the database, it does not guarantee that our corpus will be free of "false positives," or articles that are incorrectly labeled as political because they contain one or more of the above keywords. For instance, any article that calls someone a good "candidate" for a COVID booster will end up in our dataset, even if it makes no mention of political elections, simply because "candidate" happens to be one of our keywords.

To ensure that our corpus contains only documents that are relevant for studying the distinctions in language used about male and female political candidates, we build a classifier that can algorithmically distinguish political and non-political articles. We then use it to identify and remove the non-political articles from the corpus altogether.

## Data Source

In a classification problem such as this one, the ideal training data for a model is a large, labeled text dataset. Fortunately, a dataset of precisely this nature is available to us in the form of the [News Category Dataset](https://rishabhmisra.github.io/publications/) (hereafter NCD), created by Rishabh Misra. NCD was created by randomly sampling articles of various categories from the Huffington Post from 2012 and 2022. The data, which has been made availabla [here](https://www.kaggle.com/datasets/rmisra/news-category-dataset/data) in JSON format, consists of `link`, `authors`, `headline`, `short_description`, `date` and `category` columns and over 200,000 rows.

## Scraping & Pre-Processing

Since NCD does not contain the actual text of the articles, the most important step is to use the `link` article to web-scrape the text of each article and store it for future analysis. Many of the links are either broken or redirect to the [huffpost.com](https://huffpost.com) homepage, but we still scraped over 11,000 articles' worth of data from the remaining valid links. The functions used to perform the web-scraping are available in `code/Scraping.ipynb`. The resulting csv, which is identical to NCD with the addition of `text` and `class` columns, is available in `data/classifierdata_raw.csv`. Note that the `text` column is the sum of the `headline`, `short_description`, and `article_text_x` columns (`article_text_x` is only the scraped text from the article's body). The `class` column simply maps NCD's original `category` column to 1 for politics and 0 otherwise.

For training the classifier itself, we keep only the `text` and `class` columns. We then perform basic pre-processing by lowercasing all words in every document, removing punctuation and whitespace, and lemmatizing. All of the preprocessing code is available in `code/Preprocessing.csv` and the processed, two-column data is available in `data/classifierdata_processed.csv`.

## Classifier

After scraping and pre-processing, we end up with a dataset that has approximately 1 political article for every 2 non-political articles. We fit the data to a TF-IDF vectorizer, and then to a support vector machine with a linear kernel, perform a grid search to determine the optimal hyperparameters, and calculate several performance metrics that take into account our class imbalance. All of the relevant code and results are available in `code/PoliticsClassifier.ipynb`. We include some relevant results here:

![classification report](/Users/dianakazarian/Documents/politicsclassifier/images/classreport.jpg "Classification Report")

![roc-auc score](/Users/dianakazarian/Documents/politicsclassifier/images/rocplot.jpg "ROC-AUC Score")