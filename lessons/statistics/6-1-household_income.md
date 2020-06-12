[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

```python
def InterpolateSample(df, log_upper=6.0):
    """Makes a sample of log10 household income.

    Assumes that log10 income is uniform in each range.

    df: DataFrame with columns income and freq
    log_upper: log10 of the assumed upper bound for the highest range

    returns: NumPy array of log10 household income
    """
    # compute the log10 of the upper bound for each range
    df['log_upper'] = np.log10(df.income)

    # get the lower bounds by shifting the upper bound and filling in
    # the first element
    df['log_lower'] = df.log_upper.shift(1)
    df.loc[0, 'log_lower'] = 3.0

    # plug in a value for the unknown upper bound of the highest range
    df.loc[41, 'log_upper'] = log_upper

    # use the freq column to generate the right number of values in
    # each range
    arrays = []
    for _, row in df.iterrows():
        vals = np.linspace(row.log_lower, row.log_upper, row.freq)
        arrays.append(vals)

    # collect the arrays into a single sample
    log_sample = np.concatenate(arrays)
    return log_sample

def RawMoment(xs, k):
    return sum(x**k for x in xs) / len(xs)

def Mean(xs):
    return RawMoment(xs, 1)

import hinc
import thinkstats2
import thinkplot

# read data
income_df = hinc.ReadData()

# interpolate and convert to log10
log_sample = InterpolateSample(income_df, log_upper=6.0)

# generate CDF of interpolated sample
log_cdf = thinkstats2.Cdf(log_sample)
thinkplot.Cdf(log_cdf)
thinkplot.Config(xlabel='Household income (log $)', ylabel='CDF')

# convert back to non-log
sample = np.power(10, log_sample)

# generate CDF of resulting sample
cdf = thinkstats2.Cdf(sample)
thinkplot.Cdf(cdf)
thinkplot.Config(xlabel='Household income ($)', ylabel='CDF')

# calculate mean and median
print(f'Mean: {Mean(sample)}')
print(f'Median: {Median(sample)}')

# calculate the skewness and Pearson median skewness
print(f'Skewness: {Skewness(sample)}')
print(f'Pearson Median Skewness: {PearsonMedianSkewness(sample)}')

# calculate percentage of population less than Mean
print(f'Population below mean income: {cdf.Prob(Mean(sample))}')
```

The mean is `74278.7075` and the median is `51226.4545`. This alone is an
initial indication that the data skews right.

The skewness is `4.9499` is the Pearson median skewness is `0.7361`, both of
which are positive, also indicating a right skew.

The percentage of the population that have less income than the mean is `66%`.

This analysis assumes that the upper limit of income is 1 million. However, we
know that a small amount of the population has income ranging from 1 million to
over 100 billion dollars. Therefore, the higher the upper bound you consider in
the analysis, the longer the distribution's right tail would be, therefore
increasing the skew. 
