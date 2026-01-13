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

![aqi-description](aqi-description.png).
Image taken from the governmental website, AirNow. Air Quality Index (AQI) and the descriptor for each range and category. The higher the AQI, the greater the impact on health. An AQI of 101 or greater is considered by the EPA to be harmful to public health. 

We are interested in querying for ground-level ozone because it is created when volatile organic compounds and sunlight chemically react, which may be an outcome of the Ohio train derailment. We are also interested in querying for particulate matter because the release and burn-off of chemicals can be a source of particulate matter. 

In an effort to understand the prevalence of train accidents similar to the Ohio train derailment on February 3rd, 2023, we are also querying the number of train accidents that occurred in a single year from January 2022 to December 2022 across the United States through the Federal Railroad Administration Office of Safety Analysis website. The number of train accidents we query for each county in a state include collisions, derailments, and other (accidents not specified by the Federal Railroad Administration website). In addition to accessing the number of train accidents, we also access the Federal Information Processing Standards (FIPS) code which allows identification of the counties in the United States, allowing us to connect the number of accidents to a specific county. 

[The AirNow API can be accessed here](https://gispub.epa.gov/airnow/?contours=ozonepm&monitors=ozonepm&tab=archive&xmin=-10473936.43838753&xmax=-6775607.261838546&ymin=3951206.54221147&ymax=5741667.492762963)
[The Federal Railroad Administration API can be accessed here](https://safetydata.fra.dot.gov/OfficeofSafety/publicsite/query/inctmap.aspx)

