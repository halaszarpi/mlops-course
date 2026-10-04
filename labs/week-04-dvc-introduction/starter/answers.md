# **Exercise 2**

## Q1:
Git gained a pointer a file (about 112 bytes) that describes the data version with its descriptors (hash, hash algo, size, path).

Silo gained the snapshot of the data (about 19 KBs), with an ETAG (this is the hash).

This difference is the idea, because this way we can store small files in commits and repos that point to larger, separately managed data files, and version them.

## Q2:
The *build_measurements* function concats the frames in order of their batch names (1) without considering their original indexes (2). Keeping the original indexing would break it.

## Q3:
Needs to run:

`dvc pull`

Conditions:

(1) The actual bytes should be pushed in the Silo before it can be pulled (other teammate's responsibility).

(2) This teammate has the exact keys to the Silo, and the Silo should be up, reachable.

# **Exercise 4**

## Q1:
The `git checkout` changed the measurements.csv.dvc file's content. 

The  `dvc checkout` copies the bytes from the local cache based on the *measurements.csv.dvc* file.

Neither can do the other's job, because the `dvc` only knows what's inside the *measurements.csv.dvc* file (and works based on it) and does not know the commit hsitory. The `git` doesn't know the file's bytes (never commited because of data/.gitignore). 

## Q2:
Fails, no v1 in the local cache (successful `git checkout`, unsuccessful `dvc checkout`).

# **Exercise 5**

## Q1:

Dependencies:
- models/model.pkl
- models/mlflow_run_id.json
- data/processed/test.csv
- src/week_04_dvc_introduction/pipeline.py
- src/week_04_dvc_introduction/model.py
- src/week_04_dvc_introduction/data.py

If the *test.csv* file is missing, the evaluation stops, because the model cannot be tested.

Baseline metrics:
- accuracy: 0.7344
- f1: 0.5785

And `make metrics` scored:
- accuracy: 0.7552
- f1: 0.5913

The reason the metrics differ is that the rows' order are different due to the batching.

# **Exercise 6**

## Q1:
DVC md5 (32 chars): the raw bytes of the whole `measurements.csv` file, the same value as in the pointer
and in the object path in Silo. 

MLflow digest (8 chars): a short hash MLflow computes over the
in-memory DataFrame it was given (here the train split, after parsing), not over the file.

For an auditor I would give the DVC md5. It names exact bytes, and `dvc pull` can fetch them back.
The MLflow digest is for convenience inside MLflow, it shows at a glance in the UI whether two runs
saw the same dataset input.

## Q2:
Filter: `tags.dvc_md5 = '<md5>'`.
It answers "which runs trained on this exact data version?". Week 3 only knew a file path, so this question could not be asked.

## Q3:
New line: `5. Data version: <md5>` (plus `Data location: s3://...`).
The chain now reaches the bytes: alias → version → run → git commit + data md5 → `dvc pull`.
It also only works if the version was `dvc push`-ed.
