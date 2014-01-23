# Drought Occurance Proportion Tests

Data is binomial categorization of drought (drought year = 1, non-drought year = 0) in California from 1906 - 2014, with 2014 being assigned either a 1 or 0 value to see if classification as drought or non-drought changes statistical test results.

Chosen year groupings are the 20th century (1906-1999) and 21st century (2000-2013, or 2000-2014).

Interpretation of results actually changes depending on the test used, and are limited by way of the 21st century group being much smaller in sample size. 

My interpretation of the binomial test is slightly different than what appeared in the Excel sheet, but they should be the same test, and looks like results are as well...




## Exact Binomial Test

This test compares a single group (e.g. the 20th or 21st century year classifications) to a hypothetical value. It tests the statistical significance of deviations from a theoretically expected distribution of observations into two categories (drought or non-drought). This is basically testing against some 'expected' proportion of times you expect to see drought, which here we assume to be 50/50 - the 'null hypothesis' is that drought is as likely as no drought. This test is ok for small sample sizes.




For the 20th century group (1906-1999), the results are that we reject the null hypothesis, with a p-value < 0.05.


```
## 
## 	Exact binomial test
## 
## data:  sum(data[i20, "Drought"]) and length(i20)
## number of successes = 29, number of trials = 94, p-value =
## 0.0002617
## alternative hypothesis: true probability of success is not equal to 0.5
## 95 percent confidence interval:
##  0.2173 0.4122
## sample estimates:
## probability of success 
##                 0.3085
```


The interpretation is: we reject the null hypothesis, meaning drought years are less likely to occur in the 20th century than expected; our expectation in the way we set up the test is that drought will occur half the time... which is not a realistic expectation, but provides a way to compare to the 21st century).

For the 21st century group (2000-2013), the results are that we fail to reject the null hypothesis, with a p-value > 0.05.



```
## 
## 	Exact binomial test
## 
## data:  sum(data[i21, "Drought"]) and length(i21)
## number of successes = 6, number of trials = 14, p-value = 0.7905
## alternative hypothesis: true probability of success is not equal to 0.5
## 95 percent confidence interval:
##  0.1766 0.7114
## sample estimates:
## probability of success 
##                 0.4286
```


The interpretation is: we fail to reject the null, meaning drought years occur just as frequently in 21st century as non-drought years. This can be compared to the 20th century, but results are not really reliable as the sample size for the 21st century group is much smaller.

Adding in 2014 as either a drought (value = 1) or non-drought (value = 0) does not change the results: for the 21st century group, test results suggest drought years are just as likely to occur as non-drought years.

I also tried this with an expected probability of 0.3 (expectation that drought year proportion is 30%) [what you calculated in Excel]; in that case, we fail to reject the null for all year groupings [different than the results you got - I'm assuming because I'm using R, different calculation or test??]. Regardless of the classification of 2014, the test shows that the data do not statistically diverge from the assumption of drought years happening 30% of the time.

## Fisher Test

This is a contingency table test, meaning for a table of year groups (20th and 21st century groups) and classifications (drought year, non-drought year), it asks: are the levels of the row variables (year groups) diferentially distributed over levels of the column variables (drought classification). So, it's a more direct comparison BETWEEN the two year groups, instead of comparing each century, independnetly, to some theoretical expectation.

The Fisher test is actually a test of a null hypothesis of independence of rows and columns (as in the contingency table described above). However, the null of conditional independence is equivalent to the hypothesis that the odds ratio equals one. This is used when sample sizes are small (as opposed to a Chi Squared Test)

Here, the null hypothesis is that odds ratio is equal to 1, meaning that drought proportions in each year group are the same.




The Fisher test results for comparison of 20th and 20th century groups (not including 2014) is that we fail to reject the null of (essentially) no-difference between the groups, with a p-value > 0.05.



```
## 
## 	Fisher's Exact Test for Count Data
## 
## data:  tb
## p-value = 0.375
## alternative hypothesis: true odds ratio is not equal to 1
## 95 percent confidence interval:
##  0.1644 2.2925
## sample estimates:
## odds ratio 
##     0.5979
```


Results fail to reject the null hypothesis, therefore, the results do not provide any evidence against the assumption of independence, which means the test suggests you cannot claim any difference in the proportions of drought occurance between the two year groups.

Including 2014, either as a drought or non-drought year, does not change the results, only provides slightly more weak or strong evidence (in the form of sample estimated odds ratios and p-values) for accepting the null hypothesis of no difference.

### Different year groupings

If I decide to split the year groupings into equal groups: 1906-1959 and 1960-2013 (or 1960-2014), the results instead suggest a weak difference between the year groups. The test results suggest that the odds ratio (proportion of droughts in each group) are no longer statistically equivalent; drought in the latter period may be more likely.




The Fisher test result for comparison of 1906-1959 and 1960-2013 century groups (not including 2014) is that we reject the null of no-difference between the groups, but only with a p-value < 0.1.


```
## 
## 	Fisher's Exact Test for Count Data
## 
## data:  tb
## p-value = 0.09934
## alternative hypothesis: true odds ratio is not equal to 1
## 95 percent confidence interval:
##  0.184 1.135
## sample estimates:
## odds ratio 
##     0.4646
```


This means the test rejects the null hypothesis (of equal proportions) at the 90% confidence level only. Including 2014 as a drought or non-drought year does not change this result, only makes the p-value slightly less or more significant, respectively; the p-values for both remain at the 90% level, however.

In this alternate year grouping, the confidence interval around the odds interval is also much smaller (more reliable estimate).




