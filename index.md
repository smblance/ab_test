#A/B Test Report
_Mark Ayzenshtadt, 2016_

##Experiment overview
Web-site: Udacity.com<br>
Experiment: Time availability prompt.<br>
Expected result: Filter out students who don't have enough time before they enroll.<br>
Unit of diversion: cookie before enroll, UserID after enroll.

At the time of this experiment, Udacity courses currently have two options on the home page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. [This screenshot](https://drive.google.com/file/d/0ByAfiG8HpNUMakVrS0s4cGN2TjQ/view) shows what the experiment looks like.

The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.


##Experiment Design
###Metric Choice
__Invariant metrics:__<br>
Number of cookies: expected not to change as it is a unit of diversion.<br>
Number of clicks: the tested feature appears after the click on "Start Now" button, so number of clicks shouldn't change.<br>
Click-through probability: the tested feature appears after the click, so probability of a click shouldn't change.

__Evaluation metrics:__<br>
Gross conversion: number of enrolled UserIDs divided by the number of clicks.<br>
Practical significance boundary: 1% increase.

Net conversion: number of UserIDs who remain enrolled for at least 14 days (and thus make a payment) divided by the number of clicks.<br>
Practical significance boundary: 0.75% increase.

To launch the experiment, I will need one of the evaluation metrics to pass the practical significance boundary, and the other one to at least be statistically significant. This way we are reducing familywise error rate.

<!-- List which metrics you will use as invariant metrics and evaluation metrics here. (These should be the same metrics you chose in the "Choosing Invariant Metrics" and "Choosing Evaluation Metrics" quizzes.)

For each metric, explain both why you did or did not use it as an invariant metric and why you did or did not use it as an evaluation metric. Also, state what results you will look for in your evaluation metrics in order to launch the experiment. -->

###Measuring Standard Deviation
Assuming 5000 page views by unique cookies, standard deviation would be:<br>
Gross conversion: 0.0202<br>
Net conversion: 0.0156

We could expect the empirical and analytical variances to be comparable, because the unit of analysis (cookie) is also a unit of diversion. This way we are not violating indepence between samples, and thus the assumptions for computing analytical variability are valid.

<!-- List the standard deviation of each of your evaluation metrics. (These should be the answers from the "Calculating standard deviation" quiz.)

For each of your evaluation metrics, indicate whether you think the analytic estimate would be comparable to the the empirical variability, or whether you expect them to be different (in which case it might be worth doing an empirical estimate if there is time). Briefly give your reasoning in each case. -->

###Sizing
__Number of Samples vs. Power__<br>
The Bonferroni correction isn't used here.

Gross conversion:
- baseline: 20.63%
- practical boundary: 1%
- alpha = 5%, 1 - beta = 80%<br>
With these values, we will need 25,839 clicks in each sample.

Net conversion:
- baseline: 10.93%
- practical boundary: 0.75%
- alpha = 5%, 1 - beta = 80%<br>
With these values, we will need 27,411 clicks in each sample.

Thus, to make a valid conclusion in the experiment, we'll need to have at least 27,411 clicks in each sample, which means 54,822 total clicks.<br>
Assuming 8% click-through probability, we will achieve 54,822 total clicks with 685,275 pageviews.

<!-- Indicate whether you will use the Bonferroni correction during your analysis phase, and give the number of pageviews you will need to power your experiment appropriately. (These should be the answers from the "Calculating Number of Pageviews" quiz.) -->

__Duration vs. Exposure__<br>
Given that Udacity has 40,000 unique pageviews per day, we can run our experiment on 100% of traffic for about 18 days.<br>
While the traffic portion could be reduced, I wouldn't reduce it by a factor of more than 2, so the duration of the experiment isn't too long.

The main risk that we are facing in the experiment is that some of the users who face the prompt about time-availability will either underestimate the time they have or wouldn't feel easy to commit to spending enough hours on the course.<br>
These people will probably be balanced out by the people who will actually feel better by understanding how much time they'll need to spend on the course.<br>
Also, our experimental group is only half of the traffic, and control group wouldn't be affected by the change.<br>
The prompt is easy to implement and it doesn't need to communicate with the server to operate, so probability of technical failure is very low.<br>
All in all, I wouldn't consider this experiment risky for Udacity.
<!-- Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment. (These should be the answers from the "Choosing Duration and Exposure" quiz.)

Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity? -->

##Experiment Analysis
###Sanity Checks
Number of cookies: 
- 95% CI: [0.4988, 0.5012] 
- Observed value: 0.5006
- Sanity check: Passed

Number of clicks:
- 95% CI: [0.4959, 0.5041] 
- Observed value: 0.5005
- Sanity check: Passed

Click-through probability difference:
- 95% CI: [-0.13%, 0.13%] 
- Observed value: 0.01%
- Sanity check: Passed

<!-- For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check. (These should be the answers from the "Sanity Checks" quiz.)

For any sanity check that did not pass, explain your best guess as to what went wrong based on the day-by-day data. Do not proceed to the rest of the analysis unless all sanity checks pass. -->

###Result Analysis
__Effect Size Tests__<br>
Gross conversion difference:
- 95% CI: [-2.91%, -1.20%]
- Statistical significance: Yes
- Practical significance: Yes, although in the negative side

Net conversion difference:
- 95% CI: [-1.16%, 0.19%]
- Statistical significance: No
- Practical significance: No

<!-- For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.) -->

__Sign Tests__<br>
Gross conversion difference:<br>
4 positive (gross conversion higher in experimental group) out of 23.<br>
P-value: 0.0026<br>
The result is statistically significant, although gross conversion is lower in the experiment group, which is not what we want.

Net conversion difference:<br>
10 positive (net conversion higher in experimental group) out of 23.<br>
P-value: 0.6776<br>
The result isn't statistically significant.

<!-- For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.) -->

__Summary__<br>
I didn't use Bonferroni correction, as I only have two metrics, and I addressed the familywise error rate by choosing to recommend the feature only if both metrics are at least statistically significant.

For both metrics, effect size test and sign test agree.
<!-- State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose. -->

###Recommendation
The gross conversion difference is significantly negative, and even passes the practical significance boundary of 1%.<br>
Although the net conversion difference isn't statistically significant, it also leans to the negative side in both effect size test and sign test.

It is clear that the proposed change will reduce our evaluated metrics, and thus we shouldn't launch this change.
<!-- Make a recommendation and briefly describe your reasoning. -->

##Follow-Up Experiment
It's expected that adding a prompt could reduce the conversion rates: every time a user encounteres an "obstacle" on his path to a desired action, he could reconsider.<br>
We might also guess that if a user is interested in a course, he already knows that he has enough time available.

Another experiment to try could to ask a person to enroll into paid version only after a person completes "Lesson 0", where he's given an overview on the topics covered, the depth level, time required to complete and other things  instuctors find entertaining. This lesson should't take more than a couple hours.<br>
We need to make sure that all courses contain such overviews before starting the experiment.

The hypothesis is that when a person really understands what the course is about and what's its structure, he will better know if he's ready to take the paid version.<br>
The metrics that we'd want to measure could be: gross conversion (number of cookies who start the course divided by number of clicks by unique cookies), Lesson 0 completion rate, and retention rate (number of UserIDs who make a payment divided by the number of users who complete lesson 0). This way we will measure how people progress on all three steps of the path "click -> lesson 0 -> lesson 0 complete -> first payment".<br>
Unit of diversion: cookie, and userIDs after the user has enrolled. This way we'll ensure consistent exerience for the users.

<!-- Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices. -->

