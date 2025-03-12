# **Global maritime trade analysis: port activity & trade value**

## **Introduction**

This project analyzes international maritime trade by examining the relationship between **port activity** and **trade value**. Seaborne trade is the backbone of global commerce, with ports playing a crucial role in facilitating imports and exports. By exploring port call frequency, vessel capacity, and trade values, we aim to uncover key insights into the efficiency and trends of maritime trade flows.

## **Data source: UN Comtrade**

The data for this analysis is sourced from **UN Comtrade**, a leading database for global trade statistics. The dataset includes **world trade data** and **seaborne trade activity**, focusing on key trade indicators:

### **Trade value metrics**
- **Primary Value** – The total reported trade value.
- **CIF (Cost, Insurance, and Freight) Value** – The cost of goods including insurance and freight to the destination.
- **FOB (Free on Board) Value** – The cost of goods excluding insurance and freight, reflecting the value at the export point.

### **Port activity data**
- **Number of Port Calls (Num Pc)** – The total ship arrivals at a port, indicating traffic volume.
- **Dead Weight Tonnage (DWT)** – The carrying capacity of a vessel, including cargo, fuel, and crew.
- **Metric Ton Capacity (MTC)** – The total weight of cargo a vessel can transport.

## **Data limitations**

While UN Comtrade provides valuable trade insights, it has several limitations:
- **Restricted Data Access** – Covers only major ports, omitting smaller but significant trade hubs.
- **Historical Trade Focus** – Lacks real-time port operations data such as congestion and delays.
- **Reporting Variability** – Differences in country reporting may cause inconsistencies in trade volumes.
- **Exclusion of Other Transport Modes** – Maritime-only focus underestimates total trade, as air, rail, and road transport are not included.
- **Limited Economic Context** – The absence of key economic indicators like fuel costs and GDP restricts broader trade impact analysis.

Despite these constraints, this analysis provides crucial insights into the efficiency of global shipping routes and port activity trends.

## Exploratory visual analysis

This section presents a visual exploration of global trade patterns, focusing on two main hypotheses:

1. **Trade is influenced by port activity** – This includes the number of port calls, vessel capacity (deadweight tonnage and metric tons carried), and trade value (CIF, FOB, and primary value).
2. **Weather conditions influence trade and port activity** – We investigate whether factors like temperature, wind, and precipitation impact trade efficiency.

### Correlation analysis of trade and port activity
To understand the relationship between port activity and trade value, a correlation matrix was generated. The key findings are:

   <p align="center">
  <img src="https://github.com/user-attachments/assets/03531afd-0b4a-42ed-b404-c62902300ee5" width="500">
</p>

- **Number of port calls (num_pc) is positively correlated with metric tons carried (mtc) and deadweight tonnage (dwt)**. This suggests that higher vessel traffic generally translates to higher trade volumes.
- **Trade value metrics (primaryValue, CIF, and FOB) are moderately correlated with vessel capacity**. This indicates that while larger ships tend to transport higher-value goods, the relationship is not strictly linear.
- **Seasonal and time-based variations** do not significantly affect trade patterns, reinforcing the notion that global trade operates continuously, despite minor fluctuations.

### Weather impact on trade: correlation analysis
To test the second hypothesis, weather variables such as temperature, wind speed, and precipitation were analyzed against trade and port activity:

   <p align="center">
  <img src="https://github.com/user-attachments/assets/6d198a7e-37c8-443c-b082-b2d635d6e5b9" width="500">
</p>

- **No strong correlation was found between weather conditions and trade efficiency**.
- **Wind speed and gusts** showed only a minor connection to port calls, suggesting that extreme weather events may disrupt specific routes but do not significantly impact overall trade.
- **Temperature and precipitation had no meaningful impact on trade volume**.

These findings suggest that **trade flows are more dependent on port capacity and infrastructure than on seasonal or meteorological factors**.


### Key takeaways:
- **Port activity is a key driver of trade volume**, with vessel size and port calls playing an important role.
- **Trade value is more influenced by the type of goods transported rather than vessel traffic alone**.
- **Weather conditions have minimal impact on global trade efficiency**, reinforcing the resilience of shipping infrastructure.


## Why supervised regression is not suitable

Initial attempts to model trade efficiency and port activity through supervised regression revealed significant limitations. The regression models aimed to predict the number of port calls based on key factors such as metric ton capacity (MTC), deadweight tonnage (DWT), and trade value. However, several issues arose:

 <p align="center">
  <img src="https://github.com/user-attachments/assets/2e90278d-aea3-49ee-a56c-b1f4c0024fcf" width="500">
</p>

1. **Weak linear relationships**:  
   - The regression slope for MTC vs. Number of Port Calls was **8.97e-5**, indicating a negligible direct impact of cargo volume on port frequency.
   - For trade value, the slope was **6.20e-8**, showing an extremely weak influence on port activity.
   - These slopes suggest that although statistical correlations exist, they lack meaningful predictive power.

2. **High R² scores but limited practical use**:  
   - The R² score for MTC regression was **0.8097**, implying that 81% of the variance in port calls was explained by the model. However, the tiny slope values question the practical significance.
   - Similarly, trade value regression achieved an R² of **0.56 (test set)** and **0.58 (train set)**, explaining a moderate portion of variance but still leaving substantial prediction error.

3. **Impact of outliers**:  
   - Scatterplots of the regression models indicate that data points are widely dispersed around the trend line, with notable clusters of high and low-density points.
   - Significant outliers distort the model’s predictive capacity, further weakening its reliability.

4. **MSE and prediction error**:  
   - The Mean Squared Error (MSE) for the test set in trade value regression was **112,664.23**, highlighting a high level of prediction uncertainty.
   - The MSE for the train set was **103,834.03**, showing that even training data lacked perfect fit.

### Introducing clustering

Given the inefficacy of linear regression for accurately predicting port calls based on trade and cargo metrics, a **clustering approach** is more appropriate. Instead of assuming a direct linear relationship, clustering helps **identify distinct trade patterns** by grouping ports and trade flows based on shared characteristics.

- **Clustering groups ports by trade behavior rather than forcing a strict numerical prediction.**
- **It accounts for non-linear relationships**, capturing complex interactions between trade volume, ship traffic, and economic factors.
- **By clustering ports based on shared cargo capacity and trade frequency, we can better analyze different trade strategies.**

In the next section, we explore clustering results and how different clusters represent unique trade behaviors.

 <p align="center">
  <img src="https://github.com/user-attachments/assets/b9c35982-96dd-4409-b7eb-8d4584852a4f" width="500">
</p>

## Clustering analysis overview

Clustering reveals distinct patterns in port activity and trade behavior, categorizing ports based on their trade volume, vessel frequency, and cargo capacity. The clusters indicate variations in shipping strategies, ranging from frequent, low-volume shipments to fewer but high-value trades.

### **Cluster interpretations**

 <p align="center">
  <img src="https://github.com/user-attachments/assets/519180e9-0171-4811-8206-02734470b7fa" width="500">
   <img src="https://github.com/user-attachments/assets/dcf4006a-ffca-40d4-a92d-78a12c144518" width="500">
</p>
- **Pink cluster (Cluster 0)**  
  Represents smaller-scale operations with moderate port calls and trade value. Ports in this group balance volume and value without reaching extremes.

  **Key stats:**  
  - Port calls (num_pc) mean: **523.36**; median: **490.5**  
  - Metric ton capacity (mtc) mean: **6.92 × 10⁶**  
  - Dead weight tonnage (dwt) mean: **1.36 × 10⁷**  
  - Primary value mean: **7.12 × 10⁹**

- **Purple cluster (Cluster 1)**  
  The busiest trade routes, featuring the highest number of port calls and the largest trade value. These ports handle frequent shipments with substantial throughput.

  **Key stats:**  
  - Port calls (num_pc) mean: **1,352.52**; median: **1,316.0**  
  - Metric ton capacity (mtc) mean: **1.20 × 10⁷**  
  - Dead weight tonnage (dwt) mean: **2.67 × 10⁷**  
  - Primary value mean: **1.62 × 10¹⁰**

- **Dark purple cluster (Cluster 2)**  
  Represents low-frequency, low-volume trade routes. These ports likely serve specialized or localized trade, with minimal cargo volume and trade value.

  **Key stats:**  
  - Port calls (num_pc) mean: **71.02**; median: **51.0**  
  - Metric ton capacity (mtc) mean: **6.91 × 10⁵**  
  - Dead weight tonnage (dwt) mean: **1.69 × 10⁶**  
  - Primary value mean: **5.71 × 10⁸**

- **Black cluster (Cluster 3)**  
  Handles the largest cargo capacity with frequent port calls but falls slightly below the purple cluster in total trade value. This group signifies major trade routes with high cargo throughput.

  **Key stats:**  
  - Port calls (num_pc) mean: **1,260.25**; median: **1,208.0**  
  - Metric ton capacity (mtc) mean: **5.24 × 10⁷**  
  - Dead weight tonnage (dwt) mean: **4.93 × 10⁷**  
  - Primary value mean: **9.48 × 10⁹**

### **Future steps**
The clustering findings provide a foundation for further research:

- **Mapping clusters to trade lanes:** Identifying whether certain clusters dominate specific regions or trade corridors.  
- **Seasonal analysis:** evaluating how clusters shift across different periods due to seasonal demand fluctuations.  
- **Vessel-level insights:** Analyzing ship types, age, and ownership to uncover correlations between vessel characteristics and trade clusters.  
- **Operational validation:** Cross-referencing clusters with industry data, such as financial performance, service reliability, and trade policies.  

Ultimately, refining the clustering model with deeper data, stakeholder feedback, and additional trade variables (e.g., commodity types, regional regulations) can enhance its applicability in global maritime trade analysis.

## Time Series Analysis & ARIMA Forecasting

 <p align="center">
  <img src="https://github.com/user-attachments/assets/3b0668a6-5056-4d34-b0c4-fcba6e7f6f02" width="500">
</p>

### Metric ton capacity (mtc) analysis
The decomposition of **mtc** highlights key patterns in trade capacity:
- **Trend:** A sharp decline from 2015 to 2017, followed by relative stability, then a strong spike in 2021, before stabilizing again.
- **Seasonality:** Repeating cycles of fluctuations around ±2,500, indicating a consistent seasonal pattern.
- **Residual:** Large outliers appear in 2016 and 2021–2022, suggesting significant disruptions that neither trend nor seasonality account for.

This decomposition suggests that while **mtc** follows seasonal trends, external factors create disruptions that make simple forecasting challenging.

### Primary trade value (primaryValue) analysis

<p align="center">
  <img src="https://github.com/user-attachments/assets/7131f753-d83e-4541-87cb-c91f824cda61" width="500">
</p>

- **Trend:** Gradual increase with a notable rise in 2022–2023.
- **Seasonality:** Despite fluctuations, its influence is minor compared to the long-term trend.
- **Residual:** Low volatility for most of the timeline but becomes unstable post-2022, implying unpredictable external events.

The **primaryValue** trend suggests that trade value is driven largely by long-term economic or policy changes, rather than strong seasonal cycles.

### ARIMA forecasting for deadweight tonnage (dwt)
Applying the ARIMA model to **dwt**, we observe:
- **Training Data (2015–2021):** Shows clear patterns in port activity.
- **Test Data (2022–2025):** Some instability arises, with sharp deviations in 2022.
- **Forecast:** The model struggles with high fluctuations, particularly past 2022, indicating potential economic or operational shocks.

### Limitations of ARIMA
- **Sensitivity to external shocks:** Sudden economic shifts or policy changes disrupt forecasts.
- **Seasonality vs. Trend:** While ARIMA captures general trends, it struggles with abrupt shifts in port trade.
- **Future Considerations:** A hybrid approach incorporating external economic indicators might improve predictions.

Overall, while ARIMA provides useful insights into trade movements, its accuracy diminishes under high volatility, requiring complementary models to enhance forecasting reliability.

## Global takeaways

### Port activity and trade efficiency
- A higher number of port calls does not necessarily translate into higher trade value. Instead, **ship size and cargo efficiency** play a more critical role.
- Ports with **fewer calls but higher trade values** (dark purple cluster) likely specialize in bulk commodities or high-value shipments.

### Cluster analysis reveals trade patterns
- Ports operate under different trade models: **high-frequency, low-value shipments** vs. **low-frequency, high-value trade**.
- The clusters suggest that **trade infrastructure should be tailored**—some ports require **capacity for high-volume shipments**, while others need optimization for **high-value goods**.

### Seasonality and weather effects
- While there are **seasonal variations in trade**, overall trade remains stable across seasons.
- **Weather conditions have little impact on port activity**, reinforcing the idea that **modern logistics mitigate disruptions** effectively.

### Trade value growth and market shifts
- **Imports have surged by 300% since 2015**, reflecting a major shift in trade patterns.
- **Exports, however, have shown volatility**, with a notable decline by the end of 2024.
- This evolving trade balance suggests **global supply chain shifts**, influenced by **economic changes, policy adjustments, or fluctuating demand**.

### Final insights
The analysis underscores the need for **adaptive trade strategies**, efficient **port infrastructure**, and **data-driven decision-making** to navigate the complexities of global maritime trade.

## Interactive Visualization
Explore the interactive Tableau dashboard for a deeper understanding of maritime shipping trends:  
[View the Tableau Dashboard](https://public.tableau.com/app/profile/sherezade.maqueda.lafuente/viz/Maritimeshippinganalysis/Shipping?publish=yes)

