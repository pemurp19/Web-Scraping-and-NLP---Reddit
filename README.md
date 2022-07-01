# Project 3: Web APIs & NLP: Examining the r/mlb and r/redsox Subreddit Channels

-----
### Problem Statement

I’m a data scientist with the StubHub Marketing team. We’re planning to run an ad campaign on reddit to boost ticket sales. In order to use our advertising budget most effectively, we want to strike a balance between casting a wide net and also tailoring our messaging to an audience that’ll be most receptive. Effectively, we want to determine whether it’s better to target an individual team’s fan page or broader MLB fan page?

My goal is to analyze language in posts from both the Red Sox and MLB subreddits and answer the following two questions:
1) Is it possible to build a classification model that accurately predicts whether a post came from an MLB or Red Sox reddit user with at least 70% accuracy? 
2) Do posts on either subreddit tend to skew toward positive sentiment, and if so, which has higher positive sentiment and may be more receptive to the ticket advertisements?

-----
#### Datasets

The web-scraped data, including posts from both the r/mlb and r/redsox subreddits, was combined in one dataset.  

#### Data Dictionary

You may find full descriptions of the data found in the Reddit API .json in the API's ['documentation.'](https://github.com/reddit-archive/reddit/wiki/JSON)


---

### Preprocessing and Modeling

To provide some background on the two subreddits, the MLB page has nearly double the members, yet the distribution of word counts per post is relatively in line. Utilized Reddit’s Pushshift API to pull a total of 3,000 posts from each page from June, 2022 going back to September 2021. Having started by pulling 1,000, I realized in cleaning the data that I needed to pull significantly more posts to end up with 1,000 post to train my model on. For instance, I noticed that nearly 584 Red Sox posts were removed or deleted by the moderator, and 1,700 were null. Additionally, I removed video posts as I would need to train a classifier on text data, and realized that I could remove stop words to improve my models' accuracies (the top 15 words showing up on each page's post were stop words). 

![alt text](https://git.generalassemb.ly/pemurp96/project-3/blob/master/images/word_count_dist_dark.png)


In terms of defining my problem statement, I relied on my EDA to inform realistic thresholds to shoot for - in other words, 70% accuracy, 70% F1 score and 70% precision. False positives and false negatives are equally harmful in this context which is why I've focused on optimizing the F1 score and precision. Note that each page includes similar words, such as "mlb" and "game," and I wanted to ensure that my target would be realistic given similarities across the pages and their posts.

![alt text](https://git.generalassemb.ly/pemurp96/project-3/blob/master/images/mlb_topwords_dark.png)

![alt text](https://git.generalassemb.ly/pemurp96/project-3/blob/master/images/sox_topwords_dark.png)

Using a combination of Count Vectorizers or Tfidf Vectorizers, I iterated through 11 classification model variations (including Logistic Regression, Multinomial Naive Bayes, Random Forest, and Boosting), tuning the optimal hyperparamaters by grid searching. A majority of the models were severely overfit, and particularly the boosting models. Wanted to strike a balance with the bias-variance tradeoff, while hitting the 70% accuracy target. Hence, I identified the best model as a Random Forest that implemented a Count Vectorizer. The model achieved 71.3% accuracy on the training data, a 70.6% F1 score, and 71.8% precision. 

![alt text](https://git.generalassemb.ly/pemurp96/project-3/blob/master/images/randomforest_confusion_dark.png)


-----
### Findings and Recommendations

**Recommendations:**

Given the model's ability to discern between pages at least 70% of the time, the next step involved running a sentiment analysis, to determine which page might be more positive and hence more receptive to advertising. I ran a sentiment analysis using the Vader Lexicon, in which both were positive. My recommendation is to run the campaign on the MLB page first. Slightly more positive sentiment and could be more receptive to advertising based on this. Additionally, I'd note that we're reaching more users on the MLB side - MLB has nearly double the community members.


**Limitations to this analysis:** 
There's limited grounds for comparison, given I've just compared one fan base's subreddit to the MLB subreddit. By this logic, there could be more or less negativity depending on how a team is playing), and this would likely vary based on a fixed period of time (less than a full season). Hence, it would be important to gather additional samples from prior seasons to bolster the findings. 

Specifically, an author for the Red Sox subreddit, "RedSox Gameday, is likely skewing the sentiment toward neutral, which makes us more likely to target the MLB. "RedSox Gameday" posted 127 out of the 1,260 posts analyzed - likely neutral sentiment as it's more of an informational account posting game information (how to watch, starting lineup, etc.), rather than fan opinion.

Of course, it'll also be important to verify how the campaign performs by comparing the conversion rates, click through rates, to other marketing metrics we've examined. As a next step, it would be worth looking at other fanbases relative to the MLB to supplement this analysis
