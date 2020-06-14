[Think Stats Chapter 9 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2010.html#toc90) (resampling)

```python
import thinkstats2
import numpy as np
import first

# this is the class that I need to inherit from
class DiffMeansPermute(thinkstats2.HypothesisTest):

    def TestStatistic(self, data):
        group1, group2 = data
        test_stat = abs(group1.mean() - group2.mean())
        return test_stat

    def MakeModel(self):
        group1, group2 = self.data
        self.n, self.m = len(group1), len(group2)
        self.pool = np.hstack((group1, group2))

    def RunModel(self):
        np.random.shuffle(self.pool)
        data = self.pool[:self.n], self.pool[self.n:]
        return data

# create a new class called DiffMeansResample, that inherits from DiffMeansPermute, and overrides RunModel
# to implement resampling
class DiffMeansResample(DiffMeansPermute):

    def RunModel(self):
        group1 = np.random.choice(self.pool, self.n, replace=True)
        group2 = np.random.choice(self.pool, self.m, replace=True)
        return group1, group2

        def perform_diff_mean_resample_test(data1, data2):
            """Data 1 and Data 2 are two pandas.Series or arrays to compare."""
            ht = DiffMeansResample((data1, data2))
            p_value = ht.PValue(iters=3000)
            return p_value

        def perform_diff_mean_permute_test(data1, data2):
            """Data 1 and Data 2 are two pandas.Series or arrays to compare."""
            ht = DiffMeansPermute((data1, data2))
            p_value = ht.PValue(iters=3000)
            return p_value

# load data
live, firsts, others = first.MakeFrames()

# clean data and compare test methods on birth weight
firsts_weights = firsts.totalwgt_lb.dropna().values
others_weights = others.totalwgt_lb.dropna().values
p_value_weight_with_resample = perform_diff_mean_resample_test(firsts_weights, others_weights)
p_value_weight_with_permute = perform_diff_mean_permute_test(firsts_weights, others_weights)
print(f'Birth Weight - P Value with Resample: {p_value_weight_with_resample}')
print(f'Birth Weight - P Value with Permutation: {p_value_weight_with_permute}')

# compare test methods on preg length
firsts_preg_length = firsts.prglngth.values
others_preg_length = others.prglngth.values
p_value_length_with_resample = perform_diff_mean_resample_test(firsts_preg_length, others_preg_length)
p_value_length_with_permute = perform_diff_mean_permute_test(firsts_preg_length, others_preg_length)
print(f'Preg Length - P Value with Resample: {p_value_length_with_resample}')
print(f'Preg Length - P Value with Permutation: {p_value_length_with_permute}')
```

The resulting p-values using both the permutation and resampling methods on
birth weight and pregnancy length are as follows:

```
Birth Weight - P Value with Resample: 0.0
Birth Weight - P Value with Permutation: 0.0003333333333333333
Preg Length - P Value with Resample: 0.17066666666666666
Preg Length - P Value with Permutation: 0.174
```

The models do not appear to affect the results in any significant way.
