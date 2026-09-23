## Exercise 4:

Three things to store about a run: code, metadata and artifacts. 

These things have different planes, namely: **Git** (version-controlled), **Postgres** (relational DB) and **MinIO** (object store).

- Git stores: the code's text
- Postgres: metadata describing parameters, metrics, tags, runs
- MinIO: larger files (model file, plots etc.)

I think they are not stored in Postgres because of their size (BLOBs of multiple MBs, GBs), so it is not efficient to store them there, that is why we have the MinIO as the object store.

## Exercise 5:

***Now I can answer these:*** 

Which hyperparameter configuration produced the best test score? (no scroll-back in the terminal)

Which run is the one producing the best result and what do I need to reproduce it?

Where is the best resulting model's artifact stored?

How do two or multiple runs differ from each other (params, metrics etc.)?