---
layout: post
title: "Analyzing Recsys 2015 e-commerce data."
date: 2020-08-13 00:00:00 -0400
background: "/img/posts/04/background.png"
categories: datascience
---

# Analyzing Recsys 2015 e-commerce data.

This post is simple data analysis for a e-commerce data. First we will show how the data is organized and which question we can achieve using the analisys.

## Data Description

This section contains data information, source, organization and interest about. 

### Where is the dataset from?

- This dataset was constructed by YOOCHOOSE GmbH to support participants in the RecSys Challenge 2015.
- The dataset can be download in https://www.kaggle.com/chadgostopp/recsys-challenge-2015
- The YOOCHOOSE dataset contains a collection of sessions from a retailer, where each session is encapsulating the click events that the user performed in the session. For some of the sessions, there are also buy events; means that the session ended with the user bought something from the webshop. The data was collected during several months in the year of 2014, reflecting the clicks and purchases performed by the users of an online retailer in Europe. 

 Unfortunately, the majory of e-commerce data is private. This data can help to identify internal patterns in the e-commerce area. We can identify what kind of users a certain store is interacting with, how often they access the sites, what location they establish and how each product and user is distributed.
 Also this data is quite large, which can truly simulate e-commerce data in general.

## How the data is organized?

We have a sequential data of customers that can lead to buy something or not.
The file yoochoose-clicks.dat comprising the clicks of the users over the items:

Each record/line in the file has the following fields/format: Session ID, Timestamp, Item ID, Category

    - Session ID – the id of the session. In one session there are one or many clicks. Could be represented as an integer number.
    - Timestamp – the time when the click occurred. Format of YYYY-MM-DDThh:mm:ss.SSSZ
    - Item ID – the unique identifier of the item that has been clicked. Could be represented as an integer number.
    - Category – the context of the click. The value "S" indicates a special offer, "0" indicates  a missing value, a number between 1 to 12 indicates a real category identifier, any other number indicates a brand. E.g. if an item has been clicked in the context of a promotion or special offer then the value will be "S", if the context was a brand i.e BOSCH, then the value will be an 8-10 digits number. If the item has been clicked under regular category, i.e. sport, then the value will be a number between 1 to 12. 


The file yoochoose-buys.dat comprising the buy events of the users over the items.


  - Each record/line in the file has the following fields: Session ID, Timestamp, Item ID, Price, Quantity
  - Session ID - the id of the session. In one session there are one or many buying events. Could be represented as an integer number.
  - Timestamp - the time when the buy occurred. Format of YYYY-MM-DDThh:mm:ss.SSSZ
  - Item ID – the unique identifier of item that has been bought. Could be represented as an integer number.
  - Price – the price of the item. Could be represented as an integer number.
  - Quantity – the quantity in this buying.  Could be represented as an integer number.


## Data Construction

The objective of this session is to create clean data and extract new information (as columns) for better data analysis. To clean the data we are going to remove:

  - Possible null values.
  - Duplicate data.
  - Possible noise data.


In addition, to create more insights for exploratory analysis we create:

  - A column with times (hour, month, week of day, etc).
  - A column with the time between two clicks (dwelltime)
  - An item rank* column, that is, a value that informs how much in quantity an item was purchased from the purchase items dataset (yoochoose-buys.dat)
  - A label column that can be True or False. True if a user session terminates with purchased items, False if not.


Note: The dataset is very large with a total of 9249729 sessions and 33 million rows. To simplify the problem we reduce the dataset to 10% so that it is easier to calculate the columns.


## Exploratory Data Analysis

After having built the entire dataset, our goal is to generate value through e-commerce data.

It is often important for companies to understand how purchases are flowing, how users interact with products. Given that, we want to answer the following questions:

- How many buyer-sessions (sessions that end with purchased items) and non-buyer sessions (sessions without any purchase)
- How sessions length is distributed?
- How the sessions length affects buying rate?
- How much a time data (hour, weekday, month, etc) affect the buying rate? 
- How does the time between sessions affect (dwelltime) purchasing intention? 
- How does the number of clicks per item affect the purchase rate?
- How does the quantity of purchases per item (item rank) affect the purchase rate?
- Does the dataset have outliers?


The next subsections show the analyzes made for each question.

### How many buyer-sessions (sessions that end with purchased items) and non-buyer sessions (sessions without any purchase)

It is important to understand the relationship between buyers and non-buyers for a store. With this, it is possible to estimate the profit return by the store and know if the data is unbalanced. The graph generated below shows the distribution of buyers and non-buyers.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/buyers-and-non-buyers.png" style="max-width: 100%; float:left; margin-right:2em; border-radius:00%"/></center>


The figure shows that the data classes are well unbalanced, buyers are between 2.5 percent and non-buyers 97.5 percent.

### How sessions length is distributed?

As the dataset contains a high number of sessions, it is interesting to know how they are distributed, ie the number of clicks per session. Knowing the number of sessions is important to know if there is an inconsistency regarding the size. For example, it is possible that there are robots collecting data on the site and that of course, will have a very large number of clicks.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/length-distribution.png" style="max-width: 100%; float:left; margin-right:2em; border-radius:00%"/></center>

As we can see in Figure above, most user sessions are among the first 20 clicks. This shows that if we need to develop a data representation model, such as neural networks, it is reasonable to remove very old clicks.

### How the sessions length affects buying rate?

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/sessions-length-buying-rate.png" style="max-width: 100%; float:left; margin-right:2em; border-radius:00%"/></center>

Since we know the distribution of session sizes, it’s interesting to know if they affect purchase intent. The figure shows that size 100 sessions do not contain any purchases and reinforces the idea that there are not very important sessions in the dataset.

### How much a time data (hour, weekday, month, etc) affect the buying rate?

Something interesting to know is whether time is important in the purchase intention. Intuitively, it is possible to say that there are days and times more likely for someone to buy. 

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/hour-buying-rate.png" style="max-width: 80%; float:center; margin-right:2em; border-radius:00%"/></center> 

We clearly see a pattern among the time figures in relation to the purchase rate. The purchase rate in relation to the time seems to occur more during business hours and not at dawn.

The purchase rate between the days of the week and the month seems to occur linearly. It makes sense since people tend to buy more on the weekend and in certain months.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/weekday-buying-rate.png" style="max-width: 50%; float:left; margin-right:0em; border-radius:00%"/></center> 

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/month-buying-rate.png" style="max-width: 50%; float:left; margin-right:0em; border-radius:00%"/></center>

Finally, the purchase rate in relation to the day seems to vary without a well-defined pattern.


<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/day-buying-rate.png" style="max-width: 80%; float:center; margin-right:4em; border-radius:00%"/></center>


## How does the time between sessions affect (dwelltime) purchasing intention?}

The time between sessions (dwelltime) can say a lot about a user's clicks. If the dwell time is too low, less than a second, it is possible that the session was done by some robot and maybe it cannot lead to a buy. The next figure shows the relation with buying rate by session. The dwelltime was grouped in range of one hundred for a better visualization.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/dwelltime-buying-rate.png" style="max-width: 100%; float:left; margin-right:0em; border-radius:00%"/></center>

## How does the quantity of purchases per item (item rank) affect the purchase rate?

Item rank is a category created to say how great in value an item was purchased. The higher the rank the more purchased it was. In this subsection we would like to know if the higher the rank the buying rate of item ranks. As expected, the  a linear relationship between item rank and buying rate.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/item-rank-buying-rate.png" style="max-width: 100%; float:left; margin-right:3em; border-radius:00%"/></center>


## Does the dataset have outliers?

To identify outliers we will first make an analysis of the time between sessions (dwelltime) column. Once the dwelltime have very disparate values and because it is also numeric.

### Checking evidence of outliers
First, we will check for evidence with box-plot. As the dwelltime data are very sparse, we will use a logarithmic scale based on 10.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/scaled-box-plot-dwelltime.png" style="max-width: 100%; float:left; margin-right:0em; border-radius:00%"/></center>

As the Figure above shows, the points before the first and third quartiles are possibly outliers.

### Finding outliers with z-scores
To detect z-scores, we use the scipy library and filter all clicks with an absolute z-score value greater than three.

The image below shows how the instances are distributed in percent.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/z-score-distribution.png" style="max-width: 100%; float:left; margin-right:0em; border-radius:00%"/></center>

After detecting the instances, we plot the points with outliers graphically. For this, we use the relationship between item_rank versus dwelltime. As you can see in the image above the data is very dispersed and in a way it seems that outliers are the majority but are not. This only happens because there is a very large number of clicks.

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/04/z-score-visualization.png" style="max-width: 100%; float:left; margin-right:0em; border-radius:00%"/></center>


## Results

After the analysis we can conclude that:

    + The data is extremely unbalanced in terms of buyers and non-buyers.
    + The sessions length vary a lot in size but most of them are in the top 20.
    + Very large sessions (over than 120) do not make purchases.
    + Time is related to purchasing rate.
    + At early morning and in the morning an user is less likely to buy.
    + On weekdays, a product is more likely to be purchased on weekends.
    + In May, June and July there were no purchases.
    + Days of the month and dwelltime vary on the purchase rate.
    + The higher the item_rank the greater the likelihood of purchase.
    + The dataset possibly has outliers.


## Notebooks

You can find the data analysis implementation in this [link](https://github.com/ktakanov/RecSys-2015-DataAnalysis-Notebooks)


Thanks you.
