# EDA-Project
<img src="https://user-images.githubusercontent.com/47347216/54076821-ae39c680-42d5-11e9-8cc7-1090358aca62.png?raw=true" width="840" height="260" align="middle" />
<a id=section1></a> 

## 1. Problem Statement

"US H1B Visa dataset has extracted from US Department of labour Employment site. This public disclosure file contains 2009 year data about the US employeers who employed foreign workers and also the details related to their H1B petition"
<a id=section101></a> 
### 1.1. Introduction
This Exploratory Data Analysis is to practice Python skills learned till now on a structured data set including loading, inspecting, wrangling, exploring, and drawing conclusions from data. The notebook has observations with each step in order to explain thoroughly how to approach the data set. Based on the observation some questions also are answered in the notebook for the reference though not all of them are explored in the analysis. 

<a id=section102></a> 
### 1.2. Data source and dataset

__a__. How was it collected? 

- __Name__: "US H1B Visa-2009 Data Set"
- __Sponsoring Organization__: US Department of Labour Employment
- __Year__: 2009
- __Description__: "US H1B Visa dataset has extracted from US Department of labour Employment website. This public disclosure file contains 2009 year data about the US employeers who employed foreign workers and also the details related to their H1B petition"

__b__. Is it a sample? If yes, was it properly sampled?
- Yes, it is a sample. H1B Dataset have collected from the reliable source. This public disclosure file contains 268244 of Visa cases, however, I have considered One lakh records of H1B petition for the year 2009 but it appears *not* to be a random sample, so we can assume that it is not representative.

DataSource 
https://www.foreignlaborcert.doleta.gov/performancedata.cfm > Disclosure Data Tab > LCA  Programs (H-1B, H-1B1, E-3)
DataSheet Link : https://www.foreignlaborcert.doleta.gov/docs/lca/H-1B_Case_Data_FY2009.xlsx

<a id=section2></a> 
### 2. Load the packages and data 

#### Install SeaBorn & pandas_profiling
```python
pip install pandas_profiling
pip install seaborn
```             

# Import packages
import pandas as pd
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt

pd.set_option('display.max_columns', 100) 
H1_status = pd.read_csv ('https://raw.githubusercontent.com/karthikeyanbalusamy/project/master/H1B_data.csv')
city_master = pd.read_csv ('C:/INSAID/Project/visa/city_repo.csv') # This CSV file contains the master US city data 
H1_status.sample (3)

<a id=section3></a> 
### 3. Data Profiling

Review the data types and sample data to understand what variables we are dealing with?<br>
Which variables need to be transformed in some way before they can be analyzed?

Below is the reference link for the Column Description
https://www.foreignlaborcert.doleta.gov/pdf/H-1B%20Efile%20Record%20Layout%20FY09.rtf

Additionally, below key-column of the data will be analyzed and perform the necessary transformations as part of Data Engineering.
SUBMITTED_DATE, CASE_NO, EMPLOYER_NAME, NBR_IMMIGRANTS, JOB_TITLE, APPROVAL_STATUS, WAGE_RATE_1,
PROGRAM_DESIGNATION,PREVAILING_WAGE_1,CITY_1

#### Observations

#Noticed the CITY column contains many invalid data. For Example:
'1','chicago illinois metro area analysis','bethesda-gaithersburg-frederick, md metro'  etc. 
Hence, Master city csv file is created with all the US CITY names, which will be compared with the CITY field in the H1B Dataset. Invalid city data will be identified and replaced with NaN.

#Analysis of Program Designation 
'PROGRAM_DESIGNATION' has the following values : 'R', 'A', 'C', 'S'

#### Observations
Program Designation have the code, which indicates the type of temporary application submitted for processing. R = H-1B; A = E-3 Australian; C = H-1B1 Chile; S = H-1B1 Singapore. Hence the description column will be created and map the code with Visa description for better understanding & readability.

#PREVAILING_WAGE_1
#### Observations
While analyzing, noticed that the JOB_TITLE column have invalid data '2222' also which has extremely high salary '222222222'. Hence this is listed top in high salaried job. JOB TITLE '2222' will be dropped.

<a id=section307></a> 
### 3.7. Final observations 
- Summary of data types in this dataset:

It has been observed that most of the data are seems to be fine, except few. For Instance,
- __CITY_1__ has many invalid data. To clean up, data will be compared with the master city list (another CSV file). If it does not match, then the invalid data will be replaced with NaN. Refer the section 3: Data Profiling.

- __Program Destignation__ column has code to indicate the Visa Type. Hence, for better readibility and graph plotting, description column will be created and map the code with Visa description.

- __JOB_TITLE__ column has the invalid data '2222'. which also has the highest salaried value '222222222', which will impact the highest salaried job. Hence this outlier row will be dropped.

<a id=section4></a> 
### 4. Data Normalization

### 4.1. Clean up the CITY data

#### Observations
Invalid CITY names are replaced with NaN. Invalid data observed in section 3 is not available.

### 4.1. Map the PROGRAM_DESIGNATION Code with the VISA Type Description

#### Observations
Visa Type column is created with the Visa description. It enables the user to interpret the Visa description easily.

### 4.2. Remove Outliers in JOB TITLE column

#### Observations
After dropping the JOB TITLE  = '2222' (outlier), valid jobs are shown in the data.

### 5. Analysis
#### Number of H1B Visa applied for each Visa Category
<img src="https://github.com/karthikeyanbalusamy/EDA-Project/blob/master/Images/VisaCategory.PNG" width="439" height="352" align="middle" />

H1B 97% E3 Australian 1.8% 
H1B1 Singapore 0.306% H1B1 Chile 0.25%

#### Observation

It can be inferred from the plot that the 97% of Visa are applied for H1B. Other visa categories are less than 2%. It indicates that most of the visa are applied for H1B

# Top 20 Employeer applied for H1B Visa
<img src="https://github.com/karthikeyanbalusamy/EDA-Project/blob/master/Images/TopEmployeer.png" width="383" height="527" align="middle" />

#### Observation
Microsoft is the top company applied for H1B Visa, followed by CTS, PATNI, IBM & Infosys. As opposed to other industry, IT companies are the topper in applying for the visa

### # Top Jobs applied for H1B Visa
<img src="https://github.com/karthikeyanbalusamy/EDA-Project/blob/master/Images/TopJobs.png" width="383" height="527" align="middle" />

#### Observations:
- It has been observed that Programmer Analyst has the top in applying for the H1B visa, followed by Software Engineer, Computer Programmer, System Analyst, Assistant Professor. It clearly indicates that more job demand for Software industry.

### # Top Salaried jobs
<img src="https://github.com/karthikeyanbalusamy/EDA-Project/blob/master/Images/TopSalariedJob.png" width="405" height="462" align="middle" />

#### Observations:
System Analyst, Research Worker are the top most paid job in US. Followed by Sr. Applications Engineer, Sr. Software Engineer, Computer Support Specialist.

### # Approval Status by Visa Type
<img src="https://github.com/karthikeyanbalusamy/EDA-Project/blob/master/Images/ApprovalStatus.png" width="697" height="370" align="middle" />

#### Observations:
Rejected Visas are in H1B Visa catagory, other visa types such as Australian; H-1B1 Chile, H-1B1 Singapore have no rejection.

### # Employee Preferred City
<img src="https://github.com/karthikeyanbalusamy/EDA-Project/blob/master/Images/EmpPreferredCity.png" width="436" height="357" align="middle" />

#### Observations:
- Most of the workers (aound __50%__) prefers to work in __New York__ city.
- Next 50% of the people prefers the below cities:
  Houston (18%), Chicago (12%), Redmond (11%) & Atlanta (10%)
- Overall, New York stands in top, while comparing to other cites.

### Conclusion
- H1B Visa petition analysis being conducted helps us to know the Demand on Jobs, Highly paid salary on Jobs, Employers creation more opportunities & Employee preferred city.
- Based on the result, this certainly helps the US Job workers to make a decision on the above data.
- IT Industry is the top in different parameters such as Salary, Demand and employers as opposed to other industry.
