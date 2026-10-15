# **Exercise 2**

## Q1

I would drop it before validation, since if it is
a free text, it may not contain any crucial information
for the classification task. Leaving it would only cause me
more work to write tests and write code for extending the data
contract. The clinic and the devs should come to an agreement.

## Q2

Glucose.

The corresponding series (column) contains an "unknown" value,
therefore the Pandera cannot convert it to float correctly, so
the whole column stays as it was read, strings.

The lazy mode allows to the validation to continue, however,
the "string" to "int" boundary comparisons are not supported in Python, that's why there are two more rows saying "TypeError".

# **Exercise 4**

## Q1

Without the gate it would reach all stages (validate, prepare, train, evaluate). So the model would use the data for training, thinking that the `age=250` row is normal.

## Q2

The prepare fails, as the validation part (`_require_validated(settings)` function call) inside the **pipeline.py prepare() function** makes sure there's no training run on a dataset, that did not "pass".

The edge is for the `dvc repro`, so running by hand hijacks this dependency.

## Q3

Stages after settint `MAX_AGE=125`:
- validate RAN
- prepare DID NOT RUN
- train RAN
- evaluate RAN

Same after setting it back to 120. Running the same training does not require to prepare anything, since there are no artifact changes (dataset, model params). The MAX_AGE=125 is more permissive than the 120, so some of the rows exceeding age=120 would stumble on validation, however, there were no rows exceeding even 120, that's why increasing it with 5 more years did not make any difference it the reproducing run.  