## Project Summary
● Perform exploratory data analysis (EDA) on a dataset with 6.3 million transaction records and 11 variables, seeking to understand the extent to which each variable predicts the occurrence of fraudulent transactions.
● Preprocess and clean the dataset by fixing data types, engineering features, and dropping unnecessary columns, outputting a csv with clean data to train a model on.
● Train 5 iterations of random forest models- tuning hyper parameters, and outputting a final model that spots ~80% of fraud cases.


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
