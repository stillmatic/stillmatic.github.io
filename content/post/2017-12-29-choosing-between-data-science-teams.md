---
title: Choosing between data science offers
author: ~
date: '2017-12-29'
slug: choosing-between-data-science-teams
categories: [articles]
tags: ['data science']
---

Not all data science roles are created equal. Everyone will tell you that "data scientists" as a role play varying parts in different companies, but how can you choose between different teams? 

{{< tweet 946061825782702081 >}}

Hilary Mason had a tweet about this that got a lot of replies along the lines of "well I work at this place and it's obviously a great place for junior data scientists, I'm here!" - but very few actually substantiate what characteristics make them a good fit for junior data scientists.

Some quick background: I graduated this spring from Wharton, after interning with Airbnb Data Science. I had 4 or 5 full-time offers of varying roles from companies of varying sizes and industries, and had to make difficult choices. These are the heuristics that I used; experiences may vary. I'm very happy with my decision to join [Quora Data Science](https://www.quora.com/careers/data_scientist) and think the team does well on many of these fronts.

These are some recommendations in evaluating what makes a good data science team. These are not really to be considered general career advice - joining an amazing team on a sinking ship isn't smart, and taking a huge paycut isn't necessarily healthy either. I also don't want to create some Manichean divide between 'junior' and 'senior' roles, hence the quotes.

## All levels

All data scientists deserve work that is meaningful, impactful, and valued within the company.

* What percent of the employees are data scientists? In my experience, having around a 10:1 ratio of engineers to data scientists is effective.
* What investments has the company made in data infrastructure? One company told me that they had a 22 hour long data proccessing pipeline; that is, data for any given day wasn't available in their analysis database until nearly a full day after. That's bad! Ideally, companies should have significant investment in both the underlying data infrastructure (smooth and efficient data pipelines) as well as in the abstractions that data scientists use, such as experiment reporting [^1], dashboarding, or sharing knowledge [^2].
* Is management really data-driven? How often are product features launched without experimentation? [^3]
* Who does the head of data science report to? Ideally you want to be somewhere that treats data science as its own job function, and doesn't report to engineering, business development,  IT/finance, or something else. The most successful places I've seen hired a data person within their first 10 hires, which goes a long way towards building a data culture.
* What's the balance between reactive and pro-active work? The more pro-active work data scientists do, the more the company values data product driven work. Reactive work can be valuable, by unblocking other product functions typically, but not enough proactive work limits the total impact of a data science team.

## 'Junior' roles

Junior data scientists should optimize for learning. I've written about [this before](http://hingeloss.com/post/2017/10/07/data-science-for-college-students-courses/) with respect to course selection, but I think that learning is just as important when picking a job.

You can maximize your learning by working somewhere which:

* Has many problems open to solve (but not fires to fight). Ideally you'd also want to be flexible about moving between teams to get exposure to more parts of the business.
* Relatively fluid job roles. The spectrum between data analytics and data science (or machine learninng, if you'd prefer) is certainly broad, and very few can excel at each skill. However, if you want to learn new skills, the best way is to actually try to implement projects with those skills (or at least work next to people who have those skills). This is notoriously difficult at large companies, which are happy to silo employees into "product analytics" and "core machine learning" roles, where the entry fee is a PhD. While this Taylor-esque division of labor could be more efficient for the company, it is emphatically *not* more efficient for individuals. How are you going to learn new skills if "regression" is considered a dirty word and you're expected to just write SQL? [^4]
* Takes mentorship seriously. I don't think formal onboardings are necessary, even though they are very popular for engineering; having good documentation and senior support is more important.
* Has smart and kind people. Google "no brilliant jerks" for any number of run-of-the-mill thinkpieces here.

## 'Senior' roles

Senior data scientists should optimize for impact - often by making other people more effective. I don't think I'm here yet, but there are a few common characteristics of the senior data scientists or data science managers in teams that I appreciate. 

* Shares their deep product intuition and accumulated tribal knowledge. This is mostly a function of experience and seeing events for themselves, but there is real skill involved in pattern-matching for the appropriate experiences to draw from. 
* Domain-specific knowledge can be put to use. An example of a mismatch would be a data scientist with a ton of expertise in self-driving car techology taking a senior technical role in an adtech company. While their general leadership skills and expertise in working with data can be useful, the overall misfit means that their total impact is limited.
* Creates abstractions that enable brand new types of analysis. My favorite examples of this generally involve system design - creating tools such as a large-scale experiment analysis framework or a machine learning model training process are extremely non-trivial and have huge benefits over manual efforts.
* Keeps junior data scientists' career interests in mind. If one data scientist is interested in data infrastructure and another is interested in machine learning, they should be pointed towards appropriate projects. This also requires management to support the growth of junior data scientists.
* Takes mentorship seriously.
* Prioritizes projects based on their potential ROI/leverage.

Teams where senior data scientists/managers are empowered to drive impact in these ways are well-positioned for success.

## Afterword

Was this useful? Totally off base? Something in between? Let [me know](https://twitter.com/hingeloss/) - as always, these are strong opinions, weakly held. 

[^1]: e.g. at [Airbnb](https://medium.com/airbnb-engineering/https-medium-com-jonathan-parks-scaling-erf-23fd17c91166) or [Pinterest](https://medium.com/@Pinterest_Engineering/building-pinterests-a-b-testing-platform-ab4934ace9f4). Quora's platform is philosophically similar to Airbnb's.

[^2]: e.g. [Knowledge Repo](https://medium.com/airbnb-engineering/scaling-knowledge-at-airbnb-875d73eff091). 

[^3]: Sometimes this is inevitable. Certain features really cannot be effectively tested, e.g. visible pricing changes. But if your business is actually dependent on these broad, untestable changes, you're either completely out of growth ideas or in a bad business. I think I just said the same thing twice.

[^4]: You know who you are.
