# Car Insurance Claims Analysis and Fraud Detection

## Project Description
This project focuses on analyzing a car insurance claims dataset to derive actionable insights and detect fraudulent claims. The dataset, spanning 1994 to 1996, includes detailed information about accidents, policyholders, vehicles, and claims. We leverage **Power BI** for interactive visualizations and **machine learning models** to identify patterns indicative of fraud.

## Objectives
- **Data Analysis with Power BI**: Create interactive dashboards to visualize claim trends, such as:
  - Claims distribution by month, day, or accident area.
  - Demographic analysis of policyholders (age, sex, marital status).
  - Vehicle characteristics (make, price, category).
- **Fraud Detection with Machine Learning**: Develop ML models to:
  - Detect fraudulent claims based on features like claim delay, past claims, or supplement counts.
  - Identify anomalies in claim patterns, such as unusual delays between accident and claim filing.

## Dataset Description
The dataset (`carclaims.csv`) contains **15,420** car insurance claim records from 1994 to 1996. Each record includes details about the accident, policyholder, vehicle, and claim processing. A key feature is the `FraudFound` column, which indicates whether a claim was flagged as fraudulent. The dataset is ideal for exploratory data analysis and fraud detection tasks.

### Metadata
The dataset consists of **34 columns** with the following details:

| Column Name            | Data Type     | Description                                      |
|------------------------|---------------|--------------------------------------------------|
| Month                 | Categorical   | Month of accident (e.g., Jan, Dec)              |
| WeekOfMonth           | Numerical     | Week of month for accident (1–5)               |
| DayOfWeek             | Categorical   | Day of accident (e.g., Monday, Sunday)         |
| Make                  | Categorical   | Vehicle manufacturer (e.g., Honda, Toyota)      |
| AccidentArea          | Categorical   | Accident location (Urban, Rural)               |
| DayOfWeekClaimed      | Categorical   | Day claim was filed                            |
| MonthClaimed          | Categorical   | Month claim was filed                          |
| WeekOfMonthClaimed    | Numerical     | Week of month for claim filing (1–5)           |
| Sex                   | Categorical   | Policyholder gender (Male, Female)             |
| MaritalStatus         | Categorical   | Policyholder marital status (Single, Married, Divorced) |
| Age                   | Numerical     | Policyholder age (0 = missing/underage)          |
| Fault                 | Categorical   | Party at fault (Policy Holder, Third Party)    |
| PolicyType            | Categorical   | Policy type (e.g., Sport - Liability)          |
| VehicleCategory       | Categorical   | Vehicle category (Sport, Sedan, Utility)       |
| VehiclePrice          | Categorical   | Vehicle price range (e.g., more than 69,000)    |
| PolicyNumber          | Numerical     | Unique policy identifier                       |
| RepNumber             | Numerical     | Claim representative ID                        |
| Deductible            | Numerical     | Deductible amount (e.g., 300, 400)             |
| DriverRating          | Numerical     | Driver rating (1–4)                            |
| Days:Policy-Accident  | Categorical   | Days between policy start and accident         |
| Days:Policy-Claim     | Categorical   | Days between policy start and claim            |
| PastNumberOfClaims    | Categorical   | Number of past claims (e.g., none, 2 to 4)     |
| AgeOfVehicle          | Categorical   | Vehicle age (e.g., new, 7 years)               |
| AgeOfPolicyHolder     | Categorical   | Policyholder age range (e.g., 31 to 35)        |
| PoliceReportFiled     | Boolean       | Police report filed (Yes, No)                  |
| WitnessPresent        | Boolean       | Witness present (Yes, No)                      |
| AgentType             | Categorical   | Agent type (External, Internal)                |
| NumberOfSuppliments   | Categorical   | Number of supplements (e.g., none, 1 to 2)     |
| AddressChange-Claim   | Categorical   | Time since address change (e.g., no change)    |
| NumberOfCars          | Categorical   | Number of insured cars (e.g., 1 vehicle)       |
| Year                  | Numerical     | Claim year (1994–1996)                         |
| BasePolicy            | Categorical   | Base policy type (Liability, Collision, All Perils) |
| FraudFound            | Boolean       | Fraudulent claim (Yes, No)                     |

## Data Cleaning
- **Remove Age = 0**: Filter out records where the `Age` column is 0, as this likely indicates missing or invalid data for underage policyholders, which could skew analysis. This step ensures the dataset contains only valid age values for meaningful insights.

## Data Transformation
- **Has Past Claim**: Create a binary column `HasPastClaim` based on the `PastNumberOfClaims` column. If `PastNumberOfClaims` is "none," set `HasPastClaim` to "No"; otherwise, set it to "Yes" to simplify fraud detection and analysis.
- **Month Diff**: Calculate a new column `MonthDiff` representing the difference in months between `Month of Accident` and `Month of Claim` to identify delays that might indicate fraud.
- **No Verification**: Create a binary column `NoVerification` based on the `PoliceReportFiled` and `WitnessPresent` columns. If both are "No," set `NoVerification` to "Yes"; otherwise, set it to "No" to flag claims lacking verification.

## Data Modeling
- **Star Schema**: Implement a star schema for data modeling, as shown below. The schema includes:
  - **Fact Table**: Central table containing claim details (`Claim Key`, `Client Key`, `Company Key`, `Date Key`, `Policy Key`, `Vehicle Key`, `FraudFound`).
  - **Dimension Tables**:
    - **Date Dim**: Contains date-related attributes (`Date_ID`, `DayOfWeek`, `DayOfWeekClaimed`, `Month of Accident`, `Month of Claim`, `MonthClaimed`, `WeekOfMonth`, `WeekOfMonthClaimed`, `Year`).
    - **Company Dim**: Includes company details (`AgentType`, `Company_ID`, `RepNumber`).
    - **Client Dim**: Holds client information (`Age`, `AgentOfPolicyHolder`, `Client_ID`, `MaritalStatus`, `Sex`).
    - **Claim Dim**: Contains claim-specific data (`AccidentArea`, `Claim_ID`, `Deductible`, `Fault`, `No Verification`, `NumberOfSupplements`, `PoliceReportFiled`, `WitnessPresent`).
    - **Policy Dim**: Includes policy details (`AddressChange-Claim`, `BasePolicy`, `DaysPolicy-Accident`, `DaysPolicy-Claim`, `DriverRating`, `HasPastClaim`, `NumberOfCars`, `PastNumberOfClaims`, `Policy_ID`, `PolicyType`).
    - **Vehicle Dim**: Covers vehicle attributes (`AgeOfVehicle`, `Make`, `Vehicle_ID`, `VehicleCategory`, `VehiclePrice`).
- **Schema Diagram**:
  ![Star Schema Diagram](https://github.com/HazemMedhat/Automobile-Insurance-Fraud-Analysis-Detection/blob/ba78d7e50ece23b1e84bde6059ad041c7965f3a4/Data%20Modeling%20.png)

## Dashboard
- **Overview of Fraud**
  ![Overview of Fraud](https://github.com/HazemMedhat/Automobile-Insurance-Fraud-Analysis-Detection/blob/ba78d7e50ece23b1e84bde6059ad041c7965f3a4/Fraud%20Overview%20Dashboard%20.png)

- **Screenshot 2: Fraud Distribution by Age Group**
  ![Customer & Company & Vehicle Profile](https://github.com/HazemMedhat/Automobile-Insurance-Fraud-Analysis-Detection/blob/ba78d7e50ece23b1e84bde6059ad041c7965f3a4/Customer%20%26%20Company%20%26%20Vehicle%20Profile.png)

- **Screenshot 3: Vehicle Price vs. Claim Amount**
  ![Claims & Policy](https://github.com/HazemMedhat/Automobile-Insurance-Fraud-Analysis-Detection/blob/ba78d7e50ece23b1e84bde6059ad041c7965f3a4/Claim%20%26%20Policy%20Dashboard%20.png)

## Key Insights
  - Fraudulent claims are most prevalent under the "Sedan - All Perils" policy type, indicating a need for enhanced scrutiny in this category.
  - Urban areas exhibit a significantly higher incidence of fraudulent claims compared to rural areas, suggesting a focus on urban claim patterns.
  - The majority of fraudulent claims are associated with policyholders aged 31 to 35, pointing to this age group as a key area for fraud prevention.
  - Married policyholders account for the largest share of fraudulent claims, suggesting targeted monitoring of this demographic.
  - Fraudulent claims with no supplements are the most frequent, highlighting the importance of enforcing stricter documentation requirements.


## Preprocessing
- **Encoding Features**: Use target encoding for categorical variables like `Make`, `AccidentArea`, and `PolicyType` to encode them based on the mean of the `FraudFound` target variable. Apply label encoding to binary columns like `PoliceReportFiled` and `WitnessPresent` to convert "Yes"/"No" to 0/1.
- **Standard Scaling**: Apply standard scaling to numerical features (e.g., `Age`, `Deductible`, `MonthDiff`) to normalize the data, ensuring all features contribute equally to the ML models.
- **SMOTE for Imbalanced Classes**: Use Synthetic Minority Over-sampling Technique (SMOTE) to address the imbalance in the `FraudFound` column, generating synthetic samples for the minority class (fraudulent claims) to balance the dataset.

## ML Models with Results
- **Random Forest**: Achieved an accuracy of 93.7%,indicating good performance in detecting fraud.
- **XGBoost**: Achieved an accuracy of 94%, showing slightly better precision and recall compared to Random Forest.
