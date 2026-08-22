### EDA Insights
1) Fraud cases only showed up in 2/5 transaction types- cash out and transfer (0.2% and 0.8% of all transactions accordingly).
2) Fraud occurred in 0.12% of total transactions- a severe imbalance which will affect the sampling method used for the model.
3) The median transaction amount for fraudulent transactions was nearly 6x higher than for non-fraudulent transactions.
4) 98.1% of fraudulent transactions end with a zero origin account balance
5) Nearly all fraudulent transactions drained the account of it's full starting balance.
6) Non-fraudulent transactions had a starting original balance of 0 at an average rate of 0.19%- fraudulent at a rate of 0.002%, meaning fraud occurred significantly less often if the starting balance was 0. Transactions with money in the account before a transaction were fraudulent 95x more often. 


## Dropped Columns
1) Account names were dropped, assuming that accounts already detected as fraudulent in this dataset will not be active to recommit fraud, and therefore are not a useful future predictor.
2) 'oldbalanceDest', 'newbalanceDest', 'newbalanceOrig' columns were dropped due to their high correlation with 'amount' and 'oldbalanceOrg' - which showed significant differences in their distribution in the presence of fraud. While a random forest can handle highly correlated variables, I removed them to a more accurate ranking of the best predictive variable in the model.
3) 'isFlaggedFraud' was dropped because it's true when amount > 200,000, which is not useful for the model as 'amount' will already be used, and it'll return the impact of 'amount' from learning on the training data by itself.
4) 'step' was dropped because EDA found that there were no time trends of fraud rate fluctuation.

## Hyperparameter Tuning
I used random-search for hyperparameter tuning because it takes less time and less compute (encountered a memory error with grid search). Afterwards I ended up scaling down the random-search significantly due to compute time (10.3hrs and going on the 2nd try before reducing scope). Tuning was done on only 10% of the training data due to time constraints - the impacts of which are discussed below.

## Model Performance Pre-Tuning
Running the model without hyperparameter tuning, but instead using scikitlearn's default hyperparameters actually had the model perform better on the recall score (.79 vs .57), meaning fraud cases were caught more often, at a 38.6% recall improvement and 79% of all fraud cases identified correctly as fraud. The pre-tuned model had a worse score for precision (.88 vs .94) - out of all transactions it flagged as fraud, only 88% were fraud. However, an increase in false positives tolerable when accounting for the decrease in false negatives - denoted by it's higher recall score. The f1 score (.83 vs .71) means the two metrics are better balanced. My theory is that performance was better for the model without tuned hyperparameters because tuning was done on only 10% of the training data in order to reduce compute time AND that the hyperparameter options picked to search through did not include enough range and variety. For example, depth at 'None' was not tested, which limited the complexity of each tree. In any subsequent iterations I'd pick parameters that include the defaults and values around them. Additionally, I'd perform tuning on all of the training data, which will take more time but will potentially improve the resulting model's prediction metrics.

## Final F1 Score
Post hyperparameter tuning, I had a score of 0.71, representing the balance of the precision and recall metric - meaning that in this model, one metric is noticeably better than the other. Looking at the metrics themselves, precision is a high 0.94, while recall is only at 0.57, meaning the model misclassified 43% of fraud cases as non-fraud - letting a high amount of fraud go undetected.

If I had to pick one of the models to use at this time without further iteration, I'd go with the last model in the 'model.ipynb' notebook, which does not have tuned hyperparameters, because of it's better recall/ lower probability to misidentify fraud as a non-fraud transaction.