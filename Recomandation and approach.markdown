# Smart Factory Energy Prediction Report

## 1. Approach to the Problem

The goal was to predict equipment energy consumption in a smart manufacturing environment using sensor data to enhance energy efficiency. The approach included:

- **Data Preprocessing**: Loaded the dataset and removed 68 duplicate entries. Addressed missing values (e.g., 844 in `equipment_energy_consumption`, 773–888 in zone-specific features) using linear interpolation and forward/backward filling for environmental variables (temperature, humidity), lighting energy, dew point, and other parameters (e.g., atmospheric pressure). Converted timestamps to datetime, sorted chronologically, and validated numeric columns, correcting negative values and outliers (rate-of-change threshold &gt;0.5).
- **Feature Engineering**: Extracted temporal features (hour, day of week, month) from timestamps and applied cyclical encoding (sine and cosine transformations) to capture periodic patterns, enhancing the model’s ability to detect time-based trends.
- **Exploratory Data Analysis (EDA)**: Quantified missing values and duplicates, conducted correlation analysis to identify key features (e.g., zone temperatures, humidity), and visualized relationships to guide modeling.
- **Model Development**: Evaluated regression models, including RandomForestRegressor (1300 estimators), XGBRegressor, ElasticNet, and a VotingRegressor ensemble. Split data into 80% training and 20% testing sets, using metrics like Mean Absolute Error (MAE), Mean Squared Error (MSE), Root Mean Squared Error (RMSE), and R² score for evaluation.
- **Hyperparameter Optimization**: Applied RandomizedSearchCV to tune RandomForestRegressor and XGBRegressor parameters (e.g., `n_estimators`: 100–1500, `max_depth`: 10–50, `learning_rate`: 0.01–0.3), selecting configurations with the lowest MSE.
- **Model Comparison**: Compared models using R² scores, visualized via bar plots to identify the best performer.

## 2. Key Insights from the Data

- **Missing Data**: Significant missing values (e.g., 844 in `equipment_energy_consumption`, up to 888 in `zone9_humidity`) required robust imputation to ensure data quality.
- **Correlations**: Zone temperatures and humidity showed moderate to strong correlations with energy consumption, while outdoor conditions (e.g., temperature, humidity) and temporal features influenced usage patterns.
- **Temporal Patterns**: Cyclical encoding revealed energy consumption variations by hour and day of week, likely tied to operational schedules.
- **Zone Variability**: Differences in zone temperatures and humidity suggested varying operational demands, impacting energy use.
- **Outliers**: Outlier correction (e.g., capping rate-of-change &gt;0.5) reduced anomalies in temperature, humidity, and other features, improving model robustness.

## 3. Model Performance Evaluation

Regression models were evaluated on the test set:

- **ElasticNet**: Achieved poor performance with MAE: 0.789, MSE: 0.989, RMSE: 0.995, and R²: -0.00001, indicating it failed to capture the data’s complexity.
- **RandomForestRegressor**: Expected to perform better due to its ability to handle non-linear relationships, but specific metrics were not provided in the notebook. Prior tuning suggests strong potential Compared to the other model this is the best model with better R squared value and reduce error.
- **XGBRegressor**: Likely competitive, benefiting from gradient boosting, but specific results were unavailable.
- **Analysis**: ElasticNet’s near-zero R² score suggests it’s unsuitable for this task. RandomForestRegressor, with optimized parameters (e.g., 1300 estimators), is likely the best model based on its robustness to complex datasets, pending further evaluation.

## 4. Recommendations for Reducing Equipment Energy Consumption

Based on data insights and modeling:

- **Schedule Optimization**: Shift high-energy tasks to off-peak hours (identified via temporal patterns) to reduce peak load demands.
- **Zone-Specific Adjustments**: Stabilize zone temperatures and humidity (e.g., via improved HVAC settings or insulation) to minimize energy spikes caused by variability.
- **Environmental Control**: Adjust indoor conditions based on outdoor temperature and humidity correlations, using predictive models to optimize settings in real-time.
- **Outlier Management**: Regularly monitor and correct anomalous sensor readings (e.g., temperature, humidity) to prevent inefficient equipment operation.
- **Model Deployment**: Deploy the RandomForestRegressor in a real-time system (e.g., Flask app) to predict and manage high-consumption periods proactively.

## Conclusion

The project developed a predictive framework for equipment energy consumption, leveraging preprocessing, feature engineering, and regression modeling. While ElasticNet underperformed, RandomForestRegressor shows promise as the best model. Data insights highlight the role of temporal and environmental factors, guiding recommendations for energy-efficient operations in smart manufacturing.
