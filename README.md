![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) 
## Project 3: Web APIs & NLP

---

## Contents

[A. Executive Summary](#A.-Executive-Summary)<br>
[B. Datasets, Data Dictionary](#B.-Datasets,-Data-Dictionary)<br>
[C. Workflow](#C.-Workflow)<br>
[D. Summary and Insights](#D.-Summary-and-Insights)<br>
[E. Conclusion](#E.-Conclusion)<br>

---

## A. Executive Summary

### Context
The field of data science has witnessed remarkable growth and popularity as organizations increasingly rely on data-driven decision-making across diverse industries. This surge has led to the [emergence of new roles and specializations](https://www.datacamp.com/blog/data-scientist-vs-data-engineer) within the data domain, including data engineering. Data engineering is a critical function that involves the collection, processing, and organization of vast volumes of data, facilitating data scientists in extracting valuable insights and building impactful models.

However, there is a growing challenge in distinguishing between data science and data engineering roles, as their job requirements, skills, and educational qualifications often overlap. Job postings for these roles may present confusion, making it difficult for individuals to accurately classify data science and data engineering responsibilities. [The lines between these two roles are often blurred](https://datascience.virginia.edu/news/data-science-vs-data-engineering), leading to a lack of clarity for both job seekers and employers. Additionally, it is worse for people new to the industry, trying to enter the field of data.

### Problem Statement 
You are a member of the General Assembly (GA) team, responsible for evaluating the need for a new Data Engineering Immersive Course. To gain valuable insights and understand the demand for such a program, GA has plans to set up a discussion channel to gather feedback and queries from potential students. For internal analysis, the team hopes to categorize the comments and queries into the categories of data science or data engineering. This sorting and categorizing could be time-consuming and impractical if done manually.

To address this challenge, your objective is to **develop a natural language processing (NLP) model that can automatically classify the comments into two distinct categories: Data Science and Data Engineering**. This could help the team gain insights on the demand for each specialization and guide your decision-making process in setting up the new Data Engineering Immersive Course. We will be training and testing our NLP models using Reddit, through 2 subreddits r/datascience and r/dataengineering. Further investigations - **sentiment analysis** on the comments would also allow us to gain insights into the attitudes and perceptions of potential students towards each specialization. 

---
## B. Datasets, Data Dictionary

Our NLP model We used PRAW: The Python Reddit API Wrapper to pull all available posts and comments from subreddits [r/datascience](https://www.reddit.com/r/datascience/) and [r/dataengineering](https://www.reddit.com/r/dataengineering/). 

From the subreddits, these were the attributes that were pulled to conduct EDA and modelling:
|**Attributes**|Description|
|----|----|
|title|The title of the submission.|
|id|ID of the submission.|
|author|Provides an instance of Redditor.|
|created_utc|Time the submission was created, represented in Unix Time.|
|edited|Whether or not the submission has been edited.|
|is_self|Whether or not the submission is a selfpost (text-only).|
|link_flair_text|The link flair’s text content, or None if not flaired.|
|num_comments|The number of comments on the submission.|
|saved|Whether or not the submission is saved.|
|score|The number of upvotes for the submission.|
|selftext|The submissions’ selftext - an empty string if a link post.|
|stickied|Whether or not the submission is stickied.|
|upvote_ratio|The percentage of upvotes from all votes on the submission.|
|url|The URL the submission links to, or the permalink if a selfpost.|
|subreddit|Provides an instance of Subreddit.|
|title_selftext|Feature Engineered column merging title and selftext.|


Data inspection and cleaning was done first. General cleaning (missing values, duplicates) and text-based cleaning was done. Some of the text-based cleaning includes lowercasing the data, removing Puncuatations, removing Numbers, removing extra space, replacing the repetitions of punctations, removing emojis, emoticons, removing http links, converting contractions.
These cleaning processes are crucial to remove as much noise as possible, to allow our model to identify stronger text relationships to our subreddits. At the end of cleaning, we arrived at a dataset of 25502 posts/comments:

* `r/datascience`
    * 13052 rows of data
* `r/dataengineering`
    * 12450 rows of data

The cleaned data (from both subreddits) was exported for modelling:
* [`df_text.csv`](./data/df_text.csv): Cleaned and combined text data

---
## C. Workflow

Due to the scale of this project, the main processes / segments of the project were split up into separate notebooks. There are 3 notebooks in total:
* Notebook_1 - Data collection
* Notebook_2 - Data Cleaning, EDA
* Notebook_3 - Preprocessing of Text, Modelling
<br>

---
## D. Summary and Insights

### Part 1 - Data Import and Cleaning

In notebook 1, we used PRAW: The Python Reddit API Wrapper to pull all available posts and comments from subreddits r/datascience and r/dataengineering. We managed to pull the following:
* `r/datascience`
    * 961 posts
    * 12403 comments
* `r/dataengineering`
    * 963 posts
    * 11740 comments
 
This provides ample rows of data for us to further analyse + use to train our model.


### Part 2 - Exploratory Data Analysis

#### Data Inspection and Cleaning
* In notebook 2, data inspection and cleaning was done first. General cleaning (missing values, duplicates) and text-based cleaning was done. Some of the text-based cleaning includes lowercasing the data, removing Puncuatations, removing Numbers, removing extra space, replacing the repetitions of punctations, removing emojis, emoticons, removing http links, converting contractions.
* These cleaning processes are crucial to remove as much noise as possible, to allow our model to identify stronger text relationships to our subreddits. At the end of cleaning, we arrived at a dataset of 25502 posts/comments (datascience: 13052, dataengineering: 12450).

#### EDA
* EDA was split into 3 main sections: Post Attributes-related, Text-related, Sentiment Analysis. Below are some insights gained from the respective sections.
  
##### Post Attributes-related
* r/dataengineering seems to have slightly more recent activity (more edited, saved, stickied posts) - As data engineering is the newer branch of data science, there could be more interest and discussion around it.
* r/datascience has more unique authors than r/dataengineering (852 users vs 776 users) - likely because data science is the more established field which people would be more familiar with.
* r/datascience seems to have more posts with higher scores than r/dataengineering. But r/dataengineering has a larger proportion of posts with upvote ratio above 0.5, meaning more posts have scores > 0.
* For both r/datascience and r/dataengineering, the top category was 'Discussion'. This reinforces the GA team's plans to set up a discussion channel to gather feedback and queries from potential students, since there is interest to have further discussions on both r/datascience and r/dataengineering.


##### Text-related
* Top 20 words, and two-grams were extracted for both subreddits
* We see some technical terms being highlighted for each subreddit's text, examples are "python", "confidence interval", "linear regression", "power bi", etc for data science, and "sql", "ci cd", "parquet file", "star schema", etc for data engineering. This could help GA highlight people of higher interest, since we can infer that people who use these words on GA's discussion platform are likely to have done additional reading on their own to even know these terminologies.
* We also analysed for similar words between both subreddits, these words were added to stopwords to reduce noise during training of our model later on.

##### Sentiment Analysis
* Comments and posts in the subreddits had an average sentiment of 64% and 63% positive sentiments for data science and engineering respectively
* This is good for GA, as it shows that people currently working or are planning to work in these fields are positive about the roles as a whole (as compared to other industries where subreddits are visibly and mostly negative). We can expect an increase in people interested to do a bootcamp to enter these roles.


*Is EDA enough to address our problem statement?*<br>
Upon concluding EDA, we have gained initial insights into the levels of interest in data science and data engineering. This aids General Assembly in solidifying the value proposition of establishing a discussion platform for existing students and prospective individuals keen on enrolling in GA bootcamps. However, in order to fully address the root concern that sorting and categorizing comments from the discussion platform would be time-consuming and impractical if done manually, we would still need to develop a natural language processing (NLP) model that can automatically classify the comments into two distinct categories: Data Science and Data Engineering. Only after modelling would we be able to meet the main objective of the problem statement.

### Part 3 - Preprocessing, Modelling

* In order to sort text-based comments/queries into two distinct categories, we will be using several models that are appropriate for binary classification of text. The model selection process involves weighing the trade-offs between simplicity and complexity, considering factors like interpretability and computational efficiency. Various models, such as Logistic Regression, Naive Bayes, Random Forests, ADABoost and Support Vector Machines will be used for this task.<br><br>
To evaluate the models, the metrics of Accuracy and F1 Score will be used:
    * Accuracy: Since the goal is to classify comments and queries into data science or data engineering categories, an accurate measure of how often the model is correct is of primary importance. Our dataset used for the training model is also relatively balanced.
    * F1 Score: (the harmonic mean of precision and recall) As my model is just slightly imbalanced (0.51:0.48 ratio), i decided to consider F1 score as it is a good overall measure to balance precision and recall. Since the model would be used to categorize the comments and queries from the GA open discussion platform into the categories of data science or data engineering, we do not need to prioritize predicting of positives or negatives. This metric is hence useful when the consequences of false positives and false negatives are not heavily skewed. <br><br>


**Summary of model performance:**
|No|Model|Vectorizer|Train Accuracy, F1 Score|Test Accuracy, F1 Score|
|---|---|---|---|---|
|0|Base model <br>Logistic Regression|CVEC|0.9112 (Acc)|0.7481 (Acc)|
|1A|Naive Bayes|CVEC|0.7793 (Acc)<br>0.8002 (F1)|0.7752 (Acc)<br>0.7973 (F1)|
|**1B**|**Naive Bayes**|**TF-IDF**|**0.8041 (Acc)<br>0.8226 (F1)**|**0.7868 (Acc)<br>0.8075 (F1)**|
|2A|Random Forest|CVEC|0.7749 (Acc)<br>0.8158 (F1)|0.7168 (Acc)<br>0.7683 (F1)|
|2B|Random Forest|TF-IDF|0.7858 (Acc)<br>0.8245 (F1)|0.7192 (Acc)<br>0.7698 (F1)|
|3|ADABoost|TF-IDF|0.7228 (Acc)<br>0.7746 (F1)|0.704 (Acc)<br>0.7606 (F1)|
|4|SVM|TF-IDF|0.9975 (Acc)<br>0.9976 (F1)|0.7884 (Acc)<br>0.8011 (F1)|

All the models here outperformed the base model accuracy score of 0.7481. As the aim of the model is to be able to successfully perform binary classification on our text posts and comments, we select the model with the highest accuracy and F1 score -  The Naive Bayes with TF-IDF vectorizer. 
The top-performing Naive Bayes model exhibits commendable performance metrics on both the training and testing datasets. It is likely that our NLP model does not require such a complex model, and the simpler models like Naive Bayes would suffice and produce good results if trained with the right parameters. With a training accuracy of 0.7928 and an F1 score of 0.8127, and a testing accuracy of 0.7868 and an F1 score of 0.8075, the model demonstrates a consistent level of accuracy and balanced precision and recall. 

---
## E. Conclusion

### Addressing the Problem Statement
* In this project, through EDA, we gained initial insights into the levels of interest in data science and data engineering that would help General Assembly (GA) solidify the value proposition of establishing a discussion platform for existing students and prospective individuals keen on enrolling in GA bootcamps. The development of a natural language processing (NLP) model to automatically classify comments into the categories of Data Science and Data Engineering would help address downstream workflow concerns of having to sort and categorize comments from the discussion platform.

* Through the exploration of Reddit's r/datascience and r/dataengineering subreddits, we have successfully trained and tested various binary classification models, including Logistic Regression, Naive Bayes, Random Forests, ADABoost, and Support Vector Machines. Our evaluation metrics, including accuracy and F1 score, have enabled a comprehensive assessment of model performance, providing valuable insights into their effectiveness. 

* Our top model (Naive Bays, using TF-IDF vectorizer) has a training accuracy of 0.7928 and an F1 score of 0.8127, and a testing accuracy of 0.7868 and an F1 score of 0.8075, demonstrating a consistent level of accuracy and balanced precision and recall. The problem statement highlighted the need for a robust NLP model to streamline the process of categorizing discussions from the General Assembly open platform, and the performance metrics proved the model being able to accurately classify comments to a commendable accuracy level.

* This data-driven approach supports informed decision-making and enhances GA's ability to cater to the needs and preferences of students, thus solidifying its position as a pioneering tech education institution.



### Limitations and Recommendations
For further improvements and development downstream, we would like to point out several limitations of the model as well as some recommendations:

**1) Generalization to New Domains<br>**
* The current model is tailored to categorize discussions specifically within the domains of Data Science and Data Engineering. Further development is needed to adapt the model for other domains or to handle more diverse and nuanced topics. 
* GA could consider further developing a multiclassification model that also includes classification of comments relating to Software Engineering, or other increasing popular data roles in the future.

**2) Improve the model on contextual understanding<br>**
* Since we trained the model through subreddits, the model might struggle with ambiguous or context-dependent language, leading to misclassifications when faced with intricate sentence structures or word meanings. If the discussion platform is mainly catering to GA current and potential students who are based in Singapore, then we would have to consider the use of Singlish on the discussion platform. The topics discussed on subreddits as compared to GA's discussion platform may also be quite different.
* In future, we could explore advanced NLP techniques, such as transformer-based models (e.g., BERT, GPT), which excel at capturing contextual information and resolving ambiguities. We should also periodically update the model with new data (from the actual GA discussion platform) to ensure it remains relevant and effective in classifying current discussions.

**3) A standalone Sentiment Analysis study <br>**
* As we can see from our quick sentiment analysis, the study was useful to gain insights into sentiments expressed within the discussions. To achieve a more comprehensive understanding, a more detailed and thorough sentiment analysis should be conducted. While VADER has been used effectively in this study, it is imperative to establish its reliability by performing a rigorous assessment of its classifications.
* One approach to ensuring the accuracy of VADER's classifications involves conducting a manual review of a representative sample of VADER's sentiment assignments. By manually analyzing a subset of comments and comparing the manual assessment with VADER's classifications, we can quantify VADER's accuracy and identify any discrepancies.
* This validation process will provide insights into the model's performance and highlight areas where adjustments or refinements may be necessary. This validation step is crucial for building trust in the sentiment analysis results, establishing the reliability of sentiment classifications will enable more confident utilization of sentiment analysis outcomes for strategic decision-making in the future.
* Some additional business use case scenarios for our sentiment analysis model would include: helping to Tailor Course Offerings, Strategic Marketing, etc.

