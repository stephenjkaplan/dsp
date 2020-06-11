[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

```python
import nsfg
import thinkstats2
import thinkplot

def bias_pmf(pmf, label):
    """Calculates the biased probability mass function."""
    new_pmf = pmf.Copy(label=label)

    for x, p in pmf.Items():
        new_pmf.Mult(x, x)

    new_pmf.Normalize()

    return new_pmf

# load NSFG respondent dataset
resp = nsfg.ReadFemResp()

# calculate actual and biased probability mass functions of number of children
# under 18 in each family
pmf = thinkstats2.Pmf(resp['numkdhh'], label='actual')
pmf_biased = bias_pmf(pmf, label='observed')

# plot the actual and biased PMF
thinkplot.PrePlot(2)
thinkplot.Pmfs([pmf, pmf_biased])
thinkplot.Config(xlabel='Children Under 18 in Family', ylabel='PMF')

# calculate the means of the actual and biased PMF
print('Actual mean', pmf.Mean())
print('Observed mean', pmf_biased.Mean())

```
The resulting plot is:

![plot](dsp/lessons/statistics/images/bias.png)

The actual mean is `1.0242` and the biased mean is `2.4037`.

Assuming you are surveying children under 18 about their families,
the biased (observed) mean is higher than the actual mean for the number of
children under 18 in a particular family. This is because you are more likely
to survey a child from a larger family.
