# Homework 1 - Professionalism and Reproducibility

## Project Goals:

The Goals of this project are to acquire, process and analyze Wikimedia data. 
This data comes from mobile and desktop viewership of art works from the academy,
more commonly known as the oscars. 


## Terms of Use, License:

1. Wikimedia Foundation REST API terms of use: https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions

2. License
a. Content accessed via the Wikimedia API is licensed under the CC-BY-SA 3.0 and GFDL licenses (See: https://wikimedia.org/api/rest_v1/#/Pageviews%20data)

b. The software is available under Apache 2 license. 


## API Documentation:

1. Pandas: https://pandas.pydata.org/docs/index.html

2. Matplotlib: https://matplotlib.org/stable/

3. Requests: https://requests.readthedocs.io/en/latest/


## How to use this project:

1. Run all cells in "HW2_Data_Collection.ipynb" in the order that they appear.
	- You would observe that the following files are created:


2. Run all cells in "HW2_ORES.ipynb" in the order that they appear.
	- You would observe that the following tables are created.

## Intermediate Files Created Chronologically:



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

Note: The above representation can be represented as a Pandas Dataframe using
"orient = 'records'" argument when reading the json file using 'pandas.read_json'

2. Issues with WikiMedia API timing out:

Do not try to collect the ORES data for all the individual article_titles. 
There are over 22,000 article_titles, and I observed that the API would often
time out with the result "{'httpReason': '', 'httpCode': 429}".

One solution to this problem is to collect the data in batches of a fixed size,
and include a pause for a short duration. This would allow you to obtain the ORES
for the majority of the article_titles. 



