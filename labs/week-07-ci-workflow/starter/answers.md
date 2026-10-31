CI: https://github.com/halaszarpi/mlops-course/actions/runs/41230564207
CT: https://github.com/halaszarpi/mlops-course/actions/runs/41232015440

# Exercise 1

## Q1

The `autograde.yml` file is for automatically running the UV package manager installation, checking every lab's uv.lock is up-to-date and running every lab's tests.

The folders: *starter* folder inside every lab's repo folder.

# Exercise 5

## Q1

Both workflows run the same Makefile pipeline (`make sync`, `make data-v2`, repro and gate, `make gate-summary`, artifact upload), but `ci.yml` is triggered by a push whose diff a developer is waiting on and a reviewer approved, while `ct.yml` is triggered by a clock or a button with no code diff at all, so nobody is waiting for the result and the changed input is the data, which no one reviewed line by line.

## Q2

A GO only means the model passed the automated acceptance criteria on today's test split, which is necessary but not sufficient for a release, because a human still has to judge things the gate cannot see (why the data changed, whether the new behaviour is acceptable to the business and the affected groups, and whether to roll out gradually), so registering and promoting a decision.

## Q3

GitHub can start a scheduled run late or drop it entirely when Actions is under heavy load, and it silently disables `schedule:` triggers in a public repository after 60 days without repository activity, and in neither case does anyone get a notification.