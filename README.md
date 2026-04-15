# American_Airlines_Flight_Arrival_Delay_Prediction

✈️ Flight Arrival Delay Prediction with Weather Data

📌 Project Overview

This project focuses on predicting flight arrival changes after departure using operational flight data and external weather data. The goal is to understand whether weather conditions (temperature, wind, rainfall) significantly impact arrival delays.

🎯 Objectives
Predict whether a flight’s arrival time will change after departure
Analyze key factors influencing arrival delays
Evaluate the impact of weather data vs operational features
Compare multiple machine learning models

📊 Datasets Used
1. Flight Data
Source: American Airlines dataset 
Contains:
Flight schedules and actual timings
Departure & arrival delays
Airport information
Flight status updates

3. Weather Data
Source: NOAA Global Hourly Dataset
Features used:
Temperature
Wind Speed
Rainfall

🛠️ Data Preprocessing

🔹 Flight Data Cleaning
Filtered only post-departure records
Selected first record per flight to avoid duplicates
Removed rows with missing target variable
Created unique flight_id for tracking flights

🔹 Feature Engineering
Created:
arrival_change_flag (classification target)
arrival_variance (regression target)
Selected key features:
Departure delay
Block time variance
Time to scheduled departure
Airport characteristics

🔹 Weather Data Processing
Selected relevant columns: DATE, TMP, WND, AA1
Extracted numeric values from encoded strings
Converted:
Temperature → numeric (°C)
Wind speed → numeric
Rain → numeric
Handled missing values and outliers

🔹 Data Integration
Aggregated weather data at daily level
Merged with flight data using departure date

🤖 Models Used

1. Logistic Regression
Used for classification (arrival change prediction)
Applied class balancing

2. Random Forest
Captures non-linear relationships
Handles feature interactions

3. Gradient Boosting
Improves performance using sequential learning
Focuses on reducing prediction errors

📈 Evaluation Metrics

Classification:
Accuracy
Precision
Recall
F1-score

Regression:
RMSE (Root Mean Squared Error)
R² Score

📊 Key Findings

Operational features (like departure delay) had strong impact
Weather features showed minimal improvement in model performance

Possible reasons:
Missing weather values
Weak correlation with target
Aggregation reduced granularity

⚠️ Challenges Faced

Timestamp mismatch between flight and weather data
Missing values in weather features
Feature alignment issues during merging
Handling multiple records per flight

🚀 Conclusion

Flight delays are more influenced by operational factors than weather
Weather data requires better alignment and richer features for stronger impact
Future improvements can enhance prediction accuracy

🔮 Future Improvements

Use hourly weather matching instead of daily aggregation
Include more weather features (visibility, pressure, storms)
Apply advanced models (XGBoost, Neural Networks)
Perform feature importance analysis

🧰 Tech Stack
Python
Pandas, NumPy
Scikit-learn
Matplotlib, Seaborn

📁 Project Structure
├── data/
├── notebooks/
│   └── Modelwithresults.ipynb
├── README.md


👩‍💻 Author
Vaishnavi Bhamare
Master’s in Advanced Data Analytics
University of North Texas
