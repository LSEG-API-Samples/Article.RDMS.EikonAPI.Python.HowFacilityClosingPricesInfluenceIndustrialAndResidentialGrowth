# [German Market Trends: How Facility Closing Prices Influence Industrial and Residential Growth - Insights and Forecasts](https://developers.lseg.com/en/article-catalog/article/German-Market-Trends--How-Facility-Closing-Prices-Influence-Industrial-and-Residential-Growth-Insights-and-Forecasts)
## Overview and Hypothesis
Natural gas is a physically traded commodity. Gas consumption and therefore the future price of gas can be described and understood through its “fundamental market drivers” such as volume of supply and weather.

Residential and commercial gas consumption is the amount of gas used by gas consumers connected to the local gas distribution networks as opposed to consumers connected directly to the high-pressure grid, such as power stations and big industrial facilities. Residential and commercial gas consumption forecasts are crucial to operational planning and gas pricing. The difference between sectors in the way they consume gas also determines which fundamental drivers you might want to observe to understand the future supply-demand picture and therefore the price. For instance, residential demand is highly coupled to temperature whereas gas-for-power demand is less coupled to weather but more price sensitive.

To observe any correlation that may exist we can combine in a chart RICs pricing data from the Eikon Data API and consumption actuals and forecasts via RDMS REST API. To demonstrate this, we will consider the largest gas demand Country in Europe, Germany, and compare it to Europe's most liquid future hub price, TTF. The German market actual consumption data is reported by Trading Hub Europe (THE) as two distinct sectors:

- SLP - Residential Local Distribution Zone consumption (LDZ),
- RLM - combination of Industrial and Gas-for-power Consumption (RLM).

Below we will plot the TTF Daily Close price RIC against actual consumption. We will also take this further by benchmarking several available forecasts from RDMS for gas consumption which are produced by the European Gas Team.

## Prerequisites
- A Python environment with the Eikon Data API package installed.
  - Every application using the **Eikon Data API** must identify itself with an Application Key or App Key for short. This Key, that is a unique identifier for your application, must be created using the App Key Generator application in Eikon Desktop/ LSEG Workspace that you can find in via the toolbar of Eikon/ Workspace. How to run it and create an App Key for a new application is available in the [Eikon Data API Quick Start Guide](https://developers.lseg.com/en/api-catalog/eikon/eikon-data-api/quick-start#create-app-key) and here's the [Eikon Data API Overview page](https://developers.lseg.com/en/api-catalog/eikon/eikon-data-api)
  - The list of Python package version can be found in the find requirements.txt in the GitHub code repository attached at the top right of this article
- RDMS account with a subscription to the European Gas Package.
  - **Refinitiv Data Management Solutions** (RDMS), more detail can be found [here](https://www.refinitiv.com/en/trading-solutions/commodities-trading/data-management-solutions-commodities-trading). You will get an account to access the specific instance of RDMS that contains the data set that you're interested in. It's recommended to follow the [RDMS REST API Quick Start Guide](https://developers.refinitiv.com/en/api-catalog/rdms/rdms/quickstart) to do the User Setup, Registration, Manage your account, Access the RDMS Administrator Web Application, and Access the RDMS Swagger REST API Page.
The guide to generating the RDMS API key, which is required for accessing data via RDMS API, is also provided in the quick start guide.
  - Generated API keys for both **Eikon Data API** and **RDMS API** to enable direct data feeds which are stored within a text file named **api_key.txt** on your computer in the format below.
*Replace your valid keys on YOUR_EIKON_DATA_API_KEY and YOUR_RDMS_API_KEY*
```
 ,value  
eikon_api_key,YOUR_EIKON_DATA_API_KEY  
RDMS_api_key,YOUR_RDMS_API_KEY
```

## Step-by-Step Guide to visualizing data
A full article can be found at [German Market Trends: How Facility Closing Prices Influence Industrial and Residential Growth - Insights and Forecasts](https://developers.lseg.com/en/article-catalog/article/German-Market-Trends--How-Facility-Closing-Prices-Influence-Industrial-and-Residential-Growth-Insights-and-Forecasts) and the notebook file with the codes, visualisation, comments, and guidance is uploaded into this repository

This is an example of how to connect to Eikon Data API and RDMS API to plot curve and RICS data together. This will enable the observation of any correlation between price and fundamental price driver.

- We access the TTF Daily close price from the Eikon Data API.
- We access EU Gas Actuals and Forecast data from RDMS.
At the end of this article, we will plot:

1. Actual consumption
2. The latest EC15 and EC46 forecasts
3. A range of EC15 forecasts from the past year
4. We will use the Meta data from RDMS to populate the graph automatically
5. Plot DA close price to observe correlations.

#### Step 1 ) Premble
In this step, we do the package import, API keys loading (from text file api_key.txt), set up connections for RDMS and Eikon Data API, and define the Curve IDs and RICs that we're interested.
- The RIC used in this example is TRNLTTFD1, which is the daily close price of TTF Gas. Below is its information in the Workspace application.
- Curve IDs used here are the German actuals and forecasts of Methane gas of industrial and residenial below is their metadata printed out with the code in the notebook
Two sets of curve ID are used, each three of them are the actual and forecast of Industrial Consumption (RLM) and Redisential Consumption (SLP).
  - First we are going to plot for the SLP, so the RLM set is commented out
  - To plot the RLM, uncomment the RLM set and comment the SLP set out instead
```
"""
# DE RLM
DE_actuals=142173968
DE_Forecast_EC15=117622266 # EC15, freq = hours
DE_Forecast_EC46=117622613 # EC46, freq = twice a week
"""
# DE SLP
DE_actuals=142173967
DE_Forecast_EC15=117622222 # EC15, freq = hours
DE_Forecast_EC46=117622569 # EC46, freq = twice a week
```
#### Step 2) Useful times and dates
Generate the start and end datetime that we are going to retrieve the forecast and actual value. To cover all the data needed, we go back 6 years and forwards to the end of next calendar year. For example, if this year is 2023, we're retrieving the data from year 2017 - 2024.

#### Step 3) My Functions
These four functions are written.
- get_RDMS_meta: retrieves curve metadata from RDMS
- get_RDMS_data: retrieves data from RDMS
- sort_forecast: produces dictonary of forecast data using the datetime stamp as a key to allows easy plotting of various forecasts
- create_daily_forecast: creates a daily forecast from either EC15 or EC46 data i.e. Day ahead forecast is day Which_forecast_day= 1  

#### Step 4) Curves and metadata
Extract the metadata to be used in the graph decoration

#### Step 5) Get RDMS data and format them
1. Get actuals and forcast data from RDMS
2. Sort forecasts in dict and convert to UTC
3. Create a day ahead of forecast
If we take the first day of each forecast, this will be the Day Ahead forecast which will be the most predictive forecast in terms of weather forecast error impact predicability. We can do this for any of our forecasts (EC,GFS, ENS or Op). Here, we use our EC12 forecast which used for our morning comment.

#### Step 6) Get prices
Use get_timeseries function in Eikon Data API to get the close prices of the instrument defined

#### Step 7) Plot actuals and forecast
Using the interactive notebook plots with Matplotlib widget, we do the graph plotting from the data we have retrieved.

## Observations and Conclusions
Using the notebook, we were able to easily plot data using a matplotlib a chart to compare TTF price, actual consumption data and forecast consumption data. Two graphs can be seen in Figures 1 and 2 respectively. Firstly, SLP plotted against TTF and secondly RLM plotted against TTF.
![DE_LDZ_Chart](https://github.com/Refinitiv-API-Samples/Article.RDMS.EikonAPI.Python.HowFacilityClosingPricesInfluenceIndustrialAndResidentialGrowth/assets/89068039/56f58a17-d3ab-4487-a1ff-558154ac2c0a)

**Figure 1** – German SLP demand actuals benchmarked against weather based, EC15, EC46 and Day Ahead forecasts. TTF Daily Close price is also plotted on a second axis.
![de_ldz_chart_2](https://github.com/Refinitiv-API-Samples/Article.RDMS.EikonAPI.Python.HowFacilityClosingPricesInfluenceIndustrialAndResidentialGrowth/assets/89068039/e638bdbc-24e1-47fc-be95-1e210bbc20b2)

**Figure 2** – German RLM demand actuals benchmarked against weather-based forecasts and plotted with TTF Daily Close price.
- Both the EC15 and EC46 forecasts correlate well with historical actuals consumption data.
- SLP and RLM have different consumption profiles.
  - SLP is seasonal, temperature driven and increases significantly in winter.
  - RLM demand has an added weekly periodicity to its profile due to demand being lower at weekends.
- There are clear times of the year such as at the start of the calendar year where the drop in consumption correlates to a general fall in TTF price for both SLP and RLM.
- TTF price in summer 2022 was primarily driven by supply-side concerns and influenced heavily by sentiment-based volatility. This meant prices decoupled from fundamental drivers and prices spiked. This is clear when comparing this to 2023 which saw a more “typical” price profile for TTF which is decreasing in summer as European Storages get filled.

## References
1. EU RESIDENTIAL & COMMERCIAL GAS CONSUMPTION MODEL Methodology paper on Workspace
2. Trading Hub Europe: https://www.tradinghub.eu/

For any questions related to the RDMS and Eikon Data API usage, feel free to visit or ask you question in the [Developers Community Q&A Forum](https://community.developers.refinitiv.com/index.html).
