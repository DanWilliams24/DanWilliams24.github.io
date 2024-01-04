---
layout: page
title: COVID-19 Risk Data Analytics
description: Using Random Forest and SVMs to classify COVID Risk by U.S County.
img: https://i.imgur.com/rpTO1EH.png
importance: 1
permalink: /projects/covid19
category: Social Impact
related_publications: einstein1956investigations, einstein1950meaning
---

The COVID-19 Pandemic had a widespread impact across the United States during 2020. It is clear that while the pandemic affected all counties throughout the US, some counties were hit harder than others. We sought to answer the following three questions in particular:

Which counties were hit the hardest?
What factors were the cause?
Why were some counties hit harder than others?
Attempting to answer these questions precisely reveal a complex reality that takes into account public health policy, viral transmission, social structures/dynamics, inequity, population demographics among an enumerable number of other risk factors. We generalize this problem as so: RQ: What types of indicators can we use to identify high risk counties, and classify them into risk groups?

In this project, we present a Machine Learning method that identifies high risk counties, as well as classifies US counties into risk groupings for all counties reporting data to the CDC. As a whole, our method is able to:

Derive high level normalized county statistics from large patient level datasets
Perform data cleaning and ground truth label assignment through a numerical scoring method
Predict and visualize county risk groups clearly and at high accuracy.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>


Methodology
---
### Stage One - Cleaning
Firstly, we derive county statistics from patient level data. More information on the data sources we use can be found in the [Data Sources section](#Data-Sources). For each county, we tally up stats for each of the features used (total # of cases, # of hospital admittance, # of people with underlying conditions, # of deaths ). After this procedure we normalize the totals by the individual counties population, to account for population differences between areas. Finally, we compose a new compiled dataset that contains the stats for all us counties for which there is data. This is used for stage two.

### Stage Two - Training
One of the first things we need to do with the compiled dataset is assign ground truth class labels for training. To do this, we perform a K=3 binning on individual features in the data to develop a small scoring metric. The scoring metric serves as a way to decompose a county's multidimensional performance in terms of case rate, mortality rate, etc, into a 1 dimensional score that is descriptive enough that it can be used to assign a class label.


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="https://i.imgur.com/AHjL3vp.png" title="class labels" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>


Next, given that there are only so many counties (3,006 total, but less represented based on case reporting) and the nature of our classification task means that there are fewer high risk counties compared to low risk and moderate risk, we are dealing with a unbalanced multi-classification problem. Specifically, one we solve by using Synthetic Minority Oversampling Technique(SMOTE) with combined under and over sampling. SMOTE is a type of data augmentation technique to even out the number of datapoints in each of our classes to improve training overall.


After SMOTE resampling class 3 which started off with only 17 datapoints.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="https://i.imgur.com/oNXQWmw.png" title="SMOTE" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>


After resampling, we can then feed the datapoints into a Random Forest Decision Tree classifier from scikit-learn library.
The Random Forest pipeline will use N estimators to learn 3  partitions corresponding to Low, Medium, and High risk. Once trained, it simply assigns an additional column to each county data record corresponding to class prediction. From there, we can plot(see top of README) and evaluate our model's results.

Results + Analysis
---
By using intuitive indicators like county total cases, county mortality, county hospital admittance, and county underlying conditions [all normalized by each county’s population], we are successful in classifying each county into three separate levels of risk.
Looking at the results, our classifier very clearly highlights the severe and disproportionate impact of COVID-19 across counties in the US due to various factors. From this, we get a very good idea of where to look. For instance, the predictor shows that quite a few counties in New York City ranked as 'high' risk areas. From here we can investigate reasons for the trend that our model illuminates. We can look into other stats such as population density, age groups, and demographics to get a detailed idea as to what exactly makes these counties high risk for COVID. Specifically, it is likely that one of the leading reasons for the high risk classification here can be attributed to a high population density which is linked to higher transmission rates due to inability to social distance.

Overall, We are successful in our aim, which was to get a comprehensive view of the risk for each county, insightfully showing each counties performance in pandemic response. We created a classifier that provides answers to our three initial questions from earlier:
- Which counties were hit the hardest?
- What factors were the cause?
- Why were some counties hit harder than others?*

Looking forward, this gives us a clear idea of which counties need additional resources to get COVID transmission and case severity under control, as well as likely county risk groups for future national emergencies. 




Data Sources
---
Main Dataset -
> Read more about the CDC's COVID-19 Case Surveillance Public Use Data with Geography here: https://data.cdc.gov/Case-Surveillance/COVID-19-Case-Surveillance-Public-Use-Data-with-Ge/n8mc-b4w4

Census Data -
> Census Tables: https://www.census.gov/data/datasets/time-series/demo/popest/2010s-counties-total.html#par_textimage_70769902
Download Link: https://www2.census.gov/programs-surveys/popest/tables/2010-2019/counties/totals/co-est2019-annres.xlsx


###### tags: `Classification` `Machine Learning` `BYTES` `Documentation`