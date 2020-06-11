[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

```python
import scipy.stats

# generate distribution of male height in US population
mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)

# convert height range from feet to cm
def feet_inch_to_centimeter(feet, inches):
    total_inches = 12*feet + inches
    cm_per_inch = 2.54
    return round(total_inches * cm_per_inch, 2)

height_lower = feet_inch_to_centimeter(5, 10)
height_upper = feet_inch_to_centimeter(6, 1)

# calculate percentage of US males in this range by subtracting the CDF
# calculated at each height
percentage_in_range = dist.cdf(height_upper) - dist.cdf(height_lower)
```

The percentage of males in the US population between 5'10" and 6'1", and
therefore fulfilling the height requirements of the Blue Man Group, is
`34.275%`.
