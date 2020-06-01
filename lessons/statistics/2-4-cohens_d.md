[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

```python
import nsfg
import math

def calculate_cohen_effect_size(group1, group2):
    """Calculates Cohen's effect size. The two inputs should be both Series or both DataFrames."""
    mean1 = group1.mean()
    mean2 = group2.mean()
    variance1 = group1.var()
    variance2 = group2.var()
    n1, n2 = len(group1), len(group2)
    pooled_variance = (n1*variance1 + n2*variance2)/(n1 + n2)
    pooled_standard_deviation = math.sqrt(pooled_variance)

    d = (mean1 - mean2) / pooled_standard_deviation
    return d

# load dataset and filter only data for live births
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]

# filter live birth data into two groups: those related to a first pregnancy, and all others
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]

cohen_effect_size_totalwgt = calculate_cohen_effect_size(firsts.totalwgt_lb, others.totalwgt_lb)
cohen_effect_size_pregnancy_length = calculate_cohen_effect_size(firsts.prglngth, others.prglngth)

print(cohen_effect_size_totalwgt)
print(cohen_effect_size_pregnancy_length)
```

The Cohen's effect size calculated for total weight between live births
resulting from a first pregnancy ("firsts") and live births resulting from all
other pregnancies ("others") is `-0.08867`. This would indicate that "firsts"
are born with a lower weight than "others", but only by 0.0887 standard
deviations, which is small.

When Cohen's effect size is calculated for the same two groups, but for
pregnancy length, the results is `0.02888`. This would indicate that "firsts"
are born after longer pregnancies than "others", but only by 0.02888 standard
deviations which is also quite small.

In both cases, the differences are trivial.
