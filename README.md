# Data Acquisition, Processing, and Analysis of the Ohio Train Derailment

This project was completed for the UC Davis STA 220 course in WQ 2023.  
Primary Author: Ada Kanapskyte  
Secondary Author: Kelly Chau

## Introduction

On February 3rd, 2023 a freight train of 38 rail cars– 11 of which carried toxic chemicals– derailed and caused an explosion and fire that lasted for several days in East Palestine, Ohio. A major chemical spill of hazardous chemicals including vinyl chloride, butyl acrylate, ethylhexyl acrylate and ethylene glycol monobutyl ether were released into the air, soil, and water sources. An estimation of 3,500 fish were found dead in local streams and waterways. Exposure to these chemicals such as vinyl chloride are hazardous and are associated with cancers. This was an enormous cause for concern and a threat to public health, especially when these chemicals can spread quickly by the wind. The initial clean-up involved burning off these hazardous chemicals in order to prevent more explosions. However, this resulted in the release of hydrogen chloride into the air, further compounding the severity and scale of the disaster. An evacuation order of 1-mile-by-2-mile radius surrounding East Palestine Ohio was issued. 

In an effort to investigate the hazardous consequences of the disaster, we query data from the web API offered by the Environmental Protection Agency, AirNow, to access and evaluate the air quality before and after the train derailment in several major cities surrounding East Palestine, Ohio. To understand how prevalent such train accidents are in the U.S, we also query publicly available data on train accidents provided by the Federal Railroad Administration Office of Safety Analysis. Our goal in doing this is to better understand the prevalence and spread of train derailments across the US.

## Data Description

We query air quality data from the AirNow API which was developed by the Environmental Protection Agency (EPA) to report levels of ozone, particle pollution, and other air pollutants on the same scale. Daily measurements are reported as the Air Quality Index (AQI) by federal, state, or local air quality agencies for a particular area such as a city or a county. 

We are interested in several air pollutants including fine particulate matter 2.5 microns in diameter or less (PM2.5), particulate matter 10 microns in diameter or less (PM10), and ground-level ozone. 

![aqi-description](aqi-description.png)
_Image taken from the governmental website, AirNow. Air Quality Index (AQI) and the descriptor for each range and category. The higher the AQI, the greater the impact on health. An AQI of 101 or greater is considered by the EPA to be harmful to public health._ 

We are interested in querying for ground-level ozone because it is created when volatile organic compounds and sunlight chemically react, which may be an outcome of the Ohio train derailment. We are also interested in querying for particulate matter because the release and burn-off of chemicals can be a source of particulate matter. 

In an effort to understand the prevalence of train accidents similar to the Ohio train derailment on February 3rd, 2023, we are also querying the number of train accidents that occurred in a single year from January 2022 to December 2022 across the United States through the Federal Railroad Administration Office of Safety Analysis website. The number of train accidents we query for each county in a state include collisions, derailments, and other (accidents not specified by the Federal Railroad Administration website). In addition to accessing the number of train accidents, we also access the Federal Information Processing Standards (FIPS) code which allows identification of the counties in the United States, allowing us to connect the number of accidents to a specific county. 

[The AirNow API can be accessed here](https://gispub.epa.gov/airnow/?contours=ozonepm&monitors=ozonepm&tab=archive&xmin=-10473936.43838753&xmax=-6775607.261838546&ymin=3951206.54221147&ymax=5741667.492762963)
[The Federal Railroad Administration API can be accessed here](https://safetydata.fra.dot.gov/OfficeofSafety/publicsite/query/inctmap.aspx)

## Data Acquisition and Processing

We accessed publicly available air quality data from the governmental website AirNow which is managed by the Environmental Protection Agency. AirNow has an API that allows users to query air quality data. First, we created an AirNow API account and received an API key to access the data. Then, we utilized the [AirNow interactive map](https://gispub.epa.gov/airnow/?showgreencontours=false&xmin=-12594605.351130897&xmax=-5780091.405452674&ymin=3293236.602732849&ymax=6174606.820970087) to identify the city closest to East Palestine, OH that had an air monitoring site and data available. The chosen city was Youngstown, OH. For the data scraping, 6 big cities and/or regions east of Youngstown, with multiple air monitoring sites were selected: Pittsburg, PA, Baltimore, MD, Philadelphia, PA, Dove, DE, New York City, NY and Washington DC. Eastern cities were chosen because of the wind patterns near Youngstown that appear to trend eastward and therefore, pollutants are assumed to also travel eastward along with the wind. The timeline scraping data was selected as 30 days before and after the derailment on February 3rd. Pollutants that were queried, also called parameters, were PM2.5, PM10, and Ozone.

The data scraping for the AirNow API consisted of the following steps:
Creating a function that takes in zipcode and date values as part of a URL which is then sent off as a request to the website API
Creating a function to fetch the data and process it. This consisted of using an html parser and BeautifulSoup to parse the xml response. Regex was utilized to extract parameter, aqi, date, and location values. The processed data was then converted into a dataframe
The function was then used to extract data for each city of interest. The resulting data was compiled into a new dataframe, reindexed and AQI values were converted to integers. 
Next, each parameter - PM2.5, PM10, and Ozone - were located from the large data frame and compiled, sorted, and plotted using python’s plotly express library.
Interactive plots for each parameter were created, allowing each parameter AQI values to be compared between cities.

Next, we accessed train accident data from the Federal Railroad Administration Office of Safety. The website uses an “aspx” format which is somewhat outdated compared to newer website formats. This presented several challenges in identifying how to scrape the website, however train accident data was difficult to come by and this was the most comprehensive source provided by the government. The website was scraped for train accidents with the following parameters: all reporting levels, all states, all track types and starting from January 2022 to December 2022. 

The data scraping for the FRA website consisted of the following steps:
1. Sending a request to retrieve and process a text file provided by the Federal Communications Commission that lists the Federal information Processing System (FIPS) codes for each state and county in the US. FIPS codes allow for unique identification of geographic areas and are useful for mapping county and state data.
2. This FIPS data was then cleaned and split into data frames of county and state data.
Creating a function that utilizes the selenium web driver to remotely select each state and generate a report of train accidents.
    - In this function, the webdriver was structured to take in a state FIPS value, find the state on the website by xpath, wait a selected amount to allow location of the element, and select the state from the dropdown. Then the webdriver clicked a button to generate a report which output a state map (outdated and not always matching the input state) and table of train accident data.
    - The table was selected and the following data was extracted: county names for the state and accident totals for each county (derailments, collisions and other).
    - This data was matched with state FIPS codes and output as a comprehensive data frame.
3. Once this function was run and the data of train accidents for each state and its corresponding counties were collected, the data was converted into a large data frame and processed further 
    - Each county was matched with a corresponding FIPS code.
    - State abbreviations were appended to the large data frame as an additional identifier to the FIPS code. This made it possible to map total train accidents by state later using plotly.
    - Accident values were converted into integers and the data was grouped and sorted.
4. Creating GeoJSON files for county and state boundaries was necessary to plot the processed data on interactive maps. This was achieved by sourcing publicly available maps and converting them into GeoJSON files that could then be imported as templates into plots.
5. Plotly express and its choropleth function were used to create interactive maps of train accidents totals by state and county across the US. A histogram displaying train accident numbers by state was also created to further visualize the data.

## Research Question and Methodology

### Research Question 1
What was the air quality following the Ohio train derailment in the vicinity and east of where the derailment occurred (since wind currents tend to travel east) and how did it compare to the air quality prior to the train derailment?

### Methodologies for Research Question 1
This research question was addressed by selecting 5 cities with numerous air monitoring sites east of the train derailment (Pittsburg, PA, Baltimore, MD, Philadelphia, PA, Dove, DE, New York City, NY and Washington DC) and accessing air quality data (PM2.5, PM10, and Ozone) 30 days before and after the derailment. The city closest to East Palestine that had an air monitoring site and data available was also selected. By using line plots, we can observe the trends and compare the air quality before and after the derailment. The details for the methodology of the data scraping and structuring can be found in the Data and Processing section.

### Research Question 2
What is the prevalence and spread of train accidents across the United States in a single year from January 2022 to December 2022? 

### Methodology for Research Question 2
This research question was addressed by querying train accident data for all states and corresponding counties for the 2022 year. We decided to display the information– number of train accidents– for each state and county on a U.S map with a color coded scheme. This allows visualization of the spread of train accidents across the U.S map. To show the number of accidents more clearly, we also plotted the data on a bar graph. The details for the methodology of the data scraping and structuring can be found in the Data and Processing section.
