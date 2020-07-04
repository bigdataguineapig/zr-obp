# Examples with OBD
Here, we use the dataset and pipeline to implement and evaluate OPE.
We first evaluate well-known off-policy estimators with the ground-truth performance of a counterfactual policy.
We then select and use such an estimator to improve the platform’s fashion item recommendation.

## Additional Implementations

- [`dataset.py`](./dataset.py):
    We implement two ways of engeering original features.
    This includes **CONTEXT SET 1** (user features only, such as age, gender, and length of  membership) and **CONTEXT SET 2** (Context Set 1 plus user-item affinity induced by the number of past clicks observed between each user-item pair).

## Running experiments

**Example 1. Evaluating Off-policy Estimator**

We select the best off-policy estimator among Direct Method, Inverse Probability Weighting, and Doubly Robust.

```bash
python evaluate_off_policy_estimators.py\
    --n_boot_samples 10\
    --counterfactual_policy bts\
    --behavior_policy random\
    --campaign all

# relative estiamtion erros and their 95% confidence intervals of OPE estimators
# ==================================================
# random_state=12345
# --------------------------------------------------
#          mean  95.0% CI (lower)  95.0% CI (upper)
# dm   0.218148           0.14561           0.29018
# ipw  1.158730           0.96190           1.53333
# dr   0.992942           0.71789           1.35594
# ==================================================
```


**Example 2. Evaluating Counterfactual Bandit Policy**

We evaluate the performance of counterfactual policies based on logistic contextual bandit with OPE estimators.

```bash
python evaluate_counterfactual_policy.py\
    --context_set 1\ # a used context set for a counterfactual policy
    --counterfactual_policy logistic_ucb\ # a used counterfactual bandit algorithm
    --epsilon 0.1\ # an exploration hyperparameter
    --behavior_policy bts\
    --campaign men

# estimated policy values relative to the behavior policy (Bernoulli TS here) of a counterfactual policy (logistic UCB with Context Set 1 here) by three OPE estimators (IPW: inverse probability weighting, DM; Direct Method, DR: Doubly Robust)
# ======================================================================
# random_state=12345: counterfactual policy=logistic_ucb_0.1_1
# ----------------------------------------------------------------------
#      estimated_policy_value  relative_estimated_policy_value
# ipw                0.008000                         2.105263
# dm                 0.003898                         1.025915
# dr                 0.007948                         2.091689
# ======================================================================
```
