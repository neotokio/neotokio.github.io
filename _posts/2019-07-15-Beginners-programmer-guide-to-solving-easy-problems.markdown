---
layout: post
title:  "Beginners programmer guide to solving easy problems."
date:   2019-07-15 10:21:22 +0200
---
Writing code can be hard. Writing good code is even harder. **Being stuck on a seemingly easy task while writing code is the hardest.** For this you can find tremendous amount of resources available  to help you get unstack, all under your fingertips. However, even in the information age society, where there are thousands of experienced programmers willing to help you in their spare time and abundance of online communities (both free and paid) which exists solely for a purpose of **solving ‘this-one-thing-you-cannot-wrap-your-head-around’ it will be still up to you to know what the heck are you trying to do.**  

To illustrate how entangled you can get in your own thought process, I will use recent example of very easy task I was faced with, **transforming JSON API response in form of a nested dictionary into multindexed pandas dataframe.** And yeah, I got stuck with it, but it literally took one line of code to solve. Moreover, all was given in very precise single stackoverflow answer. 

**So why did it took 2 days to solve?**

There are a few reasons, first being … lack of experience. I will not focus on this. That’s obvious if you are beginner programmer (<1yr of programming experience). You can read it in almost every tutorial, for every language or library that exists. It can only be remedied by gaining experience, which you do by writing more code, making more mistakes and learning how to correct them. Obviously.

Having more experience, besides typing functions faster, gives you **ability to clearly define what the heck are you trying to accomplish.** You can easily single out novice programmer questions not only by how entry-level question those are, but also by how well formed (and formated) they are. 

Stackoverflow is flooded with duplicate questions, question which try to accomplish something VERY atypical, questions which try to ‘reinvent the wheel’ or simply questions which cannot be understood **until somebody will rephrase it 6 times … only to realize he actually wanted to do something different.**

**Why I failed to realize what I really wanted to do?**

Let’s define task again:

We have **a JSON API response** which returns **nested dictionary.** We want to **load** one of the dictionaries into **multi-index** pandas **dataframe.**

*Example of a dictionary with nested sub-dictionary (‘points’ key).*

![Key value pairs example](/assets/post_img/key-value.png){:class="img-responsive"}

*Example of job finished, dictionary translated to dataframe.*

![Key value pairs example](/assets/post_img/key-value-finished.png){:class="img-responsive"}

If we break it into smaller chunks, we get the following: 

1. JSON API response
2. Return .json file
3. JSON file contains multiple keys and values, few in form of nested dictionaries
4. Dataframe is a pandas object
5. Multi-index dataframe is special pandas object

As you can see this is a multi-stepped process. Most of tutorials, courses or documentation operates (not without a reason) on generalized examples, all of responses returned are always in readily readable format, dictionaries are in simplest and well formated form which doesn’t need to be adjusted for things like array length or types to fit well into dataframes. In principle, if you read about programming it seems to have tremendous flow, where each puzzle fits perfectly into its place. But, in practice, you first try to find a picture you need to compose **(what the heck I am trying to do?),** then realize that puzzles are scattered all around the house **(how the heck will I do that?)** and only then start to place separate puzzles into full picture **(the heck! I am doing it!).**

Not knowing the right question to ask will stop you from creating anything beyond some easily repeatable code from online course. It is similar to the problem of not having a puzzle picture to follow through. **You don’t need to ask question on stackoverflow, first ask it to yourself,** there is a very high probability that you will solve it within minutes without outside help and if not, you may realize that you actually don’t exactly know what you want to build. The biggest help you can get at this stage lies outside of computer screen, **use pen and paper to describe or even draw what exactly do you want to do, label everything, write your thoughts about the problem at margins.** This solves easily 80% of cases at spot.

**Back to our problem.**

I had two possible approaches here (there is probably even more)

I can choose to load .json file directly into pandas dataframe in full, then select interesting sub-dictionary (column in df), store it as separate variable, **transform it to new dataframe** while unstacking nested key-value pairs within and assigning appropriate index.

I can choose to load .json file with json library first, then select interesting key, create empty dictionary to store key-value pairs from for loop over needed values, **transform it to dataframe.**

Both accomplish exactly the same but share only last step, namely- ‘**transform it to new dataframe’ .** That should be a cue from the beginning as that is actually what I want to do, I don’t want to know how to ‘read nested dictionaries’, ‘iterate over key-value pairs’, ‘create nested dictionary’ or ‘load dictionary into dataframe’ . But, boy, I tried to do all of that first, getting more and more frustrated why exactly my data seems not to fit well or simply fails to be loaded into dataframe. Trying to hunt down every single scattered piece of puzzle only to randomly fit them together, hoping that some will ‘click’. Of course, that’s natural learning process, but it’s expensive. It is also obvious decision, if you follow the process without clearly defined goal, to check on the last step... last.

It took 94 questions to get to working solution, below you can see most common words in titles of those questions. All of this could be easily avoided by stopping for a minute and thinking – what exactly do I want to do? 

![Most common words in questions](/assets/post_img/wordfreq_stackoverflow.png){:class="img-responsive"}

**There are multiple ways to accomplish a goal, but you need to have a goal.**

Actually, stackoverflow has pretty neat guide to asking questions. 

<https://stackoverflow.com/help/how-to-ask>

Not without a reason they also have text editor which supports writing questions in specific way and early ‘solution recommendation’ system in place. Truth is that your problem is a) most likely solved somewhere b) only a problem because you can’t define it well. Look at one of paragraphs 

If you're having trouble summarizing the problem, **write the title last**- sometimes writing the rest of the question first can make it easier to describe the problem. 

This applies very well to the whole problem and it’s very similar to proposed trick with handwriting  description of your goal. It is very possible that you don’t have a much of a problem, but just struggle to define what exactly you want to accomplish.

**The Solution**

It was as clear as sky when on a third morning I sat down to a PC and just typed in well formed question, well, actually it wasn’t well formed, it was just precise. It didn’t focus on parts of a problem, it didn’t try to reinvent the wheel by querying documentation nor I didn’t try to adjust my somehow not-so-much-working code. And solution came within 5 minutes. Second click to stackoverflow provided me with a one-line solution I failed to figure out on my own for 2 days.
{%highlight python%}
new_df = pd.concat([pd.DataFrame(v) for k,v in points_list.items()], keys=points_list)
{%endhighlight%}
I already knew concat element of pandas library, same as DataFrame, I knew list comprehension too, I knew elements of dictionary like .items(). All in all, I even knew how to write it myself, my biggest problem was that I just didn’t know what the heck I am actually trying to write! Do I want to somehow iterate through dictionaries? Do I want to create new dictionary from the one I have? Do I want to get some single values from dictionaries? No. I just wanted new_df.

