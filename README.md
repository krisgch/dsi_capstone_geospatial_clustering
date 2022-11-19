# DSI CAPSTONE - Geospatial Clustering to label grids

## Table of Content
[1. Executive Summary](#Executive-Summary) <br>
[2. Data Dictionary](#Data-Dictionary) <br>
[3. Repository Structure](#Repository-Structure) <br>
[4. File Description](#File-Description)

---

## Executive Summary

**GeoPulse** is a smart platform utilizing locational data, mobility data, internet data, and other sources combining them into a product specialises in analysing customer's persona within a specific area (referred to as grid) at a specific time. Currently **GeoPulse** has various use case in many industries that looks to utilize customer persona information such as businesses looking to open/close their branches, marketing strategy planning based on customer persona at each location, and foot traffic comparison for dynamic pricing of media products.

The approach taken in this project was different from a typical Data Science project as show below: <br>

1. Obtain the data from GeoPulse team
2. Explore the data
3. Formula business ideas
4. Model to explore
5. Evaluate to verify
6. Conclude on business opportunities

Upon exploring the data there are a few limitations as listed below: <br>
1. Encoded area code makes locating grids relative to each other impossible
2. Information was acquired from only 1 out of 4 customer segments
3. Information was acquired from only 1 out of 24 hours of the day

Leaving us with approximately 1% of the available data. 

Data was prepared by binning the data set into 4 subsets separated by customer segments (worker, visitor, resident, work at home).

Unsupervised learning model used in this project is KMeans where the optimial number of clusters, identified by 'elbow method' and 'silhouette coefficients', is 4 for all segments.

Each segments were then clustered and label, and those labels were used as a target for Random Forest Classifier to verify how well the clusters are separated as well as identify the most important feature ('Population Density').

After labeling each subsets, the label were evaluated and relabeled using a more user friendly terms such as 'low density residential area'. These were decided using the following criteria: <br>

Density: <br>

- Low <= 40
- 40 < Medium <= 80
- 80 < High

Cluster Type: <br>

- Residential : Have residents all day except during working hours
- Community : Have residents all day including some residents during working hours (this grid has work available)
- Office : Worker mostly during work and afterwork
- Community_office : Worker approximately the same during morning and work time
- Public : Visitors during all hours (Note: at medium or high density)
- Others : Visitors during all hours (but low density level)

This method allows GeoPulse to expand their businesses to new partners without having to invest further into new data collection. Potential uses are: <br>
- Logistic company wanting to improve the delivery time by optimizing their offline branches/warehouses, and ultimately improving their customer satisfaction <br>
"Identify high density residential and community area"

- Dynamic pricing based on location and grid types <br>
"Different prices for worker-oriented accomodations around high density office area"

Note that with actual dataset, the grid types and density could be verified much more accurately and diversely.

---

## Data Dictionary

| no | Column name | Column Description|
|---|---|---|
| 1 | id | area id|
| 2 | hour | hour of data|
| 3 | f_profile_customer_segment | There are four main location-based customer segments: Residents, Workers, Work at Home, Visitors - Residents are people who spend home hours in the area, Workers are people who spend work hours in the area, Work at Home refers to people who are both residents and workers in the area, and Visitors are people who are neither residents or workers in the area |
| 4 | Density | Number of customer who visited in the area|
| 5 | prepaid_user | prepaid subscription|
| 6 | postpaid_user| postpaid subscription|
| 7 | gender_male| the gender distribution of customer who visited in the area|
| 8 | gender_female ||
| 9 | gender_unknown||
| 10| age_1_19 | the age distribution of customer who visited in the area|
| 11 | age_20_29||
| 12 | age_30_39||
| 13 | age_40_49||
| 14 | age_50_59||
| 15 | age_more_than_60||
| 16 | 0_no_payment| the monthly bill payment distribution of TrueMove H customers in the area: Prepaid - Monthly Top Up, Postpaid - Monthly Bill Payment, No Payment refers to Prepaid users who did not top up in that month and Postpaid users who did not pay in that month (paid in advance/paid late).|
| 17 | 1_pay_0to99||
| 18 | 2_pay_100to199||
| 19 | 3_pay_200to599||
| 20 | 4_pay_600to999||
| 21 | 5_pay_1000_ ||
| 22 | music_streamer_user| Number of customer who have usage on music application (JOOK,spotify,coolism,etc.)|
| 23 | video_streaming_app_user| Number of customer who have usage on VDO streaming application (Netflix,TrueID,WeTV,etc.)|
| 24 | merchant_app_user| Number of customer who have usage on merchant application (SCB Mae manee,KTB pao tung,etc.)|
| 25 | bank_app_user| Number of customer who have a usage on bank application (SCB,K Plus,KTB, etc.)|
| 26 | fastfood_app_user| Number of customer who have a usage on Fast food application (The Pizza Company,Mc Donalds,KFC,etc.)|
| 27 | food_delivery_app_user| Number of customer who have a usage on Food Delivery application (GRAB,LINE Man,Foodpanda,etc.)|
| 28 | grocery_delivery_app_user| Number of customer who have a usage on grocery Delivery application (7-11,Makro click,Lotus's,etc.)|
| 29 | online_shopping_user| Number of customer who have a usage on Online shopping application (Shopee,Lazada,JD Central,etc.)|
| 30 | inte_coffee_lover| Number of customer who are interested in coffee.|
| 31 | inte_travel_and_information | Number of customer who interested in travel.|
| 32 | inte_investment             | Number of customer who interested in investment (Finance Investment,Digital Currency,etc.)|
| 33 | inte_realestate             | Number of customer who interested in realestate.|
| 34 | inte_insurance              | Number of customer who interested in insurance.|
| 35 | inte_automobile             | Number of customer who interested in automotive(car band and car content). |
| 36 | inte_fitness_and_wellness   | Number of customer who interested in fitness and wellness.|
| 37 | par_day                     | date of data (2020 - Nov - 04 : Wednesday)|

---

## Repository Structure 
```
dsi_project_nlp
|
|__ code
|   |__ 00_data_cleaning.ipynb  
|   |__ 01_exploratory_data_analysis.ipynb
|   |__ 02_regression_check.ipynb
|   |__ 03_clustering_by_density_bin_segment.ipynb
|   |__ 04_clustering_by_density_bin_time_of_day.ipynb 
|  
|__ datasets
|   |__ reducted for security reasons
|
|__ README.md

```
<br>

---

## File Description
<br>

    00_data_cleaning.ipynb
- Combining files and cleaning up

<br>

    01_exploratory_data_analysis.ipynb
- Exploratory Data Analysis to understand the limitations of dataset

<br>

    02_regression_check.ipynb

- Experimental regression on the dataset to verify how each app usage / preferences columns can be predicted (spoiler: easily predicted based on just the population density)

<br>

    03_clustering_by_density_bin_segment.ipynb
- Clustering and relabeling based on subset division by customer segment

<br>

    04_clustering_by_density_bin_time_of_day.ipynb

- Clustering and relabeling based on subset division by time of the day