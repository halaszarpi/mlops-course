# Exercise 2

## Q1

Prefect: 
    - run history, run duration, logs from tasks
    - retries with delays
    - scheduled and parameterized runs

DVC:
    - due to content-based hashing only the changed stages need to rerun
    - also model and data versions contribute to totally reproducible runs

## Q2

Both of dvc and prefect graphs would contain the stages, redundant.  

# Exercise 5

Scheduled run: brown-earthworm (flow diabetes-training-with-pull, deployment weekly-retrain) — Completed.

## Q1

No, gate isn't enough. Human-check is necessary and also we need to check data-drift.

## Q2

Things to notify: a NO-GO run, an infra error (for example network), uncommited params like in exercise 4.

Who to notify: The team, the infra team (or operations or whatever it's named), the dev.

## Q3

After a second failure in a row, the schedule should be reconsidered, people need to be notified via
tickets for example. The schedule can be stopped to avoid a sequence of failures.

## Q4

If the Linux cron job itself did not run. We have to somehow monitor that as well. Or another thing could be a GO-run, which was done on drifted data, where nothing's wrong technically, but semantically the data is too outdated to be trained on.