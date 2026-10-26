# Diagnosing Underpowered Studies and P-Hacking

## Objective
I checked two flawed hypothesis tests, one that was too small to find a real effect and one that picked a lucky result out of many tests, and fixed both.

## Methodology
- Ran a t-test on a pilot study with 15 people per group and checked its power against the true effect.
- Used a power analysis to find how many people per group are needed for 80% power, then reran the study at that size.
- Applied a Benjamini-Hochberg correction to the p-values from 20 subgroup tests.
- Calculated the chance of at least one false positive in 20 tests, and checked it with a simulation of 1,000 replications.
- Wrote my own Benjamini-Hochberg function and checked it against statsmodels, then tested it on a version with two real effects planted.
- Compared four tests on heavy-tailed earnings data: Welch's t-test, Yuen's trimmed t-test, and two permutation tests.
- Saved a power analysis helper module as `src/power_utils.py`.

## Key Findings
- The pilot study had only 10.9% power against the true effect, so its non-significant result did not mean the program had no effect. 80% power needed 222 people per group, and at that size the test gave p = 0.0012.
- After the Benjamini-Hochberg correction, 0 of the 20 subgroups stayed significant, so the "significant" subgroup was a false positive.
- When all 20 nulls were true, 65.8% of 1,000 replications had at least one p < 0.05, against 64.2% in theory.
- My Benjamini-Hochberg function matched statsmodels, and with two real effects planted it flagged 2 subgroups.
- On the heavy-tailed earnings data, the four tests split by the statistic tested, not by parametric versus permutation: the two tests on trimmed means agreed with each other, and the two tests on regular means agreed with each other.
