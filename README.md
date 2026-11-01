Can one-month changes in employment across US industry sectors predict whether total non-farm employment will rise or fall in the following month? 
Aim
- Is it possible to predict whether overall non-farm employment will increase or decrease in the next month based on one-month changes in employment across US industry sectors? Gaussian Naive Bayes and a decision tree are two supervised classifiers that I compared to a majority-class baseline.

Data
- Monthly US employment statistics (120 months, 24 columns, no missing numbers) from January 2006 to December 2015
- Target: 1 if the nonfarm_change for the following month is more than 0, else 0.
- The one-month change (.diff()) in each of the 11 non-overlapping sectors is one of the features.
- 118 observations remained after the first row (no previous month) and last row (no future month) were eliminated."Rise" months made up 75.4% of the total.

Method
- Training took place from February 2006 to December 2008 (35 months), and testing began in January 2009 (83 months). The model would be able to learn from the following months with a random split.
- Why 2009? Given all 29 'autumn' months fall between June 2007 and August 2010. A test set with only raises would result from a later cut-off, like 2012.
- Baseline: consistently predicting "rise" scores 81.9% of the test set.
- Gaussian Naive Bayes: makes use of Bayes' theorem, assuming that features are independent and modelling each feature as a normal distribution within each class.
- To prevent overfitting, decision trees are restricted to a depth of three. Accuracy alone ignores how well the unusual outcome is identified, therefore metrics for falls include recall and precision in addition to accuracy.

Results
Model	                 Accuracy	          Falls caught (recall)	                Warnings correct (precision)
Always “rise”	            0.82	                   0.00	                                0.00
Naive Bayes	                0.87	                   0.87                                 0.59
Decision tree	            0.90	                   0.53                                 0.89

- Naive Bayes caught 13 of 15 falls but gave 9 false alarms.
- 8 out of 15 falls were detected by the decision tree, with one false alert. Professional_and_business_services_chg = -13 was its initial split; a fall followed each of the 11 training months below that cutoff.
- Overfitting check:

Max depth	                                     Training accuracy	                    Test accuracy
1	                                                    0.914	                            0.916
2	                                                    0.914	                            0.916
3	                                                    0.971	                            0.904
None	                                                1.000	                             0.904

- Training accuracy rose to 100% while test accuracy did not improve, which indicates overfitting. A single split already achieved 91.6%.

Discussion
- Although by narrow margins, both models outperform the baseline: Naive Bayes gains 4 extra months out of 83, while the tree gains 7.
- The models make different trade-offs. Naive Bayes is better for early warning applications when it is expensive to miss a downturn. The tree is expensive if false alerts are costly.
- The greatest indication was a decline in employment in business services, indicating that job losses in certain industries anticipate those in the overall economy.
- When sectors move together during a downturn, Naive Bayes' premise of independent features is broken. It continued to function fairly well.

Limitation
- About 35 of the 118 months in the training set and 15 of the "fall" months in the test set make up the small sample. A few months' difference would significantly alter the outcomes.
- Every autumn falls within the 2007–2010 recession; therefore , the models may be picking up on that experience rather than a general norm.
- Single split: I used the 2009 cut-off to ensure that both outcomes were present in the test set.
- Test set used for comparison: The 0.904 to 0.916 differences are not an objective prediction of future performance because I compared tree depths on the test set.
- No cross-validation or tuning: A time-series cross-validation would provide a more accurate estimate.

Conclusion
- The small sample size and one recession limit the data, but sector-level job changes do provide some insight into the direction of the upcoming month, primarily through business services. Rather than being definitive, the results are encouraging.