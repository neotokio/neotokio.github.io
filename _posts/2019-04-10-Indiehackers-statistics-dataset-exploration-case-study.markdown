---
layout: post
title:  "IndieHackers Statistics, Dataset Exploration Case Study"
date:   2019-04-18 14:51:12 +0200
---
Motivation for writing this article was mostly educational. It started as a quest to learn web scraping of JavaScript based websites and basic data exploration using python. 

IndieHackers was a perfect fit for the purpose of drawing general view on small, mostly solo, enterprises. Of course, there is more than only IndieHackers when it comes to micro startup scene. I will hopefully cover the rest of existing services in future. I found only one article dealing with IH economy and it was limited to survey conducted through community. Surveys are great way to find out hard to quantify vectors, but in case of IH a lot of data is public. I started with creating web crawler and, obviously, throttling its speed to avoid issues with IH staff.

**TL;DR**

Can micro startup be profitable? Some can. Can I make money as solo founder? Yes, if you beat the market. What are the odds of success? ~30%. What niche should I choose? There is more niches than mainstream products! How much of competition should I expect? A lot, but you can limit your exposure to competition. Will my project have edge over already existing ones? You can find edge easily and I even show you how.

In short, we will see:

- Overall IH economy condition

- Products distribution on IH

- Revenue distribution on IH

- Creators profile

To keep things interesting we will try to make some competitive/price analysis using available dataset assuming role of potential creator who looks for a new product niche.

Choosing interesting data vectors
------------------

My **dataset contains 22 columns with 1786 rows which currently represent most of products available on IH** (I dropped collecting remaining 30 products because of problems with my VPS provider, those were 0$ projects). Each row represents one product and each column specifies different vector of a product, starting from the most obvious ones like ‘ProductName’ through ‘TagLine’ or ‘NumberOfFollowers’, up to the most interesting one — ‘Revenue’. For the sake of completion I also followed with links to ‘Creators Page’ to learn more about IH creators.

Let’s check what collected dataset tells us about it.
Basics

Total monthly revenue from 1786 products is: **22357823$**

Mean revenue from 1786 products is: **12518.37$**

Median revenue from 1786 products is: **25.5$**

Maximum revenue: **2.5M $** Minimum revenue: **0$**

Since it’s easy to check, maximum and minimum revenue should not be a surprise. Mean revenue was also within expected values; there obviously is a lot of successful projects on IH. But what about median? Why is it so low? This is first warning sign for aspiring entrepreneur. We usually focus on a ‘big thing’, next we use ‘average’ as validation of our views and we end up discarding failures as something that happens only to others. In case of solo ventures, well, you have a big chance to land a spot among ‘others’.

**Number of products within revenue bands:**

![Revenue Bands](/assets/post_img/revenue_band.png){:class="img-responsive"}

>There is 804 products with 0$ revenue. Contrary to 4 products with above 1mln revenue.

Yes. **45.6% of projects do not earn even 1$.** Of course, some of projects defined as ‘0$ revenue’ were never designed to earn money (ie. ‘meetup’), but its overstatement to say that 45% of creators didn’t intend to earn money for their work. **A lot of projects in this revenue band are ‘spin offs’ of already existing ones, lacked sell power or were simply abandoned too early.** If you are looking for an idea there are probably some diamonds hiding here, but be ready to dig through a lot of rhinestones (btw. there is such project already on IndieHackers, <https://www.indiehackers.com/product/1kprojects>)

On a more positive note; we can also see considerable degree of success within IH products. It’s good that IH shares 0$ revenue projects, this doesn’t sweeten statistics, any solo founder should realize that work can, and most likely will, be rough. However, let it be said that when you make it, you usually make it big! Yes, to this point article seems to be pessimistic, but actually it’s quite opposite, we will move to some good news soon enough.

**We need to have a general overview of tag occurrence. Excluding non-category items for clarity.**

![Tag occurence](/assets/post_img/tag_occurence.png){:class="img-responsive"}

>There is visible pattern when it comes to categories of products chosen by solo founders. We will expand this topic a little in a moment.

**Now, let’s check description-type items (excluded from plot above) to learn more about who and what exactly stands behind products.**

![Description items](/assets/post_img/description_items.png){:class="img-responsive"}

If we were to make conclusions solely based on occurrence of tags it’s clear that an **average IndieHacker is a solo funder, with coding skills, most likely offering SaaS product related to her/his already ongoing work (Productivity being top used tag).** That being said, there is more than +300 micro projects which have non-programming founders. It’s not necessary to be a software developer, but it helps big. In reality, a lot of what goes for ‘programming’ in micro startup scene is ability to learn fast and glue things together. Knowing what to learn and what to glue is essential. If you don’t program — start. Even if you will not finish every single course, it doesn’t matter, creative thinkers (founders are by nature creative thinkers!) need something to anchor their dreams to, without it you will either spend a lot of money on outsourcing or just fail to materialize your ideas. In next article we will try to learn more about community itself and their actual skill set using non-quantifiable data, so consider following me if you are interested > click.

Digging Deep into Specifics
----------------------

We already know total median & mean income, but how does it look like within each revenue band?

![Mean/Median Revenue Bands](/assets/post_img/mean_median.png){:class="img-responsive"}

>That’s definitely not a standard distribution.

**It’s totally opposite to occurrence of products within same revenue bands.** Obviously, 0$ revenue project median is 0, but another three bands do not even make a dent comparing to the last one or even two before that. **Most of income generated on IH comes from 30.4% products (Pareto distribution).** Another thing worth noting is that the median revenue does not look so grim if we exclude 804 projects without any revenue. That’s something you will be aiming for. Jumping through first three revenue bands is essential if micro service is suppose to be your main income source. Note that IndieHackers gives only monthly revenue, not yearly, it doesn’t (in collected dataset) specify expenses nor it doesn’t give low/high income during a year. This statistics (nor anything for that matter) will not recreate the actual year-to-year earnings. Only 567 products are able (assuming you live in one of most popular destinations for founders) to fully support their creators.

**We already seen occurrence of a category, below we can see monthly revenue per each category in total.** (1.0 = 10 millions)

![Revenue Per Tag](/assets/post_img/revenue_per_tag.png){:class="img-responsive"}

>Revenue is not strongly correlated with popularity of category.

Surprise! It’s different than number of occurrences of each category, which means that **choosing non-mainstream market is more profitable.** If you are planning to start a new product it might be useful to notice that categories like ‘Communication’, ‘APIs’ or ‘Advertising’ are less utilized, but on average bring higher monthly revenue. For example, **‘Payments’ moved itself 26 positions up, which means that there is lesser amount of ‘Payments’ products but on average they bring more income.** You can use two of those plots (occurrence vs revenue) to find perfect equilibrium between supply, demand and competitiveness of the market you are trying to enter.

It becomes clear that focus of micro startup developers is within personal niche. It means that most of founders make IH products as side projects. Look at it this way, **if I already spend a lot of time thinking about my productivity, why not make a product out of it?** Of course, this is not true all across the scale. But compare ‘Productivity’ tag to tags like ‘Legal’, ‘Art’ or ‘Games’, those are very specific industries, while ‘Productivity’ is more than often a side project. This also shows actual demand, if we assume that supply/demand always goes in pair, for products. However, as we will check in a second, this is often a rocky road for a fresh founder. IndieHackers is also marketplace in itself so founders are also buyers of other products presented on a website, we can conclude that a lot of products are created with IH community in mind.

Okay, so let’s do exactly that. Let’s explore interesting markets in more detail. This step can be repeated for each of categories, but for now we will focus on four already mentioned above. Our data already tells us that they bring higher revenue and are less competitive than ‘Productivity’ or ‘Utilities’. Of course, **we need to remember that IH does not have any limit on tags used, there is no reason to assume that product within ‘Productivity’ category will not also be in ‘APIs’.**

![Strategy](/assets/post_img/mean_median_cate.png){:class="img-responsive"}

>This strategy may help choosing your next target market.

That’s it. ‘Productivity’ category has still biggest share of mean income, but notice how many projects with 0$ revenue reside within this category, **we can conclude that ‘Productivity’ is definitely full of competition.** We notice that mean income in ‘Communication’ does not differ too much from ‘Productivity’. What’s interesting is that **‘Communication’ has only 25.9% discount in mean revenue, while being 47.7% less popular.** ‘APIs’ and ‘Advertising’ seem to be very similar markets but they also share a feature with ‘Communication’ — **small discount in mean revenue, big popularity gap.** ‘Payments’ is most interesting, if we plan to ‘make it big’, **44.9% of discount to ‘Productivity’, but this goes hand-in-hand with 87.6% difference in popularity and really small amount of 0 revenue projects.** In this example, all of 4 categories are interesting targets for product development, each one provides income opportunity while minimizing systemic risks (ie. competition, overall market size, market capacity, probability of project failure).

Bonus! Alternative Data from IH Dataset
----------------------

Wordcloud based on occurrence of word (excluding stopwords) in IH taglines

![Wordcloud](/assets/post_img/wordcloud.png){:class="img-responsive"}

>Most popular word is… “app” (we needed to convert taglines to lowercase characters because combination of app+App was very popular), it has 98 occurrences. IndieHackers very often contain category name in which they are selling their products, we can see keywords like ‘Management’, ‘Wordpress’, ‘API’, ‘Community’ or even ‘Instagram’ high in total word count.

Creator Profile
----------------

IndieHackers are very fond of Twitter, out of 1786 product creators collected we found out that **1224 did specify their personal twitter account.** Its **204 more twitter accounts than email addresses** (1021 people specified personal email address).

Community is however less eager to share their age. Only 633 people specified their age. Median age is … 30. A perfect age to start a business!

**1226 creators shared their location.** Community on IH is very diverse, we have people from all around the world, US, Morocco, Israel, Ukraine, United Kingdom or Australia. It was hard sorting through geolocation on IH, mostly because IH allows non-standarized input (people can specify only country or city, use non-unicode characters etc.), but here is overall distribution of location on IH assuming more than 10 occurrences.

![Where are IndieHackers from?](/assets/post_img/geo.png){:class="img-responsive"}

>There was no surprise in location of IndieHackers, US being most popular, but (what could not be reliably represented in any plot because we had 543 different locations of creators in just 1786 products) there is tremendous diversity among IH community. IH-craze is definitely world-wide.

Conclusions
------------

It was fun to work with IH dataset. The biggest experience came from thinking about data, technical aspects of work could always be solved by just browsing Internet, but there is not much one can do to better her/his analytical thinking than just simply dive into a dataset. It’s comparable with what we can often read on IndieHackers, **don’t over think tools, focus on delivery, because there is far more non-technical obstacles than technical ones.**

As for IH economy. We have a a lot of very successful projects, we have a lot of non-successful projects. This also reflects advices we often hear from established creators on IH. **According to dataset, you have about 70% chance of failure with your product on IH, however, if you will push above this 70%, your median income should be above 5000$/month.** There is also a lot of untapped market in which potential creator will face far less competition and bigger monthly revenue on average, so saying that ‘Everything already was’ is untrue in this case. **On average we can find 30% monthly revenue income edge if we choose less popular category for our product.**

Full code can be found here:
