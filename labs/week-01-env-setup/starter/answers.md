# **Q1**

Different random seeds changes which datapoint is selected in the traning set / test set and therefore
create a slightly different result in terms of the metrics.
Changing the random seed causes the model's performance metrics (accuracy, precision, recall etc.) to be incosistent.

# **Q2**

**Metric notes:**

Every metric has a downside.

 - accuracy: correct classifications out of all
    (TN + TP) / (ALL),
    problem is the distribution of the data
 - precision: 
    (TP) / (TP + FP)
 - recall: (TP) / (TP + FN),
    how many true positives did we find
 - f1 score: (harmonic mean of the precision and the recall), 
    2 * (precision * recall) / (precision + recall)

Business side: ask what are the goals for precision and recall.

**Best F1 score params:**
- 0.6116 with ***(n_estimators=300, max_depth=6)***
- 0.6508 with ***(n_estimators=100, max_depth=15)***
- 0.6457 with ***(n_estimators=100, max_depth=30)***

# **Q4**

It breaks in the ***test_config.py*** module, inside the ***test_load_settings_returns_valid_settings()*** test function.
It's better if it breaks in this test module, because I can track 
exactly which parameter causes the error and the error message clears what value I need to chose for a successful split.

# **Q4**

