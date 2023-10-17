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

1. Run all cells in "HW1_preprocessing.ipynb" in the order that they appear.
	- You would observe that the following files are created:
		a. "academy_monthly_mobile_201501-202310.json"
		b. "academy_monthly_desktop_201501-202310.json"
		c. "academy_monthly_cumulative_201501-202310.json"

2. Run all cells in "HW1_Analysis.ipynb" in the order that they appear.
	- You would observe that the following graphs are created:
		a. Maximum Average and Minimum Average
		b. Top 10 Peak Page Views
		c. Fewest Months of Data

## Intermediate Files Created Chronologically:

1. desktop_data.json: created to hold the desktop data acquired through the WikiMedia API

2. academy_monthly_desktop.json: modified version of desktop_data.json, complies with json structure.
 
3. all_data.json: created to hold the cumulative data acquired through the WikiMedia API.

4. academy_monthly_cumulative.json: modified version of all_data.json, complies with json structure. 

5. mob_web_data.json: created to hold the mobile-web data acquired through the WikiMedia API

6. mob_web.json: modified version of mob_web_data.json, complies with json structure. 

7. mob_app_data.json: created to hold the mobile-app data acquired through the WikiMedia API

8. mob_app.json: modified version of mob_app_data.json, complies with json structure. 


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

2. Pandas Interpreting Timestamps as Epoch Times:
The Wikimedia API represents timestamps as "YYYYMM00", however, this timestamp
is interpreted by pandas to be in Epoch Time. 

Example: '20200100' would be interpreted by pandas as '2034-01-04 11:55:00'

The timestamp would have to be converted back into the original timestamp for
meaningful time-series analysis. A solution to this problem is used in "HW1_Analysis.ipynb"


