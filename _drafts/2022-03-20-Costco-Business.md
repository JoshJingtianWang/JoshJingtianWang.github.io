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

See the presentation slides here([.ppt](https://github.com/JoshJingtianWang/Costco-Business/blob/main/presentation_josh_wang.pptx)/[.pdf](https://github.com/JoshJingtianWang/Costco-Business/blob/main/presentation_josh_wang.pdf)).

## Abstract
As a wholesaler, Costco excels in selling large quantities of selected goods at a cheap price, and because of that, Costco’s shelf space cannot accommodate a large selection of brands. Costco needs to efficiently allocate its limited shelf space to goods specific to the regions of the United States. Particularly, Costco is interested in tapping into the work-from-home (WFH) market and wants to know which region/demographic of the US has an emerging WFH population.

## Design
My work aims to inform Costco on which region of the US they should build new warehouses in, and in the existing warehouses, which ones they should adjust their catalog to accommodate the WFH customers. Adjusting warehouse location and catalog for the WFH customers will help reduce Costco’s shipping cost and attract more customers.

## The Data
•	US Census Pulse Survey Data: 
https://www.census.gov/programs-surveys/household-pulse-survey/data.html
•	Costco Warehouse Location data: https://www.costco.com/WarehouseListByStateDisplayView
•	Google Mobility Data: 
https://www.google.com/covid19/mobility/

## Tools
Costco Warehouse Location data was scraped using BeautifulSoup and Selenium. Data cleaning and visualization were done using Excel and Tableau.

## Findings
### Exploratory data analysis

*Figure 1*
![Figure 1](https://raw.githubusercontent.com/JoshJingtianWang/MoreyBall_Linear_Regression/main/plots/shotlocation_whitebg.png "Figure 1")

First I wanted to see if "MoreyBall's" 3-point-dominant strategy became popular in the league over the past decade. I scraped shot location data and found that long-range 2 pointers (hump at around 18ft) flattened over time, while the hump at 25ft (3 pointer) increased in height. The gaps in between the bins was possibly due to how the play-by-play data was manually called.

### Feature selection and engineering
The output variable is the net rating (an estimate of point differential per 100 possessions).

From correlation plot, a feature is removed if it has a correlation score >0.5 with another feature. Features that cannot be directly affected by a team's coaching strategy are also removed (for example: shots made, shooting percentage, turnover rate).

To further simplify the model, when the opponent stat is available, the differential is taken to replace the team feature and the opponent feature. For example:

`differential field goal attempted = team field goal attempted - opponent field goal attempted`

'team field goal attempted' and 'opponent field goal attempted' were discarded and 'differential field goal attempted' was kept.

The final list of features used to linear regression are as follows:


FEATURE NAME | DEFINITION | 
| ---- | ---- |
diff_fta | differential free throw attempted |
pace_adv | pace |
diff_orb | differential offensive rebounds |
diff_drb | differential diffensive rebounds |
diff_blk | differential blocks |
diff_stl | differential steals |
diff_pct_fga_00_03 | differential percentage of field goal attempted from 0-3ft |
diff_pct_fga_03_10 | differential percentage of field goal attempted from 3-10ft |
diff_pct_fga_10_16 | differential percentage of field goal attempted from 10-16ft |
diff_pct_fga_16_xx | differential percentage of field goal attempted from 16ft to 3pt line |
diff_pct_ast_fg2 | differential percentage of 2 pointers made that were assisted |
diff_pct_ast_fg3 | differential percentage of 3 pointers made that were assisted |
diff_pct_fga_fg3a | differential percentage of field goal attempted that were 3 pointers |
diff_pct_fg3a_corner | differential steals |
diff_fg_dunk | differential steals |
drives | drives |
catchshootattempt | catch-and-shoot attempts |
potentialasst | passes that potentially led to assists |
pullupfgattempt | pull-up shot attempts |
distance | distance travelled in a game |
speed | player speed in a game |
touches | touches (similar to passes, a measurement of ball movement) |
elbowtouches | touches at the elbows of the court |
postups | post-up shots attempted |
painttouches | touches in the paint of the court |

### ElasticNet Regression

The data was split into 80% training and 20% testing, with the testing set being the later NBA seasons to facilitate prediction into the future.

Data was then scaled and subjected to ElasticNet regularization. Two arrays of alpha values and L1 penalties were tested:
```
alphas = 10 ** np.linspace(-2, 2, 200)
l1_ratio=np.linspace(0, 1, 100)
```

Results after 5 fold cross-validation:
```
alpha:  0.021
l1_ratio:  0.05050505050505051
Adjusted R2 test: 0.6787019017187393
MAE =  1.5967825558232696
```


*Figure 2*
![Figure 2](https://raw.githubusercontent.com/JoshJingtianWang/MoreyBall_Linear_Regression/main/plots/coef_plot_nba_bball_ref_whitebg.png "Figure 2")

Coefficient values after ElasticNet regularization confirm that "MoreyBall's" 3-point and free throw shooting("diff_pct_fga_fg3a" and "diff_fta") positively add to team's net rating, and long range 2 pointers ("diff_pct_16_xx") negatively affect the net rating. However, the most important features are the defensives: defensive rebounds and steals ("diff_drb" and "diff_stl").

### Performance
*Figure 3*

![Figure 3](https://raw.githubusercontent.com/JoshJingtianWang/MoreyBall_Linear_Regression/main/plots/residuals.png "Figure 3")

Residual plot shows that the model residuals are relatively homoscedastic.
I then looked at the performance of the model on the 3 NBA teams in the most recent season:

![Figure 4](/assets/images/MoreyBall/performance.jpg "Figure 4")


## Conclusions
- My model is able to predict the season net rating of an NBA team with the MAE of 1.59, based on the playstyle of the team.
- “MoreyBall”’s shot selection philosophy is beneficial to the game, but not as important as defense.

Thank you for reading. The code for my project can be found [here](https://github.com/JoshJingtianWang/MoreyBall_Linear_Regression/). 