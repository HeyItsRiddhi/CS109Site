## Introduction

Welcome to Voter Turnout CS109 Data Science Project with this project we choose to look at minority ethinicity's voter turnout in elections and build a prodiction model that can predict minority ethnicity turn outs for future elections! Due to the time constraints as well as the limitation of data available in about some states i.e paying for state data we choose a swing state as the focus of our project. 

We choose Florida as it has a diverse population throughout the state which makes it a good representative of the country. and Different districts in Florida have different build up of ethinicities such that some district are predominately white whilt other are majority minorities. 

The figure below shows the diversity of Florida:

![](FloridaMinority.jpg)

## Data Exploration

The data for this project came from census data for the state of Florida but the quite extensive and spilt into two data files thus we need to merge the relevant data from both files into a single file. 

#### Data Gathering

The first data set contained information about "Voter History" from the past 85 elections in Florida from 2006 - 2016. This data set contained variables for when the individual vote i.e general election, their district and type of vote. Below is an example of what the data looked like in this data set. 

![](VoterHistoryDS.png)

The second data set contained information about the voter such as his or her address, district, date of birth and so on.Below is an example of what the data looked like in this data set. How ever a lot of there columns were data that could be derieved from other columns, duplicates or data thatnot predictive of voter turn out thus moved on to clean the data to make it focused.

![](VoterRegDS.png)

#### Data Cleaning

The first step we took in data cleaning was to merge the two files. We merged on id as upon look at the two data sets we saw that each voter had an id which was unique and this id was in both file to represent that particular voter. This merge was also done to capture all registered voters in the state regardless of whether he/she voted in a particular election. If the individual did not vote he/she would have an NaN under the election data column (number 22 below). Upon merging we have 250 million rows of voters for the entire state so we quickly moved on to drop column that we deemed were represented by other column or not predictive of voter turn out. For example we dropped street address as it is not really relevant to whether the individual will voter and is specific to each voter whereas we kept zip code because sip code can represent entire demographic or ethnicity and thus can play a role in voter turn out. Similiarly we dropped middle name as we already have id to indetify the use and first and last name thus keeping it would be repeatitive. Below is an image of the data frame after this cleaning.

![](Clean1.png)

We knew that this was still a huge data set to work with and it contained a lot of duplicaties. Thus we moved on to further clean the data by reducing the data to just include the general election by removing instances of “PPP (special, local, etc.), “PRI” (Primary Elections), and “OTH” (Other). The data was reduced to include only general elections because historically the the general election has the highest turnout and because Florida has a closed primary which heavily affect the primary elections voter turnout. Below is an image of what the data set looked like after this cleaning. 

![](Clean2.png)

As you can see  above Jaqueline votes frequently thus appear multiple time in the data set thus our next step was to intelligently had these duplicates such that each voter has only a single row! In order to do this we created column for each general election i.e "GEN16" and "GEN12" and indicated in this column whether the individual voted or not. This voted or not was determined by looking at the typeofvote section and filling it in the correct election column. Futhermore NaN were not removed as they represent individual that registered but did not vote which we thought was relevant thus NaN were instead turned into 0 and a vote was turned into 1 as shown in the secon image below.

![](Clean3.png)

![](Clean4.png)

Upon doing the above clean where an individual would have a 1 if he or she voted, made us notice some inconsitencies in teh data where we notice that individual can move district or switch political parties in between two election i.e general 2012 and general 2016. Thus we decide to employ the same clean method as described above but focus solely on 2016 thus removing the issue of voter changing district or parties. However, given more time we would like to look at each individual year i.e 2012. In addition, based off of "date of birth" we decided to add a column called age tot he data set so that we can see how voter turnout in various ethnicities varies later in our model. Lastly we choose to subsample the 10 million+ rows data set into a 100 000 samples, as logistic regression is a parametric test requiring a large sample size. We will also be using a large number of dummy variables, so our sample size needed to be larger than our number of predictors. 

Below shows is an image of our data with the newly created age column:

![](Clean5.png)

#### EDA

Now that we had a clean sampled data set, we began to explore the data for the 2016 election to see how various variable such as gender affected voter turnout. 

Below is a graph show the voter turnout based on gender in the 2016 Florida general election. Based off this data we can immediately see that slightly more women voted then men in 2016, perhaps this has something to do with the candidates thoguh we won't talk about that here. 

![](GenderPlot.png)

Next we looked explore how different age group voted and noticed that as the age group increased the percentage of voter turnout increased with a dramatic increase between the "under 35" age group and the "35 to 45" age group and a slight increase in all age groups after. 

![](AgePlot.png)

Then we moved on to see the vovter turnout per district, to see if particular district had significantly higher or lower turnout then others in the state. From this exploration we found LIB had the highest voter turnout in Florida's 2016 general election and GLA had the lowest. 

![](DistrictPlot.png)

Next, we looked at the type of vote distribution in the 2016 election and notice that most individuals registered did intend vote and that most individual voted at the polls, while 31% voted early and 28.3 voted absentee. Essentially, difference betweent the types of voting methods was not significant but it is important to note thata small fraction of registered voter did not vote.

![](TypeVotePlot.png)

Lastly, we looked at the breakdown of voters by "race" and the the percentage of each race that voted. This showed that a high percentage of White, non hispanics voted and but this proporation was not significantly higher then the proportion that voted in other ethinicties. Plot 1 below show the racial make up of the voter of Florida and Plot two shows the proportion of each race that voted.

![](RacePlot1.png)
![](RacePlot2.png)

## Models

All models were built using exclusively our 100,000 sample from the Florida dataset. This was done by using a sample representative of the population.

#### Logistic Regression

Our analysis uses the following variables in different analysis: District, political party, age, and gender and race to predict voter turnout. The result of this is included in Table X.  

These analyses were conducted using the 100,000 sample gathered in the data cleaning process.  While our 100,000 sample had 431 missing values in our “age” variable, a further look demonstrated that no more than 3% of any district was missing age. Most districts were missing less than 1%, while Washington County (WAS) was missing 3%. This suggests that the missing data is fairly evenly distributed across our counties, and relatively few instances, so we feel comfortable dropping these registered voters (rows). The only other rows containing missing values were columns dropped (like “First Name”). Dummy variables were implemented for categorical variables with more than 2 options (for example, gender was not a dummy variable).  All logistic regressions were performed with  5- fold cross validation with different alphas (1, 10, 100, 10000, or 100000) and L2 regularization. The scoring metric was “area under the curve.” 

#### Assumptions of Logistic Regression

Logistic regression requires the dependent variable to be binary. Here, we are predicting whether of not a given person voted in the 2016 general election. The independent variables should be independent of each other. That is, the model should have little or no multicollinearity. The independent variables are linearly related to the log odds. Logistic regression requires quite large sample sizes, as the number of samples needs to exceed the number of prodictors (after dummy varibales are created)
Because logistic regression uses MLE rather than OLS, it avoids many of the typical assumptions tested in statistical analysis. Does not assume normality of variables (both DV and IVs). Does not assume linearity between DV and IVs. Does not assume homoscedasticity. Does not assume normal errors. MLE allows more flexibility in the data and analysis because it has fewer restrictions.
