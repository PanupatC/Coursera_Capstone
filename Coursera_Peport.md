# Analysing Swedish cities for child raising

Panupat C.

January 2021
<br><br><br>

## 1. Introduction

<br>

### 1.1 Background

Growing up in Thailand, the short comings of our education system was apparant. This subject is even more important now that I have become a parent. And especially after the woke of  [recent events where Thai teachers were caught on tape allegedly beating  kinddergarten students](https://thethaiger.com/news/national/nonthaburi-teacher-allegedly-beat-students-witnesses-may-face-charges-video). The need to seek refuge to provide our children with better education has been accentuated like never before.

And the country that are highly revered amongst Thai community for their education system, is **Sweden**

Moving our family to a foreign country is taking a huge leap into the unknow. It is intimidating. However, choosing a place with familiar food and many desired facilities based on our preference may help the transition easier.

<br>

### 1.2 Problem

We are requesting information from Foursquare API. Apparantly, even if we set the limit of data to 250, the returned json still only contains at most 100 venues. This caused me to change my approach mid way through my study as will be explained in later sections.

<br>

### 1.3 interest

Thai parents, especially will younger children attending, or about to attend, kindergarten school through out middle-school would be very interested in this subject.

<br>

---

<br>

## 2. Data acquisition and cleaning

<br>

### 2.1 Data Sources

- https://www.edarabia.com/schools/sweden contains table of top rated schools in Sweden. 

- Fousquare Venues API provide and end point we can request venues from.

- https://developer.foursquare.com/docs/build-with-foursquare/categories contains the category ID used by Foursquare API

<br>

### 2.2 Early Data engineering and visualization

Firstly I wanted to visualize how the schools are distributed. Utilizing `Selenium` and `Firefox driver`, I scraped the top rating schools from this website: 

https://www.edarabia.com/schools/sweden

The result gave me a total of 10 schools along with their addresses. From there I utilized Python's `Geopy` module to aquire their coordinates and created the following Dataframe.

![](https://i.imgur.com/4C8OrtT.png)

Plotting the location on map with Python's `Folium` revealed that most schools are clustered quite close together.

![](https://i.imgur.com/bG1CFSO.png)

*Stockholm* is where majority of the best schools reside. A few were scattered around *Arsta, Solna*, and *Upplands Vaesby*. There is 1 in a distance *Malmoe*. From this point on I narrowed my exploration down to these 5 cities.

The School and Address features were no longer of interest and were dropped. Unique city from the dataset were then condensed down with their coordinates average out.

![](https://i.imgur.com/NcKy2UQ.png)

<br>

### 2.3 Initial Foursquare API request

I requested over-all venues from Foursquare API per city. From the result I manually explored the data to figure out how I could count the venues I was interested in. Asian food, gyms, parks, bookstores, steak houses. A mix of what I thought was desired for raising kids and my own preference.

<br>

### 2.4 Problem with dataset

I was suspecious of the data since the result seemed much smaller than initially expected. Especially the number of bookstore. For country of this high educational standard I was expecting many more.

So going back to check on the dataset aquired, I quickly realized that the number of venues returned from API were limited to 100 per call despite me sending a *limit* argument way above that number. This meant that there were many more venues cut off from my dataset, deeming it incomplete and inaccurate.

<br>

### 2.5 API requesting, second attemp

Researching more into Fousquare API, I realized I could request data per category instead of doing an over-all. With this approach, each category would have a room of 100 data separate from oneother. 

The category ID were scraped from this URL:

https://developer.foursquare.com/docs/build-with-foursquare/categories 

The ID were then used for requesting venues from API. The result has a much venue count than before.


<br>

## 3 Exploratory Data Analysis

<br>

### 3.1 The initial data

This contains the bar chart of venue count, before breaking down into individual category. As mentioned in 2.4, the count were suspiciously low.

![](https://i.imgur.com/RvVu4pJ.png)

![](https://i.imgur.com/5HzkCtO.png)

![](https://i.imgur.com/08f8BTC.png)

![](https://i.imgur.com/EwnrC1e.png)

![](https://i.imgur.com/GThgxz7.png)

![](https://i.imgur.com/hETXyx9.png)

<br>

### 3.2 The improved data per cetegory

To make the graph more appealing I normalized the count down to 0-1. So the numbers along the X axis are not the actual count but rather the percentage representation of each.

The venue types were also categorized by color. 

- Blue for self-improvement type venues.
- Violet for recreational and sports.
- Orange for restaurant.
- Green for places where children can enjoy themselves.

![](https://i.imgur.com/AvGXkxk.png)

![](https://i.imgur.com/VWpekya.png)

![](https://imgur.com/cTj3oUL.png)

![](https://imgur.com/3rJF9oj.png)

![](https://imgur.com/roKYRUO.png)


<br><br>

## 4 Conclusion

In this study I analyzed potential cities to migrate my kids to. The cities were narrowed down based on top rating schools, and the criteria were based on venues that matches our family preference.

Out ofthe 5 candidate, only *Upplands Vaesby* is missing certain venues we were expecting. The other 4 all contains at least 1 venue of our interest, with Stockholm being the clear winner follow by Arsta.