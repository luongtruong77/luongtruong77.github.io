---
title: 'King County Neighborhoods'
subtitle: 'Best place to live in King County - Washington State'
featured_image: '/images/king-county/background.jpg'
---

![](\images\king-county\ORM-Location-Bellevue-Hero.jpg)

## 1. Introduction
---
- In this project, I choose **King county, Washington, USA** to be my geographical area of investigation. King county is most populous and important county of Washington state with the estimated population of more than two million people. As of 2018, the median household income of King county was **$95009**, which is approximately ***45%*** higher than national average; and is one of the most educated regions in the US with **53.2%** of [its residents age 25 or holder holding a bachelor's or higher in 2018](https://www.kingcounty.gov/), which makes king county is of the best place to live in the US.
- More specifically, as a long-time Washingtonian, I will focus on only some citites that I personally think that they are major cities of the county (namely Seattle, Bellevue, Renton, etc.) with top companies having their headquarters (Amazon, Starbucks, Microsoft, Boeing, etc.)
- The main **question** to answer in this project is that: if a family of four with a upper-middle class income wants to move to this area, which city (neighborhood) would be best to live in terms of **property value, safety, community**, etc.
- We will use data science tools to explore, analyze, and visualize the data obtained from variety of sources to decide which neighborhood is the best for the stakeholders.

## 2. Data
---
- For the safety factor, I obtained King county's incidents 2019 dataset from [King county's Sheriff Office](https://data.kingcounty.gov) to determine which city is the safest among the county's major cities.
- For the property factor, I used King county house price dataset from Kaggle and King county zipcode centroids dataset from [Amazon AWS](https://prod-hub-indexer.s3.amazonaws.com) to determine which most appropriate neighborhood of the chosen city.
- We will explore nearby and common venues of the area using [Foursquare API](https://developer.foursquare.com/docs/places-api/endpoints/) and cluster them into several clusters. The coordinates used in the algorithms will be extracted from house price dataset and zipcode centroid dataset.

## 3. Methodology
---
In this section, I will do exploratory analysis and visualize the data obtained to have a first look at our chosen area. To complete this task, I decided to divide my methodology into 4 sub-sections:

### 3.1 Crime rates analysis
In this section, we will take a quick look at the crime rates reported in 2019 by the Sheriff's office in the major cities of King County to determine which city is the safest. Once we find which one is the safest, we will pick that city to become the city of interest to dig deeper into it.

### 3.2 House prices analysis
In this section, we will do a quick house prices analysis of our chosen city to get a broad feeling about how expensive house prices are around the neighborhood.

### 3.3 Explore the neighborhoods
In this section, we will:
- Use [geopy](https://geopy.readthedocs.io/en/stable/) library to get the latitude and longitude values of our chosen city.
- Use [Folium](http://python-visualization.github.io/folium/) library to create a city's map and its neighborhoods.
- Use [Foursquare API](https://developer.foursquare.com/docs/places-api/endpoints/) to explore the neighborhoods to get to know more about the city itself.

### 3.4 Cluster the neighborhoods
In this section, we will:
- Use [K-Mean](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) algorithm to cluster neighborhoods into different clusters.
- Explore nearby venues for each cluster and assign names for each cluster based on its own characteristics.

## 4. Results
---
### 4.1 Crime rates analysis
This is the first 10 rows of King County's crime dataset after loading data in pandaframe and cleaning it. The original dataset has 15 columns but I only choose features that are in our interest to display.

![](/images/king-county/4.1p1.png)

I then define 5 major cities, which are SEATTLE, BELLEVUE, KENT, RENTON, and FEDERAL WAY based on criteria of kingcounty.gov and use **matplotlib library** to plot the graph.

This graph shows number of crimes commited by five major cities in 2019 in King County.

![](/images/king-county/4.1p2.png)

As we can see from the graph, **Bellevue** is the safest city with the least crime counts reported in 2019. Even though **Seattle** is the most popular and biggest city of King County, with almost 8000 cases committed in 2019, it is not a very safe city to consider buying properties or to live a peaceful life.

Now that we have chosen **Bellevue** as our city of interest, let's dive deeper into it and explore its house prices in the past.

### 4.2 House prices analysis
In this section, I use zipcode dataset of King county from kingcounty.gov and house price dataset from Kaggle to merge and filter the data. Even though we are not digging too deep into this house price dataset, let's look at the overal correlation between its features by the heatmap created by using **seaborn library**.

![](/images/king-county/4.2p1.png)

Even though the dataset about house prices look very interesting, we are not going to analyze the price as a whole, but rather comparing the average house prices by **Bellevue**'s zipcodes. If you are interested in the house price dataset, for more in depth of King County's house prices analysis and prediction, please visit [HERE](https://github.com/luongtruong7793/House-Sales-in-King-County-WA/blob/master/House%20Sales%20in%20King%20County%2C%20USA.ipynb) . Throughout this project, I will use *zipcode* as an indicator of the neighborhood since I do not have much data on the neighborhoods themselves and the maps (will be created later) will tell pretty much everything we need to know about **Bellevue**. 

I then filter zipcodes that belong to Bellevue from zipcode dataset to get the average house prices based on zipcode from house price dataset and graph the data as below.

![](/images/king-county/4.2p2.png)

At a quick glance, every neighborhood of the city of Bellevue has a relatively high on the house price average. Specifically, in average, houses in 98004 neighborhood cost about **1.4 million US dollars** and they all seem to set a very high standards for houses in this city. So, my recommendation for buying a house around this city is to have a budget with at least $600k.

### 4.3 Explore the neighborhood
As mentioned above, I use **geopy library** to get the latitude and longitude values of Bellevue City and **folium library** to create the map with its neighborhoods circled as below.

![](/images/king-county/4.3p1.png)

I then use **Foursquare API** to explore the neighborhoods to get to know more about the city itself.

As mentioned above, the **98004** neighborhood seems to be the most expensive neighborhood, let's explore it and see why its property value is that expensive compared to other neighborhoods.

I then set the limit of 100 venues around the neighborhood and the radius of 500m to see the most common venues around this neighborhood. I create URL using 'explore' endpoint and 'get' method to get the results and put it in **pandaFrame**

![](/images/king-county/98004_top10venues.png)

I then use **pandas library** to sort through and get the data of the top 10 most frequent venues around this small neighborhood. Once again, I plot it using bar chart from **seaborn library**

![](/images/king-county/98004_freq_venues.png)

**Observation:** This is a small neighborhood with mostly coffee shop, hotel, stores, mall, and restaurants, etc. saying that this is most likely the city center area, which is for offices, tourists, bussinesses, and large companies. This is the reason why property and houses are very expensive around this neighborhood. 

### 4.4 Cluster the neighborhood
First of, I use **Foursquare API**'s *explore* endpoint to get top 15 venues of **Bellevue** along with the venues' categories.

![](/images/king-county/bellevue_top15_venues.png)

Then I use get_dummies method of **pandas** to one_hot_encode the data. Later, I use groupby method to group the data by 'Neighborhood' and take the mean() of the results to get the frequencies of venues around the city. Below is the data of the most 10 common venues by zipcodes.

![](/images/king-county/bellevue_top10_most_common.png)

I then use k-means clustering algorithm to cluster our neighborhoods. After the clusters generated, I use **Folium** to map the clusters with different colors for different clusters where:
- **RED** : CLUSTER 0
- **PURPLE** : CLUSTER 1
- **GREEN** : CLUSTER 2

![](/images/king-county/clusters_map.png)

This is when I use **WordCloud** to see the insights of clusters and the reasons behind the algorithm runs the way it runs. I also name each cluster based on what I observe. 

***CLUSTER 0 :***

![](/images/king-county/cluster0_cloud_name.png)

***CLUSTER 1 :***

![](/images/king-county/cluster1_cloud_name.png)

***CLUSTER 2 :***

![](/images/king-county/cluster2_cloud_name.png)

Crime count by clusters:

![](/images/king-county/crime_count_by_clusters.png)

## 5.Discussion
---
First of all, King County is one of the best place to live in the US right now. I used public dataset of King County Sheriff's crimes reported in 2019 to conclude that among five major cities of King County, **Bellevue** is the safest city to consider buying properties and settling down with comparatively low crime cases reported in 2019. 

I have noticed that house prices distribution around this city is relatively high, which sets a unique standards for individuals who want to move in this city's neighborhood. **98004** neighborhood is the most expensive neighborhood which has averagely 1.4 million US dollars for a house. For more in depth of King County's house prices analysis and prediction, please visit [HERE](https://github.com/luongtruong7793/House-Sales-in-King-County-WA/blob/master/House%20Sales%20in%20King%20County%2C%20USA.ipynb) 

I also used **Folium** to create maps around **Bellevue** and utilized **Foursquare API** to segment and cluster neighborhoods of **Bellevue** into 3 clusters, each of cluster has its own characteristics. Furthermore, I used **WordCloud** to visualize the frequencies of venues around 3 clusters to emphasize their aspects.

I've then deduced 3 clusters of **Belevue** into:
- **CLUSTER 0: Residential neighborhood by Parks and Playgrounds**
- **CLUSTER 1: City Center and Bussinesses Venues**
- **CLUSTER 2: Residential neighborhood by the Lake**

## 6.Conclusion
---
Based on what we have learned about **Bellevue** city, I would recommend stakeholders and customers (in this case a family of four with upper-middle class income) to consider neighborhood around **CLUSTER 0 (zipcode 98008)** since it has the lowest crime rate, house prices around 600k-700k US dollars, there are parks and playgrounds for kids, etc. 

There is no doubt that **Bellevue** never stops growing since there are many top companies located here (Microsoft, Amazon, T-Mobile, Boeing, etc.) and it is also very close the famous University of Washington. As a data scientist, I have found that **Bellevue** is considered one of the best places for jobs, schools, communities, entertainments, etc.

---
**Thank you for your time!** If you're interested in this project, you can see the source code [here](https://github.com/luongtruong77/House-Sales-in-King-County-WA) on Github, or you can reach out to me [here](https://www.linkedin.com/in/luongtruong77/) on LinkedIn.