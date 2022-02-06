FTCaseStudy_Git
================
Sb Fuller
2/6/2022

1.  First I installed and loaded all packages I might need:

``` install.packages('tidyverse')
install.packages('readr')
install.packages('lubridate')
install.packages('janitor')
install.packages('dplyr')
install.packages('tidyr')
install.packages('skimr')
install.packages('ggplot2')
install.packages('openair')
install.packages('psych')
install.packages('DataExplorer')
install.packages('VennDiagram')
install.packages('formattable')
library('tidyverse')
library('readr')
library('lubridate')
library('janitor')
library('dplyr')
library('tidyr')
library('skimr')
library('ggplot2')
library('openair')
library('psych')
library('DataExplorer')
library('VennDiagram')
library('formattable')
```

2.  Next, I loaded and verified all the datasets I had cleaned and
    planned to work with for this analysis:

<!-- -->

    daily_activity <- read.csv("daily_activity_merged_sc.csv")
    fitness_trackers <- read.csv("fitness_trackers_sc.csv")
    sleep_day <- read.csv("sleep_day_merged_sc.csv")
    weight_log <- read.csv("weight_log_info_merged_sc.csv")
    head(daily_activity)
    colnames(daily_activity)
    head(fitness_trackers)
    colnames(fitness_trackers)
    head(sleep_day)
    colnames(sleep_day)
    head(weight_log)
    colnames(weight_log)

3.  From these uploaded datasets, I created a new dataframe of only rows
    corresponding to days that users actively logged an activity
    distance:

<!-- -->

    logger_activity <- filter(daily_activity, LoggedActivitiesDistance > 0)
    head(logger_activity)

4.  Then, quick checks were made as to how this new data frame compared
    to the prior datasets in terms of number of rows and unique user
    ids:

<!-- -->

    nrow(daily_activity)
    nrow(logger_activity)
    nrow(sleep_day)
    nrow(weight_log)
    n_distinct(daily_activity$Id)
    n_distinct(logger_activity$Id)
    n_distinct(sleep_day$Id)
    n_distinct(weight_log$Id)

5.  A venn diagram was then put together to visualize and identify
    overlaps of distinct user Ids of each logging interest/capability:

<!-- -->

    daily_ids <- unique(daily_activity$Id, incomparables = FALSE)
    logger_ids <- unique(logger_activity$Id, incomparables = FALSE)
    sleep_ids <- unique(sleep_day$Id, incomparables = FALSE)
    weight_ids <- unique(weight_log$Id, incomparables = FALSE)
    venn <- venn.diagram(x = list(daily_ids, logger_ids, sleep_ids, weight_ids),
            category.names = c("Passive Dist Loggers", "Active Dist Loggers", 
                                "Sleep Loggers", "Weight Loggers"),
            filename = "features_venn.png",
            output=TRUE, imagetype="png",
            lwd = 2, fill = c("thistle1", "pink1", "mistyrose1", "seashell1"), 
            cex = 1.7, fontface = "bold", fontfamily = "serif",
            cat.cex = .7, cat.fontface = "bold", 
            cat.default.pos = "outer", cat.fontfamily = "serif")

6.  Next, subsets of user Ids needed to be created based on what data
    they logged in order to compare these numbers with those of other
    groups and with total users by statistical analysis:

<!-- -->

    daily_sleep_ids <- daily_ids[daily_ids %in% sleep_ids]
    daily_nosleep_ids <- daily_ids[!(daily_ids %in% sleep_ids)]
    daily_sleep_noweight_ids <- daily_sleep_ids[!(daily_sleep_ids %in% weight_ids)]
    daily_sleep_weight_ids <- daily_sleep_ids[daily_sleep_ids %in% weight_ids]
    daily_sleep_noweight_nolog_ids <- daily_sleep_noweight_ids[!(daily_sleep_noweight_ids %in% logger_ids)]
    daily_nosleep_noweight_nolog_ids <- daily_nosleep_ids[!(daily_nosleep_ids %in% weight_ids)]
    daily_sleep_weight_nolog_ids <- daily_sleep_weight_ids[!(daily_sleep_weight_ids %in% logger_ids)]
    daily_sleep_noweight_log_ids <- daily_sleep_noweight_ids[daily_sleep_noweight_ids %in% logger_ids]
    daily_nosleep_weight_nolog_ids <- daily_nosleep_ids[daily_nosleep_ids %in% weight_ids]
    daily_sleep_weight_log_ids <- daily_sleep_weight_ids[daily_sleep_weight_ids %in% logger_ids]

7.  The datasets of daily_activity and sleep_day were then merged into a
    single df for analysis of sleep data among different user groups
    that logged their sleep data:

<!-- -->

    daily_activity <- rename(daily_activity, Date = ActivityDate)
    sleep_day <- rename(sleep_day, Date = SleepDay)
    daily_sleep_merged <- merge(daily_activity, sleep_day, by = c("Id", "Date"))
    daily_sleep_merged <- daily_sleep_merged[!duplicated(daily_sleep_merged), ]
    View(daily_sleep_merged)

8.  A quick check was performed to confirm that the users that didn’t
    record sleep were no longer accounted for in this new df:

<!-- -->

    daily_sleep_merged_ids <- unique(daily_sleep_merged$Id, incomparables = FALSE)
    daily_sleep_merged_ids[daily_sleep_merged_ids %in% daily_nosleep_noweight_nolog_ids]
    daily_sleep_merged_ids[daily_sleep_merged_ids %in% daily_nosleep_weight_nolog_ids]

9.  Despite prior cleaning in Sheets and SQL, some column class types
    reverted to characters and had to be reconverted back into integers
    in order to proceed with statistical analysis:

<!-- -->

    daily_activity <- transform(daily_activity, 
                      TotalSteps = as.integer(TotalSteps), 
                      SedentaryMinutes = as.integer(SedentaryMinutes), 
                      Calories = as.integer(Calories))
    str(daily_activity)
    daily_sleep_merged <- transform(daily_sleep_merged, 
                      TotalSteps = as.integer(TotalSteps), 
                      SedentaryMinutes = as.integer(SedentaryMinutes), 
                      Calories = as.integer(Calories))
    str(daily_sleep_merged)

10. Quick statistical analysis was performed of primary dfs:

<!-- -->

    daily_activity %>% 
      select(TotalSteps,
             TotalDistance,
             VeryActiveDistance,
             ModeratelyActiveDistance,
             LightActiveDistance,
             SedentaryActiveDistance,
             VeryActiveMinutes,
             FairlyActiveMinutes,
             LightlyActiveMinutes,
             SedentaryMinutes,
             Calories,
             LoggedMinutes,
             UnloggedMinutes ) %>%
      summary()
    daily_sleep_merged %>% 
      select(TotalSteps,
             TotalDistance,
             VeryActiveDistance,
             ModeratelyActiveDistance,
             LightActiveDistance,
             SedentaryActiveDistance,
             VeryActiveMinutes,
             FairlyActiveMinutes,
             LightlyActiveMinutes,
             SedentaryMinutes,
             Calories,
             LoggedMinutes,
             UnloggedMinutes,
             TotalSleepRecords,
             TotalMinutesAsleep,
             TotalTimeInBed) %>%
      summary()
      fitness_trackers %>% 
      select(Rating..Out.of.5.,
             Average.Battery.Life..in.days.) %>%
      summary()

11. A scatterplot was then produced showing negative correlation between
    Sedentary Activity and Sleep Duration:

<!-- -->

    ggplot(data=daily_sleep_merged, aes(x=TotalMinutesAsleep, y=SedentaryMinutes)) + 
      geom_point(color='deeppink4') + geom_smooth(color='deeppink4') +
      labs(title="Negative Correlation between Sedentary Activity and Sleep Duration") +
      theme(text=element_text(size=10, family="serif"))
    ggsave("sed_act_sleep_dur.jpg")

12. A scatterplot showing correlation between time in bed and time
    asleep confirmed obvious correlation between sleep time and time in
    bed:

<!-- -->

    ggplot(data=daily_sleep_merged, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + geom_point()
    ggsave("asleep_inbed.jpg")

13. Numerous scatterplots were then produced to try to find any
    correlations between varying varying levels and sedentary minutes,
    varying activity levels and minutes asleep, and steps and minutes
    asleep, but no correlations were found in these general dataframes:

<!-- -->

    ggplot(data=daily_activity, aes(x=LightlyActiveMinutes, y=SedentaryMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_activity, aes(x=FairlyActiveMinutes, y=SedentaryMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_activity, aes(x=VeryActiveMinutes, y=SedentaryMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_sleep_merged, aes(x=LightlyActiveMinutes, y=TotalMinutesAsleep)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_sleep_merged, aes(x=FairlyActiveMinutes, y=TotalMinutesAsleep)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_sleep_merged, aes(x=VeryActiveMinutes, y=TotalMinutesAsleep)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_sleep_merged, aes(x=TotalSteps, y=TotalMinutesAsleep)) + 
    geom_point() + geom_smooth(color='deeppink4')

14. Next, more scatterplots were produced in order to find correlation
    between varying activity levels and minutes logged in general and
    again no such correlation was found in these general dataframes:

<!-- -->

    ggplot(data=daily_activity, aes(x=LightlyActiveMinutes, y=UnloggedMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_activity, aes(x=FairlyActiveMinutes, y=UnloggedMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_activity, aes(x=VeryActiveMinutes, y=UnloggedMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_activity, aes(x=LightlyActiveMinutes, y=LoggedMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_activity, aes(x=FairlyActiveMinutes, y=LoggedMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')
    ggplot(data=daily_activity, aes(x=VeryActiveMinutes, y=LoggedMinutes)) + 
    geom_point() + geom_smooth(color='deeppink4')

15. Next, I moved onto the Indian fitness tracker market analysis
    dataset. Here, a bar graph was produced showing the amount of
    different ratings scored of the different strap materials:

<!-- -->

    ggplot(data=fitness_trackers, aes(x=Rating..Out.of.5.)) + geom_bar() + facet_wrap(~Strap.Material)
    ggsave("material_rating.jpg")

16. Next, a bar graph was produced showing the amount of different
    ratings scored of the different brands:

<!-- -->

    ggplot(data=fitness_trackers, aes(x=Rating..Out.of.5.)) + geom_bar() + facet_wrap(~Brand.Name)
    ggsave("brand_rating.jpg")

17. A scatterplot was produced to find correlation between battery life
    and rating depending on the brand name, but no such correlation was
    found.

<!-- -->

    fitness_trackers <- transform(fitness_trackers, Selling.Price = as.integer(Selling.Price))
    str(fitness_trackers)
    ggplot(data=fitness_trackers, aes(x=Rating..Out.of.5., y=Average.Battery.Life..in.days.)) + 
    geom_point() + facet_wrap(~Brand.Name)
    ggsave("batterylife_brand_rating.jpg")

18. Next, columns were added to the original daily_sleep_merged df to
    better classify users by group based on logging features they had
    access to and were actively using. A new df was then produced
    showing the average time in bed and time asleep for these different
    user groups:

<!-- -->

    daily_sleep_merged_grouped <- daily_sleep_merged %>%
      mutate(Group = case_when(Id %in% daily_sleep_noweight_nolog_ids ~ "group_01",
                               Id %in% daily_sleep_weight_nolog_ids ~ "group_02",
                               Id %in% daily_sleep_noweight_log_ids ~ "group_03",
                               Id %in% daily_sleep_weight_log_ids ~ "group_04"))
    View(daily_sleep_merged_grouped)
    daily_sleep_merged_grouped_mean <- daily_sleep_merged_grouped %>%                                        
      group_by(Group) %>% 
      summarise_at(c("TotalMinutesAsleep", "TotalTimeInBed"), mean, na.rm = TRUE)
    View(daily_sleep_merged_grouped_mean)

19. Here a bar chart was produced to show the differences in average
    total minutes asleep for the different groups. This showed that
    group 4 got the most sleep on average and group 2 got the least, but
    overall the differences were very minor:

<!-- -->

    ggplot(data=daily_sleep_merged_grouped_mean, aes(y=TotalMinutesAsleep, x=Group)) +
      geom_bar(position= "dodge", stat = "identity", fill = "plum4") + 
      scale_y_continuous(name="TotalMinutesAsleep") + scale_x_discrete(name="Group") + 
      theme(axis.text.x = element_text(face="bold", color="plum4", size=10, angle=0), 
            axis.text.y = element_text(face="bold", color="plum4", size=10, angle=0))
    ggsave("group_sleep.jpg")

20. Again, a column was added to the original daily_activity dataset to
    better classify users by group based on logging features they had
    access to and were actively using. A new df was then produced
    showing the average total steps, very active minutes, fairly active
    minutes, lightly active minutes, sedentary minutes, logged minutes,
    and unlogged minutes for these different user groups:

<!-- -->

    daily_activity_grouped <- daily_activity %>%
      mutate(Group = case_when(Id %in% daily_sleep_noweight_nolog_ids ~ "group_01",
                               Id %in% daily_sleep_weight_nolog_ids ~ "group_02",
                               Id %in% daily_sleep_noweight_log_ids ~ "group_03",
                               Id %in% daily_sleep_weight_log_ids ~ "group_04",
                               Id %in% daily_nosleep_noweight_nolog_ids ~ "group_05",
                               Id %in% daily_nosleep_weight_nolog_ids ~ "group_06",))
    View(daily_activity_grouped)
    daily_activity_grouped_mean <- daily_activity_grouped %>%                                        
      group_by(Group) %>% 
      summarise_at(c("TotalSteps", "VeryActiveMinutes", "FairlyActiveMinutes", 
                     "LightlyActiveMinutes", "SedentaryMinutes", "LoggedMinutes", 
                     "UnloggedMinutes"), mean, na.rm = TRUE)
    View(daily_activity_grouped_mean)

21. A bar graph was produced showing the differences of un-logged
    minutes between different user groups. Group 4 showed a
    significantly more unlogged minutes. Groups 5 and 6 showed the
    markely least amount of unlogged minutes per logged day:

<!-- -->

    ggplot(data=daily_activity_grouped_mean, aes(y=UnloggedMinutes, x=Group)) +
      geom_bar(position= "dodge", stat = "identity", fill = "plum4") + 
      scale_y_continuous(name="UnloggedMinutes") + scale_x_discrete(name="Group") + 
      theme(axis.text.x = element_text(face="bold", color="plum4", size=10, angle=0), 
            axis.text.y = element_text(face="bold", color="plum4", size=10, angle=0))
    ggsave("group_unlogged.jpg")

22. A bar graph was produced showing the differences of sedentary
    minutes between different user groups. Groups 5 and 6 showed the
    most sedentary minutes per logged day:

<!-- -->

    ggplot(data=daily_activity_grouped_mean, aes(y=SedentaryMinutes, x=Group)) +
      geom_bar(position= "dodge", stat = "identity", fill = "plum4") + 
      scale_y_continuous(name="SedentaryMinutes") + scale_x_discrete(name="Group") + 
      theme(axis.text.x = element_text(face="bold", color="plum4", size=10, angle=0), 
            axis.text.y = element_text(face="bold", color="plum4", size=10, angle=0))
    ggsave("group_sedentary.jpg")

23. A bar graph was produced showing the differences fairly active
    minutes between different user groups. Group 4 showed the most
    fairly active minutes. Groups 5 and 6 showed the least fairly active
    minutes of logged days:

<!-- -->

    ggplot(data=daily_activity_grouped_mean, aes(y=FairlyActiveMinutes, x=Group)) +
      geom_bar(position= "dodge", stat = "identity", fill = "plum4") + 
      scale_y_continuous(name="FairlyActiveMinutes") + scale_x_discrete(name="Group") + 
      theme(axis.text.x = element_text(face="bold", color="plum4", size=10, angle=0), 
            axis.text.y = element_text(face="bold", color="plum4", size=10, angle=0))
    ggsave("group_fairlyactive.jpg")

24. A bar graph was produced showing the differences of very active
    minutes between different user groups. Groups 3 and 6 showed the
    most very active minutes while Groups 1 and 5 showed the least very
    active minutes per logged day:

<!-- -->

    ggplot(data=daily_activity_grouped_mean, aes(y=VeryActiveMinutes, x=Group)) +
      geom_bar(position= "dodge", stat = "identity", fill = "plum4") + 
      scale_y_continuous(name="VeryActiveMinutes") + scale_x_discrete(name="Group") + 
      theme(axis.text.x = element_text(face="bold", color="plum4", size=10, angle=0), 
            axis.text.y = element_text(face="bold", color="plum4", size=10, angle=0))
    ggsave("group_veryactive.jpg")
