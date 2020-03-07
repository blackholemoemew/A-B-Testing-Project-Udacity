# A-B-Testing-Project-Udacity
## 1.Experiment Overview: Free Trial Screener
Thanks to Udacity to offer such a great free online course about A/B Testing, which provided me the opportunity to get hands-on experience in practicing real-world A/B testing. This experiment is the final project of the course.

The experiment was run by Udacity to test the impact of adding a step to distinguish users by the time that they have time to devote to the course (more or less than 5hrs) before processing to checkout for free trial. In this way, the proportion of students who have a greater tendency to complete the course will be larger in the students who enrolled in the paid version of course. The business goal of the experiment is to maximize course completion and to most efficiently use limited coaching resources. 

### 1.1 Current Condition
At the time of this experiment, Udacity courses currently have two options on the page of course overview page: “Start Free Trial”, and “Access Course Materials”.

If the student clicks "Start Free Trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first.

If the student clicks "Access Course Materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

### 1.2 Condition After Change
In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course.

If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual.

If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.

## 2.	Experiment Hypothesis
The hypothesis was that this change might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

Null hypothesis: the change causes no significant difference to the number of students that past the free trial and eventually complete the course.

## 3.	Experiment Design
### 3.1	Explaining on the [funnel](https://drive.google.com/file/d/1xxuEJBjKOlU24Cez000Z0n8VpTtiyvDF/view?usp=sharing)
-	Course overview page visits: unique cookies

-	Click the “Start Free Trial” button: unique cookies

-	Complete checkout and enrolled in the free trial: user-id

-	Cancel the payment before the free trial ends: user-id

-	Remain enrolled after 14-day free trial: user-id

-	Complete the course: user-id

### 3.2 Unit of Diversion
The unit of diversion is how we define what an individual subject is in the experiment. In this case, the unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.
### 3.3 Metric Choice
Since it’s the first time to try the change, it’s better to run an A/A test simultaneously to help us debug the experiment setup. So, we need two kinds of metrics here. The first kind is invariant metrics and used for A/A testing. They are expected to have a similar distribution in both control and experiment groups. Another kind is evaluation metrics. We expect to see a change in evaluation metrics and set a minimum difference (Dmin), that is, the difference that would have to be observed before that was a meaningful change for the business for each metric. We can see from the funnel that some metrics keep unchanged and some metrics are expected to change after the new method. It’s easy to pick the metrics from the funnel.

Any place "unique cookies" are mentioned, the uniqueness is determined by day. (That is, the same cookie visiting on different days would be counted twice.) User-ids are automatically unique since the site does not allow the same user-id to enroll twice.

#### 3.3.1 Invariant Metrics
- Number of cookies: The number of unique cookies to view the course overview page.

-	Number of clicks: The number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger).

-	Click-through-probability: The number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page.

#### 3.3.2 Evaluation Metrics
-	Gross conversion: The number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (Dmin= 0.01)

-	Retention: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (Dmin=0.01)

-	Net conversion: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (Dmin= 0.0075)

#### 3.3.3 Unselected Metrics
-	Number of user-ids: The number of users who enroll in the free trial.

This metric could change and be used as an evaluation metric, but it is not suitable. Because the number can’t directly give us an insight into the impact of the change on our business goal. (We don't just care about how many users enrolled in the free trial) We still need to convert it to other forms to help us evaluate.

#### 3.3.4 Baseline Values for These Metrics
The rough estimates of the baseline values for metrics provided by Udacity are listed in the table below. Attention, the numbers have been changed from Udacity's true numbers.
| Metric Name |	Description	| Estimates |
| ----------- | ----------- | :-------: |
| Number of cookies	| Unique cookies to view course overview page per day	| 40,000 |
| Number of clicks | Unique cookies to click "Start free trial" per day	| 3,200 |
| Number of user-ids | Enrollments per day | 660 |
| Click-through-probability, CTP | Click-through-probability on "Start free trial" | 0.08 |
| Gross conversion |Probability of enrolling, given click	| 0.20625 |
| Retention	| Probability of payment, given enroll | 0.53 |
| Net conversion | Probability of payment, given click | 0.1093125 |
### 3.4 Measuring Standard Deviation
#### 3.4.1 Analytical Estimates or Empirical Estimates?
For each evaluation metric, we need to estimate its standard deviation analytically. The project instruction gives us a sample size of 5,000 cookies visiting the course overview pages to calculate the analytical standard deviation. 
| Valuation Metrics	| Analytical Standard Deviation |
| ----------------- | :---------------------------: |
|Gross Conversion	| 0.0202 |
|Retention | 0.0549 |
|Net Conversion	| 0.0156 |

The unit of analysis is basically what the denominator of the metric is.

*Gross Conversion = (The  number of users to enroll in the free trial) / (The number of cookies clicking the free trial)*

*Net Conversion = (The number of users remain enrolled after 14 days free trial) / (The number of cookies clicking the free trial)*

In these two cases, the unit of analysis is cookies(click), which is the same as the unit of diversion. We expect the analytical standard deviation is close to the empirical estimates.

*Retention = (The number of users remain enrolled after 14 days free trial) / (The number of users to enroll in the free trial)*

In this case, the unit of analysis is the number of users who enrolled, which is not equal to the unit of diversion. So, an analytical estimate is not sufficient to use. We need to calculate the empirical standard deviation if we decide to use retention as an evaluation metric.

### 3.5 Sizing
#### 3.5.1 Number of Samples vs. Power
Because the three evaluation metrics are correlated and tend to move at the same time, in this case, the Bonferroni correction may be too conservative for our need. So, I will not use the Bonferroni correction during my analysis phase.

Use the online calculator to get the sample size. The outcomes are listed below:
| Inputs | Gross Conversion	| Retention	| Net Conversion |
| :----: | :--------------: | :-------: | :------------: |
| Baseline conversion rate | 20.625% | 53% | 10.931% |
| Minimum detectable effect	| 1% | 1%	| 0.75% |
| Alpha	| 0.05 | 0.05	| 0.05 |
| Beta | 0.2 | 0.2 | 0.2 |
| 1-beta | 0.8 | 0.8 | 0.8 |
| **Output** |
| Sample size in each group	| 25,835 unique cookies to click | 39,115 users enrolled | 27,413 unique cookies to click |
| Total sample size	| 51,670 | 78,230	| 54,826 |
| Pageview required	| 645,875	| 4,741,213	| 685,325 |

So, we need 4,741,213 pageviews.

#### 3.5.2 Duration vs. Exposure
If we divert 100% of traffic, that is, 40,000 pageviews per day, we would need about 119 days (4 months) to run the experiment. It’s impractical because the time cost is high and we may want to run other experiments at the same time. Considering this, we decide not to use retention as an evaluation metric.

For the left two metrics, 685,325 is the sample size, and the experiment would take about 18 days to run at full traffic, 22 days to run at 80% of traffic, and 35 days to run at 50% of traffic. We think the experiment is not a risky one that may cause a continuous decline in the number of users enrolled. So, we may not want to limit the number of users exposed and use 100% of traffic to run 18 days and get the outcome quickly. The final decision needs to consider more things in practice, such as a holiday or running multiple tasks at the same time.

## 4.Experiment Analysis
### 4.1 A/A Testing – Sanity Checks
As we mentioned before, since it is our first time to run the experiment, we need to run an A/A testing to check our experiment is set up correctly and is going on as we expected. Only if all the sanity checks pass, we can start analyzing the experiment. If sanity checks fail, we need to go back to discuss with our team members to find out why.
#### 4.1.1 Number of cookies
This metric is a simple count metric. We expect the metric not to change because of the experiment, that is, there is no significant difference between the outcomes from the control and experiment group.

When we compare the counts directly, we find the number of cookies from the two groups are close.

The number of unique cookies to view the course overview page in control is 345,543.

The number of unique cookies to view the course overview page in experiment is 344,660.

It's an intuitive conclusion. And we need to verify our guess statistically. We assume the data is binomial distributed. Because 1) The outcomes are of two types (control or experiment); 2) Each event is independent of the other; 3) Each event has an identical distribution (p=0.5 if the groups are divided randomly and p is the same for all). According to the central limit theorem, when n is large enough, a binomial distribution approximates the normal distribution, which simplifies the estimate of variance.

The sample p (p-hat) is (the number of cookies in experiment group/the total number of cookies) or (the number of cookies in control group/the total number of cookies). Our task now is to test the p-hat has no significant difference with p (0.5) in 95% confidence interval.

p-hat = 344660/(345543+344660) = **0.4994**

The confidence interval is **[0.4988, 0.5012]**, p-hat is in this range, which means no significant difference detected with 95% confidence interval. We **pass** the first sanity check.

#### 4.1.2 Number of clicks
Number of clicks is also a count metric. We use the same method as number of cookies to test it.

The number of unique cookies to click the "Start free trial" button in control is 28,378.

The number of unique cookies to click the "Start free trial" button in experiment is 28,325.

They are also close numbers.

p-hat = 28325/(28378+28325) = **0.4995**

The confidence interval is **[0.4959, 0.5041]**, p-hat is in this range. We **pass** the sanity checks, too.

#### 4.1.3 Click-through-probability
CTP is a probability metric. We expect the CTP of control group and CTP of experiment have no difference, ie the difference is zero with an acceptable confidence interval. To compare two samples, we need to get the pooled standard deviation.

The difference is **-0.0001**. The 95% confidence interval is **[-0.0013, 0.0013]**. The difference is in the range of confidence interval, which means it is not significant. Hooray! We **pass** all the sanity checks. Finally, we can proceed to analyze the experiment results.

#### 4.1.4 Sanity Checks Results
| Metrics	| Measurement	| Observed Value | Expected Value	| CI Lower Bound | CL Upper Bound	| Results |
| :-----: | :---------: | :------------: | :------------: | :------------: | :------------: | :-----: |
| Number of cookies	| (Experiment Counts) / (Total Counts) | 0.4994	| 0.5	| 0.4988 | 0.5012	| Pass |
| Number of clicks | (Experiment Counts) / (Total Counts)	| 0.4995 | 0.5 | 0.4959	| 0.5041 | Pass |
| CTP	| Experiment CTP – Control CTP | -0.0001 | 0 | -0.0013 | 0.0013	| Pass |

### 4.2Result Analysis
Before analyzing the experiment results, let’s take a look at the data and check the data integrity. We notice that only 23 days out of the 37 days have complete information for enrollments and payments. So, the first thing to do is removing the null value from the data frame.
#### 4.2.1 Effect Size Tests
For each evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Because gross conversion and net conversion are both probability metrics, the method we will use is the same as what we used for the probability metric in sanity checks. The only difference is that we expect the difference is significant here.

The evaluation criteria are:

1)	Statistically significant: the confidence interval doesn’t include 0.

2)	Practically significant: the confidence interval doesn’t include the Dmin we set before either. To be specific, the lower bound is greater than Dmin for a positive change, the upper bound is smaller than -Dmin for a negative change.

The results are listed below:

| Metrics | Dmin | Observed Diff.	| CI Lower Bound | CI Upper Bound	| Statistically Significant	| Practically Significant |
| :-----: | :--: | :------------: | :------------: | :------------: | :-----------------------: | :---------------------: |
| GC | 0.01	| -0.0206	| -0.0292	| -0.012 | Yes | Yes |
|NC	| 0.0075 | -0.0049 | -0.0116 | 0.0018	| No | No |

#### 4.2.2 Sign Tests – Cross Checking
The sign test is one of the non-parametric methods to analyze the data without making an assumption about what the data distribution is. We may use this method to compare the results to what we got from our parametric hypothesis test.

For each evaluation metrics, use the day-by-day data to calculate the gross conversion and net conversion. When the experiment group metric is larger than the control group metric, we take this as a successful case. We use the online calculator to get the p value. The given alpha is 0.05.

| Metrics	| Number of Success	| Total Trials | p value | Statistically Significant |
| :-----: | :---------------: | :----------: | :-----: | :-----------------------: |
| GC | 4	| 23 | 0.0026	| Yes |
| NC | 10	| 23 | 0.6776	| No |

The conclusion from sign tests aligns with our parametric hypothesis test.

## 4.	Recommendation
The business goal of the experiment is to increase user experience by targeting more engaged students and providing them higher quality coaching resources. The condition to launch the experiment is that the null hypothesis is rejected for **all** evaluation metrics and that **all** the difference is practically significant. Now that we only see a statistically and practically significant decrease in Gross Conversion, not in Net Conversion, I recommend **not** to launch the experiment.
