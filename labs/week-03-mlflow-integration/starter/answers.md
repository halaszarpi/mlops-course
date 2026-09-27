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

### Q1:
1. version = client.get_model_version_by_alias(name, alias): returns a ModelVersion object
2. run = client.get_run(version.run_id): returns the version's attribute, the run_id, fetches the run object
3. evidence = {"params": run.data.params, "metrics": run.data.metrics, "tags": run.data.tags}: returns the params, metrics and tags of the selected run as the underlying evidence
4. code_commit = run.data.tags.get("mlflow.source.git.commit"): returns the exact commit fromt the run's tags that I can 'git checkout' and see the code myself

### Q2:
Hop 4 in Part 2 gave me a git commit hash, but on its own, that hash is not
trustworthy evidence. If the working tree was dirty when the run was logged,
the commit does not fully represent the code that actually ran (uncommitted
changes are invisible to it). 

Recording git_dirty fixes this by telling me
whether I can trust the commit hash at all. It still doesn't give me the
actual diff of what was dirty if git_dirty=true.

### Q3:
My side: it should reject it, for the sake of traceability, since a promoted model
whose exact training code cannot be recovered is not a safe production artifact, no matter how good its metrics look, if there's an issue in the future, we cannot trace it back to the exact code that produced it.

Cost: the promotion slows down, no direct promotion from dirty code, which could cost time and computational resource if the training is costly. Losing the exact result could also be matter here, if the data or the ways of computing (or some libraries) changes in the meantime. 

### Q4:
1. Easier rollback to a previous model version, just change where the alias points to.
2. Multiple aliases can have different purposes, they are independent.

### Q5:
It means that this current data file contributed to these metrics. The problem is that data's version is not clear, so we would need logging for that as well, in order to be able to reproduce the run.

## Exercise 7

First rollback is rejected --> Version 1 has not been promoted.

### Q1:
What changed? Alias resolves to a different version (version 2), run_id and git_commit and metrics according to the trace.
What didn't change? The URI.

### Q2:
The promoted_at tag would tell the auditor, that version 3 was once a champion, but nothing else.

### Q3:
Not a safe target, since it hasn't got a 'git_dirty' tagging. So we cannot tell whether it has a clean state or not.
