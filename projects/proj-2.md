---
layout: post
title: 'Clustering Bangalore's Neighborhoods
---

## Introduction

This was the capstone assignment in Coursera's Data Science Professional course.
<br>

### Background
Bangalore (Bengaluru) is a bustling metropolis of India. People from all walks of life find their way into this diverse city. Due to a large tech industry that creates thousands of jobs a year, there is a huge influx in population. 
<br>

#### Problem
Every person that moves in is faced with the question of where to stay. One major factor in determining that is what facilities and attractions a neighborhood offers. If we analyzed different neighborhoods in Bangalore clustered them on their most popular attractions, it could serve as a good starting point in a search for accommodation.  
When business owners determine the location of their store/business they need to look into what other attractions offer. Lack of similar businesses may offer a competitive advantage but might indicate a lack of consumer interest. Similarly a lot of similar businesses can mean either there is a lot of demand and more businesses are welcome or that the market has become saturated.
<br>

#### Interest
Neighborhoods analysis is can be an important tool for businesses in determining where to open their shop. It can also help people moving into a new area find similar neighborhoods that they can choose from
We will be clustering the different neighborhoods in Bangalore based on their 10 most popular attractions.
<br>
<br>
## Data
### Acquisition, Cleaning and Feature Selection
Pincodes and their corresponding post office were obtained from data.gov.in(https://data.gov.in/resources/all-india-pincode-directory-contact-details-along-latitude-and-longitude). 
Only the ‘officename’ and ‘regionname’ columns were chosen from this dataset. This dataset was then filtered to contain only pincodes in the region Bangalore. The post office name was taken as the neighborhood name, and trailing characters were cleaned. Each neighborhood name was geocoded using Nominatim. Then the most popular attractions from each neighborhood were obtained using the Foursquare Places API. The API returned the top 100 results.  Only the venue category was used.  
The resulting dataset was filtered to contain just the top ten attractions of each neighborhood (based on frequency of occurrence). This data was used for further analysis and clustering.
<br>
### Analysis and Preprocessing
The neighborhoods were initially plotted on a map 
<br>

{% include image.html image="projects/proj-2/stretch.jpg" %}

The number of venue categories for each neighborhood was displayed. It was found that there were 172 unique categories.
<br>
{% include image.html image="projects/proj-2/stretch.jpg" %}

The categories were then one-hot encoded as clustering is not meaningful when done on categorical data.  
<br>>
Clustering 
K Means Clustering 
K means clustering was applied to the dataset with varying values of k and the mean squared errors were plotted. There were 104 values in the dataset so the value of k was iterated between 2 and 11 (square root of 104 is 10.2).  The resulting elbow plot was inconclusive so the silhouette score was computed for various values of k. k=8 was determined to be the best choice as it had the highest silhouette score. The resulting clusters were visualized using folium.
