# Exercise 2

## Q1

The lowest ROC-AUC group: age_band, 50+  
Its n: 19  
Its pos: 7  
Its fpr is 0.4167 (**0.3482** more than the 21-29 group).

The group with F1 0 and ROC-AUC 1: bmi_band, under_normal  
Its pos is 1.  

The model is worst on: age_band, 50+. Its worse with **0.2274** from the aggregate (0.8345).
The group's n is one less then 20 (the slice size), so I should trust it. 

## Q2
I could change the threshold, but in a reviewed commit (needs a reason, an approver), then update the criteria version.

## Q3
These numbers are all down to 1 patient with diabates, therefore not reliable.

# Exercise 4

## Q1

In Week 5 the gate worked because prepare listed the verdict file as a dependency, so DVC couldn't run anything downstream of a failed check. Here the gate is the last stage, and there's nothing after it in the pipeline that could depend on it. The actual release, register and promote, happens outside DVC, in the MLflow registry. So instead of a file dependency, those commands check the verdict themselves through require_go and refuse to do anything unless reports/gate.json says GO. On top of that, the gate exits with a non-zero code, which stops dvc repro.

## Q2

Then a NO-GO model goes into the registry without anyone noticing. The model file is still on disk after a failed gate, so make register just works. Nobody would catch it at release time. At best it shows up later in an audit or when someone traces the champion back to its gate report. At worst, the first people to notice are clinicians or patients, once the model starts missing diabetes cases in production.

## Q3

Neither metric is right on its own. It depends on how the model's output is used. If the predictions are turned into yes/no decisions at the fixed 0.5 threshold, for example to decide who gets sent for a follow-up test, then F1 is what matters and the drop is a real regression. If the model is used to rank patients by risk and someone picks the cutoff later, ROC-AUC is the better measure, and the new model is actually slightly better. To decide, you need to know where the threshold sits and how much a missed patient costs compared to a false alarm. Setting acceptance.regression.primary_metric means committing in advance, and in version control, to one metric deciding the regression check. You can't switch to whichever metric looks better once the numbers are in. If you do want to change it, that's a new set of criteria with a new criteria_version.

# Exercise 5

## Q1

The selection-rate gap on age_band reflects a real difference in base rates, not unfair treatment. Diabetes is much more common in older patients: 22% of the 21–29 group are positive, against 55% of the 40–49 group. A model that gets the risk right should flag older patients more often. Forcing equal selection rates (demographic parity) would mean either missing diabetic older patients or over-flagging healthy young ones. Age is a legitimate clinical risk factor. What we do want equal across age groups is the TPR, meaning a diabetic patient has the same chance of being caught whatever their age.

## Q2

The age groups have few positives each: 7 to 22. With 7 positives, one patient moves the TPR by about 0.14, so a tight limit would mostly measure noise. The insulin groups are larger, with 42 and 25 positives. Insulin being missing is also an artefact of how the data was collected, not a clinical factor, so the model has no good reason to behave differently there. I'd tighten the age limit once there is more test data per age band, so the confidence intervals get narrower, or if there were clinical evidence or complaints that missed diagnoses in a particular age group are especially harmful.

## Q3

This dataset contains only women of Pima Indian heritage aged 21 or over. We can't check how the model performs for men or for any other ethnic group, because they aren't in the data at all. Auditing this would have needed a more diverse population, with sex and ethnicity recorded at the time the data was collected, with consent and a legal basis, since these are special-category data under GDPR Article 9. That can't be added afterwards. It has to be planned before data collection starts.