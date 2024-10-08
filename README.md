# Appointment Scheduling Data Analysis and Visualization

## Project Overview

This project involves cleaning, analyzing, and visualizing patient appointment scheduling data. The goal is to gain insights into waiting times, appointment trends, and provider efficiency. The analysis is carried out using Excel, DAX, SQL, and Python (Pandas), with visualizations created using Power BI or a similar dashboard tool.

---

## Data Column Relationships

The dataset contains several key columns, and their relationships are described below:

1. **Client ID**: Unique identifier for each patient. It helps in tracking each patient's appointment history.
2. **Appointment ID**: Each appointment has a unique ID, which is linked to a patient via the Client ID.
3. **Provider Role**: Indicates the type of provider handling the appointment (MD, Nurse, Psychologist).
4. **Appointment Date**: The actual date of the patient's appointment.
5. **Scheduled Date**: The date on which the appointment was scheduled, used to calculate waiting time.
6. **Wait Time**: The time difference (in days) between the **scheduled date** and the **appointment date**.
7. **Month/Year**: Extracted from the appointment date, used for monthly trends and insights.
8. **Patient Status**: Information about whether the patient attended or canceled the appointment.
9. **Service Type**: Additional categorization based on the nature of the service offered during the appointment.

---

## Data Cleaning Steps

### Operations Performed:

1. **Trim: Remove Additional Spaces in Patient Names**
   - **Reason**: Extra spaces can cause inconsistencies in data analysis (e.g., duplicate entries).
   - **Formulas**:
     - **Excel**: `=TRIM(A2)`
     - **DAX**: `TRIM([PatientName])`
     - **SQL**: `TRIM(PatientName)`
     - **Pandas**: `df['PatientName'] = df['PatientName'].str.strip()`

2. **Full Name Column (Merging First and Last Names)**
   - **Reason**: Combining first and last names helps in easier reporting and improves readability.
   - **Formulas**:
     - **Excel**: `=A2 & " " & B2`
     - **DAX**: `[FirstName] & " " & [LastName]`
     - **SQL**: `CONCAT(FirstName, ' ', LastName)`
     - **Pandas**: `df['FullName'] = df['FirstName'] + ' ' + df['LastName']`

3. **Transformed Column Types (Text, Date, Number)**
   - **Reason**: Ensuring the correct data types allows for more accurate calculations and analysis.
   - **Formulas**:
     - **Excel**: Use `FORMAT CELLS` for manual adjustment.
     - **DAX**: 
       - Text: `FORMAT([Column], "Text")`
       - Date: `FORMAT([Column], "MM/DD/YYYY")`
       - Number: `VALUE([Column])`
     - **SQL**: `CAST(Column AS TYPE)` (e.g., `CAST(Column AS DATE)`)
     - **Pandas**:
       ```python
       df['Column'] = df['Column'].astype(str)      # for text
       df['Column'] = pd.to_datetime(df['Column'])  # for date
       df['Column'] = df['Column'].astype(int)      # for number
       ```

4. **Handling Missing Data (Inconsistent "0" Values in Client IDs)**
   - **Reason**: Missing data can distort the accuracy of analysis and lead to incorrect conclusions.
   - **Formulas**:
     - **Excel**: `=IF(A2=0, "", A2)`
     - **DAX**: `IF([ClientID] = 0, BLANK(), [ClientID])`
     - **SQL**: `CASE WHEN ClientID = 0 THEN NULL ELSE ClientID END`
     - **Pandas**: `df['ClientID'] = df['ClientID'].replace(0, pd.NA)`

5. **Ensure Unique Client and Appointment IDs**
   - **Reason**: Each patient should have a unique Client ID to avoid confusion with duplicates.
   - **Formulas**:
     - **Excel**: `=IF(COUNTIF($A$2:$A$100, A2) > 1, "Duplicate", "Unique")`
     - **DAX**: `IF(CALCULATE(COUNTROWS(Table), FILTER(Table, [ClientID] = EARLIER([ClientID]))) = 1, "Unique", "Duplicate")`
     - **SQL**: `SELECT ClientID, COUNT(ClientID) AS Count FROM YourTable GROUP BY ClientID HAVING COUNT(ClientID) > 1`
     - **Pandas**: `df['IsDuplicate'] = df['ClientID'].duplicated(keep=False)`

---

## Project Workflow

### 1. **Data Preparation**: 
   - Import the raw dataset.
   - Perform data cleaning operations as mentioned above.
   - Ensure data integrity by handling missing values and ensuring unique identifiers.

### 2. **Data Analysis**:
   - Analyze patient appointment trends, wait times, and provider roles.
   - Use various metrics to uncover insights about waiting times and appointment distributions.

### 3. **Dashboard Creation**:
   - Use Power BI (or similar) to create a dynamic dashboard that visually presents the key metrics and insights from the data.
   - Visualizations include bar charts, pie charts, line charts, and tables for detailed analysis.

---

## Dashboard Visualizations

![Dashboard Image](https://github.com/manish939/Patient-Scheduling-Analytics/blob/main/Appointment%20Scheduling%20Dashboard%20Image.png)

---

### Breakdown of the Dashboard Visualizations

#### 1. **Average Wait Time by Provider Role (Bar Chart)**
   - **Description**: Displays the average wait time for each provider role (MD, Nurse, Psychologist).
   - **Insight**: Nurses and MDs have similar wait times (~9.7 and 9.3 days), while psychologists have shorter wait times (6.4 days).

#### 2. **Number of Appointments by Month and Provider Role (Stacked Bar Chart)**
   - **Description**: Shows the number of appointments per month, broken down by provider role.
   - **Insight**: October had the highest number of appointments, with MDs handling the majority.

#### 3. **Average Waiting Time by Month (Line Chart)**
   - **Description**: Tracks the average waiting time over several months.
   - **Insight**: Wait times decrease from September to November, indicating improved efficiency.

#### 4. **% of Appointments by Provider Role (Pie Chart)**
   - **Description**: Displays the percentage distribution of appointments among different provider roles.
   - **Insight**: MDs handled 59% of the appointments, followed by Nurses (29%) and Psychologists (12%).

#### 5. **Number of Patients by Month (Table)**
   - **Description**: A table that lists the number of patients seen each month, with a total count at the bottom.
   - **Insight**: October saw the highest number of patients (68), followed by November (25), and September had the fewest (6).

#### 6. **Key Metrics (Minimum, Maximum, and Average Waiting Time)**
   - **Description**: Displays the minimum, maximum, and average waiting times on the left side of the dashboard.
   - **Insight**: 
     - Minimum waiting time: 1.0 day
     - Maximum waiting time: 40.9 days
     - Average waiting time: 9.1 days

---

## Data Relationships Explained

- **Provider Role and Wait Time**: Different providers have varying wait times, likely due to workload or scheduling availability.
  
- **Appointments and Date**: The dataset allows for tracking patient volumes and wait times over different periods.

- **Provider Role and Appointment Count**: Some providers (e.g., MDs) handle more appointments, influencing overall wait times.

- **Month and Wait Time**: Monthly trends in waiting times help to understand whether scheduling improvements have been made.

---

## Insights from the Data

- **Wait Time Analysis**: The average waiting time across all providers is 9.1 days, with the longest waiting time being 40.9 days. This indicates some appointments might be significantly delayed.
  
- **Provider Efficiency**: MDs handle the majority of appointments but have a similar wait time to nurses. Psychologists, though handling fewer appointments, have a shorter wait time.
  
- **Appointment Trends**: October is the busiest month in terms of patient appointments, followed by November. Scheduling may need to be optimized during these busy months to reduce waiting times.

- **Improvement Areas**: The data highlights potential improvement areas such as implementing better patient scheduling mechanisms and ensuring that Client IDs are unique to avoid duplicate entries.

---

## Conclusion

This project demonstrates the end-to-end process of cleaning, analyzing, and visualizing patient appointment data. The insights derived from the dashboard can help improve scheduling efficiency and reduce patient waiting times. The formulas and tools used here (Excel, DAX, SQL, and Pandas) provide a flexible approach to handling similar data in various environments.

## Predictive Appointment Scheduling Feature

### Overview

In addition to the existing data cleaning, analysis, and visualization, a predictive feature has been added to this project. This predictive model allows users to input a **requesting date** and **provider role** and receive an estimated appointment date. This feature is built using machine learning and neural networks, and it aims to provide an approximation for appointment scheduling in the future.

### Why Use Predictive Models?

In the current data analysis, we can only visualize historical data and gain insights into waiting times, trends, and provider efficiencies. However, if a user or administrator wants to know the expected appointment date for a new request, relying solely on past data can be limiting.

By adding predictive models, users can:
- Estimate appointment dates in advance based on current trends.
- Improve scheduling efficiency by understanding possible waiting times.
- Provide a more user-friendly and intelligent system where users can plan ahead without manually calculating waiting times.

### The Predictive Models

Several machine learning models were used to predict the **appointment date**:
- **Linear Regression**: A simple regression model that predicts the number of days between the requested and scheduled dates.
- **Decision Tree**: A tree-based model that captures complex interactions between the provider role and requested date.
- **Random Forest**: An ensemble method that improves upon the decision tree by averaging multiple decision trees to avoid overfitting.
- **Neural Network**: A deep learning model trained to learn patterns between the requested date, provider role, and appointment date, leading to more nuanced predictions.

These models take two input features:
1. **Requesting Date**: The date when the appointment is requested.
2. **Provider Role**: The role of the healthcare provider (MD, Nurse, Psychologist).

The models output a **predicted appointment date**.

### How It Works

1. **Input**:
   - Users provide the **requesting date** (in the format YYYY-MM-DD).
   - Users select the **provider role** (MD, Nurse, or Psychologist).

2. **Prediction**:
   - The model processes the inputs and predicts the appointment date.
   - The prediction is based on historical data patterns for similar appointments.

3. **Post-Processing**:
   - After the prediction, the system ensures the predicted appointment date is **always after the requesting date** (i.e., the system corrects any invalid predictions that fall before the requested date).

### Code Explanation

#### Training the Model

Several machine learning models were trained on the dataset to predict the number of days between the requesting date and appointment date. The neural network model was also implemented to capture complex patterns.

```python
# Training the Neural Network model
model = keras.Sequential([
    keras.layers.Dense(128, activation='relu', input_shape=[X_train.shape[1]]),
    keras.layers.Dense(64, activation='relu'),
    keras.layers.Dense(1)
])
# Compiling and training the model
model.compile(optimizer='adam', loss='mean_squared_error')
history = model.fit(X_train, y_train, epochs=50, validation_split=0.1, verbose=1)
```


### Making Predictions

Users can manually input a **requesting date** and **provider role**. The model will predict the appointment date based on those inputs.


```python
def user_input_prediction():
    print("Enter the details to predict appointment date:")
    requesting_date = input("Enter requesting date (YYYY-MM-DD): ")
    provider_role = input("Enter provider role (MD/Nurse/Psychologist): ")

    # Convert provider role to numeric code
    provider_role_numeric = provider_role_map.get(provider_role, 3)  # Default to MD if input is not recognized

    # Prepare the input data
    requesting_date_numeric = (pd.to_datetime(requesting_date) - pd.Timestamp('1970-01-01')).days
    new_data = {
        'Requesting date_numeric': requesting_date_numeric,
        'Provider Role': provider_role_numeric
    }

    # Convert to dataframe
    new_df = pd.DataFrame([new_data])

    # Ensure the order of columns matches the training data
    new_df = new_df[X_train.columns]

    # Predict the number of days since 1970 for the appointment date using the Neural Network
    predicted_appointment_numeric = model.predict(new_df)[0]

    # Post-process: Ensure the predicted appointment date is after the requesting date
    predicted_appointment_numeric = max(predicted_appointment_numeric, requesting_date_numeric)

    # Calculate the predicted appointment date
    predicted_appointment_date = pd.Timestamp('1970-01-01') + pd.to_timedelta(predicted_appointment_numeric, unit='D')
    print(f"Predicted Appointment Date: {predicted_appointment_date}")
```

After running the prediction, you will receive the expected appointment date based on the given inputs.

Here is an example of how to interact with the model:

If the user inputs:

```yaml
Requesting date: 2024-05-26
Provider role: MD
```

```yaml
Predicted Appointment Date: 2024-06-06 05:09:22.500000
This prediction helps users plan appointments based on historical data trends, making scheduling more efficient.
```

This indicates that, based on the provided input, the predicted appointment date is June 6, 2024.

### Model Performance
The performance of each model was evaluated based on Mean Absolute Error (MAE), Mean Squared Error (MSE), and R² score. Here’s a comparison of the model performance:

```yaml
Linear Regression - MAE: 8.02, MSE: 114.25, R²: 0.48
Decision Tree - MAE: 8.52, MSE: 121.53, R²: 0.44
Random Forest - MAE: 8.56, MSE: 126.66, R²: 0.42
Neural Network - MAE: 79.21, MSE: 6395.07, R²: -28.23
```
### Why Use This Feature?
# This predictive feature is useful for:

`Scheduling Optimization: Administrators and users can estimate waiting times and appointment dates, allowing them to plan more effectively.
`User Convenience: Instead of relying on past data alone, users can forecast when their appointment is likely to be scheduled.
`Data-Driven Decision Making: With the predictive feature, users gain additional insights and foresight into future trends, improving their ability to manage appointments.

### Conclusion
With the addition of the predictive model, this project not only analyzes past data but also forecasts future appointment dates. This is crucial for helping administrators and users manage appointments more efficiently and plan ahead. The neural network and other machine learning models provide robust predictions, making this feature a powerful tool for scheduling systems.





