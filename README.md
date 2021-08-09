# MechaCar_Statistical_Analysis

## Overview

Analyze production data to understand issues impacting the manufacture of MechaCar.

* Perform multiple linear regression analysis to identify which variables in the dataset predict the mpg of MechaCar prototypes
* Collect summary statistics on the pounds per square inch (PSI) of the suspension coils from the manufacturing lots
* Run t-tests to determine if the manufacturing lots are statistically different from the mean population
* Design a statistical study to compare vehicle performance of the MechaCar vehicles against vehicles from other manufacturers. 

## Findings

### Linear Regression to Predict MPG

The image below shows the output of the multiple linear regression of miles per gallon on vehicle_length, vehicle_weight, spoiler_angle, ground_clearance and AWD.

![lm_summary](/images/lm_summary.png)

Looking at the the p-values, vehicle_length and ground clearance make a significant contribution to the model. Their p-values being < .05 indicate they are unlikely to contribute random amounts of variance to the model. (In the language of the challenge question, vehicle length and ground clearance provide a non-random amount of variance to the mpg values in the dataset.)

The concept of slope in multiple linear regression can be thought of as the coefficient of each independent variable in the model. The p-values of the coefficients are used to determine whether the coefficients (the slope) are equal to zero. In this model, vehicle_length and ground clearance are statistically significently difference from zero at the .05 level. The remaining independent variables have Pr(|t|) > .05, meaning we cannot reject the hypothesis that the slope is zero.

The p-value of the intercept is 5.08e-08, indicating it explains a significant amount of variability of mpg when the independent variables are zero. The estimate of the intercept is -1.040e+02 = -104, which sounds unreasonable, suggesting some dependent variables may need to be scaled differently or transformed.

The r-squared of .7149 means that the model explains about 71% of the variability in mpg, which is pretty good. But, a plot of residuals vs. fitted values (see below) is concerning. Some of the residuals are very large, some larger than 10 miles/gallon. Investigation of these data points is necessary to determine if they are outliers, or if something else is going on.

![lm_residuals](/images/lm_residuals.png)

In my opinion, the model does not do an effective job of predicting mpg for MechaCar prototypes. It needs some more work to determine whether the mpg can be predicted based on the independent variables available with an acceptable level of precision.

## Summary Statistics on Suspension Coils

Below is an image with summmary statistics across all three lots amd the three lots separately. The design specification dictates the variance of the coils not exceed 100 psi. 

![coils_summary_all_lots](/images/coils_summary_all_lots.png)

![coils_summary_all_lots](/images/coils_summary_lots.png)

Box plots comparing the lots show that lot 3 has a wildly greater spread compared to the other two lots, indicating there is a problem with lot 3.

![coils_box_plots](/images/coils_box_plots.png)

The design specifications dictate the variance of the suspension coils must not exceed 100 psi. A single test of variance can be performed using the chi-square distribution. The null hypothesis is that the sample variance is less than or equal to 100 psi. The alternative hypothesis is that the sample variance is greater than 100 psi. The test statistic is X^2 = (n-1) * sample variance / the target variance of 100 psi.

Looking at all 3 lots together, the test-statistic is 92.8174, p-value=.9999. We cannot reject the null hypothesis that the variance is less than or equal to 100. 

![variance_test_all_lots](/images/chi_sq_all_lots.png)

However, looking at each lot separately reveals the problem. Lots 1 and 2 are consistent with sample variance less than or equal to 100. 

![variance_test_lot_1](/images/chi_sq_lot_1.png)

![variance_test_lot_2](/images/chi_sq_lot_2.png)

Lot 3, however has a sample variance of 170.286, p-value=.00156, meaning we reject the hypothesis that the sample variance is less than or equal to 100.

![variance_test_lot_3](/images/chi_sq_lot_3.png)

## T-Tests on Suspension Coils

The image below shows the results of a t-test evaluating whether the mean value of psi across all three lots equals the population mean of 1,500 psi. The sample mean is 1498.78. The t-statistic for the test of the null hypothesis that the sample mean = 1,500 psi is -1.8931, with p-value = 0.06028. This is just over the .05 threshold, so we cannot reject the hypothesis that the sample mean is not statistically different from 1,500 psi.

![coils_one_sample_ttest](/images/coils_one_sample.png)

The following images show the same test of the null hypothesis for each of the three lots separately. Lots 1 and 2 have the same result: cannot reject the null hypothesis that the sample mean is not statistically significantly different from 1,500 psi. Lot 3 however, has a p-value of .04168, meaning we can reject the null hypothesis and conclude that the sample mean is statistically significantly different from 1,500 psi.

![coils_ttest_lot_1](/images/coils_ttest_lot_1.png)

![coils_ttest_lot_2](/images/coils_ttest_lot_2.png)

![coils_ttest_lot_3](/images/coils_ttest_lot_3.png)

Again, evidence indicates there is an issue with lot 3.

## Study Design: Mechacar vs. Competition

Performance is a very broad term, so this needs to be narrowed down substantially. Consumer Reports, is a good starting point to see which data elements they evaluate to understand safety, reliability and owner satisfaction. Some of these data points can be objectively determined, such as acceleration, noise, mpg, reliability of certain parts and crash tests. Some are related to customer preferences, such as price, styling, comfort and convenience features. Some are only determined long after the customer purchase, for example repair records, accident reports and owner satisfaction. All of these considerations can also vary by the target customer segment and use cases.

We don't know anything about the type of vehicle or the target customer. We also don't know the value proposition for this prototype: what is supposed to make MechaCar a more compelling purchase to the customer than all other alternatives? We need to determine why this question is being asked: what decision or action will be taken based on the outcome of the test?

To simplify, I have chosen the following scenario:

Our target customer enjoys outdoor recreation, and needs to transport equiment for this purpose. A typical excursion for this customer includes towing a boat, camper or small vehicles such as jet skis and sno-cats. Thus horsepower and towing capacity, are key decision drivers for this customer. Required horsepower is determined by the weight of the object being towed. Towing capacity takes into consideration the horsepower consumed by the vehicle itself, and thus the remaining horsepower available for towing.

Another concern for this customer is not just the cost to purchase the vehicle, but also daily operating cost. Usually the customer uses the same vehicle for everyday driving, so mpg in this use case needs to be reasonable.

To determine whether MechaCar can be a contender in this market, they will need to match, or ideally beat, competitors on this combination of mpg, price and towing capacity. Data for these parameters on existing competitor vehicles is readily available. 

### The Study Design

The first step is to gather data on mpg, horsepower, towing capacity and price for vehicles in the set of competitors. This data informs the targets MechaCar must meet to match or exceed the competition. A rule-of-thumb is that a vehicle will lose about 7 mpg pulling a camper. One of the researchers has a 4,000 lb camper that can be used for the test.

The research question: Will MechaCar be able to tow a 4,000 lb. camper and see a loss of less than 7 mpg.

Null Hypothesis: MechaCar loses more than 7 mpg

Alternate hypothesis: Mecha Car loses less than 7 mpg

Data Collection: 10 prototype cars will make 2 test drives each, one with camper and one without camper. Mpg will be calculated for each trip.

Test Statistic: Paired T-Test, where each pair is the 2 test drives made with one prototpye car. A paired t-test is used when the same subject is measured under different conditions. 


