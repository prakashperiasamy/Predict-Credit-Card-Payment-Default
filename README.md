# Predict-Credit-Card-Payment-Default
Built a classification model to predict the likelihood of credit card default 
# 1. Executive Summary

- Goal/Task: To develop a build to predict the likelihood of default
- Metric: Optimizing for ROC_AUC and Recall (since I assume False Negative is critical for our goal)
- Engineered two new features namely "credit_utilization_rate" and "revolving_balance_rate"
    - "credit_utilization_rate" is defined as the ratio of average statement balance to credit limit
    - "revolving_balance_rate" is defined as the ratio of average revolving balance to credit limit, where revolving ratio is defined as the difference between statement balance and payment amount
- Key Indicators of Default:
    - Model suggests, prior payment history/status as a key indicator in addition to "credit_utilization_rate" to predict the likelihood of default. 
    - In the payment history, specifically the payment status of the immediate last month is a strong indicator among the other past months. For example, if you like to predict the default in October, then payment status is September is a key indicator rather than May of that year.

- EDA Summary:
    - Statement balance are highly correlated to each other and hence can be condensed into one variable such as average statement balance over a defined period
    - By the numbers: Within the given dataset,
        - median credit limit is about 140K NT dollar
        - median statement balance is about 20K NT dollar
        - median payment is about 1500 to 2000 NT dollar
- Model Info: 
    - Based on ROC_AUC, Logistic regression, Random Forest and Light GBM all perform similarly, with the latter two slightly better. ROC_AUC scores are about 0.7 with Recall of about 0.51
    - Hence depending on other business/technical needs one choose from Logistic Regression or Random Forest or Light GBM
    - I'm personally choosing Random Forest because once trained, making predictions requires traversing the tree going through roughly on the order of log(#records) and independent on the number of features. So even with large training sets, time complexity of predictions are less.
- Business Suggestions:
    - Some of the customers have a high credit_utilization_rate but do make their payments on-time with good standing. So we could have a product that could recommend to increase their credit limit for such customers or suggest new credit cards, after possibly collecting additional info.
    - Also there are customers who are inactive. So there could be some reach out programs designed to follow up on these customers

# 2. Next Steps
- Feature Engineering: Newly created features such as credit_utilization_rate and revolving_balance_rate seems to have high feature importance. Hence with more time, would continue to think of new features from exisiting variables
    - Feature engineering seems have to good effect on the model prediction

- Additional Data: From a product perspective, I would seek additional data such as income, past bankruptcy and other external data such as other loans/debt that can improve our model performance to predict the likelihood of default
    - Clearly as identified in the univariate analysis, some customers albeit their available features being equal to non-defaulters are indeed labelled as defaulters

- Hyper Parameter Tuning: Considering time, did a first pass hyper parameter tuning. With more additional time, it may be worth looking at it further, although it seems the effect is not significant
