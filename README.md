# Contents
- [Problem Statement](#Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [File Directory](#File-Directory)
- [Data Dictionary](#Data-Dictionary)
- [Conclusions](#Conclusions)

# Problem Statement

Predict whether the given text of a submission came from the r/Fitness or r/Bodyweightfitness subreddit.  The goal is to determine how differntiable the posts in these two seemingly similar forums are.

# Executive Summary

The first step to creating a predictive model was to obtain data to use in training and testing the model.  This was achieved using the Pushshift API along with a custom function to accumulate a reasonable amount of data for the task.  The next step was cleaning the data for analysis.  This involved removing post data with no value for analysis such as posts that had been removed or deleted.  Posts that included the name of the subreddit in the body of the test to prevent this information from leaking into the training and influencing the results.  After the data was cleaned, 7052 submissions from r/Fitness and 6711 submissions from r/Bodyweightfitness.

Next, I completed an exploratory analysis of the data.  I looked at the distribution of the number of characters in the posts from each subreddit as a potential way to differentiate the two subreddits.  However I found them to be similar enough as to not be usefull.  I also looked at the most frequently occuring words in each subreddit both before and after preprocessing the data.  After preprocessing there were more terms related to fitness at the top of the list in both forums.  However, there were many of the same words showing up in both lists which indicated that it may be challenging to individuate.

Finally, to select a model I first created a null model to form as the basis for comparison.  The null model was based on predicting the most frequently occuring class which was r/Bodyweightfitness.  The accuracy of the null model was 51%.  Because the classes were balanced, and neither subreddit was more important than the other in terms of making correct classifications, accuracy was used as the primary metric of evaluation.  I then searched several types of models with default parameters to narrow it down to a few for further investigation.  The few models selected were logistic regression, multinomial naive bayes, random forest, and SVC.  Further tuning of parameters with these models, I was consistently able to score between approximately 76% and 78% accuracy.  I was able to achieve 79% accuracy with both a logistic regression and SVC model, but logistic regression was chosen as the final model because of the easy of interpreting the most important features.

# File Directory
```
project-3
|__ code
|   |__ 01_Data_Collection.ipynb   
|   |__ 02_Data_Cleaning.ipynb   
|   |__ 03_EDA.ipynb
|   |__ 04a_Model_Selection.ipynb
|   |__ 04b_Bayes_Models.ipynb
|   |__ 04c_Logistic_Regression_Models.ipynb
|   |__ 04d_Random_Forest_Models.ipynb 
|   |__ 04e_SVC_Models.ipynb
|__ Data
|   |__ bodyweight_clean.csv
|   |__ bodyweight.csv
|   |__ fitness_clean.csv
|   |__ fitness.csv
|__ Presentation
|   |__ Images
|       |__ char_count_dist.png
|       |__ confusion_matrix.png
|       |__ top_10_words.png
|   |__ project_3.pdf
|__ Scratch
|   |__ Code
|       |__ 04d_NN_models.ipynb
|   |__ Data
|       |__ running.csv
|       |__ ultra.csv
|__ README.md
```

# Data Dictionary

The data was collected from [reddit.com](https://www.reddit.com) using the [Pushshift API](https://github.com/pushshift/api).  The subreddits used were r/Fitness and r/Bodyweightfitness.

|Feature|Type|Description|
|---|---|---|
|**author**|*object*|Username of the submission| 
|**created_utc**|*int*|Epoch timestamp of submission|
|**num_comments**|*int*|Number of comments on the submission|
|**num_crossposts**|*int*|Number of crossposts|
|**score**|*int*|Number of upvotes|
|**selftext**|*object*|Body of the submission|
|**subreddit**|*object*|Subreddit submission was posted to|
|**title**|*object*|Title of the submission|

[Cleaned r/Bodyweightfitness data](./data/bodyweight_clean.csv)  
[Cleaned r/Fitness data](./data/fitness_clean.csv)

# Conclusions

Based on the accuracy scores of the tested models, a logistic regression model was chosen as the production model with an accuracy score of around 79% depending on how the data was split between training and testing.  This model used TF-IDF with custom preprocessing and a lemmatizing to vectorize the post data.  It was selected over other similarly performing models due to the ease of interpreting the models coefficients to determine what the important words were for classification to one subreddit over the other.

Given that many models tested all approached the same accuracy, it may be that the models were approaching the limit of how differentiable these two subreddits are.  Since the topic of bodyweight fitness is a subset of the general topic of fitness.  It is reasonable to assume that any post in r/Bodyweightfitness could be equally at home in r/Fitness, and in fact there are no rules in either forum to prevent this.  However, I believe that there is more research that could be done to maximize performance.

For future research, it would be benefitial to further explore the idea of building a custom vocabulary of terms by which to evaluate posts.  This was touched upon but could be expanded further by building a bigger vocabulary, and using that vocabulary with other types of models such as a more advanced neural network.  There is also the possibility that doing more preprocessing of the data to further eliminate common terms, or aquiring more posts could lead to increased accuracy of the final model.