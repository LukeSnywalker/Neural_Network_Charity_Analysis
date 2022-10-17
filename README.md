# Neural Network Charity Analysis
 
## Overview

In this project, we attempted to create a binary classifier that is capable of predicting whether charity organizations will be successful if funded by fictional foundation Alphabet Soup. The model was trained on a CSV database of over 34,000 organizations that have received funding from Alphabet Soup. Three attempts were made at optimizing the model to acheive an accuracy score above 75%.

## Results

### Data Preprocessing

* The only variable considered to be the target for the model was "IS_SUCCESSFUL", or whether or not applicants used the money effectively after being funded by Alphabet Soup. This was a binary variable, with 1 for "yes" and 0 for "no", making it a perfect target for a binary classifier.
* The variables considered the features for the model were all of the descriptors for each organization: "APPLICATION_TYPE", "AFFILIATION", "CLASSIFICATION", "USE_CASE", "ORGANIZATION", "STATUS", "INCOME_AMT", "SPECIAL_CONSIDERATIONS", and "ASK_AMT". The model would be predicting whether the organization "IS_SUCCESSFUL" after funding from Alphabet Soup, based on all of these qualities.
* "EIN" and "NAME" were neither used as targets nor features, because they were purely identification columns. They were dropped from the input data.
* During preprocessing, we picked out APPLICATION_TYPE and CLASSIFICATION for binning, since they had more unique values than most of the other columns and could have potentially confused the model. Density plots were used to determine that APPLICATION_TYPE values under 500 and CLASSIFICATION values under 1000 should be put into respective "Other" categories for simplification purposes.

![Density plot of APPLICATION_TYPE](/images/application_type_density.png)
![Density plot of CLASSIFICATTION](/images/classification_density.png)

### Compiling, Training, and Evaluating the Model

* Our initial model contained two hidden layers, with the first containing 80 nodes and the second containing 30 nodes. Considering the goal was binary classification, with the model predicting whether or not an organization used the money successfully, it was unlikely that we would need more than two or three hidden layers. We also followed the rule of thumb of having about twice as many nodes in the first hidden layer as there were inputs (80 versus 43).
* Both hidden layers used the ReLU activation function, while the output layer used the sigmoid function. ReLU is a very effective function and would not have much of an altering effect on the data, since there were no negative values to turn into 0's. The sigmoid function transforms the output to a range between 0 and 1, which matches the values of the target variable, "IS_SUCCESSFUL".
* The first iteration of the model was not able to achieve the target performance of 75% accuracy and was only able to reach 72.7%.
* The first step taken to attempt to optimize the model was looking at the input data to determine if more preprocessing was needed. After another look at the value counts for each feature, ASK_AMT looked like another possible candidate for binning. The requested amount of money seemed like it would be relatively varied, but it turned out that an extreme majority of the organizations asked specifically for $5000, with all other asking amounts having three or less instances in the data. Therefore, all of the asking amounts besides $5000 were binned into an "Other" category, and the ASK_AMT column was modified to be a binary classification of "is $5000" or "is not $5000". Unfortunately, this did not improve the accuracy of the model, and in fact, it decreased from 72.7% to 72.65%.

![Value counts for ASK_AMT](/images/ask_amt_value_counts.png)

* For the second optimization attempt, the changes made in the first attempt were maintained, but this time the number of neurons in the second hidden layer was increased from 30 to 40. An increase in the number of model parameters would probably produce at least a slight improvement in the accuracy. This was proven correct, although the target performance still was not achieved. The accuracy was increased to only 72.9%.
* All of the above changes were kept for the third optimization attempt, but a third hidden layer was also added that contained 20 neurons and used the sigmoid activation function. Since the goal was to predict a binary classification of "is successful" vs "is not successful", and the ASK_AMT variable was converted to a binary format, the reasoning was that a sigmoid function may do a better job of classifying the data than ReLU alone due to its binary nature. Interestingly though, this change produced a slightly lower accuracy score than the previous attempt: 72.86% compared to 72.9%. Either adding a third hidden layer was redundant, or ReLU was the better activation function for the job after all.

## Summary

* The attempts at optimizing the data were not able to make a significant change to the accuracy of the model, and they all averaged out at about 72.8% accuracy. It seems unlikely that further attempts at optimizing this model would bring the accuracy to the target of 75% unless there were significant changes to the number of parameters. One method that wasn't attempted was increasing the number of epochs or changing the batch size, but this might also be redundant, since the accuracy tended to approach the final value long before 100 epochs were completed.
* A different model that could help solve this classification problem is a support vector machine (SVM). Since this is a binary classification problem, an SVM might be better for this job than a neural network because it would try to maximize the separation between the "is successful" and "is not successful" data points.