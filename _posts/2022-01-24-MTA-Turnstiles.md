---
layout: single
title: "Exploring MTA Turnstiles Data"
category: Projects
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

## Abstract
The goal of this project was to investigate the trend of the New York City metro ridership and factors that contribute to that trend. I worked with turnstile data from the Metropolitan Transportation Authority (MTA), and looked at how the COVID19 pandemic, rideshare services, weather, time of year and time of week potentially affect the subway traffic.

## Design
The COVID-19 pandemic has been devastating to many sectors of the society – public transportation being of them. In this project, the MTA has asked us to investigate when and where they can reduce the number of trains to reduce their budget. Identifying factors that affect the subway ridership will help the MTA maintain their service in a more cost-effective manner.

## The Data
- The primary dataset is the MTA turnstile data of New York City (2015-2022): http://web.mta.info/developers/turnstile.html

	The following columns were used:
	
	| NAME | DESCRIPTION |
	| ---- | ----------- |
	| C/A | Control Area (A002) |
	| UNIT | Remote Unit for a station (R051) |
	| SCP | Subunit Channel Position represents an specific address for a device (02-00-00) |
	| STATION | Represents the station name where the device is located |
	| DATE | Represents the date (MM-DD-YY) |
	| TIME | Represents the time (hh:mm:ss) for a scheduled audit event |
	| ENTRIES | The cumulative entry register value for a device |
	| EXIST | The cumulative exit register value for a device |
	
	The number of passengers in a time interval was obtained by substracting the earlier _ENTRIES_ from the later _ENTRIES_.
	
- NYC weather data from the National Oceanic and Atmospheric Administration (NOAA) website (2017-2019): https://www.ncdc.noaa.gov/cdo-web/search

- Rideshare services data will come from the NYC Taxi & Limousine Commission (TLC): https://www1.nyc.gov/site/tlc/about/aggregated-reports.page

## Tools
Data retrieval was be done via SQLite. Data cleaning was be done via Python’s pandas package. Data visualization was done with Python’s matplotlib and seaborn packages.

## Findings
![Figure 1](https://github.com/JoshJingtianWang/NYC-MTA-EDA/tree/main/plots/COVID_MTA.png?raw=true "Figure 1")

Figure 1 shows the relationship between NYC metro traffic and NYC COVID case daily cases. It seems that every 're-opening event' (metro traffic waves: 2020-08, 2021-05, 2021-11) is immediately followed a wave of COVID cases. This potentially shows opening up the society leads to the spread of COVID.

![Figure 2](https://github.com/JoshJingtianWang/NYC-MTA-EDA/tree/main/plots/Uber_MTA_transparentbg.png?raw=true "Figure 2")

Next, I looked at the relationship between NYC metro traffic and the usage of rideshare services. The conclusions are as follows:
1. Rideshare services have become increasingly popular in the past 6 years, while there seems to be a downward trend of MTA ridership.
2. Rideshare services seem to recover faster from COVID lockdown.

![Figure 3](https://github.com/JoshJingtianWang/NYC-MTA-EDA/tree/main/plots/Weather_MTA.png?raw=true "Figure 3")

Figure 3 shows that bad weathers negatively affect MTA ridership, while warmer weather increases ridership. It can also be seen that the data points are clustered into two groups: weekdays and weekends.

![Figure 4](https://github.com/JoshJingtianWang/NYC-MTA-EDA/tree/main/plots/Temperature_MTA.png?raw=true "Figure 4")

Zooming in on the temp vs MTA plot, we see a slight drop of ridership in summer months (June and July). This could be due to the summer break of universities in NYC.

![Figure 5](https://github.com/JoshJingtianWang/NYC-MTA-EDA/tree/main/plots/year_heatmap_2019.png?raw=true "Figure 5")

In Figure 5, I compiled the daily ridership of 2019 into a [calendar plot](https://calplot.readthedocs.io/en/latest/). We can see the drop of ridership on weekends and holidays when people do not go to work.

![Figure 6](https://github.com/JoshJingtianWang/NYC-MTA-EDA/tree/main/plots/week_heatmap_2019.png?raw=true "Figure 6")

Lastly, I looked at the metro ridership of an average week in 2019. It is clear that ridership is low during night time and weekends. Unexpectedly, ridership from 4-8am on weekdays is relatively low. This could be due to the fact that I only used _ENTRIES_ to calculate metro traffic. Because of that, traffic coming from outside of NYC limits in the mornings may not have been considered. In the future, it could be interesting to look at _EXITS_ in the MTA dataset.

## Conclusions
- COVID waves and rideshare services potentially affect NYC metro ridership
- Weather may not be a strong enough factor to warrant train scheduling change
- Fewer trains can be scheduled on holidays, weekends and at nighttime.

Thank you for reading. The code for my project can be found [here](https://github.com/JoshJingtianWang/NYC-MTA-EDA). 