# Algorithmically Distinguishing Political vs. Non-Political Articles

## Motivation

TDM Studio provides access to U.S. Newsstream: a large database of digitized news articles written for U.S.-based publications. For this study on the portrayal of female politicians in the American media, we were interested solely in the *political* articles in this database—particularly those concerning elections. In order to filter out the political articles from U.S. Newsstream, we used the following keywords:

```
election OR elections OR candidate OR primaries OR gubernatorial or incumbent 
```

While this filtering process does indeed eliminate many of the non-political articles loaded by the database, it does not guarantee that our corpus will be free of "false positives," or articles that are incorrectly labeled as political because they contain one or more of the above keywords. For instance, any article that calls someone a good "candidate" for a COVID booster will end up in our dataset, even if it makes no mention of political elections, simply because "candidate" happens to be one of our keywords.

To ensure that our corpus contains only documents that are relevant for studying the distinctions in language used about male and female political candidates, we build a classifier that can algorithmically distinguish political articles. We then use it to identify and remove the non-political articles from the corpus altogether.

## Data Source

In a classification problem such as this one, the ideal training data for a model is a large, labeled text dataset. Fortunately, a dataset of precisely this nature is available to us in the form of the [News Category Dataset](https://rishabhmisra.github.io/publications/) (hereafter NCD), created by Rishabh Misra. NCD was created by randomly sampling articles of various categories from the Huffington Post from 2012 and 2022. The data, which has been made availabla [here] (https://www.kaggle.com/datasets/rmisra/news-category-dataset/data) in JSON format, consists of `link`, `authors`, `headline`, `short_description`, `date` and `category` columns.

