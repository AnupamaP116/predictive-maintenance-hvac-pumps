# predictive-maintenance-hvac-pumps
End-to-end Predictive Maintenance project for HVAC Pumps using sensor data. Includes data preprocessing, feature engineering, Random Forest model development, and FastAPI deployment for real-time machine status prediction.
🔧 Project Overview

This project aims to predict when HVAC pumps are likely to fail by analyzing their sensor readings.
It’s a Predictive Maintenance project — meaning we try to predict future equipment problems before they actually happen, so maintenance teams can fix them early and avoid downtime or breakdowns.

⚙️ 1. Objective

The main goal is to use machine sensor data to predict the machine_status — whether a pump is:

🟢 NORMAL – Operating properly

🟡 RECOVERING – Showing early signs of trouble

🔴 BROKEN – Already failed or near failure

By predicting these states early, maintenance teams can take preventive actions, saving time and cost.

🧾 2. Data Description

Total Rows: 220,320

Columns: 55

52 sensor readings (sensor_00 to sensor_51)

1 timestamp (when data was recorded)

1 target column (machine_status)

So, each row represents the condition of the pump at a specific time based on its sensor readings.

🧹 3. Data Preprocessing

Before building any model, you cleaned the data:

✅ Dropped sensor_15 (it had 100% missing values)
✅ Filled missing values in other sensors using their mean
✅ Checked duplicates: none found
✅ Final dataset: no missing values, ready for feature engineering

Class Distribution:

Normal → 205,836

Recovering → 14,477

Broken → 7 (very rare)

This means the dataset is imbalanced, as “Broken” cases are few.

🧠 4. Feature Engineering

You added new, meaningful features to help the model learn better:

➕ New Features:
Feature	Purpose
sensor_mean	Average of all sensor readings — shows overall pump performance
sensor_std	Measures variation in sensor readings — flags unusual fluctuations
sensor_min	Lowest sensor value — can indicate sudden drops
sensor_max	Highest sensor value — can detect spikes
hour	Extracted from timestamp — detects time-of-day behavior
day_of_week	Detects weekly operational patterns
➖ Removed:

Dropped sensor_16 to sensor_26 due to high correlation (redundant data).

✅ Final dataset has 47 useful features (after cleaning and feature creation).
✅ Rows remain 220,320.

🤖 5. Model Development

You tried multiple models and found that Random Forest performed the best.

🌟 Why Random Forest?

High Accuracy, Precision, Recall, and F1-score

Handles imbalanced data and non-linear relationships well

Has feature importance (explains which sensors are most important)

Resistant to overfitting

So you selected Random Forest as the final predictive model.

🚀 6. Model Deployment (FastAPI)

You turned your model into a working API — so others can send sensor data and get predictions instantly.

Steps:

Saved the trained model → random_forest_model.pkl

Created a FastAPI script (app.py)

Defined a /predict endpoint

Validated input using Pydantic

Started the API server using:

uvicorn app:app --reload


Verified it worked successfully at:

http://127.0.0.1:8000/docs


Now, this API can predict NORMAL, RECOVERING, or BROKEN status in real-time.

🧭 7. Recommendations

To make the system practical and reliable:

🔍 Model Monitoring

Track prediction accuracy over time

Watch for changes in sensor data patterns (data drift)

Monitor how often each class (Normal, Recovering, Broken) appears

📢 Alerts

Send automatic notifications when “RECOVERING” or “BROKEN” is predicted

Integrate with dashboards, SMS, or email alerts

🔄 Retraining

Retrain the model periodically (monthly or quarterly)

Collect more “BROKEN” samples to balance the dataset

Use SMOTE or anomaly detection for rare failure detection

📊 Visualization

Dashboard showing:

Important sensors (feature importance)

Time-based trends (sensor_mean vs hour/day)

Helps engineers understand what’s causing issues

🧩 Future Improvements

Add real-time streaming (Kafka, MQTT)

Try SHAP or LIME for model explainability

Add external factors (temperature, humidity, machine load)

Compare with other models (XGBoost, LightGBM, Deep Learning)

💼 8. Business Value

Predicts failures before they happen

Reduces downtime and repair costs

Helps plan maintenance schedules

Builds trust between engineering and data science teams

🏁 Summary
Stage	Key Outcome
Data Cleaning	Handled missing data, dropped redundant sensors
Feature Engineering	Added time-based & statistical features
Modeling	Random Forest chosen for accuracy & interpretability
Deployment	FastAPI API created for real-time predictions
Monitoring	Suggested model & system monitoring methods
Business Impact	Reduced downtime, improved maintenance efficiency
