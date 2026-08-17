## Project Summary
Create a machine learning model that detects fraudulent account charges.

## Report
### EDA Insights
1) Fraud cases only showed up in 2/5 transaction types- cash out and transfer (0.2% and 0.8% of all transactions accordingly).
2) Fraud occurred in 0.12% of total transactions- a severe imbalance which will affect the sampling method used for the model.
3) The median transaction amount for fraudulent transactions was nearly 6x higher than for non-fraudulent transactions.
4) 98.1% of fraudulent transactions end with a zero origin account balance
5) Nearly all fraudulent transactions drained the account of it's full starting balance.
6) Accounts that started at 0 were fraudulent at a 0.19% rate, while being fraudulent 0.002% of the time, meaning that accounts with money in the account before a transaction were 95x more often fraudulent.


## Dropped Columns
1) Account names were dropped, assuming that accounts already detected as fraudulent in this dataset will not be active to recommit fraud, and therefore are not a useful future predictor.
2) 'oldbalanceDest', 'newbalanceDest', 'newbalanceOrig' columns were dropped due to their high correlation with 'amount' and 'oldbalanceOrg' - which showed significant differences in their distribution in the presence of fraud. While a random forest can handle highly correlated variables, I removed them to a more accurate ranking of the best predictive variable in the model.
3) 'isFlaggedFraud' was dropped because it's true when amount > 200,000, which is not useful for the model as 'amount' will already be used, and it'll return the impact of 'amount' from learning on the training data by itself.
4) 'step' was dropped because EDA found that there were no time trends of fraud rate fluctuation.

## Hyperparameter Tuning
I used random-search for hyperparameter tuning because it takes less time and less compute (encountered a memory error with grid search). Afterwards I ended up scaling down the random-search significantly due to compute time (10.3hrs and going on the 2nd try before reducing scope).

## Model Performance Pre-Tuning
Running the model without hyperparameter tuning, but instead using scikitlearn's default hyperparameters actually had the model perform better on the recall score (.79 vs .57), meaning fraud cases were caught 22% more often. The pre-tuned model had a worse score for precision (.88 vs .94) - out of all transactions it flagged as fraud, only 88% were fraud. However, more false positives are a more desirable result than false negatives - which this model has less of due to it's higher recall score. The f1 score (.83 vs .71) means the two metrics are better balanced. My theory is that performance was better without tuning hyperparameters because tuning was done on only 10% of the training data in order to reduce compute time AND possibly that the hyperparameter picked for the search did not include depth at 'None', for example, limiting the complexity and the potential for better hyperparameter discovery. In any subsequent iterations I'd pick parameters that circle the defaults, and perform tuning on all of the training data, which will take more time but could potentially improve the resulting model.

## Final F1 Score
Post hyperparameter tuning, I had a score of 0.71, representing the balance of the precision and recall metric - meaning that in this model, one metric is better than the other. Looking at the metrics themselves, precision is a high 0.94, while recall is only at 0.57, meaning the model misclassified 43% of fraud cases as non-fraud - letting much fraud slip by undetected. If having more false positives is more desirable than false negatives, a reworked model's outcome should aim for a higher recall.

After going back and creating a model without hyperparameter tuning, if I had to pick the model to use at this time, I'd actually go with the latter, due to it's better recall and ability to identify more actual fraudulent cases.


## Data Dictionary
● Step: A unit of time that represents hours in the dataset. Think of this as the timestamp of the transaction (e.g. hour 1, hour 2, … hour)
● Type: The type of transaction 
● Amount: The amount of money transferred 
● NameOrig: The origin account name
● OldBalanceOrg: The origin accounts balance before the transaction 
● NewBalanceOrg: The origin accounts balance after the transaction 
● NameDest: The destination account name 
● OldbalanceDest: The destination accounts balance before the transaction 
● NewbalanceDest: The destination accounts balance after the transaction
● IsFlaggedFraud: A “naive” model that simply flags a transaction as fraudulent if it is greater than 200,000 (note that this currency is not USD) 
● IsFraud: Was this simulated transaction actually fraudulent? In this case, we consider “fraud” to be a malicious transaction that aimed to transfer funds out of a victim’s bank account before the account owner could secure their information. 
