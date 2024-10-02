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

![Dashboard Image](path/to/dashboard_image.png)

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
