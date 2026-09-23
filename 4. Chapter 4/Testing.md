
### Hypothesis Testing

*Statistical method used to make decisions or inferences about population parameters based on simple data*

Five-step process:
- State null and alternative hypotheses
- Choose a significance level (a)
- Collect and analyze data
- Make a decision based on the P-value
- Interpret results with emphasis on "failing to reject" rather than "accepting" the null

#### Null and alternative hypotheses
- The **null hypothesis** (H_0H0): This is the default assumption that there is no effect or no difference.
- The **alternative hypothesis** (H_a Ha): This is what we want to test for  -- it represents a new effect or a difference.

For example, suppose a manufacturer claims that the average weight of its cereal boxes is 500 grams. We might set:

H_0 H0: The mean weight is 500 grams.
H_a Ha: The mean weight is not 500 grams.

The general steps in hypothesis testing are:

1. **State the hypotheses** (H0​ and Ha​).
    
2. **Choose a significance level** (α), often 0.05.
    
3. **Collect and analyze the data** — compute the test statistic from the sample.
    
4. **Make a decision**: Compare the p-value to α. If p-value ≤ α, reject H0​; otherwise, fail to reject H0​.
    
5. **Interpret the results** in the context of the problem.

A **p-value** is the probability of observing a test statistic at least as extreme as the one obtained, assuming the null hypothesis is true. A small p-value indicates that the observed data is unlikely under H_0 H0​, providing evidence against it.

It’s important to note that failing to reject H_0 H0​ does not mean H_0 H0​ is true; it simply means we don’t have strong enough evidence to conclude otherwise.

There are two common types of errors in hypothesis testing:

- **Type I error**: Rejecting H0​ when it is actually true. The significance level α represents the probability of making this error.
    
- **Type II error**: Failing to reject H0​ when it is actually false. The probability of avoiding this error is called the **power** of the test (1−β).


### Significance Tests

Two types of tests:
- Comparing Means
- Comparing Proportions

#### Comparing Means:

If we have two independent samples and we want to compare their means, we can use a **two-sample t-test**. If the population variances are unknown but assumed equal, the test statistic is:

t=sp​n1​1​+n2​1​​x_bar_1​−x_bar_2​​

where sp​ is the pooled standard deviation:

sp​=n_1​+n_2​−2(n_1​−1)s_1^2​+(n_2​−1)s_2^2​​​

If variances are not assumed equal, we use Welch’s t-test, which adjusts the degrees of freedom.

In Python, we can use SciPy’s `ttest_ind()` function:

python

```
from scipy.stats import ttest_ind 
t_stat, p_value = ttest_ind(sample1, sample2, equal_var=True) # or False for Welch's test
```

#### Comparing Proportions

For comparing proportions, we use a **two-proportion z-test**. Suppose we have two proportions p^​1​ and p^​2​ from sample sizes n1​ and n2​. The pooled proportion is:

p^​=n1​+n2​x1​+x2​​

The test statistic is:

z=p^​(1−p^​)(n1​1​+n2​1​)​p^​1​−p^​2​​

In Python, we can perform a two-proportion z-test using the `proportions_ztest()` function from `statsmodels`:

python
```
from statsmodels.stats.proportion import proportions_ztest 


count = np.array([x1, x2])
 
# number of successes 
nobs = np.array([n1, n2])
 
# number of trials 
stat, pval = proportions_ztest(count, nobs)
```

After computing the p-value, we compare it to the chosen α level and make a decision.

To summarize:

- Hypothesis testing allows us to make inferences about population parameters.
    
- P-values help us determine whether the evidence is strong enough to reject H0​.
    
- Type I and Type II errors are key considerations in interpreting results.
    
- Python provides functions for performing t-tests and z-tests to compare means and proportions.