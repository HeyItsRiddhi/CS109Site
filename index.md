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


#### EDA

## Section Name
#### Subsection
