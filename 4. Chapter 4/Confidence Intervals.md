But point estimates alone can be deceiving because they don't provide any information about the estimates precision or reliability.

That's where confidence intervals come into play.

#### Definition
-  A range of values derived from the sample data and likely contains the true parameter.

Point estimate:
- Gives a specific value that could represent the population parameter

Confidence intervals:
- gives a range of possible values that the parameter could take


Y = (Q_1 + Q_3) / 2

This gives us our average that we can form a confidence interval around this point estimate to give us a range that we believe (with a certain level of confidence) contains the true center of the distribution.

y +/- (critical value x standard error)
## Point Estimate
Provide a single value that serves as a good guess for the unknown parameter of interest.
Y = (Q_1 + Q_3) / 2

e.g.
x_bar -> mu

## Critical Values

-inf < z < inf
mu + z(sigma) = 0 + z(1) = z

z at 2 standard deviations (2sigma) = +/- 1.96 which equals to 95% confidence level

This critical value is a factor that adds or subtracts from the point estimate and we choose it based on the desired level of the confidence level

#### Reminder
- *The standard error measures the statistical accuracy of the estimate (dispersion of the point estimate if we drew multiple samples from the population)

When we calculate the confidence intervals we end up with a lower and upper limit. This means that if we were to take many samples and create confidence intervals in the same way, approximately 95% (Depending on chosen confidence level, 95% is used here because that was the example given in the video) of the intervals would contain the true population parameter and this gives us a sense of the reliability of our point estimate.

A wider confidence interval at a given confidence level suggests a larger degree of uncertainty about the point estimate.

A narrower confidence interval, we have a higher degree of precision

x_bar +/- z (s/sqrt(n)) if confidence level is 95% then z = 1.96

1. calculate sample mean (x_bar) and standard deviation(s)
2. determine our desired confidence level (not interval) and z-value
3. margin of error (z(s/sqrt(n))
4. subtract or add the margin of error from our sample mean (x_bar) so x_bar +/- z (s/sqrt(n))

#### Reminder
- *Width of confidence interval gives a measure of the precision of our point estimate
- *Confidence level gives a measure of certainty
- *Trade-off between precision and certainty
- *Increasing certainty (higher confidence level) -> lower precision
- *lower precision -> wider confidence interval*

## Central Limit Theorem

With a large enough n (independent identically distributed random variables) the distribution of the means will be approximately normally distributed (bell-curve) and it doesn't matter the shape of the original distribution.

Large n is n >= 30


## Likelihood Function

Likelihood Principle: all information in the data about the parameter we're interested in is contained in the likelihood function

Likelihood function is used to estimate the parameters of a model that we base on observed data. (probability of getting the observed data for different model parameters)

i.e. model with parameters theta, n
L(theta | n) = P(n | theta)

## Maximum Likelihood Estimation (MLE) (This means most likely)

theta_hat = arg max_theta L(theta | n)
  ^
MLE

arg max = value of theta that maximizes the likelihood function of L(theta | n)

Example using python:

```
import numpy as np

# True parameter value
true_prob = 0.7

# Generate some binary data with a known probability
np.random.seed(0)
data = np.random.choice([0, 1], size = 100, p = [1 - true_prob, true_prob])

# Perform maximum likelihood estimation
estimated_prob = max(np.mean(data), 1 - np.mean(data))

# Print the true and estimated parameter values
print(f"True Probability: {true_prob}")
print(f"Estimated Probability: {estimated_prob}")
```

## Confidence Intervals for Means of Normal Populations

```
import numpy as np
from scipy.stats import norm

# Generate some normally distributed data
np.random.seed()
data = np.random.normal(loc=5, scale=2, size=100)

#Calculate sample statistics
sample_mean = np.mean(data)
sample_std = np.std(data, ddof=1) # ddof=1 for unbiased estimate
sample_size = len(data)

# Set the desired confidence level (e.g., 95%)
confidence_level = .95

# Calculate the critical value (based on the standard normal distribution)
z_critical = norm.ppf(1 - (1 - confidence_level) / 2)

# Calculate the standard error
standard_error = sample_std / np.sqrt(sample_size)

# Calculate the margin of error
margin_of_error = z_critical * standard_error

# Calculate the confidence interval
confidence_interval = (sample_mean - margin_of_error, sample_mean + meagin_of_error)

# Print the confidence interval
print(f"Confidence Interval: {confidence_interval}")
```

## Comparing Population Means

pivotal quantity: function of the data and the unknown parameter and the unknown parameter actually has a known distribution

(X_bar_1 - X_bar_2) +/- Z * sqrt(((s_1^2) / n_1) + ((s_2^2) / n_2))

## The Bootstrap

X = {x_1, x_2, ... , x_n}

get b samples, each sample has n size

n from X w/ replacement

Each each sample is represented with theta_b

```
import numpy as np
import matplotlib.pyplot as plt

# Generate sample data
np.random.seed(42)
data = np.random.normal(loc=10, scale=2, size=100)

# Number of bootstrap interations
n_bootstrap = 200

#Perform bootstrap process
bootstrapped_means = []
for _ in range(n_bootstap):
	bootstrap_sample = np.random.choice(data, size = len(data), replace = True)

	bootstapped_means.append(np.mean(bootstrap_sample))	
	
# Plot histogram of bootstrapped means
plt.hist(bootstrapped_means, bins=10, alpha=0.5)
plt.xlabel('Bootstrap Sample Mean')
plt.ylabel('Frequency')
plt.title('Distribution of Bootstrap Sample Means')

# Show the plot
plt.show()

```

## Bayesian Approach to Inference

Start with prior beliefs represented by the 'prior' (a distribution) from module 2. Lets make an assumption that we have beta(2, 2) prior distribution and this represents a relatively uniform prior since we have an equal weight on both heads and tails.

Conduct an experiment and observe the outcome of several coin tosses

Suppose we have 7 heads and 3 tails we want to update out beliefs using the likelihood function

This quantifies the probability of observing the data given different values of the parameters of interest

In our case this likelihood function is the binomial distribution.

We have a fixed num of coin tosses (10) and want to know the probability of getting 7 heads.

Mathematically this can be expressed as P(d | theta) = binom(7; 10; p) = (P (theta | d) = P(d | theta) x P(theta)) / P(d)

- p = probability of heads
- d = data
- P(theta | d) = posterior distribution
- P(d | theta) = Likelihood function
- P(theta) = Prior Distribution
- P(d) = Marginal Likelihood

```
import numpy as np
from scipy.stats import beta

# Prior parameters 
alpha_prior = 2
beta_prior = 2

# Data 
num_heads = 7
num_tails = 3

# Posterior parameters
alpha_posterior = alpha_prior + num_heads
beta_posterior = beta_prior + num_tails

# Compute posterior distribution
posterior_dist = beta(alpha_posterior, beta_posterior)

# Calculate point estimates
mean_estimate = posterior_dist.mean()
median_estimate = posterior_dist.median()

# Calculate credible interval
credible_interval = posterior_dist.interval(0.95)

# Print results
print("Posterior distribution parameters: alpha={}. beta={}".format(alpha_posterior, beta_posterior))
print("Mean estimate: {:.3f}".format(mean_estimate))
print("Median estimate: {:.3f}.format(median_estimate))
print("95% Credible interval: [{:.3f}, {:.3f}]".format(credible_interval[0], credible_interval[1]))
```