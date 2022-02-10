# FTCaseStudy
Fitness Tracker Case Study in R

This case study analysis, hypothetically deliverable to co-founders and the larger Bellabeat© marketing analytics team, used available datasets to identify trends in smart device usage and the ways in which these trends apply to current and future Bellabeat© customers. Through descriptions of data sources used, supplementary documentation of data cleaning using SQL, supplementary R Markdown documentation of analysis in R, and a clear summary and visualization of key findings, this report confidently presented data indications of Bellabeat© market growth areas as well as top actionable data-driven recommendations for forthcoming Bellabeat© marketing strategy.  The report was guided by the following questions:

-Where can Bellabeat© position itself relative to the health device monitor market?
-What external data sources are available to analyze current market trends?
-What can analysis of general smart device usage data tell us about how consumers use non-Bellabeat© smart devices?
-What are the similarities and differences between the capacities and design of Bellabet© and non-Bellabeat devices?
-In what ways can the insights derived from analysis of non-Bellabeat© smart device data be applied to Bellabeat© products and customers?
-In what ways can these data comparisons indicate areas for growth?
-What are those areas for growth?
-In what ways can these datasets inform recommendations for Bellabeat© marketing strategy?
-How long will it take to complete this report?

Two datasets were analyzed to determine trends in the current fitness tracker market as well as the ways in which current fitness tracker usage can affect user health and activity. These datasets were exceptionally limited in their usability, statistically insignificant to say the least, and for more complete understanding of the market and fitness tracker usage different datasets would be required. The data was also insufficient to determine the gender identity of the users. This was unfortunate as I was seeking to make recommendations for a company that specifically targets women. None-the-less, these datasets alone were useful in the interim as preliminary analysis to show how consumers use and enjoy non-Bellabeat© smart devices.

![](/Visualizations/asleep_inbed.jpg)
![](/Visualizations/sed_act_sleep_dur.jpg)

Firstly, it was found that in general, the amount of time that a user spent in bed positively correlated with the amount of time they spent asleep, and, furthermore, the amount of time a user spent sedentary negatively correlated to the amount of time the user spent asleep on any given day. This was all relatively unsurprising. Users sleep while they're in bed and when they rest less during the day, then they are more likely to sleep in the evening. 

Other than this, little correlation was found between any other variables. Numerous scatterplots were produced to find any correlations between varying varying levels and sedentary minutes, varying activity levels and minutes asleep, and steps and minutes asleep, but no correlations were found in any of these general analyses. More scatterplots were produced in order to find correlation between varying activity levels and minutes logged in general and again no such correlation was found in any of these general analyses. 

![](/Visualizations/material_rating.jpg)
![](/Visualizations/brand_rating.jpg)
				
Next, I visited the fitness tracker market data to compare the ratings of different brands, the ratings of different strap materials, and the general correlation between rating and battery time depending on the brand. The above left visualization shows that the highest rated and most frequently highly rated strap materials were aluminum and silicone. Silicone straps had a greater range of ratings, while aluminum did not. This gave aluminum wrist brands the highest average rating of 4.6, while silicone came in 6th in terms of average rating with a rating of 4.1. 

Similarly, the above right visualization showed that the brands which had the greatest market share and greatest rating were APPLE, FitBit, and FOSSIL. While FOSSIL and FitBit had a greater range of ratings, APPLE did not. This made it so that APPLE on average had the highest rating of 4.5, while FitBit and Fossil had 4.2. This visual suggests possible brand loyalty or market dominance at play in Indian fitness tracker markets.

Finally, the below plot was produced to investigate correlation between battery life and rating. No correlation at all was found between these variables, regardless of brand.

![](/Visualizations/batterylife_brand_rating.jpg)

Returning to the usage data, users were organized into 6 categories depending on the degrees to which their usage overlapped:

![](/Visualizations/features_venn.png)

≈45% (15) of users were passive distance and sleep logging- (daily_sleep_noweight_nolog_ids)

≈15% (5) of users were passive distance, sleep, and weight logging. (daily_sleep_weight_nolog_ids)

≈9% (3) of users were passive distance, active distance, and sleep logging. -(daily_sleep_noweight_log_ids)

≈3% (1) of users were passive distance, active distance, sleep, and weight logging. -(daily_sleep_weight_log_ids)

≈21% (7) of users were only passive distance logging.  -(daily_nosleep_noweight_nolog_ids)

≈6% (2) of users were passive distance, weight logging. -(daily_nosleep_weight_nolog_ids)

0 users were logging anything without also passive distance logging and 0 users were passive and active distance logging, but nothing else. These breakdowns were likely a result of the capabilities of the devices being used, but whether or not devices had capabilities, users chose to buy devices with certain capabilities and chose to engage with different uses differently. This made each of these user types distinct from one another. There was insufficient data to analyze weight logged or active logged data, but given the major overlap of passive distance logging data and sleep data I was able to proceed with calculating average sedentary time, activity levels, and logged vs unlogged hours of each of these user types subsets. Furthermore, I was able to calculate average sleep duration of each of the 4 subsets that logged their sleep data. 

![](/Visualizations/group_veryactive.jpg)
![](/Visualizations/group_fairlyactive.jpg)

Using bar graphs, differences in the healthy and unhealthy habits of the different user groups became apparent. The upper left bar graph showed that Groups 3 and 6 had the most “very active” minutes per logged day, while Groups 1 and 5 showed the least very active minutes per logged day. The upper right bar graph showed that Group 4 showed the most “fairly active” minutes, while Groups 5 and 6 showed the least “fairly active” minutes of logged days.

The bottom left bar graph showed that Groups 5 and 6 spent the most amount of time sedentary per logged day, while the bottom right bar graph showed that when it came to wearing the device and passively logging activity, Group 4 showed a significant tendency for the most unlogged minutes while Groups 5 and 6 showed the least amount of unlogged minutes per logged day.

![](/Visualizations/group_sedentary.jpg)
![](/Visualizations/group_unlogged.jpg)

Finally, the bar chart below was produced to show the differences in average total minutes asleep for the different groups. This showed that Group 4 got the most sleep on average and Group 2 got the least, but overall the differences were very minor.

The overall sample size of this dataset makes the results pretty meaningless, and when the sample sizes are even further subdivided into smaller sets of user types, the results become arguably that much more meaningless, but as just preliminary analysis, these charts potentially tell us a lot about different types of users. 

![](/Visualizations/group_sleep.jpg)

Currently most users (Group 1) are only passive distance and sleep logging, and compared to other user groups these users measure up pretty close to average when it comes to activity and sleep times. 15 % of users (Group 2) only log passive distance, sleep, and weight and, again, compared to other user groups these users measure up pretty close to average when it comes to activity, and come in the lowest in terms of sleep but only slightly. Perhaps in this user type, there is a correlation between weight logging and a reduction in sleep. 9% of users (Group 3) passively log sleep and distance, but also sometimes actively long activity events and compared to other user groups these users measure up pretty close to average when it comes to everything except very active minutes. This makes sense as users that more actively log their activity are likely more motivated to be more active and log that activity. These users are proud. 

3% of users (Group 4) log sleep and distance passively, but also actively log distance and weight. This user type is average in terms of very active minutes, but has the least sedentary minutes, the most fairly active minutes, and gets the most sleep.  Interestingly this user has, by far, the most unlogged minutes per day. This user type’s healthy habits are the most solidly above average, but they are the least likely to passively log their entire day. There is only one individual in this user type subset and this could have a lot to do with the outlier nature of this user's data, but otherwise this user types data seems to indicate that general health negatively correlates to constant monitoring. 

Just under a quarter of users (Group 5) are only passive distance logging. These users are average in terms of sleep and fairly active minutes, but have some of the least very active minutes, the most sedentary minutes, and the least unlogged minutes. This user type, given the sample size, has much greater statistical significance than Group 4, and in that registers even more threateningly most solidly below average in terms of healthy habits, while, by far, being the most thorough passive trackers of distance. This makes some sense. Those that only passively log distance need to improve their health the most, but only want to do so passively and in only one way. The simplicity of this strategy perhaps makes it easiest to stick to most thoroughly. 

Finally,  6% of users (Group 6) only passively log distance and weight.  These users have the least fairly active minutes and the most sedentary minutes, but much like Group 5, these users are the most thorough in terms of logged hours per logged day. The simplicity of the strategy perhaps makes it easiest to stick to most thoroughly. Where these users differ from Group 5 is that, with the addition of weight logging, these users exhibit the absolute highest amounts of very active minutes, comparable to but higher than even those proud active distance loggers. These users have a long way to go, but they are definitely making changes.

No users are logging anything without also passive distance logging and no users are passive and active distance logging without logging anything else.

Just in terms of where the market is at, passive distance logging is at the core of the fitness tracker ethos, and even more appealing if paired with sleep or weight monitoring. The market landscape is appears to be influenced to some degree by brand loyalty or brand cominance and potentially somewhere in the area of strap material there is room for further analysis to understand correlations around metallic materials and improved rating. Perhaps this has to do with perceived quality, weight, or water resistance. This preliminary analysis, while exceptionally limited, mostly has surprising use value in terms of simply defining user types and their goals/tendencies.

As for applications of this analysis to a Bellabeat© product like the Bellabeat© app, it appears to be very important to improving the health and proactivity of the user that they do more than simply passively log their distance. All Bellabeat products, by design, should log at least passive distance and sleep, but beyond this, Bellabeat© customers should be encouraged, by design of the product and associated marketing, to log one more thing as well, such as active distances or weight. Multi-modal monitoring is more important to user health than constant monitoring, and furthermore this aligns with the bottom line of a company like Bellabeat©. 

The more types of data that a user is logging, the healthier they generally are, but also less likely to be thorough about passive distance logging. It may be important to look into the reasons for this variation in logging, as it could be a design issue, but also it may not be a design issue but instead a demonstration that fitness tracking culture is primarily a gateway practice to improved health. As Bellabeat© grows, business strategy should depend on establishing brand loyalty and units sold of diverse metrics and less so on increasing data vendor potential and dataset completeness by increasing user dedication.
