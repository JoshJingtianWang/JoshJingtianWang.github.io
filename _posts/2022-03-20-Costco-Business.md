---
layout: single
title: "Helping Costco Adapt to the Work-From-Home Market"
category: Projects
tags: [EDA, Web Scraping, Tableau]
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
#classes: wide
tagline: "The WFH trend is here to stay, but how can Costco capitalize on it?"
header:
  overlay_image: /assets/images/Costco/costco.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Unsplash**](https://www.unsplash.com/)"
  teaser: /assets/images/Costco/costco.jpg
  og_image: /assets/images/Costco/costco.jpg
---

## Abstract
As a wholesaler, Costco excels in selling large quantities of selected goods at a cheap price, and because of that, Costco’s shelf space cannot accommodate a large selection of brands. Costco needs to efficiently allocate its limited shelf space to goods specific to the regions of the United States. Particularly, Costco is interested in tapping into the work-from-home (WFH) market and wants to know which region/demographic of the US has an emerging WFH population.

See the presentation slides here([.pdf](https://github.com/JoshJingtianWang/Costco-Business/blob/main/presentation_josh_wang.pdf)/[.ppt](https://github.com/JoshJingtianWang/Costco-Business/blob/main/presentation_josh_wang.pptx)).

## Design
My work aims to inform Costco on which region of the US they should build new warehouses in, and in the existing warehouses, which ones they should adjust their catalog to accommodate the WFH customers. Adjusting warehouse location and catalog for the WFH customers will help reduce Costco’s shipping cost and attract more customers.

![opportunites](/assets/images/Costco/opportunities.jpg "opportunities")

## The Data
- US Census Pulse Survey Data: 
https://www.census.gov/programs-surveys/household-pulse-survey/data.html
- Costco Warehouse Location data: https://www.costco.com/WarehouseListByStateDisplayView
- Google Mobility Data: 
https://www.google.com/covid19/mobility/

## Tools
Costco Warehouse Location data was scraped using BeautifulSoup and Selenium. Data cleaning and visualization were done using Excel and Tableau.

## Findings
*1. The WFH trend has been growing since before the pandemic.*

<iframe seamless frameborder="0" src="https://public.tableau.com/views/Applymobility_popdensity/WFH2004-2019?:showVizHome=no&:embed=true&:display_count=true" width = '700' height = '700' scrolling='yes' ></iframe> 

*2. During the pandemic, people go to work less, stay at home more, but grocery/retail shopping see little change.*

<iframe seamless frameborder="0" src="https://public.tableau.com/views/Applymobility_popdensity/GoogleMobility?:showVizHome=no&:embed=true&:display_count=true" width = '700' height = '900' scrolling='yes' ></iframe> 

*3. The majority of WFH population are above the age of 40. The majority of WFH population make more than 100k per year.*

<iframe seamless frameborder="0" src="https://public.tableau.com/views/Applymobility_popdensity/CensusPulse?:showVizHome=no&:embed=true&:display_count=true" width = '700' height = '900' scrolling='yes' ></iframe> 

## Conclusions
- My project provides Costco with information on the growing demographics of the WFH customers.
- Costco can accordingly update their catalog and recommender system.

Thank you for reading. The code for my project can be found [here](https://github.com/JoshJingtianWang/Costco-Business). 