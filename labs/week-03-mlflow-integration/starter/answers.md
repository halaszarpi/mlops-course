## Exercise 2

On the test set the model made:
- False Positives: 19
- False Negatives: 32

## Exercise 4

F1 picks this run:
- run_name: ***rf-n_estimators=300***
- run_id: ***8d4555940fb343d98a9583df5cadcd99***
- metrics.f1: ***0.6240***

ROC-AUC picks this run:
- run_name: ***logreg-C=1.0***
- run_id: ***d6b3baa8d84f4395905b5d01fb1b28fd***
- metrics.roc_auc: ***0.8320***

Run that I pick:
- run_name: ***rf-n_estimators=300***
- run_id: ***8d4555940fb343d98a9583df5cadcd99***

**Reason:**
Compared to its competitor in F1 it is better. Breaking down the ***Recall and Precision*** metrics in detail, it is better again, no trade-off. The other model (logreg-c=1.0) made more False Negatives (4 more, checked in their confusion matrix), which is an important aspect in this domain, and helps opting for my current choice. Also, ROC-AUC picks generally the better, but the Recall and Precision metrics are aligned with a basic safe 50% threshold, so practically better/safer in this domain I believe.

## Exercise 6

