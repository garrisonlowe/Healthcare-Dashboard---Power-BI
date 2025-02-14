Check out the dashboard here!

# Healthcare Data Analytics Dashboard

## **Project Description:**

This project aims to analyze healthcare data to gain insights into patient demographics, medical conditions, hospital stays, billing amounts, and test results. The goal is to create a Power BI dashboard that provides an intuitive and interactive overview of key healthcare metrics. By analyzing trends in patient admissions, medical conditions, and financial aspects, healthcare administrators can make data-driven decisions to improve hospital efficiency and patient care.

## **Problem Statement:**

Hospitals and healthcare providers struggle with managing large volumes of patient data effectively. Without clear visualizations and analysis, identifying patterns in admissions, medical conditions, billing, and treatment outcomes can be challenging. This project seeks to develop an interactive dashboard that presents key insights, allowing stakeholders to monitor hospital performance, financial trends, and patient demographics efficiently.

## **Dashboard Requirements:**

1. **Yearly Overview Page**
    - KPIs:
        - Total Admissions
        - Total Billing Amount
        - Average Length of Stay
        - Readmission Rate
    - Visuals:
        - Monthly Admissions Trend (Column Chart)
        - Billing Amount Over Time (Line Chart)
        - Readmission Rate by Medical Condition (Bar Chart)
        - Count of Admission Type (Pie Chart)
        - Test Result Breakdown (Pie Chart and Column Chart)
    - Filters:
        - Year
2. **Patient Demographics**
    - Visuals:
        - Total Patients (KPI)
        - Age distribution (Histogram)
        - Gender breakdown (Pie Chart)
        - Patients by Race (Column Chart
        - Blood type distribution (Bar Chart)
        - Admissions by insurance provider (Bar Chart)
    - Filters:
        - Year
        - Month
3. **Hospital Admissions & Treatments**
    - Visuals:
        - Average Length of Stay (KPI)
        - Total Billing (KPI)
        - Top 5 Medical Conditions (Column Chart)
        - Admissions by hospital (Bar Chart)
        - Admission types (Pie Chart)
        - Medical conditions by frequency (Treemap)
    - Filters:
        - Year
        - Month
4. **Financial Overview**
    - Visuals:
        - Total billing amount (KPI Card)
        - Billing amount by hospital (Bar Chart)
        - Average billing by admission type (Column Chart)
        - Highest billed patients (Table)
    - Filters:
        - Year
        - Month
5. **Patient Outcomes & Test Results**
    - Visuals:
        - Readmission Rate (KPI)
        - Patient outcomes (Bar Chart)
        - Test results by medical condition (Stacked Column Chart)
        - Top 5 Average length of stay by medical condition (Bar Chart)
        - Readmission rates (Line Chart)
        - Most readmitted patients (Table)
    - Filters:
        - Year
        - Month

## **Data Cleaning Steps:**

1. **Standardizing Text Data:**
    - Convert all text fields (e.g., Name, Doctor, Hospital) to proper case.
    
    ```python
    # Normalizing the Name column
    df['Name'] = df['Name'].str.title()
    ```
    
    - Remove any trailing or leading spaces in string fields.
    
    ```python
    # Fixing some naming issues
    df['Name'] = df['Name'].str.replace(' Md', '')
    df['Name'] = df['Name'].str.replace('Jr.', ' Jr')
    df['Name'] = df['Name'].str.replace(' Dds', '')
    df['Name'] = df['Name'].str.replace(' Iii', ' III')
    ```
    
2. **Handling Missing Data:**
    - Identify and document any missing values.
    
    ```python
    # Checking the values in the 'Gender' column
    df['Gender'].value_counts()
    ```
    
    - If applicable, fill missing values with appropriate placeholders (e.g., ‘Unknown’ for missing hospital names).
    - For numerical fields (e.g., billing amount), consider imputation using median values.
3. **Date Formatting:**
    - Convert ‘Date of Admission’ and ‘Discharge Date’ to standard date format (YYYY-MM-DD).
    
    ```python
    # Converting the Date of Admission column to datetime
    df['Date of Admission'] = pd.to_datetime(df['Date of Admission'])
    
    # Converting the Date of Discharge column to datetime
    df['Discharge Date'] = pd.to_datetime(df['Discharge Date'])
    ```
    
    - Calculate ‘Length of Stay’ as the difference between discharge and admission dates.
    
    ```python
    # Creating a column to calculate the length of stay
    df['Length of Stay'] = df['Discharge Date'] - df['Date of Admission']
    
    #Converting the length of stay to a number
    df['Length of Stay'] = df['Length of Stay'].dt.days
    ```
    
4. **Data Consistency:**
    - Ensure gender values are standardized (e.g., ‘Male’, ‘Female’ instead of variations like ‘M’ or ‘F’).
    - Check and validate blood type categories.
5. **Outlier Detection:**
    - Identify extreme billing amounts and investigate potential errors.
    - Flag anomalies in patient age (e.g., negative or unrealistic values).

## **Custom Data Creation:**

- For this project I did a lot off custom data creation to enhance the dataset to be more realistic and skewed in certain areas. The original dataset was very uniform (every column had the same amount for all values).
- Here is what I did:
1. Added more conditions with random weights for distribution
    
    ```python
    import random
    
    # Adding in more conditions and giving them random weights.
    conditions = [
        'Diabetes', 'Hypertension', 'Asthma', 'Cancer', 'HIV', 'Malaria', 'Tuberculosis', 'Pneumonia', 'Bronchitis', 'Arthritis',
        'Osteoporosis', 'Heart Disease', 'Stroke', 'Kidney Disease', 'Liver Disease', 'Obesity', 'Anemia', 'Epilepsy', 'Migraine',
        "Alzheimer's Disease", 'Parkinson\'s Disease', 'Multiple Sclerosis', 'Lupus', 'Sickle Cell Disease', 'Cystic Fibrosis',
        'Chronic Fatigue Syndrome', 'Fibromyalgia', 'Rheumatoid Arthritis', 'Chronic Obstructive Pulmonary Disease', 'Endometriosis',
        'Polycystic Ovary Syndrome', 'Hypothyroidism', 'Hyperthyroidism', 'Gout', 'Glaucoma', 'Cataracts', 'Macular Degeneration',
        'Retinal Detachment', 'Retinitis Pigmentosa', 'Color Blindness', 'Hearing Loss', 'Tinnitus', 'Vertigo', 'Meniere\'s Disease',
        'Motion Sickness', 'Insomnia', 'Sleep Apnea', 'Narcolepsy'
    ]
    
    weights = [
        0.200, 0.150, 0.100, 0.050, 0.040, 0.030, 0.025, 0.020, 0.018, 0.017,
        0.016, 0.015, 0.014, 0.013, 0.012, 0.011, 0.010, 0.009, 0.008, 0.007,
        0.006, 0.005, 0.004, 0.003, 0.002, 0.001, 0.001, 0.001, 0.001, 0.001,
        0.001, 0.001, 0.001, 0.001, 0.001, 0.001, 0.001, 0.001, 0.001, 0.001,
        0.001, 0.001, 0.001, 0.001, 0.001, 0.001, 0.001, 0.001
    ]
    
    df['Medical Condition'] = random.choices(conditions, weights=weights, k=len(df))
    ```
    
2. Adding in more hospitals and giving them random weights.
    
    ```python
    # Adding in more hospitals and giving them random weights.
    hospitals = [
        "Riverside General Hospital", "Summit Medical Center", "Grandview Health System",
        "Oakwood Community Hospital", "Evergreen Regional Medical Center", "Harmony Valley Hospital",
        "Crestview Medical Institute", "Pioneer Healthcare Center", "Northgate Memorial Hospital",
        "Sunridge Medical Plaza"
    ]
    weights = [0.172, 0.145, 0.133, 0.121, 0.110, 0.098, 0.087, 0.065, 0.037, 0.032]
    df['Hospital'] = random.choices(hospitals, weights=weights, k=len(df))
    
    ```
    
3. Adding more admission types and gave them random weights.
    
    ```python
    # Define admission types and their corresponding weights
    admission_types = ['Emergency', 'Urgent', 'Elective', 'Newborn', 'Trauma']
    weights = [0.421, 0.179, 0.222, 0.128, 0.050]
    
    # Generate admission types based on the specified weights
    df['Admission Type'] = random.choices(admission_types, weights=weights, k=len(df))
    ```
    
4. Creating a more realistic distrobition of blood types, the dataset had them all the same.
    
    ```python
    # Define blood types and their corresponding weights
    import numpy as np
    
    blood_types = ['A+', 'A-', 'B+', 'B-', 'AB+', 'AB-', 'O+', 'O-']
    blood_type_weights = [0.34, 0.06, 0.09, 0.02, 0.04, 0.01, 0.37, 0.07]
    
    # Create a dictionary to store the blood type for each name
    name_to_blood_type = {}
    
    # Assign random blood type to each unique name with weights
    for name in df['Name'].unique():
        name_to_blood_type[name] = np.random.choice(blood_types, p=blood_type_weights)
    
    # Map the blood type to each record based on the name
    df['Blood Type'] = df['Name'].map(name_to_blood_type)
    ```
    
5. Added more genders.
    
    ```python
    # Define possible genders and their weights
    genders = ['Male', 'Female', 'Other', 'Non-Conforming']
    gender_weights = [0.42, 0.39, 0.08, 0.11]
    
    # Create a dictionary to store the gender for each name
    name_to_gender = {}
    
    # Assign random gender to each unique name with weights
    for name in df['Name'].unique():
        name_to_gender[name] = np.random.choice(genders, p=gender_weights)
    
    # Map the gender to each record based on the name
    df['Gender'] = df['Name'].map(name_to_gender)
    ```
    
6. Creating a more realistic age distribution.
    
    ```python
    # Generate random ages between 0 and 100
    import numpy as np
    
    # Define age ranges and their corresponding probabilities
    age_ranges = [(0, 9), (10, 19), (20, 29), (30, 39), (40, 49), (50, 59), (60, 69), (70, 79), (80, 89), (90, 99)]
    age_weights = [0.10, 0.05, 0.05, 0.05, 0.10, 0.20, 0.20, 0.15, 0.08, 0.02]
    
    # Create a dictionary to store the age for each name
    name_to_age = {}
    
    # Assign random age to each unique name within the specified ranges and probabilities
    for name in df['Name'].unique():
        age_range = np.random.choice(len(age_ranges), p=age_weights)
        name_to_age[name] = np.random.randint(age_ranges[age_range][0], age_ranges[age_range][1] + 1)
    
    # Map the age to each record based on the name
    df['Age'] = df['Name'].map(name_to_age)
    ```
    
7. Enhanced the billing amounts
    
    ```python
    import numpy as np
    
    # Generate a log-normal distribution for the billing amounts
    main_billing_amounts = np.random.lognormal(mean=10, sigma=1, size=int(len(df) * 0.95))
    
    # Scale the values to the desired range (100 to 90,000)
    main_billing_amounts = 100 + (main_billing_amounts - main_billing_amounts.min()) * (90000 - 100) / (main_billing_amounts.max() - main_billing_amounts.min())
    
    # Generate outliers
    outliers = np.random.lognormal(mean=12, sigma=1, size=int(len(df) * 0.05))
    
    # Scale the outliers to the desired range (100 to 90,000)
    outliers = 100 + (outliers - outliers.min()) * (90000 - 100) / (outliers.max() - outliers.min())
    
    # Combine the main billing amounts and outliers
    billing_amounts = np.concatenate([main_billing_amounts, outliers])
    
    # Shuffle the billing amounts to mix the outliers with the main data
    np.random.shuffle(billing_amounts)
    
    # Assign the billing amounts to the DataFrame
    df['Billing Amount'] = billing_amounts
    
    df['Billing Amount'] = billing_amounts.round(2)
    ```
    
8. Added more insurance companies
    
    ```python
    insurance_companies = ['Aetna', 'Anthem', 'Blue Cross Blue Shield', 'Cigna', 'Humana', 'UnitedHealthcare', 'Kaiser Permanente', 'Molina Healthcare', 'Centene', 'Oscar Health']
    weights = [0.20, 0.18, 0.15, 0.12, 0.10, 0.08, 0.07, 0.05, 0.03, 0.02]
    
    # Generate blood types based on the specified weights
    df['Insurance Provider'] = random.choices(insurance_companies, weights=weights, k=len(df))
    ```
    
9. Added more races
    
    ```python
    races = ['Asian', 'Black', 'Hispanic', 'White', 'Other']
    
    # Create a dictionary to store the race for each name
    name_to_race = {}
    
    # Assign random race to each unique name
    for name in df['Name'].unique():
        name_to_race[name] = random.choices(races, weights=[0.27, 0.21, 0.25, 0.61, 0.11])[0]
    
    # Map the race to each record based on the name
    df['Race'] = df['Name'].map(name_to_race)
    ```
    
10.  Added more test results
    
    ```python
    # Define possible test results and their weights
    test_results = ['Normal', 'Abnormal', 'Inconclusive']
    weights = [0.70, 0.20, 0.10]
    
    # Generate test results based on the specified weights
    df['Test Results'] = random.choices(test_results, weights=weights, k=len(df))
    ```
    
11.  Added in an Outcomes column.
    
    ```python
    #Adding in a patient outcome column
    outcomes = ['Discharged', 'Transferred']
    weights = [0.87, 0.13]
    
    # Generate patient outcomes based on the specified weights
    df['Patient Outcome'] = random.choices(outcomes, weights=weights, k=len(df))
    ```
    
12.  Adjusted the readmitted column
    
    ```python
    # Changing readmitted column to say "Readmitted" or "Not Readmitted"
    df['Readmitted'] = df['Readmitted'].map({True: 'Readmitted', False: 'Not Readmitted'})
    
    # Determine if any row for each name has been readmitted
    name_readmitted = df.groupby('Name')['Readmitted'].transform(lambda x: 'Readmitted' if 'Readmitted' in x.values else 'Not Readmitted')
    
    # Map the result back to the DataFrame
    df['Readmitted'] = name_readmitted
    ```
    

## **Creating the Data Model:**

1. Create a Date Table

```
DateTable = 
ADDCOLUMNS(
    CALENDAR(DATE(2019, 1, 1), DATE(2024, 12, 31)),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Month Short", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Week Number", WEEKNUM([Date]),
    "Day of Week", FORMAT([Date], "dddd"),
    "Day Short", FORMAT([Date], "ddd"),
    "Is Weekend", IF(WEEKDAY([Date]) IN {1, 7}, TRUE, FALSE)
)
```

2. Assign relationships in Data Model

![Image](https://github.com/user-attachments/assets/15e943d6-d66d-494e-897a-5c594a69ad57)

## **Photos of finished dashboard:**

Yearly Overview page:

![Image](https://github.com/user-attachments/assets/d9d397e0-982b-4409-8861-65baa6e0ba3c)

Patient Demographics page:

![Image](https://github.com/user-attachments/assets/6e5b2506-825d-415c-9547-1d8cb4694956)

Hospital Details page:

![Image](https://github.com/user-attachments/assets/46228d1d-8446-4701-8f6f-06c422e7a3a5)

Financial Details page:

![Image](https://github.com/user-attachments/assets/17da7dd7-d874-4433-a443-0d18d605fa7d)

Patient Outcomes page:

![Image](https://github.com/user-attachments/assets/40962529-378b-4c0a-9d3d-302c43169e01)