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

Noticed the CITY column contains many invalid data. For Example:
'1','chicago illinois metro area analysis','bethesda-gaithersburg-frederick, md metro'  etc. 
Hence, Master city csv file is created with all the US CITY names, which will be compared with the CITY field in the H1B Dataset. Invalid city data will be identified and replaced with NaN.

#Analysis of Program Designation 
'PROGRAM_DESIGNATION' has the following values : 'R', 'A', 'C', 'S'

#### Observations
Program Designation have the code, which indicates the type of temporary application submitted for processing. R = H-1B; A = E-3 Australian; C = H-1B1 Chile; S = H-1B1 Singapore. Hence the description column will be created and map the code with Visa description for better understanding & readability.
