# philly-transit-analysis
Philadelphia Transit & Economic Mobility Analysis
Project Overview

This project investigates the relationship between the structural topology of Philadelphia’s public transit network and the socioeconomic outcomes of its residents. Through modeling SEPTA transit lines via a weighted adjancey matrix, we test whether a neighborhood's "connectivity" can predict it's current household wealth (med_hhinc2016) and the long-term upward mobility of children (kfr_pooled_pooled_p25).

Methodology
1. Network Construction
Using GTFS (General Transit Feed Specification) data, we model the city’s transit system as a weighted, undirected graph:

Nodes are 13,851 unique transit stops/stations.

Edges are Scheduled trips between stations.

Weights are the calculated travel time (in seconds) between nodes based on arrival and departure schedules.

Data Cleaning: Identified and removed "Isolated Nodes" (abandoned or duplicate "ghost stops") that lacked connecting edges to ensure accurate centrality scores. Abondoned stops are no longer part of the transit "network" 

2. Connectivity Metrics
We computed two primary centrality measures to define "connectivity" for each stop:

Closeness Centrality: Measures the efficiency of a node by calculating the reciprocal of the average shortest path distance from station "u" to all other stations "v". It represents how "near" a station is to the rest of the city.

Betweenness Centrality: Measures the "bridge" or "hub" status of a node by finding the faction of all possivle shortest paths that pass through a given station. Due to the high computational cost of calculating shortest paths for n=13,851 nodes (~96 million paths), we utilized K-sampling (k=1000) to approximate the scores, scaling the results to the full network size.

We then calculated a census tract's connectivity measures, by averaging the scores from every station in the tract: we did this analysis for both betweenness centrality and closeness centrality.

3. Socioeconomic Success Metrics
Network metrics were aggregated by Census Tract (GEOID) and merged with data from the Opportunity Atlas and the U.S. Census Bureau. We focused on two dependent variables:

kfr_pooled_pooled_p25: The mean household income at age 35 for children raised in the bottom 25th percentile of income (a standardized measure of upward mobility).

med_hhinc2016: Current median household income per tract.

4. Statistical Modeling
We conducted 5 tests to observe the correlation between our connectivity scores and the socioeconomic success metric scores:

Test 1: A table displaying the correlation coefficient, for our socioeconomic success metrics in relation to our connectivity metrics 

Test 2: Second, a multivariable regression to analyze the simultaneous impact of Closeness and Betweenness on our variables. However, we identified a high condition number (~32,000), signaling a correlation between centrality measures.

Test 3 and 4: Two seperate single-variable ordinary least squares regressions to analyze the predictive power of solely Betweeness and Closeness on economic succes.

Test 5: To allow for a "fair fight" comparison between units, we calculated Standardized Coefficients (β), enabling us to determine which metric had a stronger relative influence on income and upward mobility. These coefficients are also in the table. We verified our result with a ML random forest predictive model.

Tech Stack
Language: Python

Libraries: NetworkX (Graph Analytics), Pandas/GeoPandas (Data Wrangling), Statsmodels/Scikit-learn (Regression & ML), Pygris (Census Tract Geometry)

Data Sources: SEPTA GTFS, The Opportunity Atlas, U.S. Census Bureau



=====| KEY FINDINGS (repeated also on LinkedIn) |===============================================================================

The Philly Paradox: Closeness Centrality is negatively correlated with wealth in Philadelphia, reflecting high transit density in historically disinvested urban cores.

Quality over Quantity: Betweenness Centrality (hub status) is a significantly stronger positive predictor of current household income—2.88x more powerful than closeness.

The Opportunity Signal: Transit connectivity explains 17.5% of the variance in a child’s future economic success, confirming that infrastructure remains a critical component of social mobility.
