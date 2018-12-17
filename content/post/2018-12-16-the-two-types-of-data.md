---
title: The two types of data
author: ~
date: '2018-12-16'
slug: the-two-types-of-data
categories: []
tags: []
---

Basically nothing I learned about data from school carried over when I got a data science job in industry. 

The overwhelming of data which we collect in industrial data applications (by which, I do mean tech applications at scale) is *log data*. Most of what we work with in school (or outside of large tech companies) is not that. Typically, we'll work with _log data_ or _metadata_.

In this post, I'll cover what that data model looks like.

## What does school data look like?

We can look at the data used in the (great) [Elements of Statistical Learning](https://web.stanford.edu/~hastie/ElemStatLearn/data.html) as a representative sample of what classrooms are using. There are 21 datasets in this book, and only a few offer similarly repeated observations. By my count, there are 3.5 such datasets:

1. Bones (though the age metadata stays the same, which should not really be true, so counting this as 0.5)
2. LA ozone 
3. Ozone
4. Spam

The rest are one off observation data. 

## What does industry data look like?

### A not-that-contrived example

Take for example social media posts. Let's think about what happens if a user, Amy, likes Bob's post. Who are the stakeholders in this "action," and what kinds of tasks do they want to do with this information?

1. Amy wants to see the "like" button turn blue, signfying that she liked Bob's post.
2. Bob wants to know that Amy liked his post, as well as know who else liked it.
3. If Amy unlikes the post, we should signal that immediately as well.

What about stakeholders in the social media company? While they might not "care" about Amy or Bob as individuals (and usually will/should never see their names or the content of Bob's post, for privacy-related purposes), they are still interested in the behavior and performance of the network.

4. Charlotte, a data scientist, might be running an experiment that does some treatment which she hypothesizes will change how often users like posts. She'd want to know if Amy or Bob was in the experiment, and if so, what treatment group they were in (control, variant 1, variant 2, etc).
5. David, a product manager, wants to know what the general trend of the number of likes is. He'll look at a dashboard every morning and look at the behavior, perhaps looking at if behavior in the US differed compared to the UK, or how behavior differs between users on web and users on the app.

### How is it stored?

Then, with these use cases in mind, we would typically want to write these actions to two places:

1. Ground truth database: this storage supports production use cases, such as how Amy and Bob perceive the post. An important constraint here is that the data must be realtime -- imagine not being able to see who liked your post for a day! Typically, this database will be something like MySQL or Cassandra, which offer extremely low latency data storage and retrieval. When Amy likes the post, we'll immediately write to a table that Amy liked the post.
2. Log data storage: this storage supports analytics and offline use cases. Here, we can run arbitrarily large/complex queries without worrying about taking down the production MySQL box. One example pipeline: a "message" which indicates that Amy liked Bob's post at some timestamp, with some other metadata, is "published" to an Apache Kafka service, which then processes the message and appends the message to a log file stored on Amazon S3. At that point, the action is considered persisted and is available for future use. Perhaps we read the log file directly from S3 using Spark (e.g. treating the log file as a CSV or JSON file, both of which support direct appends), or we do some other batch processing and convert the file into a Parquet file, which we again read with Spark. Or, we could upload the data into a specialized database such as Redshift, which will store the data in its own proprietary format but will try to optimize it.

The schemas for how we'd represent the above is slightly different for each of these tools too.

1. Ground truth: 
    - `timestamp` (when did this happen?)
    - `user_id` (who's liking the post?)
    - `post_id` (which post was liked?)
2. Log data: 
    - `date` (partition for our data)
    - `timestamp` (when did this happen?)
    - `user_id` (who's liking the post?)
    - `post_id` (which post was liked?)
    - `browser_information` (was this on app or web?, what app version?)
    - `location_information` (what country is the user in when they liked the post?)
    - `other_metadata` (metadata about the specific event, not about the user!)
3. Metadata about users: we'd likely have multiple tables containing different pieces of information, so each of these might belong to a different table (or maybe you join them all together into one big `core_user_info_final_v2` table)
    - `join_date` / `join_cohort` (when did a user join the site? helpful for cohort analysis)
    - `experiment_group_{experiment_name}` (what experimental group was a user in for a given experiment?)
    - `gender`, `age`, other demographic info
    - `dt`: Typically these tables will also be dt-partitioned, as they might change from time-to-time. For example, you might have a table which tracks if the user has connected their Facebook account to your site. Then, that status can change per day as the user connects/unconnects their account. 

    
Each of these datastores is *specialized* and optimized for its specific use case. You could write SQL on a MySQL database, but for any large query (e.g. joining multiple big tables, which I'll get into in a minute), you'll run the risk of competing with higher-priority realtime production queries and overloading the server, which generally means site downtime. You could write production queries against an analytics datastore, but it'll be slower (often really slow!) and potentially without the most up-to-date data.

A couple examples of where we'd want to join multiple datasources in an analytics database:

* Presumably there is a dashboard somewhere which tracks the total number of likes being registered in the system. Then, with log data, we can easily graph that data, perhaps broken down by splits such as user cohort, country, if the user is on mobile, etc. Crucially, log data must come with a timestamp, so we can aggregate the logs into a unit of time which makes sense (hours for some logs, days for others, even weeks or months for others too).
* Charlotte, the data scientist, wants to run some statistical tests which compare the number of likes made in the test variant versus those in the control variant, to determine the impact of her experiment. So, we'll need to join the log data with some metadata tables to figure out which users were in each segment of the experiment and how many likes they made.
* Some ad hoc query request comes up, e.g. if David wants to know the ten most liked news organizations, in Canada in February (probably something about hockey, but he wants a more granular result).

An example query:

```{sql}
SELECT
    a.dt
    , b.cohort_year
    , COUNT(*)
FROM fct_like_log a
INNER JOIN dim_cohort b
    ON a.user_id = b.user_id
    AND a.dt = b.dt
WHERE
    a.dt BETWEEN '2018-12-01' AND '2018-12-07'
GROUP BY 1, 2
```

## Why does this matter?

The repetition of industry data is a truly defining factor here. It's rarely enough to run an analysis once; since we have endless streams of data coming in, we want to be able to reproduce that analysis. That requires a paradigm shift: focus on writing reproducible code (whether through notebooks or practical code), thinking of data output as being daily jobs rather than one-off analysis, and rethinking what types of data we typically work with.

For students, the biggest actionable is to work with these kinds of datasets! Unfortunately, there aren't that many large open datasets following this pattern. One good option is The [Two Sigma challenge](https://www.kaggle.com/c/two-sigma-financial-news) on Kaggle, which pretty closely follows the paradigm which I outline here, of running a job daily based on log data and metadata, and (presumably) using the output of that job to make business decisions.

For more reading, Robert Chang's series on [data engineering](https://medium.com/@rchang/a-beginners-guide-to-data-engineering-part-i-4227c5c457d7) covers, in more detail, the nitty gritty of how data moves from one place to another. If you're curious about why the tables above were named `fct` or `dim`, you should also read his writing!

## Errata

There are other types of data that I didn't touch on here, such as queue transition data (e.g. for tracking the moderation history of a piece of content) or one-shot observation data (the stuff you did in school). Those are typically more specialized than these more general data structures.