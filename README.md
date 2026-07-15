\# Walmart Demand Forecasting \& Inventory Reorder Planning  

\### An End-to-End Supply Chain Analytics Dashboard



A supply chain analytics project built on Walmart retail sales data.  

This project combines \*\*machine learning-based demand forecasting\*\* with \*\*inventory reorder planning\*\* and presents the final insights through an interactive \*\*Power BI dashboard\*\*.



The main objective is to forecast weekly demand at a \*\*Store-Department level\*\* and use that forecast to identify SKUs that may require replenishment.



\*\*Dataset:\*\* Walmart Sales Forecasting Dataset  

\*\*Dashboard:\*\* Power BI  

\*\*Model:\*\* XGBoost Regression  



\---



\## Business Problem



Retail businesses often face two major inventory problems:



\- \*\*Stockouts\*\* — lost sales, poor service level, and customer dissatisfaction  

\- \*\*Overstocking\*\* — blocked working capital and higher holding cost  



This project builds a simple data-driven workflow to forecast demand and convert the forecast into inventory planning decisions such as:



\- Which SKUs require reorder?

\- Which SKUs are high-priority?

\- Which stores/departments need more attention?

\- How accurate is the demand forecast?



\---



\## Project Architecture



```text

Walmart-Demand-Forecasting-Inventory-Planning/

│

├── data/

│   ├── train.csv

│   ├── features.csv

│   ├── stores.csv

│   └── test.csv

│

├── notebooks/

│   └── walmart\_xgboost\_inventory\_project.ipynb

│

├── outputs/

│   ├── forecast\_results.csv

│   ├── model\_metrics.csv

│   ├── inventory\_planning.csv

│   └── feature\_importance.csv

│

├── powerbi/

│   └── Walmart\_Demand\_Forecasting\_Inventory\_Planning.pbix

│

├── images/

│   ├── executive\_overview.png

│   ├── forecasting\_performance.png

│   └── inventory\_reorder\_planning.png

│

└── README.md

```



\---



\## Dataset Overview



The project uses Walmart historical sales data with store-level and external features.



\*\*Files used:\*\*



| File | Description |

|---|---|

| `train.csv` | Historical weekly sales data |

| `features.csv` | External factors like temperature, fuel price, CPI, unemployment, markdowns |

| `stores.csv` | Store type and store size information |

| `test.csv` | Original test file from dataset |



In this project, each \*\*Store + Department\*\* combination is treated as one SKU/planning unit.



Example:



```text

Store 1 + Dept 92 = SKU\_ID 1\_92

```



\---



\## Tech Stack



| Category | Tools |

|---|---|

| Programming | Python |

| Data Processing | Pandas, NumPy |

| Machine Learning | XGBoost, Scikit-learn |

| Dashboarding | Power BI |

| BI Calculations | DAX |

| Output Files | CSV, PBIX |



\---



\## Methodology



\### 1. Data Cleaning \& Preparation



The three main files — `train.csv`, `features.csv`, and `stores.csv` — were merged using common columns such as `Store`, `Date`, and `IsHoliday`.



\*\*Steps performed:\*\*



\- Converted `Date` column into datetime format  

\- Filled missing markdown values with `0`  

\- Removed negative weekly sales values  

\- Created `SKU\_ID` using Store and Department  

\- Added time-based features such as year, month, week, and quarter  

\- Converted store type into model-friendly columns  



\---



\### 2. Feature Engineering



Demand forecasting depends heavily on past sales behavior, so lag and rolling features were created.



\*\*Lag Features:\*\*



| Feature | Meaning |

|---|---|

| `Sales\_Lag\_1` | Sales from previous week |

| `Sales\_Lag\_2` | Sales from 2 weeks ago |

| `Sales\_Lag\_4` | Sales from 4 weeks ago |

| `Sales\_Lag\_8` | Sales from 8 weeks ago |

| `Sales\_Lag\_12` | Sales from 12 weeks ago |



\*\*Rolling Features:\*\*



| Feature | Meaning |

|---|---|

| `Rolling\_Mean\_4` | Average sales of previous 4 weeks |

| `Rolling\_Mean\_8` | Average sales of previous 8 weeks |

| `Rolling\_Std\_4` | Demand variation over previous 4 weeks |

| `Rolling\_Std\_8` | Demand variation over previous 8 weeks |



These features helped the model capture recent demand trends and sales volatility.



\---



\### 3. Demand Forecasting Model



An \*\*XGBoost Regression\*\* model was trained to predict weekly sales.



A time-based split was used:



| Split | Period |

|---|---|

| Training Data | Older historical weeks |

| Testing Data | Most recent 8 weeks |



A random split was avoided because forecasting should be tested on future unseen periods, not randomly mixed historical data.



\---



\## Model Performance



The model was evaluated on the final 8-week validation period.



| Metric | Value |

|---|---:|

| MAE | 1316.18 |

| RMSE | 2841.65 |

| Filtered MAPE | 12.79% |

| \*\*WMAPE\*\* | \*\*8.51%\*\* |



\*\*WMAPE\*\* was used as the main metric because raw MAPE was distorted by very low-sales departments.



\---



\## Feature Importance



The most important features from the XGBoost model were:



| Rank | Feature |

|---:|---|

| 1 | `Rolling\_Mean\_4` |

| 2 | `Sales\_Lag\_1` |

| 3 | `Sales\_Lag\_4` |

| 4 | `Rolling\_Mean\_8` |

| 5 | `Sales\_Lag\_2` |



This shows that recent sales history had the strongest impact on the weekly sales forecast.



\---



\## Inventory Reorder Planning



After forecasting demand, the results were used for inventory planning.



The Walmart dataset does not include actual inventory levels or supplier lead times.  

Because of that, \*\*current stock\*\* and \*\*lead time\*\* were simulated only for the inventory planning layer.



The forecasting model itself was trained on real historical Walmart sales data.



\---



\### Safety Stock



Safety stock was calculated using a 95% service level.



```text

Safety Stock = 1.65 × Demand Standard Deviation × √Lead Time

```



\---



\### Reorder Point



```text

Reorder Point = Forecasted Weekly Demand × Lead Time + Safety Stock

```



\---



\### Stock Status Logic



```text

If Current Stock < Reorder Point:

&#x20;   Reorder Required

Else:

&#x20;   Stock Sufficient

```



\---



\## ABC Inventory Classification



ABC analysis was performed based on total sales contribution.



| ABC Class | Meaning | Priority |

|---|---|---|

| A | High-value SKUs | High |

| B | Medium-value SKUs | Medium |

| C | Low-value SKUs | Low |



A-class SKUs were treated as the most important when reorder action was required.



\---



\## Power BI Dashboard



The final dashboard contains three pages.



\---



\### Page 1 — Executive Overview



This page gives a high-level business summary of sales and inventory status.



\*\*Main visuals:\*\*



\- Total Sales  

\- Total SKUs  

\- SKUs Requiring Reorder  

\- A-Class SKUs  

\- Urgent A-Class Reorders  

\- Stock Status Distribution  

\- ABC Inventory Classification  

\- Top Departments by Sales  

\- Reorder SKUs by Store  

\- Priority SKU Table  



!\[Executive Overview](images/executive\_overview.png)



\---



\### Page 2 — Forecasting Performance



This page focuses on model accuracy and forecast behavior.



\*\*Main visuals:\*\*



\- MAE  

\- RMSE  

\- Filtered MAPE  

\- WMAPE  

\- Actual vs Predicted Sales  

\- Forecast Error by Store  

\- Forecast Error by Department  

\- XGBoost Feature Importance  



!\[Forecasting Performance](images/forecasting\_performance.png)



\---



\### Page 3 — Inventory Reorder Planning



This page focuses on replenishment decisions.



\*\*Main visuals:\*\*



\- Top 10 Reorder Gap SKUs  

\- Stock Status Split by ABC Class  

\- Reorder Action Table  

\- Store Slicer  

\- ABC Class Slicer  

\- Stock Status Slicer  



!\[Inventory Reorder Planning](images/inventory\_reorder\_planning.png)



\---



\## Key Findings



1\. \*\*XGBoost achieved 8.51% WMAPE\*\* on the validation period.  

2\. \*\*Rolling\_Mean\_4\*\* and \*\*Sales\_Lag\_1\*\* were the strongest forecasting features.  

3\. \*\*1160 out of 3115 SKUs\*\* were flagged as requiring reorder.  

4\. \*\*1005 SKUs\*\* were classified as A-class SKUs.  

5\. \*\*331 A-class SKUs\*\* required urgent reorder action.  

6\. Departments like \*\*Dept 92\*\* and \*\*Dept 95\*\* contributed strongly to total sales.  

7\. The dashboard helps convert forecast output into clear inventory actions.



\---



\## Output Files



| File | Description |

|---|---|

| `forecast\_results.csv` | Actual vs predicted sales for validation period |

| `model\_metrics.csv` | Forecasting accuracy metrics |

| `inventory\_planning.csv` | Safety stock, reorder point, stock status, ABC class |

| `feature\_importance.csv` | XGBoost feature importance values |

| `Walmart\_Demand\_Forecasting\_Inventory\_Planning.pbix` | Final Power BI dashboard |



\---



\## Assumptions



\- Weekly sales were used as a proxy for demand.  

\- Actual stock levels were not available in the dataset.  

\- Supplier lead times were simulated.  

\- Current stock values were simulated.  

\- The model predicts sales value, not physical unit quantity.  



These assumptions were made because the original dataset is mainly a sales forecasting dataset, not a complete inventory management dataset.



\---



\## Limitations



\- No real inventory quantity data  

\- No supplier-level lead time data  

\- No actual purchase order history  

\- No product-level cost data  

\- Reorder quantity optimization was not included in this version  



\---



\## Future Improvements



\- Add real inventory and supplier lead time data  

\- Add EOQ-based order quantity calculation  

\- Compare XGBoost with LightGBM, Prophet, or ARIMA  

\- Add SQL database integration  

\- Add supplier performance analysis  

\- Publish the dashboard using Power BI Service  

\- Automate refresh pipeline  



\---



\## Project Summary



This project connects \*\*demand forecasting\*\* with \*\*inventory reorder planning\*\*.



Instead of stopping at prediction, the forecasted demand was used to calculate:



\- Safety Stock  

\- Reorder Point  

\- Stock Status  

\- Reorder Gap  

\- ABC Priority  

\- Action Recommendation  



The final Power BI dashboard gives a practical view of which SKUs need attention and which reorder cases should be handled first.

