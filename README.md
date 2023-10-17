# Homework 2 - Considering Bias in Data

## Project Goals:

The Goals of this project are to acquire, process and analyze Wikimedia data corresponding to cities in states of the US.
This data comes from the cities and their respective states' wikipedia pages in the US.
In addition, the ORES machine learning model is used to evaluate the article quality
for each (city, state) combination. The goal is to obtain insights relating to the coverage and quality of the articles
across states.

## Terms of Use, License:

1. Wikimedia Foundation REST API terms of use: https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions

2. License
a. Content accessed via the Wikimedia API is licensed under the CC-BY-SA 3.0 and GFDL licenses (See: https://wikimedia.org/api/rest_v1/#/Pageviews%20data)

b. The software is available under Apache 2 license. 


## Data Sources: 

1. List of cities in the US by state: https://en.wikipedia.org/wiki/Category:Lists_of_cities_in_the_United_States_by_state

2. Population Data: https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html

3. Regional Division: https://en.wikipedia.org/wiki/List_of_regions_of_the_United_States#Census_Bureau%E2%80%93designated_regions_and_divisions


## API Documentation:

1. Pandas: https://pandas.pydata.org/docs/index.html

2. Matplotlib: https://matplotlib.org/stable/

3. Requests: https://requests.readthedocs.io/en/latest/

4. Wikimedia: https://www.mediawiki.org/wiki/API:Main_page

5. ORES API: https://www.mediawiki.org/wiki/ORES


## Procedure to Obtain Wikimedia API Access:

You will need a Wikimedia user account to get access to Lift Wing (the ML API service). You can either create an account or login. If you have a Wikipedia user account - you might already have an Wikimedia account. If you are not sure try your Wikipedia username and password to check it. If you do not have a Wikimedia account you will need to create an account that you can use to get an access token.

There is a 'guide' that describes how to get authentication tokens - but not everything works the way it is described in that documentation. You should review that documentation and then read the rest of this comment.

The documentation talks about using a "dashboard" for managing authentication tokens. That's a rather generous description for what looks like a simple list of token things. You might have a hard time finding this "dashboard". First, on the left hand side of the page, you'll see a column of links. The bottom section is a set of links titled "Tools". In that section is a link that says Special pages which will take you to a list of ... well, special pages. At the very bottom of the "Special pages" page is a section titled "Other special pages" (scroll all the way to the bottom). The first link in that section is called API keys. When you get to the "API keys" page you can create a new key.

The authentication guide suggests that you should create a server-side app key. This does not seem to work correctly. BUT, there is an option to create a Personal API token that should work for this course and the type of ORES page scoring that you will need to perform.

Note, when you create a Personal API token you are granted the three items - a Client ID, a Client secret, and a Access token - you should save all three of these. When you dismiss the box they are gone. If you lose any one of the tokens you can destroy or deactivate the Personal API token from the dashboard and then create a new one.

The value you need to work the code below is the Access token - a very long string.


## How to use this project:

1. Run all cells in "HW2_Data_Collection.ipynb" in the order that they appear.

2. Run all cells in "HW2_ORES_Analysis.ipynb" in the order that they appear.
	Note: Replace the Wikimedia API credentials with your own email and access token.


## Intermediate Files Created Chronologically:

1. 'wiki_city_data.json' - Data obtained through Wikimedia API with the lastrevid being of particular importance

2. "parsed_wiki_city_data.json" - File obtained upon modifying the file in (1) to ensure all JSON formatting requirements are obeyed (Used for ORES API calls). 

3. "Final_States_Data.csv" - CSV containing state, article_title, url, regional division, population (2022), lastrevid 

4. 45 files named like "score_files_{i}.csv" where i is an integer from 0 to 44.
	For 22157 articles, we create 45 files. 
	The files are cumulative (i.e. Each file has the records from the previous file and 500 new records)
	score_files_44.csv contains all the data from all previous files, and hence we delete all of them except score_files_44.csv

5. "wp_scored_city_articles_by_state.csv" - Final CSV required as part of submission containing state, article_title, regional_division, population, revision_id, article_quality.


## Consolidated file:

A csv file "wp_scored_city_articles_by_state.csv" is created. This file has the following schema:

- state: State in the US
- regional_division: Regional Division to which the state corresponds to
- population: Population of the state in 2022
- article_title: The title of the Wikipedia article corresponding to a 'City, State' combination.
- revision_id: revision ID of an article. This revision ID is used by the ORES API to score an article
- article_quality: Quality Rating of a Wikipedia Article based on the ORES scores. The article quality estimates are mapped as follows:
	1. FA - Featured article
	2. GA - Good article (sometimes called A-class)
	3. B - B-class article
	4. C - C-class article
	5. Start - Start-class article
	6. Stub - Stub-class article


## Issues worth considering:

1. Non-compliance with JSON format:
When the data is acquired from the API as is, it is in the form:

{
	JSON
}{
	JSON
}{
	...
}

Which is invalid.

Any user would have to modify the structure of the file in order to comply with 
json rules. One solution is to modify the structure of the data as follows:
[
	{
		JSON
	},
	{
		JSON
	},
	{
		...
	}
]


2. Issues with ORES API timing out:

Do not try to collect the ORES data for all the individual article_titles. 
There are over 22,000 article_titles, and I observed that the API would often
time out with the result "{'httpReason': '', 'httpCode': 429}" in place of the scores.

One solution to this problem is to collect the data in batches of a fixed size,
and include a pause for a short duration. This would allow you to obtain the ORES
for the majority of the article_titles. In addition, you could run this process repeatedly
such that the API is called specifically for those articles that do not have a score.
Repeat this till you get a satisfactory number of scores for the articles. 

It is suggested to take into account the frequency of your requests. Too many requests
or a high rate of requests may lead to temporary blocking. I encountered this issue, 
and did not get ORES scores for about half the article_titles until I altered my approach as above.


## Research Implications:

Findings:

Coverage and Quality:
Upon Inspection of the tables generated during analysis, it is observed that the states with the highest coverage also tend to have
higher quality. An appreciable overlap was observed in the results of tables 1 and 3. This also holds true for states with the lowest
coverage having lower quality. 

The states with the highest coverage interestingly had a substantially higher number of articles, coupled with a small population, whereas
the states with the lowerst coverage had a substantually low number of articles, while having a significantly larger population than those
with high coverage. 

Interestingly, population appears to have a negative relationship with the article_quality and even the number of articles per state. 


Surprises:
As mentioned earlier, I expected states with a larger urban population to have higher coverage and article quality. To that end,
I expected western and eastern states to dominate the top coverage and quality tables. However, I was surprised to find that
the western regions had rather poor coverage AND quality. 

In contrast, I did not expect the midwest region to do particularly well, much less outperform the western regions.


Theories for some results:

1. It is observed that the states with the top quality and coverage per capita had a small population but a larger number of articles. 
I believe it may be because the efforts of a few in these regions decided to commit themselves more than those in higher population density regions,
who might put off their desire to work on contributing under the idea that several other individuals in their already densely populated area would 
have done a better job than they could have. 

Another explanation is that the larger population could correspond to a huge number of contributors, making it difficult to accommodate or address
the much larger number of contributions.


On Dealing with Bias:

1. What biases did you expect to find in the data (before you started working with it), and why?

Response:
I expected the population and level of urbanization by state to play a role in showcasing bias in the data.

I expected west and northeast regions to have a high coverage and article quality. Admittedly, this comes from my own opinions formed
over the course of daily life. Logically, I expected the said regions to have higher coverage and quality because these regions have some 
of the largest cities in the US, and are also home to some of the world's most renowned educational institutions. I expected these regions'
communities to have a higher emphasis on education, access and adeptness at effectively handling computer systems, and an inclination to 
contribute to the Wikipedia page of their cities.

However, this does not seem to be the case. It appears that the efforts to maintain an article corresponding to a large city comes with its
problems of addressing suggestions/revisions at a much larger scale (assuming that the per capita 'effort' across regions is the same).
Or, it is possible that the fewer contributors in the smaller cities tend to make frequent and quality additions to the pages that they reside in.


2. What (potential) sources of bias did you discover in the course of your data processing and analysis?

Response:
Population Density: The population density corresponding to each city in each state could influence the frequency of article updates, and 
thus affect the quality if there are too many contributors.


3. What might your results suggest about (English) Wikipedia as a data source?

Response:
Wikipedia being a platform that relies on its users to update information, is not free of bias. In context of cities and states articles, 
the population in each city and by extension state, would affect the level of contribution as a whole and on a per-capita basis for a city.
This could be extended to any topic receiving traction proportional to its popularity in the public. 

That being said, Wikipedia is still a great platform for someone to use as a starting point for their work in a sense that it could guide their
next course of action, or for the reporting of results as they are. 


4. Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might still be appropriate and useful, despite its inherent limitations and biases?

Response:
Business Decision:
Suppose a new company specialized in assisted writing, wants to expand into a new region. They could use the population density, coverage, and quality
data to target a region for which the articles have a high coverage but a lower quality. These biases are created because of some genuine circumstances
that this company can capitalize on.

5. How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed?

Response: 
Most sources of data tend to carry some form of bias. It would thus be prudent to use multiple data sources that could supplement this data, especially for states and regions that are predominantly underrepresented. 
