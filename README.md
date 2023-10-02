## Project goal

The aim of this project is to analyse and visualise the Scottish Index of Multiple Deprivation (SIMD) using a combination of pandas, geopandas, seaborn, matplotlib, QGIS and ArcGis Pro. The insights derived could be used by Scottish Government to help shape future resource allocation strategy.

Although I recently relocated to Kraków, Poland, I was born and raised in Dundee, Scotland so this study area has an added layer of personal interest.

### Scottish Index of Multiple Deprivation (SIMD)

The Scottish Index of Multiple Deprivation 2020 is the Scottish Government’s standard tool for identifying concentrations of deprivation in Scotland. SIMD is based
on work conducted by Oxford University in 1999. SIMD 2020 is the Scottish Government’s sixth edition since 2004.

SIMD is one of four deprivation indices that cover the whole of the UK. The Scottish index differs from the other indices in the UK by following a slightly different
methodology in constructing the overall index. The following are the key differences:

- SIMD is based on data zones which are smaller geographical units compared to the lower super output areas used in the other indices.
- The data sources are different for health, education, crime and housing domains.
- SIMD includes a specific domain on geographic access to services.

### Methodology

SIMD combines seven different domains, or aspects, of deprivation. These are:

- Income
- Employment
- Health
- Education, skills and training
- Geographic access to services
- Crime
- Housing

These domains are constructed using a number of indicators to form ranks for each domain. Each of the seven domain ranks are then combined to form the overall SIMD. There are 6,976 data zones in Scotland. Data zones are ranked from 1 being most deprived to 6,976 being least deprived. This provides a measure of relative deprivation at data zone level, therefore it tells you that one data zone is relatively more deprived than another, but not how much more deprived.

The methodology used to construct SIMD 2020 remains fundamentally the same as that used to construct the previous versions of SIMD in 2004, 2006, 2009, 2012 and 2016. It is based on the methodology developed by Oxford University to produce the Scottish Indices of Deprivation in 2003. The Scottish Government produced the first SIMD in-house in 2004. As the methodology used has remained broadly consistent in each subsequent update, these technical notes provide a summary of how the SIMD is constructed and details on the indicators included in each domain of SIMD 2020. Full details of the individual methods for creating the domains and overall index are described in the SIMD 2004 Technical Report.


### Constructing SIMD

SIMD 2020 is built up from a total of 33 indicators covering the seven domains. A list of the indicators included in each domain is provided below:

![indicator_descriptions](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/ff722ebf-e6f9-4998-9dfc-f0cec09267ab)

The indicators for each domain were selected on the basis that they are:

- domain-specific and appropriate for the purpose (as direct as possible measures for the given type of deprivation);
- up-to-date;
- capable of being updated on a regular basis;
- statistically robust;
- measure major features of a given type of deprivation (not conditions just experienced by a very small number of people or areas).

The domains are calculated differently depending on the type of data used in each one.

When comparing an index over time it is critical to ensure that the underlying methodology remains consistent from year to year as any changes will distort our analysis. From review of the detailed technical guidance the 2020 and 2016 indices share the same methodology. For both these years the granularity is 6976 ***data zones***. 

### Constructing the domains

The income, employment and housing domains are created by summing counts of people and dividing by the appropriate population denominator (taken from the NRS Small Area Population Estimates (SAPE) or Census). The crime domain and some indicators in the health and education domain also use SAPE.

The `income domain` is constructed by adding all five income indicators and dividing by the 2017 mid-year total population from SAPE. Thus, the domain score is a
simple percentage.

The `employment domain` is constructed by adding the three employment indicators and dividing by the 2017 mid-year working age population estimates taken from SAPE. The domain score is a simple percentage.

The `health, education and access domains` are created by ranking the indicators and transforming them to a standard normal distribution. This standardisation process is necessary because the indicators in these domains may be measured in different ways and on different scales. A statistical technique called factor analysis is then used to create a weight for each indicator. Next, the indicators are combined to produce a domain score which is then ranked.

The `crime domain` is a count of selected recorded crimes, divided by the 2017 midyear population estimates from SAPE. It is presented as the crime rate per 10,000
population.

The `housing domain` is the sum of the two housing indicators, divided by the total household population from the 2011 Census. The domain score is a simple percentage. 


### Calculating SIMD

Once the individual domain scores are calculated, they are combined to create the overall SIMD. The overall SIMD is a weighted sum of the seven domain scores, with
different domains given different weights. The weighting is based on the original research conducted by Oxford University, when the original Scottish Index of Multiple Deprivation was first produced, and takes into consideration how up to date and robust the indicators within each domain are.

The domain weighting used for SIMD 2020 remains the same as in SIMD16. A review of the weighting was undertaken when preparing for SIMD16 and concluded that the changes to data quality and methodology were not enough to justify a change of weightings.

The table below shows the percentage of overall SIMD 2020 for each of the seven
domains:

![weightings](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/d59b9b4d-7520-46f5-a68b-af442ad0ca34)


Prior to the weighting, the domains are standardised by ranking the scores. The ranks then undergo exponential transformation to avoid high ranks in one domain
cancelling out low ranks in another. The resulting scores for the overall SIMD are then ranked from 1 (most deprived) to 6,976 (least deprived) to create the final index.

The SIMD 2020 methodology is shown in the flow diagram below :

![methodology](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/6d9cd357-7aeb-4bf0-853b-d031c3d6a54c)

For the 2012 year, the granularity was 6,505 "data zones". In the interests of direct comparability, I have decided to limit the focus of my analysis to the 2016 and 2020 years.

Scotland comprises 32 Council Areas with further granularity provided by 6,976 **data zones** which are used for the purposes of the SIMD. Each data zone represents around 760 people.

https://www.gov.scot/publications/scottish-index-of-multiple-deprivation-rural-deprivation-evidence-review-and-case-studies/

### Data Providers

The following external organisations provided data for the construction of SIMD 2020:
- Department for Work and Pensions
- Her Majesty’s Revenue and Customs 
- National Records of Scotland
- NHS Scotland Information Services Division
- Scottish Qualifications Authority
- Higher Education Statistics Agency
- Skills Development Scotland
- Police Scotland
- Ofcom

### Getting the Data & Maps

The index was introduced in 2004 (and has evolved since then). An [interactive map](https://simd.scot/) is avaialable to explore and you can also download the data zone geospatial and indicator data.

![download_data](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/f1f0d403-825a-40af-b8df-ac08d807fae3)

### Getting to know our data


```python
!ls ./data
```

    sc_dz_11.dbf  sc_dz_11.shp				simd2016_withinds.csv
    sc_dz_11.prj  sc_dz_11.shx				simd2020_withinds.csv
    sc_dz_11.qpj  simd2012_data_00410767_plusintervals.csv


Now that I have downloaded the data it is time to get my hands dirty! Data cleaning and exploration is a critical first step.

### Inport the required packages


```python
import pandas as pd # data manipulation and analysis
import numpy as np # numerical and array-based operations
import geopandas as gpd  # manipulation, analysis, and visualization of geographic datasets
from shapely.geometry import Point
import matplotlib.pyplot as plt
import seaborn as sns
```

## SIMD - 2020

The SIMD rankings and individual indicators which contribute to the index are included in the `simd2020_withinds.csv` file which we can read in using pandas :


```python
# read in our indicator data
simd_2020 = pd.read_csv('data/simd2020_withinds.csv')
simd_2020.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Data_Zone</th>
      <th>Intermediate_Zone</th>
      <th>Council_area</th>
      <th>Total_population</th>
      <th>Working_Age_population</th>
      <th>SIMD2020v2_Rank</th>
      <th>SIMD_2020v2_Percentile</th>
      <th>SIMD2020v2_Vigintile</th>
      <th>SIMD2020v2_Decile</th>
      <th>SIMD2020v2_Quintile</th>
      <th>...</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
      <th>broadband</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>894</td>
      <td>580</td>
      <td>4691</td>
      <td>68</td>
      <td>14</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>2.540103</td>
      <td>3.074295</td>
      <td>1.616239</td>
      <td>2.615747</td>
      <td>1.544260</td>
      <td>9.930833</td>
      <td>8.863589</td>
      <td>5.856135</td>
      <td>6.023406</td>
      <td>11%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>793</td>
      <td>470</td>
      <td>4862</td>
      <td>70</td>
      <td>14</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>3.915072</td>
      <td>4.309812</td>
      <td>2.555858</td>
      <td>3.646697</td>
      <td>2.849656</td>
      <td>11.042816</td>
      <td>9.978272</td>
      <td>7.515000</td>
      <td>7.926029</td>
      <td>1%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>624</td>
      <td>461</td>
      <td>5686</td>
      <td>82</td>
      <td>17</td>
      <td>9</td>
      <td>5</td>
      <td>...</td>
      <td>3.323025</td>
      <td>3.784549</td>
      <td>1.440991</td>
      <td>3.247325</td>
      <td>2.062255</td>
      <td>10.616768</td>
      <td>8.620700</td>
      <td>4.321493</td>
      <td>5.770910</td>
      <td>1%</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>537</td>
      <td>307</td>
      <td>4332</td>
      <td>63</td>
      <td>13</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>2.622991</td>
      <td>2.778026</td>
      <td>2.620681</td>
      <td>1.936908</td>
      <td>2.160142</td>
      <td>10.036471</td>
      <td>7.935112</td>
      <td>8.433328</td>
      <td>8.329819</td>
      <td>11%</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>663</td>
      <td>415</td>
      <td>3913</td>
      <td>57</td>
      <td>12</td>
      <td>6</td>
      <td>3</td>
      <td>...</td>
      <td>2.115004</td>
      <td>2.358335</td>
      <td>2.408416</td>
      <td>1.845672</td>
      <td>1.784635</td>
      <td>9.650000</td>
      <td>5.568964</td>
      <td>6.966429</td>
      <td>6.632609</td>
      <td>0%</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 49 columns</p>
</div>



### A quick overview of our data


```python
simd_2020.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6976 entries, 0 to 6975
    Data columns (total 49 columns):
     #   Column                           Non-Null Count  Dtype  
    ---  ------                           --------------  -----  
     0   Data_Zone                        6976 non-null   object 
     1   Intermediate_Zone                6976 non-null   object 
     2   Council_area                     6976 non-null   object 
     3   Total_population                 6976 non-null   int64  
     4   Working_Age_population           6976 non-null   int64  
     5   SIMD2020v2_Rank                  6976 non-null   int64  
     6   SIMD_2020v2_Percentile           6976 non-null   int64  
     7   SIMD2020v2_Vigintile             6976 non-null   int64  
     8   SIMD2020v2_Decile                6976 non-null   int64  
     9   SIMD2020v2_Quintile              6976 non-null   int64  
     10  SIMD2020v2_Income_Domain_Rank    6976 non-null   float64
     11  SIMD2020_Employment_Domain_Rank  6976 non-null   float64
     12  SIMD2020_Health_Domain_Rank      6976 non-null   int64  
     13  SIMD2020_Education_Domain_Rank   6976 non-null   int64  
     14  SIMD2020_Access_Domain_Rank      6976 non-null   int64  
     15  SIMD2020_Crime_Domain_Rank       6976 non-null   float64
     16  SIMD2020_Housing_Domain_Rank     6976 non-null   float64
     17  income_rate                      6976 non-null   object 
     18  income_count                     6976 non-null   int64  
     19  employment_rate                  6976 non-null   object 
     20  employment_count                 6976 non-null   int64  
     21  CIF                              6973 non-null   float64
     22  ALCOHOL                          6974 non-null   float64
     23  DRUG                             6974 non-null   float64
     24  SMR                              6974 non-null   float64
     25  DEPRESS                          6975 non-null   object 
     26  LBWT                             6975 non-null   object 
     27  EMERG                            6974 non-null   float64
     28  Attendance                       6974 non-null   object 
     29  Attainment                       6965 non-null   object 
     30  no_qualifications                6976 non-null   float64
     31  not_participating                6973 non-null   object 
     32  University                       6974 non-null   object 
     33  crime_count                      6976 non-null   object 
     34  crime_rate                       6975 non-null   object 
     35  overcrowded_count                6976 non-null   int64  
     36  nocentralheating_count           6976 non-null   int64  
     37  overcrowded_rate                 6976 non-null   object 
     38  nocentralheating_rate            6976 non-null   object 
     39  drive_petrol                     6976 non-null   float64
     40  drive_GP                         6976 non-null   float64
     41  drive_post                       6976 non-null   float64
     42  drive_primary                    6976 non-null   float64
     43  drive_retail                     6976 non-null   float64
     44  drive_secondary                  6976 non-null   float64
     45  PT_GP                            6976 non-null   float64
     46  PT_post                          6976 non-null   float64
     47  PT_retail                        6976 non-null   float64
     48  broadband                        6974 non-null   object 
    dtypes: float64(19), int64(14), object(16)
    memory usage: 2.6+ MB


So we have 6976 observations (rows), one for each data zone, and 49 features/indicators (columns).

### Data Types

Understanding the type of data that we have from the outset is crucial if we want to avoid headaches later. The data types for each column are included above and an explanation of each is provided below:

![datatypes](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/b1d9ed51-f93a-40a3-8002-eb9d255e6933)

### Missing Data and Quality issues

Let's check to see if there is any missing data:


```python
nan_2020 = simd_2020.isna().sum().sort_values(ascending=False)
nan_2020
```




    Attainment                         11
    CIF                                 3
    not_participating                   3
    SMR                                 2
    University                          2
    Attendance                          2
    EMERG                               2
    DRUG                                2
    ALCOHOL                             2
    broadband                           2
    crime_rate                          1
    LBWT                                1
    DEPRESS                             1
    SIMD2020v2_Income_Domain_Rank       0
    Total_population                    0
    crime_count                         0
    overcrowded_count                   0
    nocentralheating_count              0
    overcrowded_rate                    0
    nocentralheating_rate               0
    drive_petrol                        0
    drive_GP                            0
    drive_post                          0
    drive_primary                       0
    drive_retail                        0
    drive_secondary                     0
    PT_GP                               0
    PT_post                             0
    PT_retail                           0
    Council_area                        0
    Working_Age_population              0
    no_qualifications                   0
    SIMD2020_Employment_Domain_Rank     0
    SIMD2020_Health_Domain_Rank         0
    SIMD2020_Education_Domain_Rank      0
    SIMD2020_Access_Domain_Rank         0
    SIMD2020_Crime_Domain_Rank          0
    SIMD2020_Housing_Domain_Rank        0
    income_rate                         0
    income_count                        0
    employment_rate                     0
    employment_count                    0
    SIMD2020v2_Quintile                 0
    SIMD2020v2_Decile                   0
    SIMD2020v2_Vigintile                0
    Intermediate_Zone                   0
    SIMD_2020v2_Percentile              0
    SIMD2020v2_Rank                     0
    Data_Zone                           0
    dtype: int64




```python
number_nan_2020 = nan_2020.sum()
print(f'Our 2020 dataset contains {number_nan_2020} NaN values.')
```

    Our 2020 dataset contains 34 NaN values.


### Review the Technical Report for Data Quality info

#### Income domain

The population estimate for a few data zones was zero in 2017, therefore a rate could not be determined. This is denoted by ‘*’.

#### Employment domain

The population estimate for a few data zones was zero in 2017,therefore a rate could not be determined. This is denoted by ‘*’. 

#### Health domain

After calculating the standardisation there are cases where division by zero occurs due to population estimate of data zone being zero. Such cases are marked "*". 

#### Education, skills and training domain

For some indicators, the population of the considered age group was zero in some data zones during the time period considered, and indicator rates and ranks could not be determined. Missing rates and ranks are denoted by ‘*’. To calculate the overall rankings for the education domain, the normalised scores for these data zones were set to zero before combining the indicators. As a result, the indicators with missing values moved the overall domain ranking of these data zones
towards a middle ranking. 

#### Geographic access to services domain

Drive times have been imputed to 190 minutes for at least some output areas in each of the following eleven data zones (for one or more services) as there was either
no car ferry connection within the time window considered, or the journey was longer than the maximum journey time of 180 minutes:

- S01007284 Mull, Iona, Coll and Tiree (retail centre,secondary school)
- S01007287 Mull, Iona, Coll and Tiree (GP, retail centre,petrol station, secondary school)
- S01007289 Oban South (all services)
- S01007310 Loch Awe – 03 (all services)
- S01007324 Whisky Isles (retail centre, secondary school)
- S01010504 Lochaber West (GP, retail centre, petrol station, secondary school)
- S01010506 Lochaber West (GP, retail centre, petrol station, secondary school)
- S01011831 Isles (Orkney) (retail centre, petrol station, primary school, secondary school)
- S01011832 Isles (Orkney) (retail centre, secondary school)
- S01012387 Shetland South (GP, retail centre, petrol station, secondary school)
- S01012416 North and East Isles (Shetland) (GP, retail centre, petrol station, primary school, secondary school) 

#### Crime domain

In a small number of data zones, the population was zero in the year considered, and crime rates could not be determined. Missing rates are denoted by ‘*’. 

### Data Cleansing

So, we can see that there are some `*` values. This is likely to cause problems so we will need to deal with these. Let's locate them :

`*` values


```python
# Check for columns containing '*'
columns_with_asterisk_2020 = simd_2020.columns[simd_2020.eq('*').any()]

# Print the columns containing '*'
print(columns_with_asterisk_2020)
```

    Index(['Attendance', 'Attainment', 'crime_count', 'crime_rate'], dtype='object')


And replace them with NaN :


```python
simd_2020 = simd_2020.replace('*', np.nan)
```


```python
number_nan_2020 = simd_2020.isna().sum().sort_values(ascending=False)
updated_number_nan_2020 = number_nan_2020.sum()
```


```python
print(f'Our updated 2020 dataset contains {updated_number_nan_2020} NaN values.')
```

    Our updated 2020 dataset contains 1777 NaN values.



```python
simd_2020.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6976 entries, 0 to 6975
    Data columns (total 49 columns):
     #   Column                           Non-Null Count  Dtype  
    ---  ------                           --------------  -----  
     0   Data_Zone                        6976 non-null   object 
     1   Intermediate_Zone                6976 non-null   object 
     2   Council_area                     6976 non-null   object 
     3   Total_population                 6976 non-null   int64  
     4   Working_Age_population           6976 non-null   int64  
     5   SIMD2020v2_Rank                  6976 non-null   int64  
     6   SIMD_2020v2_Percentile           6976 non-null   int64  
     7   SIMD2020v2_Vigintile             6976 non-null   int64  
     8   SIMD2020v2_Decile                6976 non-null   int64  
     9   SIMD2020v2_Quintile              6976 non-null   int64  
     10  SIMD2020v2_Income_Domain_Rank    6976 non-null   float64
     11  SIMD2020_Employment_Domain_Rank  6976 non-null   float64
     12  SIMD2020_Health_Domain_Rank      6976 non-null   int64  
     13  SIMD2020_Education_Domain_Rank   6976 non-null   int64  
     14  SIMD2020_Access_Domain_Rank      6976 non-null   int64  
     15  SIMD2020_Crime_Domain_Rank       6976 non-null   float64
     16  SIMD2020_Housing_Domain_Rank     6976 non-null   float64
     17  income_rate                      6976 non-null   object 
     18  income_count                     6976 non-null   int64  
     19  employment_rate                  6976 non-null   object 
     20  employment_count                 6976 non-null   int64  
     21  CIF                              6973 non-null   float64
     22  ALCOHOL                          6974 non-null   float64
     23  DRUG                             6974 non-null   float64
     24  SMR                              6974 non-null   float64
     25  DEPRESS                          6975 non-null   object 
     26  LBWT                             6975 non-null   object 
     27  EMERG                            6974 non-null   float64
     28  Attendance                       6409 non-null   object 
     29  Attainment                       6787 non-null   object 
     30  no_qualifications                6976 non-null   float64
     31  not_participating                6973 non-null   object 
     32  University                       6974 non-null   object 
     33  crime_count                      6476 non-null   object 
     34  crime_rate                       6475 non-null   object 
     35  overcrowded_count                6976 non-null   int64  
     36  nocentralheating_count           6976 non-null   int64  
     37  overcrowded_rate                 6976 non-null   object 
     38  nocentralheating_rate            6976 non-null   object 
     39  drive_petrol                     6976 non-null   float64
     40  drive_GP                         6976 non-null   float64
     41  drive_post                       6976 non-null   float64
     42  drive_primary                    6976 non-null   float64
     43  drive_retail                     6976 non-null   float64
     44  drive_secondary                  6976 non-null   float64
     45  PT_GP                            6976 non-null   float64
     46  PT_post                          6976 non-null   float64
     47  PT_retail                        6976 non-null   float64
     48  broadband                        6974 non-null   object 
    dtypes: float64(19), int64(14), object(16)
    memory usage: 2.6+ MB


We have some columns with essentially numerical information but held in string format. From review of the indicator table the columns `income_rate`, `employment_rate`, `DEPRESS`, `LBWT`, `Attendance`, `not_participating`, `University`, `broadband`, `overcrowded_rate` and `nocentralheat_rate` represent a percentage. 

`Attainment` is a score.

`crime_count` is a count.\
`crime_rate` is a rate.

Let's convert these columns to more appropriate data types.

The percentage strings include a `%` symbol at the end. Let's first confirm the offending columns : 


```python
# Find columns containing '%' in their values
columns_with_percentage_values_2020 = simd_2020.applymap(lambda x: '%' in str(x)).any()

# Print the columns containing '%' in their values
print(columns_with_percentage_values_2020[columns_with_percentage_values_2020].index)
```

    Index(['income_rate', 'employment_rate', 'DEPRESS', 'LBWT', 'Attendance',
           'not_participating', 'University', 'overcrowded_rate',
           'nocentralheating_rate', 'broadband'],
          dtype='object')


And then remove the `%` from those strings :


```python
# Remove '%' from strings containing it
simd_2020 = simd_2020.applymap(lambda x: str(x).replace('%', '') if '%' in str(x) else x)
simd_2020.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Data_Zone</th>
      <th>Intermediate_Zone</th>
      <th>Council_area</th>
      <th>Total_population</th>
      <th>Working_Age_population</th>
      <th>SIMD2020v2_Rank</th>
      <th>SIMD_2020v2_Percentile</th>
      <th>SIMD2020v2_Vigintile</th>
      <th>SIMD2020v2_Decile</th>
      <th>SIMD2020v2_Quintile</th>
      <th>...</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
      <th>broadband</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>894</td>
      <td>580</td>
      <td>4691</td>
      <td>68</td>
      <td>14</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>2.540103</td>
      <td>3.074295</td>
      <td>1.616239</td>
      <td>2.615747</td>
      <td>1.544260</td>
      <td>9.930833</td>
      <td>8.863589</td>
      <td>5.856135</td>
      <td>6.023406</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>793</td>
      <td>470</td>
      <td>4862</td>
      <td>70</td>
      <td>14</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>3.915072</td>
      <td>4.309812</td>
      <td>2.555858</td>
      <td>3.646697</td>
      <td>2.849656</td>
      <td>11.042816</td>
      <td>9.978272</td>
      <td>7.515000</td>
      <td>7.926029</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>624</td>
      <td>461</td>
      <td>5686</td>
      <td>82</td>
      <td>17</td>
      <td>9</td>
      <td>5</td>
      <td>...</td>
      <td>3.323025</td>
      <td>3.784549</td>
      <td>1.440991</td>
      <td>3.247325</td>
      <td>2.062255</td>
      <td>10.616768</td>
      <td>8.620700</td>
      <td>4.321493</td>
      <td>5.770910</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>537</td>
      <td>307</td>
      <td>4332</td>
      <td>63</td>
      <td>13</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>2.622991</td>
      <td>2.778026</td>
      <td>2.620681</td>
      <td>1.936908</td>
      <td>2.160142</td>
      <td>10.036471</td>
      <td>7.935112</td>
      <td>8.433328</td>
      <td>8.329819</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>663</td>
      <td>415</td>
      <td>3913</td>
      <td>57</td>
      <td>12</td>
      <td>6</td>
      <td>3</td>
      <td>...</td>
      <td>2.115004</td>
      <td>2.358335</td>
      <td>2.408416</td>
      <td>1.845672</td>
      <td>1.784635</td>
      <td>9.650000</td>
      <td>5.568964</td>
      <td>6.966429</td>
      <td>6.632609</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 49 columns</p>
</div>



>If you have a column of values in a Pandas DataFrame that are stored as strings but represent floating-point numbers, you can convert that entire column to float values using the `pd.to_numeric()` function with the `errors='coerce'` parameter. This function will attempt to convert the values to numeric format, and if it encounters any invalid values, it will replace them with NaN (Not-a-Number). Here's how you can do it:


```python
# List of column names to convert to float
columns_to_convert_2020 = ['income_rate', 'employment_rate', 'DEPRESS', 'LBWT', 'Attendance', 'Attainment', 'not_participating', 'University', 'crime_count', 'crime_rate', 'overcrowded_rate', 'nocentralheating_rate','broadband']

# Use astype to convert the specified columns to float
simd_2020[columns_to_convert_2020] = simd_2020[columns_to_convert_2020].apply(pd.to_numeric, errors='coerce')                              
```


```python
simd_2020.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6976 entries, 0 to 6975
    Data columns (total 49 columns):
     #   Column                           Non-Null Count  Dtype  
    ---  ------                           --------------  -----  
     0   Data_Zone                        6976 non-null   object 
     1   Intermediate_Zone                6976 non-null   object 
     2   Council_area                     6976 non-null   object 
     3   Total_population                 6976 non-null   int64  
     4   Working_Age_population           6976 non-null   int64  
     5   SIMD2020v2_Rank                  6976 non-null   int64  
     6   SIMD_2020v2_Percentile           6976 non-null   int64  
     7   SIMD2020v2_Vigintile             6976 non-null   int64  
     8   SIMD2020v2_Decile                6976 non-null   int64  
     9   SIMD2020v2_Quintile              6976 non-null   int64  
     10  SIMD2020v2_Income_Domain_Rank    6976 non-null   float64
     11  SIMD2020_Employment_Domain_Rank  6976 non-null   float64
     12  SIMD2020_Health_Domain_Rank      6976 non-null   int64  
     13  SIMD2020_Education_Domain_Rank   6976 non-null   int64  
     14  SIMD2020_Access_Domain_Rank      6976 non-null   int64  
     15  SIMD2020_Crime_Domain_Rank       6976 non-null   float64
     16  SIMD2020_Housing_Domain_Rank     6976 non-null   float64
     17  income_rate                      6976 non-null   int64  
     18  income_count                     6976 non-null   int64  
     19  employment_rate                  6976 non-null   int64  
     20  employment_count                 6976 non-null   int64  
     21  CIF                              6973 non-null   float64
     22  ALCOHOL                          6974 non-null   float64
     23  DRUG                             6974 non-null   float64
     24  SMR                              6974 non-null   float64
     25  DEPRESS                          6975 non-null   float64
     26  LBWT                             6975 non-null   float64
     27  EMERG                            6974 non-null   float64
     28  Attendance                       6409 non-null   float64
     29  Attainment                       6787 non-null   float64
     30  no_qualifications                6976 non-null   float64
     31  not_participating                6973 non-null   float64
     32  University                       6974 non-null   float64
     33  crime_count                      6476 non-null   float64
     34  crime_rate                       6475 non-null   float64
     35  overcrowded_count                6976 non-null   int64  
     36  nocentralheating_count           6976 non-null   int64  
     37  overcrowded_rate                 6976 non-null   int64  
     38  nocentralheating_rate            6976 non-null   int64  
     39  drive_petrol                     6976 non-null   float64
     40  drive_GP                         6976 non-null   float64
     41  drive_post                       6976 non-null   float64
     42  drive_primary                    6976 non-null   float64
     43  drive_retail                     6976 non-null   float64
     44  drive_secondary                  6976 non-null   float64
     45  PT_GP                            6976 non-null   float64
     46  PT_post                          6976 non-null   float64
     47  PT_retail                        6976 non-null   float64
     48  broadband                        6974 non-null   float64
    dtypes: float64(28), int64(18), object(3)
    memory usage: 2.6+ MB


### Most and least deprived datazones - 2020

Let's find out the most and least deprived datazones using the powerful pandas `.loc` :


```python
most_deprivation = simd_2020.SIMD2020v2_Rank.min()
dz_most_deprivation = simd_2020.loc[simd_2020['SIMD2020v2_Rank'] == most_deprivation, 'Data_Zone'].values[0]
dz_name_most_deprivation = simd_2020.loc[simd_2020['SIMD2020v2_Rank'] == most_deprivation, 'Intermediate_Zone'].values[0]
```


```python
print(f"The most deprived data zone in 2020 was: {dz_most_deprivation}, {dz_name_most_deprivation}.")
```

    The most deprived data zone in 2020 was: S01010891, Greenock Town Centre and East Central.



```python
least_deprivation = simd_2020.SIMD2020v2_Rank.max()
dz_least_deprivation = simd_2020.loc[simd_2020['SIMD2020v2_Rank'] == least_deprivation, 'Data_Zone'].values[0]
dz_name_least_deprivation = simd_2020.loc[simd_2020['SIMD2020v2_Rank'] == least_deprivation, 'Intermediate_Zone'].values[0]
```


```python
print(f"The least deprived data zone in 2020 was: {dz_least_deprivation}, {dz_name_least_deprivation}.")
```

    The least deprived data zone in 2020 was: S01008861, Stockbridge.


### Visualizations - Histograms, Boxplots, Scatterplots

Let's add some basic visualizations to geta quick picture of the spread of some of our data using histograms and boxplots using the `seaborn` library : 

#### Total Population of Data Zones


```python
simd_2020.Total_population.describe()
```




    count    6976.000000
    mean      777.637615
    std       219.108923
    min         0.000000
    25%       635.000000
    50%       755.000000
    75%       886.000000
    max      3847.000000
    Name: Total_population, dtype: float64




```python
sns.histplot(data=simd_2020['Total_population'], kde=True, color='blue')
```




    <AxesSubplot: xlabel='Total_population', ylabel='Count'>




    
![output_68_1](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/da69de45-610f-4a33-b5d2-77b7bdf99503)

    



```python
sns.boxplot(data=simd_2020['Total_population'])
# Add a title to your plot
plt.title("Boxplot of Data Zone populations")
```




    Text(0.5, 1.0, 'Boxplot of Data Zone populations')




    
![output_69_1](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/2f67aab7-62f6-4bfe-a263-3cccb5091374)



As we can see the population of the datazones is generally centred around 750 but there some very low (zero) and very high (3,847) values.

#### Working Age Population of Data Zones


```python
simd_2020.Working_Age_population.describe()
```




    count    6976.000000
    mean      500.973481
    std       175.075939
    min         0.000000
    25%       395.000000
    50%       475.000000
    75%       568.000000
    max      3423.000000
    Name: Working_Age_population, dtype: float64




```python
sns.histplot(data=simd_2020['Working_Age_population'], kde=True, color='blue')
```




    <AxesSubplot: xlabel='Working_Age_population', ylabel='Count'>




    
![output_73_1](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/dd1a4758-7542-4a94-a3e6-bb82a6de3e6b)

    



```python
sns.boxplot(data=simd_2020['Working_Age_population'])
# Add a title to your plot
plt.title("Boxplot of Data Zone working age populations")
```




    Text(0.5, 1.0, 'Boxplot of Data Zone working age populations')




    
![output_74_1](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/5e562384-987e-4efa-a821-11a99223cdb3)

    


As we can see the working population of the datazones is generally centred around 500 but there some very low (zero) and very high (3,423) values.

### Relationship between different indicators 

We can also explore the inter-relationships between variables using scatterplots - e.g. is there any relationship between no qualifications and employmemt deprivation?


```python
# Create a scatterplot
simd_2020.plot.scatter(x='no_qualifications', y='employment_count', c='blue', marker='o', s=25)
```




    <AxesSubplot: xlabel='no_qualifications', ylabel='employment_count'>




    
![output_78_1](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/a9b4f24d-abbf-4231-8617-90b548bbf966)

    


As one might expect there is a general positive trend, although not very strong. Generally, although there are clearly outliers, data zones which have a high number of people with no qualifications tend to also have a high number of people who are considered employment deprived.

### Adding a geospatial dimension to our non-spatial indicator data

Whilst we were able to readily obtain some useful insights using histograms, boxplots and scatterplots, let's harness the geospatial element of our data and visualize it using a map, which is a powerful and persuasive medium,  particularly for highlighting *where* to allocate resources, which is a key task of government.

Adding geospatial data in the form of a map adds a further layer of understanding.

### Data Zone Polygons

We have a `sc_dz_11.shp` file which contains the geometry of the `Data Zones`. This is of little value on its own but we can reveal powerful insights by combiming this with the indicator data that we have just cleaned up`simd_2020`.

We can get the datazones info using GeoPandas - we simply read in the file which creates a GeoPandas DataFrame :


```python
datazones = gpd.read_file('data/sc_dz_11.shp')
datazones.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Name</th>
      <th>TotPop2011</th>
      <th>ResPop2011</th>
      <th>HHCnt2011</th>
      <th>StdAreaHa</th>
      <th>StdAreaKm2</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter - 01</td>
      <td>872</td>
      <td>852</td>
      <td>424</td>
      <td>438.880218</td>
      <td>4.388801</td>
      <td>11801.872345</td>
      <td>4.388802e+06</td>
      <td>POLYGON ((-2.27748 57.09527, -2.27644 57.09521...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter - 02</td>
      <td>836</td>
      <td>836</td>
      <td>364</td>
      <td>22.349739</td>
      <td>0.223498</td>
      <td>2900.406362</td>
      <td>2.217468e+05</td>
      <td>POLYGON ((-2.27354 57.10449, -2.27333 57.10449...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter - 03</td>
      <td>643</td>
      <td>643</td>
      <td>340</td>
      <td>27.019476</td>
      <td>0.270194</td>
      <td>3468.761949</td>
      <td>2.701948e+05</td>
      <td>POLYGON ((-2.27443 57.10171, -2.27237 57.10046...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter - 04</td>
      <td>580</td>
      <td>580</td>
      <td>274</td>
      <td>9.625426</td>
      <td>0.096254</td>
      <td>1647.461389</td>
      <td>9.625426e+04</td>
      <td>POLYGON ((-2.26611 57.10133, -2.26599 57.10092...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter - 05</td>
      <td>644</td>
      <td>577</td>
      <td>256</td>
      <td>18.007657</td>
      <td>0.180076</td>
      <td>3026.111412</td>
      <td>1.800766e+05</td>
      <td>POLYGON ((-2.26013 57.10160, -2.26050 57.10134...</td>
    </tr>
  </tbody>
</table>
</div>




```python
type(datazones)
```




    geopandas.geodataframe.GeoDataFrame



We can get a very quick plot of our map :


```python
datazones.plot()
```




    <AxesSubplot: >




    

![output_86_1](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/401009e3-2d73-4e04-a312-90f082c55099)



I think I recognise that country!

### QGIS

`QGIS`, which stands for Quantum Geographic Information System, is an open-source geographic information system (GIS) software that allows users to create, edit, analyze, visualize, and manage geographic and spatial data. Here's a brief overview of QGIS:

`Open Source:`
QGIS is open-source software, which means it is freely available to anyone, and its source code can be modified and extended by the community of users and developers. This makes it a cost-effective option for working with spatial data.

`Cross-Platform:`
QGIS is a cross-platform application, meaning it is available for various operating systems, including Windows, macOS, and Linux. This allows users to work with GIS data on their preferred platform.

`User-Friendly Interface:`
QGIS features an intuitive and user-friendly graphical interface that is accessible to both beginners and experienced GIS professionals. It provides a wide range of tools and functionalities for spatial data management and analysis.

`Data Formats:`
QGIS supports a vast array of spatial data formats, including shapefiles, GeoTIFF, PostGIS, and more. It can import and export data in various formats, making it compatible with many GIS datasets.

`Data Visualization:`
Users can create maps and visualize geographic data using QGIS. It offers extensive cartographic tools to design and customize maps, including symbology, labeling, and map layout options.

`Data Editing:`
QGIS allows users to edit spatial data, making it useful for tasks such as digitizing features, updating attribute information, and performing spatial data cleaning.

`Spatial Analysis:`
One of QGIS's strengths is its ability to perform spatial analysis. Users can conduct various geoprocessing tasks, such as buffering, spatial queries, intersection analysis, and more, to gain insights from spatial data.

`Plugins and Extensions:`
QGIS has a vibrant plugin ecosystem that allows users to extend its functionality. There are numerous plugins available for specialized tasks and data sources.

`Integration with Other Software:`
QGIS can be integrated with other GIS and geospatial software, including databases like PostGIS, as well as with programming languages like Python for scripting and automation.

`Community and Support:`
Being open source, QGIS benefits from a large and active user community. Users can find documentation, tutorials, and user forums for assistance and support.

`Customization:`
Users can customize QGIS to suit their specific needs. This includes creating custom forms, expressions, and processing models.

`Remote Sensing:`
QGIS includes tools for working with remote sensing data, allowing users to analyze and process satellite and aerial imagery.

QGIS is a powerful and versatile GIS software that serves a wide range of users, from hobbyists to professionals, for tasks related to spatial data analysis, mapping, and visualization. Its open-source nature and active community make it a valuable tool for those working with geographic and spatial data.

### Visualizing the SIMD 2020 in QGIS

Let's see how the map looks in QGIS which is better equipped for visualizing maps.

![data_zones](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/be7554f2-0c01-4d8a-8773-ba2477cf8236)


As you can see the shape file contains some basic metadata which can be accessed from by right clicking the datazones layer and selecting `Open Attribute Table`, however in order to visualize out SIMD data on the map we need to join our `simd_2020` table to our `datazones` table.

### Joining our our geospatial datazone dataset to our non-spatial SIMD dataset

In an attribute join, a GeoSeries or GeoDataFrame is combined with a regular `pandas.Series` or `pandas.DataFrame` based on a common variable. This is analogous to normal merging or joining in pandas.

In our case we can join `datazones` (a GeoDataFrame) to `simd_2020` (a pandas DataFrame) on a common variable (Data Zone - reference S12345678).

To allow us to join on this shared feature, let's first rename `Data_Zone` to `DataZone` in the `simd_2020 dataframe` :


```python
simd_2020.rename(columns={"Data_Zone": "DataZone"}, inplace=True)
simd_2020.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Intermediate_Zone</th>
      <th>Council_area</th>
      <th>Total_population</th>
      <th>Working_Age_population</th>
      <th>SIMD2020v2_Rank</th>
      <th>SIMD_2020v2_Percentile</th>
      <th>SIMD2020v2_Vigintile</th>
      <th>SIMD2020v2_Decile</th>
      <th>SIMD2020v2_Quintile</th>
      <th>...</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
      <th>broadband</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>894</td>
      <td>580</td>
      <td>4691</td>
      <td>68</td>
      <td>14</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>2.540103</td>
      <td>3.074295</td>
      <td>1.616239</td>
      <td>2.615747</td>
      <td>1.544260</td>
      <td>9.930833</td>
      <td>8.863589</td>
      <td>5.856135</td>
      <td>6.023406</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>793</td>
      <td>470</td>
      <td>4862</td>
      <td>70</td>
      <td>14</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>3.915072</td>
      <td>4.309812</td>
      <td>2.555858</td>
      <td>3.646697</td>
      <td>2.849656</td>
      <td>11.042816</td>
      <td>9.978272</td>
      <td>7.515000</td>
      <td>7.926029</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>624</td>
      <td>461</td>
      <td>5686</td>
      <td>82</td>
      <td>17</td>
      <td>9</td>
      <td>5</td>
      <td>...</td>
      <td>3.323025</td>
      <td>3.784549</td>
      <td>1.440991</td>
      <td>3.247325</td>
      <td>2.062255</td>
      <td>10.616768</td>
      <td>8.620700</td>
      <td>4.321493</td>
      <td>5.770910</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>537</td>
      <td>307</td>
      <td>4332</td>
      <td>63</td>
      <td>13</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>2.622991</td>
      <td>2.778026</td>
      <td>2.620681</td>
      <td>1.936908</td>
      <td>2.160142</td>
      <td>10.036471</td>
      <td>7.935112</td>
      <td>8.433328</td>
      <td>8.329819</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter</td>
      <td>Aberdeen City</td>
      <td>663</td>
      <td>415</td>
      <td>3913</td>
      <td>57</td>
      <td>12</td>
      <td>6</td>
      <td>3</td>
      <td>...</td>
      <td>2.115004</td>
      <td>2.358335</td>
      <td>2.408416</td>
      <td>1.845672</td>
      <td>1.784635</td>
      <td>9.650000</td>
      <td>5.568964</td>
      <td>6.966429</td>
      <td>6.632609</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 49 columns</p>
</div>



And then go ahead and join the datasets :


```python
# merge geospatial datazones to non-spatial simd_2020
simd_2020_merged = datazones.merge(simd_2020, on='DataZone')
simd_2020_merged.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Name</th>
      <th>TotPop2011</th>
      <th>ResPop2011</th>
      <th>HHCnt2011</th>
      <th>StdAreaHa</th>
      <th>StdAreaKm2</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>geometry</th>
      <th>...</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
      <th>broadband</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter - 01</td>
      <td>872</td>
      <td>852</td>
      <td>424</td>
      <td>438.880218</td>
      <td>4.388801</td>
      <td>11801.872345</td>
      <td>4.388802e+06</td>
      <td>POLYGON ((-2.27748 57.09527, -2.27644 57.09521...</td>
      <td>...</td>
      <td>2.540103</td>
      <td>3.074295</td>
      <td>1.616239</td>
      <td>2.615747</td>
      <td>1.544260</td>
      <td>9.930833</td>
      <td>8.863589</td>
      <td>5.856135</td>
      <td>6.023406</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter - 02</td>
      <td>836</td>
      <td>836</td>
      <td>364</td>
      <td>22.349739</td>
      <td>0.223498</td>
      <td>2900.406362</td>
      <td>2.217468e+05</td>
      <td>POLYGON ((-2.27354 57.10449, -2.27333 57.10449...</td>
      <td>...</td>
      <td>3.915072</td>
      <td>4.309812</td>
      <td>2.555858</td>
      <td>3.646697</td>
      <td>2.849656</td>
      <td>11.042816</td>
      <td>9.978272</td>
      <td>7.515000</td>
      <td>7.926029</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter - 03</td>
      <td>643</td>
      <td>643</td>
      <td>340</td>
      <td>27.019476</td>
      <td>0.270194</td>
      <td>3468.761949</td>
      <td>2.701948e+05</td>
      <td>POLYGON ((-2.27443 57.10171, -2.27237 57.10046...</td>
      <td>...</td>
      <td>3.323025</td>
      <td>3.784549</td>
      <td>1.440991</td>
      <td>3.247325</td>
      <td>2.062255</td>
      <td>10.616768</td>
      <td>8.620700</td>
      <td>4.321493</td>
      <td>5.770910</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter - 04</td>
      <td>580</td>
      <td>580</td>
      <td>274</td>
      <td>9.625426</td>
      <td>0.096254</td>
      <td>1647.461389</td>
      <td>9.625426e+04</td>
      <td>POLYGON ((-2.26611 57.10133, -2.26599 57.10092...</td>
      <td>...</td>
      <td>2.622991</td>
      <td>2.778026</td>
      <td>2.620681</td>
      <td>1.936908</td>
      <td>2.160142</td>
      <td>10.036471</td>
      <td>7.935112</td>
      <td>8.433328</td>
      <td>8.329819</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter - 05</td>
      <td>644</td>
      <td>577</td>
      <td>256</td>
      <td>18.007657</td>
      <td>0.180076</td>
      <td>3026.111412</td>
      <td>1.800766e+05</td>
      <td>POLYGON ((-2.26013 57.10160, -2.26050 57.10134...</td>
      <td>...</td>
      <td>2.115004</td>
      <td>2.358335</td>
      <td>2.408416</td>
      <td>1.845672</td>
      <td>1.784635</td>
      <td>9.650000</td>
      <td>5.568964</td>
      <td>6.966429</td>
      <td>6.632609</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 58 columns</p>
</div>



As you can see we now have a geospatial dataset - there is a `geometry` column which includes the data zone polygons, and the other columns represent the features related to each individual datazone.

Let's save it as a shapefile for viewing in QGIS.


```python
# save merged dataframe to a shapefile
simd_2020_merged.to_file('simd_2020_merged.shp')  
```

    /tmp/ipykernel_222/619268410.py:2: UserWarning: Column names longer than 10 characters will be truncated when saved to ESRI Shapefile.
      simd_2020_merged.to_file('simd_2020_merged.shp')


### Creating a Chloropleth Map of the SIMD - 2020

When we drop this new layer into QGIS and open the attribute table, we can see that the map is generated and all the SIMD data is included. 

![data_zones_SIMD](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/a6ab65eb-8279-4358-85a1-5c9324d3c4e0)


Choropleth maps are a type of thematic map used to represent spatial patterns and variations in data across different geographic regions. 

To create a new layer `SIMD_2020_Rank` right click on the `simd_2020_merged` layer and select Export / Save Features As. Then configure as follows :

![create_SIMD_2020_Rank_layer](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/8b3496d5-c08a-4113-910c-28d3ac42c3a5)


>Note that it is important to choose an appropriate  `projected coordinate system` (in my case EPSG:27700) and **not** a `geographical` coordinate system such as WGS84. This is something that ArcGis requires in order to perform a Optimized Hot Spot Analysis - see the later section on ArcGis.

#### Renaming attributes in shapefile attribute table

A character limit of 10 is applied to the shapefile column names, which is not very helpful. You can amend these as follows :

- Locate the top-most menu bar
- Go to Processing › Toolbox
- In the Processsing Toolbox, go to Vector Table › Rename field

The Rename Field window will be opened like so:

![rename_field](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/c07e9f2f-654a-47c0-9f56-a7069cd0775a)


#### Creating and styling a layer

We now have a new layer named `Renamed` with more meaningful column names. Let's use this as the basis to create a new `SIMD_2020_Rank` layer to replace the original version. 

Once created we can style the layer. Right click on the layer, select `Properties` then `Symbology`. Change `Single Symbol` to `Graduated`, select `2020_SIMD` from the `Value` dropdown. 

`Color Ramp`

Choosing an appropriate colour for your map is worthy of an entire book of its own! For example for elevation often a blue to red ramp is used to symbolise the ascent from sea to sun.

I have used Blues, darker shades indicate higher levels of deprivation.

`Classification Mode`

Classification mode choice can ***strongly*** influence the story that you wish to tell. Again the subject is worthy of its own book. QGIS offers a choice of predefined modes, with the option to create your own customized mode.

In my case I have used the `Natural Breaks (Jenks)` mode of classification, and edited the legend to mirror the [interactive government SIMD map](https://simd.scot/#/simd2020/BTTTFTT/9/-4.0000/55.9000/).

![SIMD_2020_Rank_styling](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/f742801d-5926-4f68-bc86-500018057538)


Hit OK and we have our map! We can clearly see the most deprived areas in dark blue, with the least deprived in light blue.


![SIMD_Rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/d1676826-3f94-48bf-8608-cf3ecb81076e)


If we open the Attribute Table we can click on the `2020_SMID` column to sort by ranking. The top 20 most deprived data zones based on the SIMD 2020 are shown below:

![20_most_deprived](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/256849c6-5202-454f-b9b6-eaa023ad957b)


And here are the 20 least deprived data zones based on the SIMD 2020.

![20_least_deprived](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/b4248fa8-a523-44eb-a477-b0f78c9d9c4d)


### Using Geopandas spatial operations to find which data zone a point lies in

As we saw earlier we have a GeoDataFrame that contains the geometry of each data zone polygon. I would like to identify the data zone that contains the street that I was raised in. We can use Geopandas to achieve this : 


```python
# Replace with the actual latitude and longitude of your location
latitude = 56.49049
longitude = -2.99560

# Create a Point geometry object from the coordinates of the location you want to check
street = Point(longitude, latitude)
```


```python
# Use the contains or within spatial operation to check which polygon in your GeoDataFrame contains the point
matching_zones = simd_2020_merged[simd_2020_merged.geometry.contains(street)]
matching_zones
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Name</th>
      <th>TotPop2011</th>
      <th>ResPop2011</th>
      <th>HHCnt2011</th>
      <th>StdAreaHa</th>
      <th>StdAreaKm2</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>geometry</th>
      <th>...</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
      <th>broadband</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1315</th>
      <td>S01007821</td>
      <td>Downfield - 06</td>
      <td>552</td>
      <td>552</td>
      <td>244</td>
      <td>10.111186</td>
      <td>0.101111</td>
      <td>2365.152085</td>
      <td>101111.862221</td>
      <td>POLYGON ((-2.99561 56.49059, -2.99494 56.48985...</td>
      <td>...</td>
      <td>2.430788</td>
      <td>1.990476</td>
      <td>1.589203</td>
      <td>2.943905</td>
      <td>3.434769</td>
      <td>4.120263</td>
      <td>7.181386</td>
      <td>7.012369</td>
      <td>10.601427</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 58 columns</p>
</div>



`matching_zones` will be a subset of your GeoDataFrame containing only the data zones that contain the given point. If there are multiple matches, it will contain all of them.

Great, so I've found the data zone that I was raised in! We can print key stats of interest :


```python
matching_zones.columns
```




    Index(['DataZone', 'Name', 'TotPop2011', 'ResPop2011', 'HHCnt2011',
           'StdAreaHa', 'StdAreaKm2', 'Shape_Leng', 'Shape_Area', 'geometry',
           'Intermediate_Zone', 'Council_area', 'Total_population',
           'Working_Age_population', 'SIMD2020v2_Rank', 'SIMD_2020v2_Percentile',
           'SIMD2020v2_Vigintile', 'SIMD2020v2_Decile', 'SIMD2020v2_Quintile',
           'SIMD2020v2_Income_Domain_Rank', 'SIMD2020_Employment_Domain_Rank',
           'SIMD2020_Health_Domain_Rank', 'SIMD2020_Education_Domain_Rank',
           'SIMD2020_Access_Domain_Rank', 'SIMD2020_Crime_Domain_Rank',
           'SIMD2020_Housing_Domain_Rank', 'income_rate', 'income_count',
           'employment_rate', 'employment_count', 'CIF', 'ALCOHOL', 'DRUG', 'SMR',
           'DEPRESS', 'LBWT', 'EMERG', 'Attendance', 'Attainment',
           'no_qualifications', 'not_participating', 'University', 'crime_count',
           'crime_rate', 'overcrowded_count', 'nocentralheating_count',
           'overcrowded_rate', 'nocentralheating_rate', 'drive_petrol', 'drive_GP',
           'drive_post', 'drive_primary', 'drive_retail', 'drive_secondary',
           'PT_GP', 'PT_post', 'PT_retail', 'broadband'],
          dtype='object')




```python
for index, row in matching_zones.iterrows():
    print("Data Zone ID:", row['DataZone'])
    print("Data Zone Name:", row['Name'])
    print("Data Zone Population 2011:", row['TotPop2011'])    
    print("Council Area:" ,row['Council_area'])    
    print("SIMD 2020 Rank:", row['SIMD2020v2_Rank']) 
    print("Income Domain Rank 2020:", row['SIMD_2020v2_Percentile'])    
    print("Employment Domain Rank 2020:", row['SIMD2020_Employment_Domain_Rank'])    
    print("Health Domain Rank 2020:", row['SIMD2020_Health_Domain_Rank'])    
    print("Education Domain Rank 2020:", row['SIMD2020_Education_Domain_Rank'])    
    print("Access Domain Rank 2020:", row['SIMD2020_Access_Domain_Rank'])    
    print("Crime Domain Rank 2020:", row['SIMD2020_Crime_Domain_Rank'])    
    print("Housing Domain Rank 2020:", row['SIMD2020_Housing_Domain_Rank'])            
```

    Data Zone ID: S01007821
    Data Zone Name: Downfield - 06
    Data Zone Population 2011: 552
    Council Area: Dundee City
    SIMD 2020 Rank: 3278
    Income Domain Rank 2020: 47
    Employment Domain Rank 2020: 2576.0
    Health Domain Rank 2020: 3729
    Education Domain Rank 2020: 2603
    Access Domain Rank 2020: 5339
    Crime Domain Rank 2020: 1335.0
    Housing Domain Rank 2020: 2154.0


### Deconstructing the index

Let's take a look at each of the seven `Domains` that contribute to the overall index in isolation. We can simply create a new layer as before for each domain.

#### Income Domain Rank - 2020 (SMID weighting - 28%)

![income_rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/c05a87e0-e405-4249-91d5-e195e7c2cf20)

The income domain rankings are broadly similar to the overall SIMD ranking, which is as to be expected given the dominant weighting allocated to this domain. 

##### Most deprived
![20_most_deprived_income](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/9b51eefb-7c63-4b34-842d-5a6ba6d8f24d)

##### Least deprived
![20_least_deprived_income](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/b78ec023-212c-4cb1-af91-1789d5be9903)


#### Employment Domain Rank - 2020 (SMID weighting - 28%)

![employment_rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/47eb62c5-d8f1-4feb-8198-47d8c8f4e119)

The employment domain rankings are also broadly similar to the overall SIMD ranking, which is agian to be expected given the dominant weighting allocated to this domain. 

##### Most deprived
![20_most_deprived_employment](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/0519404e-0216-44f1-8805-84f7f10b7b9a)

##### Least deprived
![20_least_deprived_employment](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/fb1be6eb-5e1a-49d5-9f7e-d977368ddf43)


#### Health Domain Rank - 2020 (SMID weighting - 14%)

![health_rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/d67d33d5-85b3-40b3-8102-3518db933ad3)

##### Most deprived
![20_most_deprived_health](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/9413156b-103f-4254-837f-219a6a87c32b)

##### Least deprived
![20_least_deprived_health](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/5dab0f9a-f279-4e5d-abfb-c21e53e07400)


#### Education Domain Rank - 2020 (SMID weighting - 14%)

![education_rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/77329d74-8e31-4e01-81ae-57e2139c5cfb)

##### Most deprived
![20_most_deprived_education](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/1e68c9eb-5495-47eb-8dfe-76fee687a3be)

##### Least deprived
![20_least_deprived_education](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/ad2b98cd-79eb-4be0-9c24-364de9ba9483)


#### Access Domain Rank - 2020 (SMID weighting - 9%)

![access_rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/5118dc7c-0e3c-4167-9c6f-c00ce24403f9)

This map provides a stark insight that is not captured by the overall index, due to the small weighting applied. The large swathes of blue clearly highlight the problems that Scotland's geography creates, and the challenge faced by government to ensure that rural communities are serviced.

##### Most deprived
![20_most_deprived_access](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/0a4f1ef3-9dec-4c7a-ab14-18ee2237ed46)

##### Least deprived
![20_least_deprived_income](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/3276dd5a-3ac2-4ec6-ab7d-0338c374de70)


>Note when we zoom in to the major cities, as expected we can see that these have ready access to services.

![access_cities](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/ec2dd20a-2ca3-44c7-9319-bbd48b06c7c4)


#### Access to GP 
I think it is worth looking at one particular sub-domain of the `Access` domain - access to a GP.

![public_transport_to_GP](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/cc9b06fb-ffe8-4097-b2e5-85689aeb3532)


![drive_to_GP](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/2ba8d891-1aed-4e38-aef6-28a65be379f2)


These maps further highlight the challenge faced by government to ensure that rural communities are not disproportionately isolated. The value of the life of a city dweller is of course of no less value than that of someone living in a remote area, however there is a practical dimension in terms of healthcare provision to remote communities.

#### Crime Domain Rank - 2020 (SMID weighting - 5%)

![crime_rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/9e313a7e-c500-4bbd-9c8c-ea94d1b6fe1a)

##### Most deprived
![20_most_deprived_crime](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/179ac8db-65bf-4424-bc53-f92b3fcd2de0)

##### Least deprived
![20_least_deprived_crime](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/8245013c-ec7b-466f-9d34-3373972ccf07)


#### Housing Domain Rank 2020 (Index weighting - 2%)

![housing_rank_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/693a41e8-4916-4e2d-92f9-df25dd706653)

##### Most deprived
![20_most_deprived_housing](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/7191fee6-a120-4411-a0a0-2eeeac4f1775)

##### Least deprived
![20_least_deprived_housing](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/abee193c-08e1-4784-a63d-0602d92c308a)


Hopefully I have demonstrated the capabilities of QGIS which is a full-featured, user-friendly, free-and-open-source (FOSS) geographical information system (GIS) that runs on Unix platforms, Windows, and MacOS.



## ArcGis

Another powerful GIS application for Mapping and manipulating Data is ArcGis. It is developed and maintained by ESRI.

As part of the Esri MOOC ***Spatial Data Science: The New Frontier in Analytics*** I have access to ArcGis Pro and discovered the Optimized Hot Spot and Outlier analysis tools. Let's now see those tools in action.

### Detect patterns

Statistical cluster analysis can help you minimize the subjectivity in your maps by identifying meaningful clusters in your data. The Hot Spot Analysis and Outlier Analysis tools use statistics to detect spatial patterns in your data, but each provides slightly different information about these patterns.

ArcGIS provides traditional and optimized statistical cluster analysis tools. The optimized tools interrogate your data to provide smart default values, optimizing the analysis workflow. The traditional tools allow you more flexibility in defining the spatial relationships in your data, giving you more control over your analysis. 

### Optimized Hot Spot Analysis

The `Hot Spot Analysis` tool uses the **Getis-Ord Gi*** statistic to identify statistically significant spatial clusters of high values (hot spots) and low values (cold spots).

The result of your analysis is a layer displaying hot spots in three shades of red and cold spots in three shades of blue. The varying shades correspond to three confidence intervals, indicating how confident you can be that these patterns are not the result of random chance.


#### Income Deprived - 2020 - Hotspot Analysis

![Income_Deprived_Hotspot](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/cc152285-fbc4-42ae-a38f-221fbb08245e)

We can clearly see that large parts of the Glasgow area, significant pockets of Edinburgh and Dundee, and a pocket of Aberdeen are shown to be income deprived hotspots. 

![income_deprived_hotspot_report](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/62c84a60-a30f-46b4-ab40-7266584bfc8e)


#### Employment Deprived - 2020 - Hotspot Analysis

![Employment_Deprived_Hotspot](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/8587aa96-d48d-41b8-b1a3-1b6a99ac95b3)


Interestingly the Aberdeen pocket of Income Deprived is not replicated when it comes to Employment. The Glasgow, Edinburgh and Dundee pattern broadly resembles that for income deprivation.

![employment_deprived_outlier_report](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/bc40b9af-feac-4e95-8163-60b776964986)


#### Overcrowded - 2020 - Hotspot Analysis


![Overcrowded_HotSpot](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/326e7325-a203-488c-9f82-14668c967531)


As one might expect, the four main cities are overcrowding hotspots, emphasizing the continuing need for new affordable housing.

![overcrowded_hotspot_report](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/c862eaa5-08f2-4983-b4c4-79d19fd80206)


#### Drivetime to GP surgery - 2020 - Hotspot Analysis

Ready and speedy access to healthcare is critical. The four major cities, Glasgow, Edinburgh, Aberdeen, and Dundee all have quick access to a GP, however large swathes of the country are vulnerable as illustrated by the following graphic, and highlights the challenge faced by government to ensure that rural communities are serviced.


![Drivetime_to_GP_Hotspot](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/45f9c072-dda8-4b5b-a9fd-c2271633ad40)


![drivetime_GP__hotspot_report](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/8a22079b-fa24-49f1-bc97-fc3f87ba24a5)


Now that we have seen some examples of hotspot areas in Scotland with respect to certain indicators, let's now turn our attention to Outlier Analysis.

### Outlier Analysis
The `Outlier Analysis` tool uses the **Anselin Local Moran's I** statistic to identify statistically significant clusters of high and low values and to detect spatial outliers, or features with values that are significantly dissimilar from their neighbors.

The bright red and blue features represent spatial outliers. Features with high values surrounded by areas with low values are called `High-Low` outliers and are displayed in `red`. Features with low values surrounded by areas with high values are called `Low-High` outliers and are displayed in `dark blue`. The pink and light blue colors indicate clusters of features with statistically significantly high values (pink) and statistically significantly low values (light blue). These clusters typically align with the hot spots and cold spots from the Optimized Hot Spot Analysis tool.

#### Income Deprived - 2020 - Outlier Analysis

![Income_Outlier](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/95e09241-20ba-401b-a3f9-2988330c8bed)

![income_deprived_outlier_report](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/4fa6882c-d341-4ac6-82cd-b02649b6f702)

#### Employment Deprived - 2020 - Outlier Analysis

![employment_outlier](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/e8da4153-b6d6-4613-9fa4-8af2951671f1)

![Uploading employment_deprivedoutlier_report.JPG…]()

#### Overcrowded - 2020 - Outlier Analysis

![employment_outlier](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/4395dd5c-13fb-4e30-843b-52d43d9a0895)


![overcrowded_outlier_report](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/1cbb4766-bc0e-4ce6-b2c2-cac4e8454ef1)


#### Drivetime to GP surgery - 2020 - Outlier Analysis

![Drivetime_to_GP_Outlier](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/850b8d5a-e0ea-4ddf-81e2-2785b7d07f5d)

![drivetime_GP__outlier_report](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/6ff36bf2-342d-4531-9abb-0f203df14fe5)

ArcGis has some powerful geoprocessing tools and provides an alternative option to QGIS. However, it is not Open Source and is not free to use (although there is a 21 day trial period).


### Analysing changes in the SIMD over time 

I thought it would be a useful exercise to show the changes in the 2020 index against the 2016 index to get a picture of the changing deprivation landscape across data zones. 

#### ArcGis Space-Time Cube
I had hoped to harness the [Space-Time Cube](https://pro.arcgis.com/en/pro-app/latest/tool-reference/space-time-pattern-mining/learnmorecreatecube.htm) feature in ArcGis however very frustratingly you need to have a minimum of 10 time periods to use this feature!


![ten_slices](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/91b08dd2-5531-4b58-b078-52a7626eb769)


>Also worthy of note. ArcGis requires a projected coordinate system and not a geographical coordinate system :

![geographic_coord_system](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/0fd5bb33-b5f2-485c-b479-1b0252d648d2)


![projected_coord_system](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/12048f06-aefb-4dd0-8a4b-9144aa8d241f)

Another way to view changes between 2020 and 2016 might be to create maps for both years and view them side by side, however it can often be difficult to detect differences using this method. 

### Building a dataset of differences

As a workaround to address the above shortcomings, I had the idea of creating a "differences" dataset. Although this will require the 2020 and 2016 datsets to be consistent in terms of size and column names and might involve considerable data wrangling, I think that the end product - a **single** visualization of differences - will be worthwhile. Let's get started!

Let's take a look at the data for 2016.


```python
simd_2016 = pd.read_csv('data/simd2016_withinds.csv')
simd_2016.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Data_Zone</th>
      <th>Intermediate_Zone</th>
      <th>Council_area</th>
      <th>Total_population</th>
      <th>Working_age_population_Revised</th>
      <th>Overal_SIMD16_Rank</th>
      <th>SIMD_2016_Percentile</th>
      <th>SIMD_2016_Vigintile</th>
      <th>SIMD_2016_Decile</th>
      <th>SIMD_2016_Quintile</th>
      <th>...</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_Post</th>
      <th>PT_retail</th>
      <th>crime_rate</th>
      <th>overcrowded_count</th>
      <th>nocentralheat_count</th>
      <th>overcrowded_rate</th>
      <th>nocentralheat_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>904.0</td>
      <td>605.0</td>
      <td>5272.0</td>
      <td>76.0</td>
      <td>16.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>1.5</td>
      <td>10.8</td>
      <td>8.4</td>
      <td>6.0</td>
      <td>5.7</td>
      <td>89</td>
      <td>87.0</td>
      <td>10.0</td>
      <td>10%</td>
      <td>1%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>830.0</td>
      <td>491.0</td>
      <td>4838.0</td>
      <td>70.0</td>
      <td>14.0</td>
      <td>7.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.7</td>
      <td>11.5</td>
      <td>8.3</td>
      <td>7.3</td>
      <td>6.8</td>
      <td>48</td>
      <td>85.0</td>
      <td>4.0</td>
      <td>10%</td>
      <td>0%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>694.0</td>
      <td>519.0</td>
      <td>6321.0</td>
      <td>91.0</td>
      <td>19.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>1.9</td>
      <td>11.5</td>
      <td>7.9</td>
      <td>5.8</td>
      <td>5.3</td>
      <td>58</td>
      <td>31.0</td>
      <td>8.0</td>
      <td>5%</td>
      <td>1%</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>573.0</td>
      <td>354.0</td>
      <td>5363.0</td>
      <td>77.0</td>
      <td>16.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.2</td>
      <td>10.8</td>
      <td>7.4</td>
      <td>8.3</td>
      <td>8.4</td>
      <td>0</td>
      <td>42.0</td>
      <td>6.0</td>
      <td>7%</td>
      <td>1%</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>676.0</td>
      <td>414.0</td>
      <td>4049.0</td>
      <td>59.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>1.9</td>
      <td>10.6</td>
      <td>5.1</td>
      <td>6.6</td>
      <td>6.6</td>
      <td>178</td>
      <td>50.0</td>
      <td>7.0</td>
      <td>9%</td>
      <td>1%</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 47 columns</p>
</div>



### A quick overview of our data


```python
simd_2016.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6979 entries, 0 to 6978
    Data columns (total 47 columns):
     #   Column                              Non-Null Count  Dtype  
    ---  ------                              --------------  -----  
     0   Data_Zone                           6979 non-null   object 
     1   Intermediate_Zone                   6976 non-null   object 
     2   Council_area                        6976 non-null   object 
     3   Total_population                    6976 non-null   float64
     4   Working_age_population_Revised      6976 non-null   float64
     5   Overal_SIMD16_Rank                  6976 non-null   float64
     6   SIMD_2016_Percentile                6976 non-null   float64
     7   SIMD_2016_Vigintile                 6976 non-null   float64
     8   SIMD_2016_Decile                    6976 non-null   float64
     9   SIMD_2016_Quintile                  6976 non-null   float64
     10  Income_Domain_2016_Rank             6976 non-null   float64
     11  Employment_Domain_2016_Rank         6976 non-null   float64
     12  Health_Domain_2016_Rank             6976 non-null   float64
     13  Education_Domain_2016_Rank          6976 non-null   float64
     14  Geographic_Access_Domain_2016_Rank  6976 non-null   float64
     15  Crime_Domain_2016_Rank              6976 non-null   float64
     16  Housing_Domain_2016_Rank            6976 non-null   float64
     17  Income_rate                         6976 non-null   object 
     18  Income_count                        6976 non-null   float64
     19  Employment_rate                     6976 non-null   object 
     20  Employment_count                    6976 non-null   float64
     21  CIF                                 6976 non-null   object 
     22  ALCOHOL                             6976 non-null   object 
     23  DRUG                                6976 non-null   float64
     24  SMR                                 6976 non-null   float64
     25  DEPRESS                             6976 non-null   object 
     26  LBWT                                6976 non-null   object 
     27  EMERG                               6976 non-null   float64
     28  Attendance                          6976 non-null   object 
     29  Attainment                          6976 non-null   object 
     30  Noquals                             6976 non-null   float64
     31  NEET                                6976 non-null   object 
     32  HESA                                6976 non-null   object 
     33  drive_petrol                        6976 non-null   float64
     34  drive_GP                            6976 non-null   float64
     35  drive_PO                            6976 non-null   float64
     36  drive_primary                       6976 non-null   float64
     37  drive_retail                        6976 non-null   float64
     38  drive_secondary                     6976 non-null   float64
     39  PT_GP                               6976 non-null   float64
     40  PT_Post                             6976 non-null   float64
     41  PT_retail                           6976 non-null   float64
     42  crime_rate                          6976 non-null   object 
     43  overcrowded_count                   6976 non-null   float64
     44  nocentralheat_count                 6976 non-null   float64
     45  overcrowded_rate                    6976 non-null   object 
     46  nocentralheat_rate                  6976 non-null   object 
    dtypes: float64(31), object(16)
    memory usage: 2.5+ MB



```python
nan_2016 = simd_2016.isna().sum().sort_values(ascending=False)
nan_2016
```




    DRUG                                  3
    drive_PO                              3
    LBWT                                  3
    EMERG                                 3
    Attendance                            3
    Attainment                            3
    Noquals                               3
    NEET                                  3
    HESA                                  3
    drive_petrol                          3
    drive_GP                              3
    drive_primary                         3
    SMR                                   3
    drive_retail                          3
    drive_secondary                       3
    PT_GP                                 3
    PT_Post                               3
    PT_retail                             3
    crime_rate                            3
    overcrowded_count                     3
    nocentralheat_count                   3
    overcrowded_rate                      3
    DEPRESS                               3
    nocentralheat_rate                    3
    Intermediate_Zone                     3
    Employment_Domain_2016_Rank           3
    Council_area                          3
    Total_population                      3
    Working_age_population_Revised        3
    Overal_SIMD16_Rank                    3
    SIMD_2016_Percentile                  3
    SIMD_2016_Vigintile                   3
    SIMD_2016_Decile                      3
    SIMD_2016_Quintile                    3
    Income_Domain_2016_Rank               3
    Health_Domain_2016_Rank               3
    ALCOHOL                               3
    Education_Domain_2016_Rank            3
    Geographic_Access_Domain_2016_Rank    3
    Crime_Domain_2016_Rank                3
    Housing_Domain_2016_Rank              3
    Income_rate                           3
    Income_count                          3
    Employment_rate                       3
    Employment_count                      3
    CIF                                   3
    Data_Zone                             0
    dtype: int64




```python
number_nan_2016 = nan_2016.sum()
```


```python
print(f'Our 2016 dataset contains {number_nan_2016} NaN values.')
```

    Our 2016 dataset contains 138 NaN values.



```python
# Check for columns containing '*'
columns_with_asterisk_2016 = simd_2016.columns[simd_2016.eq('*').any()]

# Print the columns containing '*'
print(columns_with_asterisk_2016)
```

    Index(['CIF', 'DEPRESS', 'LBWT', 'Attainment', 'HESA', 'crime_rate'], dtype='object')



```python
simd_2016 = simd_2016.replace('*', np.nan)
```

### Missing Data


```python
# list number of missing values per column
simd_2016.isna().sum().sort_values(ascending=False)
```




    Attainment                            17
    HESA                                   6
    DEPRESS                                5
    CIF                                    5
    crime_rate                             5
    LBWT                                   4
    drive_GP                               3
    EMERG                                  3
    Attendance                             3
    Noquals                                3
    NEET                                   3
    drive_petrol                           3
    DRUG                                   3
    SMR                                    3
    drive_primary                          3
    drive_retail                           3
    drive_secondary                        3
    PT_GP                                  3
    PT_Post                                3
    PT_retail                              3
    overcrowded_count                      3
    nocentralheat_count                    3
    overcrowded_rate                       3
    drive_PO                               3
    nocentralheat_rate                     3
    Intermediate_Zone                      3
    ALCOHOL                                3
    Council_area                           3
    Total_population                       3
    Working_age_population_Revised         3
    Overal_SIMD16_Rank                     3
    SIMD_2016_Percentile                   3
    SIMD_2016_Vigintile                    3
    SIMD_2016_Decile                       3
    SIMD_2016_Quintile                     3
    Income_Domain_2016_Rank                3
    Employment_Domain_2016_Rank            3
    Health_Domain_2016_Rank                3
    Education_Domain_2016_Rank             3
    Geographic_Access_Domain_2016_Rank     3
    Crime_Domain_2016_Rank                 3
    Housing_Domain_2016_Rank               3
    Income_rate                            3
    Income_count                           3
    Employment_rate                        3
    Employment_count                       3
    Data_Zone                              0
    dtype: int64




```python
number_nan_2016 = simd_2016.isna().sum().sort_values(ascending=False)
updated_number_nan_2016 = number_nan_2016.sum()
```


```python
print(f'Our updated 2016 dataset contains {updated_number_nan_2016} NaN values.')
```

    Our updated 2016 dataset contains 162 NaN values.



```python
simd_2016.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6979 entries, 0 to 6978
    Data columns (total 47 columns):
     #   Column                              Non-Null Count  Dtype  
    ---  ------                              --------------  -----  
     0   Data_Zone                           6979 non-null   object 
     1   Intermediate_Zone                   6976 non-null   object 
     2   Council_area                        6976 non-null   object 
     3   Total_population                    6976 non-null   float64
     4   Working_age_population_Revised      6976 non-null   float64
     5   Overal_SIMD16_Rank                  6976 non-null   float64
     6   SIMD_2016_Percentile                6976 non-null   float64
     7   SIMD_2016_Vigintile                 6976 non-null   float64
     8   SIMD_2016_Decile                    6976 non-null   float64
     9   SIMD_2016_Quintile                  6976 non-null   float64
     10  Income_Domain_2016_Rank             6976 non-null   float64
     11  Employment_Domain_2016_Rank         6976 non-null   float64
     12  Health_Domain_2016_Rank             6976 non-null   float64
     13  Education_Domain_2016_Rank          6976 non-null   float64
     14  Geographic_Access_Domain_2016_Rank  6976 non-null   float64
     15  Crime_Domain_2016_Rank              6976 non-null   float64
     16  Housing_Domain_2016_Rank            6976 non-null   float64
     17  Income_rate                         6976 non-null   object 
     18  Income_count                        6976 non-null   float64
     19  Employment_rate                     6976 non-null   object 
     20  Employment_count                    6976 non-null   float64
     21  CIF                                 6974 non-null   object 
     22  ALCOHOL                             6976 non-null   object 
     23  DRUG                                6976 non-null   float64
     24  SMR                                 6976 non-null   float64
     25  DEPRESS                             6974 non-null   object 
     26  LBWT                                6975 non-null   object 
     27  EMERG                               6976 non-null   float64
     28  Attendance                          6976 non-null   object 
     29  Attainment                          6962 non-null   object 
     30  Noquals                             6976 non-null   float64
     31  NEET                                6976 non-null   object 
     32  HESA                                6973 non-null   object 
     33  drive_petrol                        6976 non-null   float64
     34  drive_GP                            6976 non-null   float64
     35  drive_PO                            6976 non-null   float64
     36  drive_primary                       6976 non-null   float64
     37  drive_retail                        6976 non-null   float64
     38  drive_secondary                     6976 non-null   float64
     39  PT_GP                               6976 non-null   float64
     40  PT_Post                             6976 non-null   float64
     41  PT_retail                           6976 non-null   float64
     42  crime_rate                          6974 non-null   object 
     43  overcrowded_count                   6976 non-null   float64
     44  nocentralheat_count                 6976 non-null   float64
     45  overcrowded_rate                    6976 non-null   object 
     46  nocentralheat_rate                  6976 non-null   object 
    dtypes: float64(31), object(16)
    memory usage: 2.5+ MB



```python
# Find columns containing '%' in their values
columns_with_percentage_values_2016 = simd_2016.applymap(lambda x: '%' in str(x)).any()

# Print the columns containing '%' in their values
print(columns_with_percentage_values_2016[columns_with_percentage_values_2016].index)
```

    Index(['Income_rate', 'Employment_rate', 'DEPRESS', 'LBWT', 'Attendance',
           'NEET', 'HESA', 'overcrowded_rate', 'nocentralheat_rate'],
          dtype='object')



```python
# Remove '%' from strings containing it
simd_2016 = simd_2016.applymap(lambda x: str(x).replace('%', '') if '%' in str(x) else x)
simd_2016.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Data_Zone</th>
      <th>Intermediate_Zone</th>
      <th>Council_area</th>
      <th>Total_population</th>
      <th>Working_age_population_Revised</th>
      <th>Overal_SIMD16_Rank</th>
      <th>SIMD_2016_Percentile</th>
      <th>SIMD_2016_Vigintile</th>
      <th>SIMD_2016_Decile</th>
      <th>SIMD_2016_Quintile</th>
      <th>...</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_Post</th>
      <th>PT_retail</th>
      <th>crime_rate</th>
      <th>overcrowded_count</th>
      <th>nocentralheat_count</th>
      <th>overcrowded_rate</th>
      <th>nocentralheat_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>904.0</td>
      <td>605.0</td>
      <td>5272.0</td>
      <td>76.0</td>
      <td>16.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>1.5</td>
      <td>10.8</td>
      <td>8.4</td>
      <td>6.0</td>
      <td>5.7</td>
      <td>89</td>
      <td>87.0</td>
      <td>10.0</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>830.0</td>
      <td>491.0</td>
      <td>4838.0</td>
      <td>70.0</td>
      <td>14.0</td>
      <td>7.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.7</td>
      <td>11.5</td>
      <td>8.3</td>
      <td>7.3</td>
      <td>6.8</td>
      <td>48</td>
      <td>85.0</td>
      <td>4.0</td>
      <td>10</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>694.0</td>
      <td>519.0</td>
      <td>6321.0</td>
      <td>91.0</td>
      <td>19.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>1.9</td>
      <td>11.5</td>
      <td>7.9</td>
      <td>5.8</td>
      <td>5.3</td>
      <td>58</td>
      <td>31.0</td>
      <td>8.0</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>573.0</td>
      <td>354.0</td>
      <td>5363.0</td>
      <td>77.0</td>
      <td>16.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.2</td>
      <td>10.8</td>
      <td>7.4</td>
      <td>8.3</td>
      <td>8.4</td>
      <td>0</td>
      <td>42.0</td>
      <td>6.0</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>676.0</td>
      <td>414.0</td>
      <td>4049.0</td>
      <td>59.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>1.9</td>
      <td>10.6</td>
      <td>5.1</td>
      <td>6.6</td>
      <td>6.6</td>
      <td>178</td>
      <td>50.0</td>
      <td>7.0</td>
      <td>9</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 47 columns</p>
</div>




```python
# List of column names to convert to float
columns_to_convert_2016 = ['Income_rate', 'Employment_rate', 'CIF', 'ALCOHOL', 'DEPRESS', 'LBWT', 'Attendance', 'Attainment', 'NEET', 'HESA', 'crime_rate', 'overcrowded_rate', 'nocentralheat_rate']

# Use astype to convert the specified columns to float
simd_2016[columns_to_convert_2016] = simd_2016[columns_to_convert_2016].apply(pd.to_numeric, errors='coerce') 
```


```python
# show rows which include null values
null_data_2016 = simd_2016[simd_2016.isnull().any(axis=1)]
null_data_2016
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Data_Zone</th>
      <th>Intermediate_Zone</th>
      <th>Council_area</th>
      <th>Total_population</th>
      <th>Working_age_population_Revised</th>
      <th>Overal_SIMD16_Rank</th>
      <th>SIMD_2016_Percentile</th>
      <th>SIMD_2016_Vigintile</th>
      <th>SIMD_2016_Decile</th>
      <th>SIMD_2016_Quintile</th>
      <th>...</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_Post</th>
      <th>PT_retail</th>
      <th>crime_rate</th>
      <th>overcrowded_count</th>
      <th>nocentralheat_count</th>
      <th>overcrowded_rate</th>
      <th>nocentralheat_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>137</th>
      <td>S01006643</td>
      <td>Hanover_North</td>
      <td>Aberdeen_City</td>
      <td>782.0</td>
      <td>719.0</td>
      <td>5566.0</td>
      <td>80.0</td>
      <td>16.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>3.1</td>
      <td>5.6</td>
      <td>3.9</td>
      <td>5.2</td>
      <td>7.0</td>
      <td>333.0</td>
      <td>207.0</td>
      <td>48.0</td>
      <td>28.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>140</th>
      <td>S01006646</td>
      <td>George_Street</td>
      <td>Aberdeen_City</td>
      <td>1001.0</td>
      <td>913.0</td>
      <td>4907.0</td>
      <td>71.0</td>
      <td>15.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.6</td>
      <td>4.7</td>
      <td>6.8</td>
      <td>3.5</td>
      <td>7.2</td>
      <td>1050.0</td>
      <td>152.0</td>
      <td>45.0</td>
      <td>28.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>2163</th>
      <td>S01008669</td>
      <td>Meadows_and_Southside</td>
      <td>City_of_Edinburgh</td>
      <td>532.0</td>
      <td>488.0</td>
      <td>6564.0</td>
      <td>95.0</td>
      <td>19.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>3.0</td>
      <td>5.1</td>
      <td>4.1</td>
      <td>3.4</td>
      <td>9.2</td>
      <td>414.0</td>
      <td>201.0</td>
      <td>7.0</td>
      <td>35.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2343</th>
      <td>S01008849</td>
      <td>New_Town_West</td>
      <td>City_of_Edinburgh</td>
      <td>694.0</td>
      <td>533.0</td>
      <td>6499.0</td>
      <td>94.0</td>
      <td>19.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>3.4</td>
      <td>3.8</td>
      <td>7.4</td>
      <td>4.8</td>
      <td>6.7</td>
      <td>620.0</td>
      <td>114.0</td>
      <td>35.0</td>
      <td>16.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>2348</th>
      <td>S01008854</td>
      <td>Canonmills_and_New_Town_North</td>
      <td>City_of_Edinburgh</td>
      <td>932.0</td>
      <td>768.0</td>
      <td>6937.0</td>
      <td>100.0</td>
      <td>20.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>3.5</td>
      <td>3.3</td>
      <td>7.0</td>
      <td>5.5</td>
      <td>8.4</td>
      <td>97.0</td>
      <td>116.0</td>
      <td>37.0</td>
      <td>13.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2644</th>
      <td>S01009150</td>
      <td>Falkirk_-_Town_Centre_and_Callendar_Park</td>
      <td>Falkirk</td>
      <td>759.0</td>
      <td>166.0</td>
      <td>895.0</td>
      <td>13.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>3.9</td>
      <td>3.6</td>
      <td>11.2</td>
      <td>7.6</td>
      <td>10.3</td>
      <td>26.0</td>
      <td>37.0</td>
      <td>31.0</td>
      <td>5.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3217</th>
      <td>S01009723</td>
      <td>St_Andrews_Central</td>
      <td>Fife</td>
      <td>951.0</td>
      <td>834.0</td>
      <td>6505.0</td>
      <td>94.0</td>
      <td>19.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>2.2</td>
      <td>3.2</td>
      <td>8.7</td>
      <td>3.6</td>
      <td>4.6</td>
      <td>95.0</td>
      <td>192.0</td>
      <td>27.0</td>
      <td>30.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3505</th>
      <td>S01010011</td>
      <td>Mount_Florida</td>
      <td>Glasgow_City</td>
      <td>728.0</td>
      <td>508.0</td>
      <td>1634.0</td>
      <td>24.0</td>
      <td>5.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>1.4</td>
      <td>4.0</td>
      <td>4.8</td>
      <td>5.8</td>
      <td>3.8</td>
      <td>165.0</td>
      <td>124.0</td>
      <td>36.0</td>
      <td>17.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>3549</th>
      <td>S01010055</td>
      <td>Parkhead_West_and_Barrowfield</td>
      <td>Glasgow_City</td>
      <td>1200.0</td>
      <td>829.0</td>
      <td>67.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>1.7</td>
      <td>2.6</td>
      <td>4.9</td>
      <td>7.5</td>
      <td>5.4</td>
      <td>1327.0</td>
      <td>282.0</td>
      <td>50.0</td>
      <td>30.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>3700</th>
      <td>S01010206</td>
      <td>Petershill</td>
      <td>Glasgow_City</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4605.0</td>
      <td>67.0</td>
      <td>14.0</td>
      <td>7.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>4.2</td>
      <td>4.0</td>
      <td>7.4</td>
      <td>8.2</td>
      <td>9.2</td>
      <td>NaN</td>
      <td>243.0</td>
      <td>21.0</td>
      <td>49.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3721</th>
      <td>S01010227</td>
      <td>Sighthill</td>
      <td>Glasgow_City</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>5144.0</td>
      <td>74.0</td>
      <td>15.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.1</td>
      <td>3.5</td>
      <td>11.6</td>
      <td>12.2</td>
      <td>7.2</td>
      <td>NaN</td>
      <td>304.0</td>
      <td>48.0</td>
      <td>44.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>3758</th>
      <td>S01010264</td>
      <td>City_Centre_East</td>
      <td>Glasgow_City</td>
      <td>807.0</td>
      <td>726.0</td>
      <td>1464.0</td>
      <td>21.0</td>
      <td>5.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>2.8</td>
      <td>5.0</td>
      <td>8.8</td>
      <td>3.5</td>
      <td>8.0</td>
      <td>2606.0</td>
      <td>264.0</td>
      <td>61.0</td>
      <td>35.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>3759</th>
      <td>S01010265</td>
      <td>City_Centre_East</td>
      <td>Glasgow_City</td>
      <td>981.0</td>
      <td>941.0</td>
      <td>5726.0</td>
      <td>83.0</td>
      <td>17.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>2.1</td>
      <td>5.7</td>
      <td>6.4</td>
      <td>3.2</td>
      <td>5.3</td>
      <td>1736.0</td>
      <td>391.0</td>
      <td>59.0</td>
      <td>45.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>3767</th>
      <td>S01010273</td>
      <td>City_Centre_South</td>
      <td>Glasgow_City</td>
      <td>759.0</td>
      <td>714.0</td>
      <td>5072.0</td>
      <td>73.0</td>
      <td>15.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.9</td>
      <td>4.6</td>
      <td>6.2</td>
      <td>6.6</td>
      <td>6.7</td>
      <td>3813.0</td>
      <td>298.0</td>
      <td>30.0</td>
      <td>45.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>3769</th>
      <td>S01010275</td>
      <td>City_Centre_South</td>
      <td>Glasgow_City</td>
      <td>735.0</td>
      <td>697.0</td>
      <td>4178.0</td>
      <td>60.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>3.1</td>
      <td>5.4</td>
      <td>5.0</td>
      <td>4.5</td>
      <td>5.5</td>
      <td>14580.0</td>
      <td>305.0</td>
      <td>27.0</td>
      <td>52.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>3867</th>
      <td>S01010373</td>
      <td>North_Kelvin</td>
      <td>Glasgow_City</td>
      <td>813.0</td>
      <td>648.0</td>
      <td>5950.0</td>
      <td>86.0</td>
      <td>18.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>3.4</td>
      <td>3.8</td>
      <td>7.2</td>
      <td>7.4</td>
      <td>7.9</td>
      <td>111.0</td>
      <td>109.0</td>
      <td>26.0</td>
      <td>14.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>5313</th>
      <td>S01011819</td>
      <td>West_Kirkwall</td>
      <td>Orkney_Islands</td>
      <td>757.0</td>
      <td>499.0</td>
      <td>3951.0</td>
      <td>57.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>3.2</td>
      <td>4.9</td>
      <td>5.4</td>
      <td>7.6</td>
      <td>8.8</td>
      <td>174.0</td>
      <td>55.0</td>
      <td>35.0</td>
      <td>7.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>6976</th>
      <td># Error in the working age population</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6977</th>
      <td># In a previous version of this file we includ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6978</th>
      <td># SIMD16 ranks are not affected. For the calcu...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>20 rows × 47 columns</p>
</div>




```python
# remove rows 6976, 6977, and 6978
rows_to_remove = [6976, 6977, 6978]
simd_2016 = simd_2016.drop(rows_to_remove)
```


```python
simd_2016.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6976 entries, 0 to 6975
    Data columns (total 47 columns):
     #   Column                              Non-Null Count  Dtype  
    ---  ------                              --------------  -----  
     0   Data_Zone                           6976 non-null   object 
     1   Intermediate_Zone                   6976 non-null   object 
     2   Council_area                        6976 non-null   object 
     3   Total_population                    6976 non-null   float64
     4   Working_age_population_Revised      6976 non-null   float64
     5   Overal_SIMD16_Rank                  6976 non-null   float64
     6   SIMD_2016_Percentile                6976 non-null   float64
     7   SIMD_2016_Vigintile                 6976 non-null   float64
     8   SIMD_2016_Decile                    6976 non-null   float64
     9   SIMD_2016_Quintile                  6976 non-null   float64
     10  Income_Domain_2016_Rank             6976 non-null   float64
     11  Employment_Domain_2016_Rank         6976 non-null   float64
     12  Health_Domain_2016_Rank             6976 non-null   float64
     13  Education_Domain_2016_Rank          6976 non-null   float64
     14  Geographic_Access_Domain_2016_Rank  6976 non-null   float64
     15  Crime_Domain_2016_Rank              6976 non-null   float64
     16  Housing_Domain_2016_Rank            6976 non-null   float64
     17  Income_rate                         6974 non-null   float64
     18  Income_count                        6976 non-null   float64
     19  Employment_rate                     6974 non-null   float64
     20  Employment_count                    6976 non-null   float64
     21  CIF                                 6974 non-null   float64
     22  ALCOHOL                             6974 non-null   float64
     23  DRUG                                6976 non-null   float64
     24  SMR                                 6976 non-null   float64
     25  DEPRESS                             6974 non-null   float64
     26  LBWT                                6975 non-null   float64
     27  EMERG                               6976 non-null   float64
     28  Attendance                          6974 non-null   float64
     29  Attainment                          6962 non-null   float64
     30  Noquals                             6976 non-null   float64
     31  NEET                                6973 non-null   float64
     32  HESA                                6973 non-null   float64
     33  drive_petrol                        6976 non-null   float64
     34  drive_GP                            6976 non-null   float64
     35  drive_PO                            6976 non-null   float64
     36  drive_primary                       6976 non-null   float64
     37  drive_retail                        6976 non-null   float64
     38  drive_secondary                     6976 non-null   float64
     39  PT_GP                               6976 non-null   float64
     40  PT_Post                             6976 non-null   float64
     41  PT_retail                           6976 non-null   float64
     42  crime_rate                          6974 non-null   float64
     43  overcrowded_count                   6976 non-null   float64
     44  nocentralheat_count                 6976 non-null   float64
     45  overcrowded_rate                    6976 non-null   float64
     46  nocentralheat_rate                  6976 non-null   float64
    dtypes: float64(44), object(3)
    memory usage: 2.5+ MB



```python
simd_2016.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total_population</th>
      <th>Working_age_population_Revised</th>
      <th>Overal_SIMD16_Rank</th>
      <th>SIMD_2016_Percentile</th>
      <th>SIMD_2016_Vigintile</th>
      <th>SIMD_2016_Decile</th>
      <th>SIMD_2016_Quintile</th>
      <th>Income_Domain_2016_Rank</th>
      <th>Employment_Domain_2016_Rank</th>
      <th>Health_Domain_2016_Rank</th>
      <th>...</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_Post</th>
      <th>PT_retail</th>
      <th>crime_rate</th>
      <th>overcrowded_count</th>
      <th>nocentralheat_count</th>
      <th>overcrowded_rate</th>
      <th>nocentralheat_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>...</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6974.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
      <td>6976.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>766.571101</td>
      <td>493.098337</td>
      <td>3488.500000</td>
      <td>50.500000</td>
      <td>10.500000</td>
      <td>5.500000</td>
      <td>3.000000</td>
      <td>3488.500000</td>
      <td>3488.500000</td>
      <td>3488.500000</td>
      <td>...</td>
      <td>5.238733</td>
      <td>6.110751</td>
      <td>10.266972</td>
      <td>8.569696</td>
      <td>13.532010</td>
      <td>311.808288</td>
      <td>82.332569</td>
      <td>13.430619</td>
      <td>10.949828</td>
      <td>1.779960</td>
    </tr>
    <tr>
      <th>std</th>
      <td>188.267392</td>
      <td>151.225436</td>
      <td>2013.942071</td>
      <td>28.868457</td>
      <td>5.766794</td>
      <td>2.872587</td>
      <td>1.414214</td>
      <td>2013.942071</td>
      <td>2013.942071</td>
      <td>2013.942071</td>
      <td>...</td>
      <td>6.131718</td>
      <td>5.179672</td>
      <td>6.182297</td>
      <td>4.477807</td>
      <td>10.660695</td>
      <td>441.399811</td>
      <td>63.921332</td>
      <td>16.800241</td>
      <td>7.879127</td>
      <td>2.179844</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>...</td>
      <td>0.700000</td>
      <td>1.200000</td>
      <td>1.600000</td>
      <td>1.900000</td>
      <td>1.900000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>635.000000</td>
      <td>397.000000</td>
      <td>1744.750000</td>
      <td>25.750000</td>
      <td>5.750000</td>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>1744.750000</td>
      <td>1744.750000</td>
      <td>1744.750000</td>
      <td>...</td>
      <td>2.800000</td>
      <td>3.700000</td>
      <td>6.300000</td>
      <td>5.600000</td>
      <td>8.000000</td>
      <td>103.000000</td>
      <td>35.000000</td>
      <td>3.000000</td>
      <td>5.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>753.000000</td>
      <td>474.000000</td>
      <td>3488.500000</td>
      <td>50.500000</td>
      <td>10.500000</td>
      <td>5.500000</td>
      <td>3.000000</td>
      <td>3488.500000</td>
      <td>3488.500000</td>
      <td>3488.500000</td>
      <td>...</td>
      <td>4.000000</td>
      <td>4.800000</td>
      <td>8.800000</td>
      <td>7.500000</td>
      <td>11.200000</td>
      <td>207.000000</td>
      <td>66.500000</td>
      <td>8.000000</td>
      <td>9.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>877.000000</td>
      <td>563.000000</td>
      <td>5232.250000</td>
      <td>75.250000</td>
      <td>15.250000</td>
      <td>8.000000</td>
      <td>4.000000</td>
      <td>5232.250000</td>
      <td>5232.250000</td>
      <td>5232.250000</td>
      <td>...</td>
      <td>5.900000</td>
      <td>6.700000</td>
      <td>12.300000</td>
      <td>10.200000</td>
      <td>16.100000</td>
      <td>388.000000</td>
      <td>112.000000</td>
      <td>17.000000</td>
      <td>15.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>3302.000000</td>
      <td>2917.000000</td>
      <td>6976.000000</td>
      <td>100.000000</td>
      <td>20.000000</td>
      <td>10.000000</td>
      <td>5.000000</td>
      <td>6975.500000</td>
      <td>6975.000000</td>
      <td>6976.000000</td>
      <td>...</td>
      <td>190.000000</td>
      <td>116.100000</td>
      <td>108.800000</td>
      <td>40.300000</td>
      <td>190.000000</td>
      <td>14580.000000</td>
      <td>490.000000</td>
      <td>187.000000</td>
      <td>58.000000</td>
      <td>21.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 44 columns</p>
</div>



### Most and least deprived datazones - 2016


```python
most_deprivation_2016 = simd_2016.Overal_SIMD16_Rank.min()
dz_most_deprivation_2016 = simd_2016.loc[simd_2016['Overal_SIMD16_Rank'] == most_deprivation_2016, 'Data_Zone'].values[0]
dz_name_most_deprivation_2016 = simd_2016.loc[simd_2016['Overal_SIMD16_Rank'] == most_deprivation_2016, 'Intermediate_Zone'].values[0]
```


```python
print(f"The most deprived data zone in 2016 was: {dz_most_deprivation_2016}, {dz_name_most_deprivation_2016}.")
```

    The most deprived data zone in 2016 was: S01012068, Paisley_Ferguslie.



```python
least_deprivation_2016 = simd_2016.Overal_SIMD16_Rank.max()
dz_least_deprivation_2016 = simd_2016.loc[simd_2016['Overal_SIMD16_Rank'] == least_deprivation, 'Data_Zone'].values[0]
dz_name_least_deprivation_2016 = simd_2016.loc[simd_2016['Overal_SIMD16_Rank'] == least_deprivation, 'Intermediate_Zone'].values[0]
```


```python
print(f"The least deprived data zone in 2016 was: {dz_least_deprivation_2016}, {dz_name_least_deprivation_2016}.")
```

    The least deprived data zone in 2016 was: S01008405, Lower_Whitecraigs_and_South_Giffnock.



```python
# Rename colums to allow us to join on shared feature 
simd_2016.rename(columns={"Data_Zone": "DataZone"}, inplace=True)
simd_2016.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Intermediate_Zone</th>
      <th>Council_area</th>
      <th>Total_population</th>
      <th>Working_age_population_Revised</th>
      <th>Overal_SIMD16_Rank</th>
      <th>SIMD_2016_Percentile</th>
      <th>SIMD_2016_Vigintile</th>
      <th>SIMD_2016_Decile</th>
      <th>SIMD_2016_Quintile</th>
      <th>...</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_Post</th>
      <th>PT_retail</th>
      <th>crime_rate</th>
      <th>overcrowded_count</th>
      <th>nocentralheat_count</th>
      <th>overcrowded_rate</th>
      <th>nocentralheat_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>904.0</td>
      <td>605.0</td>
      <td>5272.0</td>
      <td>76.0</td>
      <td>16.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>1.5</td>
      <td>10.8</td>
      <td>8.4</td>
      <td>6.0</td>
      <td>5.7</td>
      <td>89.0</td>
      <td>87.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>830.0</td>
      <td>491.0</td>
      <td>4838.0</td>
      <td>70.0</td>
      <td>14.0</td>
      <td>7.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.7</td>
      <td>11.5</td>
      <td>8.3</td>
      <td>7.3</td>
      <td>6.8</td>
      <td>48.0</td>
      <td>85.0</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>694.0</td>
      <td>519.0</td>
      <td>6321.0</td>
      <td>91.0</td>
      <td>19.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>...</td>
      <td>1.9</td>
      <td>11.5</td>
      <td>7.9</td>
      <td>5.8</td>
      <td>5.3</td>
      <td>58.0</td>
      <td>31.0</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>573.0</td>
      <td>354.0</td>
      <td>5363.0</td>
      <td>77.0</td>
      <td>16.0</td>
      <td>8.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>2.2</td>
      <td>10.8</td>
      <td>7.4</td>
      <td>8.3</td>
      <td>8.4</td>
      <td>0.0</td>
      <td>42.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter</td>
      <td>Aberdeen_City</td>
      <td>676.0</td>
      <td>414.0</td>
      <td>4049.0</td>
      <td>59.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>1.9</td>
      <td>10.6</td>
      <td>5.1</td>
      <td>6.6</td>
      <td>6.6</td>
      <td>178.0</td>
      <td>50.0</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 47 columns</p>
</div>




```python
# merge geospatial datazones to non-spatial simd_2016
simd_2016_merged = datazones.merge(simd_2016, on='DataZone')
simd_2016_merged.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Name</th>
      <th>TotPop2011</th>
      <th>ResPop2011</th>
      <th>HHCnt2011</th>
      <th>StdAreaHa</th>
      <th>StdAreaKm2</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>geometry</th>
      <th>...</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_Post</th>
      <th>PT_retail</th>
      <th>crime_rate</th>
      <th>overcrowded_count</th>
      <th>nocentralheat_count</th>
      <th>overcrowded_rate</th>
      <th>nocentralheat_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter - 01</td>
      <td>872</td>
      <td>852</td>
      <td>424</td>
      <td>438.880218</td>
      <td>4.388801</td>
      <td>11801.872345</td>
      <td>4.388802e+06</td>
      <td>POLYGON ((-2.27748 57.09527, -2.27644 57.09521...</td>
      <td>...</td>
      <td>1.5</td>
      <td>10.8</td>
      <td>8.4</td>
      <td>6.0</td>
      <td>5.7</td>
      <td>89.0</td>
      <td>87.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter - 02</td>
      <td>836</td>
      <td>836</td>
      <td>364</td>
      <td>22.349739</td>
      <td>0.223498</td>
      <td>2900.406362</td>
      <td>2.217468e+05</td>
      <td>POLYGON ((-2.27354 57.10449, -2.27333 57.10449...</td>
      <td>...</td>
      <td>2.7</td>
      <td>11.5</td>
      <td>8.3</td>
      <td>7.3</td>
      <td>6.8</td>
      <td>48.0</td>
      <td>85.0</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter - 03</td>
      <td>643</td>
      <td>643</td>
      <td>340</td>
      <td>27.019476</td>
      <td>0.270194</td>
      <td>3468.761949</td>
      <td>2.701948e+05</td>
      <td>POLYGON ((-2.27443 57.10171, -2.27237 57.10046...</td>
      <td>...</td>
      <td>1.9</td>
      <td>11.5</td>
      <td>7.9</td>
      <td>5.8</td>
      <td>5.3</td>
      <td>58.0</td>
      <td>31.0</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter - 04</td>
      <td>580</td>
      <td>580</td>
      <td>274</td>
      <td>9.625426</td>
      <td>0.096254</td>
      <td>1647.461389</td>
      <td>9.625426e+04</td>
      <td>POLYGON ((-2.26611 57.10133, -2.26599 57.10092...</td>
      <td>...</td>
      <td>2.2</td>
      <td>10.8</td>
      <td>7.4</td>
      <td>8.3</td>
      <td>8.4</td>
      <td>0.0</td>
      <td>42.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter - 05</td>
      <td>644</td>
      <td>577</td>
      <td>256</td>
      <td>18.007657</td>
      <td>0.180076</td>
      <td>3026.111412</td>
      <td>1.800766e+05</td>
      <td>POLYGON ((-2.26013 57.10160, -2.26050 57.10134...</td>
      <td>...</td>
      <td>1.9</td>
      <td>10.6</td>
      <td>5.1</td>
      <td>6.6</td>
      <td>6.6</td>
      <td>178.0</td>
      <td>50.0</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 56 columns</p>
</div>




```python
simd_2016_merged.shape
```




    (6976, 56)




```python
simd_2020_merged.shape
```




    (6976, 58)



### Data inconsistency

So, both sets have 6976 observations as expected (one for each Data Zone) but the 2020 dataset has an extra two columns. Let's investigate this further by using set theory to pinpoint the changes to the column names.


```python
# convert column names into a set
set_columns_2016 = set(simd_2016_merged.columns)
set_columns_2016
```




    {'ALCOHOL',
     'Attainment',
     'Attendance',
     'CIF',
     'Council_area',
     'Crime_Domain_2016_Rank',
     'DEPRESS',
     'DRUG',
     'DataZone',
     'EMERG',
     'Education_Domain_2016_Rank',
     'Employment_Domain_2016_Rank',
     'Employment_count',
     'Employment_rate',
     'Geographic_Access_Domain_2016_Rank',
     'HESA',
     'HHCnt2011',
     'Health_Domain_2016_Rank',
     'Housing_Domain_2016_Rank',
     'Income_Domain_2016_Rank',
     'Income_count',
     'Income_rate',
     'Intermediate_Zone',
     'LBWT',
     'NEET',
     'Name',
     'Noquals',
     'Overal_SIMD16_Rank',
     'PT_GP',
     'PT_Post',
     'PT_retail',
     'ResPop2011',
     'SIMD_2016_Decile',
     'SIMD_2016_Percentile',
     'SIMD_2016_Quintile',
     'SIMD_2016_Vigintile',
     'SMR',
     'Shape_Area',
     'Shape_Leng',
     'StdAreaHa',
     'StdAreaKm2',
     'TotPop2011',
     'Total_population',
     'Working_age_population_Revised',
     'crime_rate',
     'drive_GP',
     'drive_PO',
     'drive_petrol',
     'drive_primary',
     'drive_retail',
     'drive_secondary',
     'geometry',
     'nocentralheat_count',
     'nocentralheat_rate',
     'overcrowded_count',
     'overcrowded_rate'}




```python
# convert colum names into a set
set_columns_2020 = set(simd_2020_merged.columns)
set_columns_2020
```




    {'ALCOHOL',
     'Attainment',
     'Attendance',
     'CIF',
     'Council_area',
     'DEPRESS',
     'DRUG',
     'DataZone',
     'EMERG',
     'HHCnt2011',
     'Intermediate_Zone',
     'LBWT',
     'Name',
     'PT_GP',
     'PT_post',
     'PT_retail',
     'ResPop2011',
     'SIMD2020_Access_Domain_Rank',
     'SIMD2020_Crime_Domain_Rank',
     'SIMD2020_Education_Domain_Rank',
     'SIMD2020_Employment_Domain_Rank',
     'SIMD2020_Health_Domain_Rank',
     'SIMD2020_Housing_Domain_Rank',
     'SIMD2020v2_Decile',
     'SIMD2020v2_Income_Domain_Rank',
     'SIMD2020v2_Quintile',
     'SIMD2020v2_Rank',
     'SIMD2020v2_Vigintile',
     'SIMD_2020v2_Percentile',
     'SMR',
     'Shape_Area',
     'Shape_Leng',
     'StdAreaHa',
     'StdAreaKm2',
     'TotPop2011',
     'Total_population',
     'University',
     'Working_Age_population',
     'broadband',
     'crime_count',
     'crime_rate',
     'drive_GP',
     'drive_petrol',
     'drive_post',
     'drive_primary',
     'drive_retail',
     'drive_secondary',
     'employment_count',
     'employment_rate',
     'geometry',
     'income_count',
     'income_rate',
     'no_qualifications',
     'nocentralheating_count',
     'nocentralheating_rate',
     'not_participating',
     'overcrowded_count',
     'overcrowded_rate'}




```python
# Find columns unique to set_2020
unique_to_columns_2020 = set_columns_2020 - set_columns_2016
unique_to_columns_2020
```




    {'PT_post',
     'SIMD2020_Access_Domain_Rank',
     'SIMD2020_Crime_Domain_Rank',
     'SIMD2020_Education_Domain_Rank',
     'SIMD2020_Employment_Domain_Rank',
     'SIMD2020_Health_Domain_Rank',
     'SIMD2020_Housing_Domain_Rank',
     'SIMD2020v2_Decile',
     'SIMD2020v2_Income_Domain_Rank',
     'SIMD2020v2_Quintile',
     'SIMD2020v2_Rank',
     'SIMD2020v2_Vigintile',
     'SIMD_2020v2_Percentile',
     'University',
     'Working_Age_population',
     'broadband',
     'crime_count',
     'drive_post',
     'employment_count',
     'employment_rate',
     'income_count',
     'income_rate',
     'no_qualifications',
     'nocentralheating_count',
     'nocentralheating_rate',
     'not_participating'}




```python
# Find columns unique to set_2016
unique_to_columns_2016 = set_columns_2016 - set_columns_2020
unique_to_columns_2016
```




    {'Crime_Domain_2016_Rank',
     'Education_Domain_2016_Rank',
     'Employment_Domain_2016_Rank',
     'Employment_count',
     'Employment_rate',
     'Geographic_Access_Domain_2016_Rank',
     'HESA',
     'Health_Domain_2016_Rank',
     'Housing_Domain_2016_Rank',
     'Income_Domain_2016_Rank',
     'Income_count',
     'Income_rate',
     'NEET',
     'Noquals',
     'Overal_SIMD16_Rank',
     'PT_Post',
     'SIMD_2016_Decile',
     'SIMD_2016_Percentile',
     'SIMD_2016_Quintile',
     'SIMD_2016_Vigintile',
     'Working_age_population_Revised',
     'drive_PO',
     'nocentralheat_count',
     'nocentralheat_rate'}



### Rename columns


```python
simd_2020_merged.rename(columns={'SIMD2020_Access_Domain_Rank': 'Access_Domain_Rank',
 'SIMD2020_Crime_Domain_Rank': 'Crime_Domain_Rank',
 'SIMD2020_Education_Domain_Rank':'Education_Domain_Rank',
 'SIMD2020_Employment_Domain_Rank': 'Employment_Domain_Rank',
 'SIMD2020_Health_Domain_Rank':'Health_Domain_Rank',
 'SIMD2020_Housing_Domain_Rank':'Housing_Domain_Rank',
 'SIMD2020v2_Decile':'Decile',
 'SIMD2020v2_Income_Domain_Rank':'Income_Domain_Rank',
 'SIMD2020v2_Quintile':'Quintile',
 'SIMD2020v2_Rank':'Rank',
 'SIMD2020v2_Vigintile': 'Vigintile',
 'SIMD_2020v2_Percentile':'Percentile'},inplace=True)

simd_2020_merged
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Name</th>
      <th>TotPop2011</th>
      <th>ResPop2011</th>
      <th>HHCnt2011</th>
      <th>StdAreaHa</th>
      <th>StdAreaKm2</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>geometry</th>
      <th>...</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
      <th>broadband</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter - 01</td>
      <td>872</td>
      <td>852</td>
      <td>424</td>
      <td>438.880218</td>
      <td>4.388801</td>
      <td>11801.872345</td>
      <td>4.388802e+06</td>
      <td>POLYGON ((-2.27748 57.09527, -2.27644 57.09521...</td>
      <td>...</td>
      <td>2.540103</td>
      <td>3.074295</td>
      <td>1.616239</td>
      <td>2.615747</td>
      <td>1.544260</td>
      <td>9.930833</td>
      <td>8.863589</td>
      <td>5.856135</td>
      <td>6.023406</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter - 02</td>
      <td>836</td>
      <td>836</td>
      <td>364</td>
      <td>22.349739</td>
      <td>0.223498</td>
      <td>2900.406362</td>
      <td>2.217468e+05</td>
      <td>POLYGON ((-2.27354 57.10449, -2.27333 57.10449...</td>
      <td>...</td>
      <td>3.915072</td>
      <td>4.309812</td>
      <td>2.555858</td>
      <td>3.646697</td>
      <td>2.849656</td>
      <td>11.042816</td>
      <td>9.978272</td>
      <td>7.515000</td>
      <td>7.926029</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter - 03</td>
      <td>643</td>
      <td>643</td>
      <td>340</td>
      <td>27.019476</td>
      <td>0.270194</td>
      <td>3468.761949</td>
      <td>2.701948e+05</td>
      <td>POLYGON ((-2.27443 57.10171, -2.27237 57.10046...</td>
      <td>...</td>
      <td>3.323025</td>
      <td>3.784549</td>
      <td>1.440991</td>
      <td>3.247325</td>
      <td>2.062255</td>
      <td>10.616768</td>
      <td>8.620700</td>
      <td>4.321493</td>
      <td>5.770910</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter - 04</td>
      <td>580</td>
      <td>580</td>
      <td>274</td>
      <td>9.625426</td>
      <td>0.096254</td>
      <td>1647.461389</td>
      <td>9.625426e+04</td>
      <td>POLYGON ((-2.26611 57.10133, -2.26599 57.10092...</td>
      <td>...</td>
      <td>2.622991</td>
      <td>2.778026</td>
      <td>2.620681</td>
      <td>1.936908</td>
      <td>2.160142</td>
      <td>10.036471</td>
      <td>7.935112</td>
      <td>8.433328</td>
      <td>8.329819</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter - 05</td>
      <td>644</td>
      <td>577</td>
      <td>256</td>
      <td>18.007657</td>
      <td>0.180076</td>
      <td>3026.111412</td>
      <td>1.800766e+05</td>
      <td>POLYGON ((-2.26013 57.10160, -2.26050 57.10134...</td>
      <td>...</td>
      <td>2.115004</td>
      <td>2.358335</td>
      <td>2.408416</td>
      <td>1.845672</td>
      <td>1.784635</td>
      <td>9.650000</td>
      <td>5.568964</td>
      <td>6.966429</td>
      <td>6.632609</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6971</th>
      <td>S01013477</td>
      <td>Broxburn South - 06</td>
      <td>605</td>
      <td>605</td>
      <td>303</td>
      <td>10.988164</td>
      <td>0.109882</td>
      <td>1775.782199</td>
      <td>1.098816e+05</td>
      <td>POLYGON ((-3.46323 55.93434, -3.46319 55.93408...</td>
      <td>...</td>
      <td>2.732500</td>
      <td>3.706674</td>
      <td>1.690504</td>
      <td>3.148465</td>
      <td>1.559616</td>
      <td>5.930057</td>
      <td>7.693930</td>
      <td>4.175574</td>
      <td>4.284855</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6972</th>
      <td>S01013478</td>
      <td>Broxburn East - 01</td>
      <td>860</td>
      <td>851</td>
      <td>358</td>
      <td>12.438169</td>
      <td>0.124382</td>
      <td>2319.192976</td>
      <td>1.243817e+05</td>
      <td>POLYGON ((-3.48355 55.93733, -3.48354 55.93728...</td>
      <td>...</td>
      <td>5.227738</td>
      <td>1.922628</td>
      <td>1.977860</td>
      <td>2.979797</td>
      <td>2.815971</td>
      <td>3.425031</td>
      <td>6.550913</td>
      <td>6.485256</td>
      <td>7.598948</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6973</th>
      <td>S01013479</td>
      <td>Broxburn East - 02</td>
      <td>786</td>
      <td>781</td>
      <td>428</td>
      <td>26.714576</td>
      <td>0.267145</td>
      <td>3234.544766</td>
      <td>2.671458e+05</td>
      <td>POLYGON ((-3.46664 55.93627, -3.46652 55.93622...</td>
      <td>...</td>
      <td>3.664326</td>
      <td>3.613521</td>
      <td>1.657220</td>
      <td>2.530326</td>
      <td>1.445423</td>
      <td>5.912698</td>
      <td>7.355121</td>
      <td>4.006667</td>
      <td>4.412252</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>6974</th>
      <td>S01013480</td>
      <td>Broxburn East - 03</td>
      <td>671</td>
      <td>671</td>
      <td>302</td>
      <td>9.624700</td>
      <td>0.096248</td>
      <td>1598.577583</td>
      <td>9.624701e+04</td>
      <td>POLYGON ((-3.46259 55.93774, -3.46243 55.93725...</td>
      <td>...</td>
      <td>3.251885</td>
      <td>4.625972</td>
      <td>2.600421</td>
      <td>3.541645</td>
      <td>2.127917</td>
      <td>6.626376</td>
      <td>10.176170</td>
      <td>6.409534</td>
      <td>6.928539</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6975</th>
      <td>S01013481</td>
      <td>Broxburn East - 04</td>
      <td>451</td>
      <td>451</td>
      <td>189</td>
      <td>511.606694</td>
      <td>5.116068</td>
      <td>13900.390027</td>
      <td>5.116067e+06</td>
      <td>POLYGON ((-3.44318 55.93923, -3.44285 55.93909...</td>
      <td>...</td>
      <td>2.330493</td>
      <td>4.745421</td>
      <td>2.732700</td>
      <td>4.283480</td>
      <td>2.577007</td>
      <td>7.106607</td>
      <td>11.469202</td>
      <td>7.889751</td>
      <td>8.615898</td>
      <td>8.0</td>
    </tr>
  </tbody>
</table>
<p>6976 rows × 58 columns</p>
</div>




```python
simd_2016_merged.rename(columns={'Crime_Domain_2016_Rank': 'Crime_Domain_Rank' ,
 'Education_Domain_2016_Rank':'Education_Domain_Rank',
 'Employment_Domain_2016_Rank': 'Employment_Domain_Rank',
 'Employment_count':'employment_count',
 'Employment_rate':'employment_rate',
 'Geographic_Access_Domain_2016_Rank': 'Access_Domain_Rank',
 'HESA':'University',
 'Health_Domain_2016_Rank': 'Health_Domain_Rank',
 'Housing_Domain_2016_Rank':'Housing_Domain_Rank',
 'Income_Domain_2016_Rank':'Income_Domain_Rank',
 'Income_count':'income_count',
 'Income_rate':'income_rate',
 'NEET':'not_participating',
 'Noquals':'no_qualifications',
 'Overal_SIMD16_Rank':'Rank',
 'PT_Post':'PT_post',
 'SIMD_2016_Decile': 'Decile',
 'SIMD_2016_Percentile': 'Percentile',
 'SIMD_2016_Quintile':'Quintile',
 'SIMD_2016_Vigintile':'Vigintile',
 'Working_age_population_Revised':'Working_Age_population',
 'drive_PO':'drive_post',
 'nocentralheat_count':'nocentralheating_count',
 'nocentralheat_rate':'nocentralheating_rate'},
                 inplace=True)

simd_2016_merged
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Name</th>
      <th>TotPop2011</th>
      <th>ResPop2011</th>
      <th>HHCnt2011</th>
      <th>StdAreaHa</th>
      <th>StdAreaKm2</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>geometry</th>
      <th>...</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
      <th>crime_rate</th>
      <th>overcrowded_count</th>
      <th>nocentralheating_count</th>
      <th>overcrowded_rate</th>
      <th>nocentralheating_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter - 01</td>
      <td>872</td>
      <td>852</td>
      <td>424</td>
      <td>438.880218</td>
      <td>4.388801</td>
      <td>11801.872345</td>
      <td>4.388802e+06</td>
      <td>POLYGON ((-2.27748 57.09527, -2.27644 57.09521...</td>
      <td>...</td>
      <td>1.5</td>
      <td>10.8</td>
      <td>8.4</td>
      <td>6.0</td>
      <td>5.7</td>
      <td>89.0</td>
      <td>87.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter - 02</td>
      <td>836</td>
      <td>836</td>
      <td>364</td>
      <td>22.349739</td>
      <td>0.223498</td>
      <td>2900.406362</td>
      <td>2.217468e+05</td>
      <td>POLYGON ((-2.27354 57.10449, -2.27333 57.10449...</td>
      <td>...</td>
      <td>2.7</td>
      <td>11.5</td>
      <td>8.3</td>
      <td>7.3</td>
      <td>6.8</td>
      <td>48.0</td>
      <td>85.0</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter - 03</td>
      <td>643</td>
      <td>643</td>
      <td>340</td>
      <td>27.019476</td>
      <td>0.270194</td>
      <td>3468.761949</td>
      <td>2.701948e+05</td>
      <td>POLYGON ((-2.27443 57.10171, -2.27237 57.10046...</td>
      <td>...</td>
      <td>1.9</td>
      <td>11.5</td>
      <td>7.9</td>
      <td>5.8</td>
      <td>5.3</td>
      <td>58.0</td>
      <td>31.0</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter - 04</td>
      <td>580</td>
      <td>580</td>
      <td>274</td>
      <td>9.625426</td>
      <td>0.096254</td>
      <td>1647.461389</td>
      <td>9.625426e+04</td>
      <td>POLYGON ((-2.26611 57.10133, -2.26599 57.10092...</td>
      <td>...</td>
      <td>2.2</td>
      <td>10.8</td>
      <td>7.4</td>
      <td>8.3</td>
      <td>8.4</td>
      <td>0.0</td>
      <td>42.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter - 05</td>
      <td>644</td>
      <td>577</td>
      <td>256</td>
      <td>18.007657</td>
      <td>0.180076</td>
      <td>3026.111412</td>
      <td>1.800766e+05</td>
      <td>POLYGON ((-2.26013 57.10160, -2.26050 57.10134...</td>
      <td>...</td>
      <td>1.9</td>
      <td>10.6</td>
      <td>5.1</td>
      <td>6.6</td>
      <td>6.6</td>
      <td>178.0</td>
      <td>50.0</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6971</th>
      <td>S01013477</td>
      <td>Broxburn South - 06</td>
      <td>605</td>
      <td>605</td>
      <td>303</td>
      <td>10.988164</td>
      <td>0.109882</td>
      <td>1775.782199</td>
      <td>1.098816e+05</td>
      <td>POLYGON ((-3.46323 55.93434, -3.46319 55.93408...</td>
      <td>...</td>
      <td>1.5</td>
      <td>5.5</td>
      <td>7.3</td>
      <td>4.9</td>
      <td>4.8</td>
      <td>648.0</td>
      <td>93.0</td>
      <td>10.0</td>
      <td>15.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>6972</th>
      <td>S01013478</td>
      <td>Broxburn East - 01</td>
      <td>860</td>
      <td>851</td>
      <td>358</td>
      <td>12.438169</td>
      <td>0.124382</td>
      <td>2319.192976</td>
      <td>1.243817e+05</td>
      <td>POLYGON ((-3.48355 55.93733, -3.48354 55.93728...</td>
      <td>...</td>
      <td>2.5</td>
      <td>3.3</td>
      <td>6.7</td>
      <td>7.3</td>
      <td>7.8</td>
      <td>238.0</td>
      <td>87.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6973</th>
      <td>S01013479</td>
      <td>Broxburn East - 02</td>
      <td>786</td>
      <td>781</td>
      <td>428</td>
      <td>26.714576</td>
      <td>0.267145</td>
      <td>3234.544766</td>
      <td>2.671458e+05</td>
      <td>POLYGON ((-3.46664 55.93627, -3.46652 55.93622...</td>
      <td>...</td>
      <td>1.4</td>
      <td>5.4</td>
      <td>7.2</td>
      <td>4.3</td>
      <td>4.2</td>
      <td>597.0</td>
      <td>96.0</td>
      <td>9.0</td>
      <td>12.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>6974</th>
      <td>S01013480</td>
      <td>Broxburn East - 03</td>
      <td>671</td>
      <td>671</td>
      <td>302</td>
      <td>9.624700</td>
      <td>0.096248</td>
      <td>1598.577583</td>
      <td>9.624701e+04</td>
      <td>POLYGON ((-3.46259 55.93774, -3.46243 55.93725...</td>
      <td>...</td>
      <td>2.1</td>
      <td>6.3</td>
      <td>8.2</td>
      <td>5.9</td>
      <td>6.2</td>
      <td>167.0</td>
      <td>107.0</td>
      <td>1.0</td>
      <td>16.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>6975</th>
      <td>S01013481</td>
      <td>Broxburn East - 04</td>
      <td>451</td>
      <td>451</td>
      <td>189</td>
      <td>511.606694</td>
      <td>5.116068</td>
      <td>13900.390027</td>
      <td>5.116067e+06</td>
      <td>POLYGON ((-3.44318 55.93923, -3.44285 55.93909...</td>
      <td>...</td>
      <td>2.4</td>
      <td>6.2</td>
      <td>11.5</td>
      <td>8.8</td>
      <td>9.4</td>
      <td>302.0</td>
      <td>67.0</td>
      <td>2.0</td>
      <td>15.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>6976 rows × 56 columns</p>
</div>



Let's check to see if there are any more column inconsistencies :


```python
# convert colum names into a set
set_columns_2020 = set(simd_2020_merged.columns)
set_columns_2016 = set(simd_2016_merged.columns)
unique_to_columns_2020 = set_columns_2020 - set_columns_2016
unique_to_columns_2020
```




    {'broadband', 'crime_count'}




```python
unique_to_columns_2016 = set_columns_2016 - set_columns_2020
unique_to_columns_2016
```




    set()



So, the only differences are that 2020 introduced a new indicator `broadband` and there is no `crime_count` column for 2016 contrary to the indicator description. Let's drop these columns and proceed.


```python
# remove the columns
simd_2020_merged.drop(columns=['broadband', 'crime_count'], inplace=True)
```

Excellent. So we now have consistent 2020 and 2016 datasets. Let's create a new dataset of differences. 


```python
simd_2020_merged.columns
```




    Index(['DataZone', 'Name', 'TotPop2011', 'ResPop2011', 'HHCnt2011',
           'StdAreaHa', 'StdAreaKm2', 'Shape_Leng', 'Shape_Area', 'geometry',
           'Intermediate_Zone', 'Council_area', 'Total_population',
           'Working_Age_population', 'Rank', 'Percentile', 'Vigintile', 'Decile',
           'Quintile', 'Income_Domain_Rank', 'Employment_Domain_Rank',
           'Health_Domain_Rank', 'Education_Domain_Rank', 'Access_Domain_Rank',
           'Crime_Domain_Rank', 'Housing_Domain_Rank', 'income_rate',
           'income_count', 'employment_rate', 'employment_count', 'CIF', 'ALCOHOL',
           'DRUG', 'SMR', 'DEPRESS', 'LBWT', 'EMERG', 'Attendance', 'Attainment',
           'no_qualifications', 'not_participating', 'University', 'crime_rate',
           'overcrowded_count', 'nocentralheating_count', 'overcrowded_rate',
           'nocentralheating_rate', 'drive_petrol', 'drive_GP', 'drive_post',
           'drive_primary', 'drive_retail', 'drive_secondary', 'PT_GP', 'PT_post',
           'PT_retail'],
          dtype='object')




```python
diff_columns = ['Total_population',
       'Working_Age_population', 'Rank', 'Percentile', 'Vigintile', 'Decile',
       'Quintile', 'Income_Domain_Rank', 'Employment_Domain_Rank',
       'Health_Domain_Rank', 'Education_Domain_Rank', 'Access_Domain_Rank',
       'Crime_Domain_Rank', 'Housing_Domain_Rank', 'income_rate',
       'income_count', 'employment_rate', 'employment_count', 'CIF', 'ALCOHOL',
       'DRUG', 'SMR', 'DEPRESS', 'LBWT', 'EMERG', 'Attendance', 'Attainment',
       'no_qualifications', 'not_participating', 'University', 'crime_rate',
       'overcrowded_count', 'nocentralheating_count', 'overcrowded_rate',
       'nocentralheating_rate', 'drive_petrol', 'drive_GP', 'drive_post',
       'drive_primary', 'drive_retail', 'drive_secondary', 'PT_GP', 'PT_post',
       'PT_retail']
```


```python
simd_2020_merged.set_index('DataZone', inplace=True)
simd_2016_merged.set_index('DataZone', inplace=True)
```


```python
# Ensure both dataframes have the same columns and are in the same order
simd_2020_merged = simd_2020_merged[diff_columns]
simd_2016_merged = simd_2016_merged[diff_columns]
```


```python
simd_diff_2016_2020 = simd_2020_merged - simd_2016_merged
simd_diff_2016_2020
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total_population</th>
      <th>Working_Age_population</th>
      <th>Rank</th>
      <th>Percentile</th>
      <th>Vigintile</th>
      <th>Decile</th>
      <th>Quintile</th>
      <th>Income_Domain_Rank</th>
      <th>Employment_Domain_Rank</th>
      <th>Health_Domain_Rank</th>
      <th>...</th>
      <th>nocentralheating_rate</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
    </tr>
    <tr>
      <th>DataZone</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>S01006506</th>
      <td>-10.0</td>
      <td>-25.0</td>
      <td>-581.0</td>
      <td>-8.0</td>
      <td>-2.0</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>-577.0</td>
      <td>-1040.0</td>
      <td>268.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.140103</td>
      <td>0.174295</td>
      <td>0.116239</td>
      <td>0.615747</td>
      <td>0.044260</td>
      <td>-0.869167</td>
      <td>0.463589</td>
      <td>-0.143865</td>
      <td>0.323406</td>
    </tr>
    <tr>
      <th>S01006507</th>
      <td>-37.0</td>
      <td>-21.0</td>
      <td>24.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>495.0</td>
      <td>-349.0</td>
      <td>-19.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.215072</td>
      <td>0.309812</td>
      <td>-0.144142</td>
      <td>0.546697</td>
      <td>0.149656</td>
      <td>-0.457184</td>
      <td>1.678272</td>
      <td>0.215000</td>
      <td>1.126029</td>
    </tr>
    <tr>
      <th>S01006508</th>
      <td>-70.0</td>
      <td>-58.0</td>
      <td>-635.0</td>
      <td>-9.0</td>
      <td>-2.0</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>-806.0</td>
      <td>-991.0</td>
      <td>105.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.123025</td>
      <td>0.184549</td>
      <td>-0.659009</td>
      <td>0.547325</td>
      <td>0.162255</td>
      <td>-0.883232</td>
      <td>0.720700</td>
      <td>-1.478507</td>
      <td>0.470910</td>
    </tr>
    <tr>
      <th>S01006509</th>
      <td>-36.0</td>
      <td>-47.0</td>
      <td>-1031.0</td>
      <td>-14.0</td>
      <td>-3.0</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>-1521.0</td>
      <td>-1370.0</td>
      <td>-87.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.122991</td>
      <td>0.278026</td>
      <td>0.620681</td>
      <td>0.536908</td>
      <td>-0.039858</td>
      <td>-0.763529</td>
      <td>0.535112</td>
      <td>0.133328</td>
      <td>-0.070181</td>
    </tr>
    <tr>
      <th>S01006510</th>
      <td>-13.0</td>
      <td>1.0</td>
      <td>-136.0</td>
      <td>-2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>-607.0</td>
      <td>21.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.015004</td>
      <td>0.158335</td>
      <td>0.708416</td>
      <td>0.345672</td>
      <td>-0.115365</td>
      <td>-0.950000</td>
      <td>0.468964</td>
      <td>0.366429</td>
      <td>0.032609</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>S01013477</th>
      <td>-8.0</td>
      <td>-29.0</td>
      <td>271.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>465.0</td>
      <td>434.0</td>
      <td>414.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.132500</td>
      <td>0.306674</td>
      <td>-0.209496</td>
      <td>0.648465</td>
      <td>0.059616</td>
      <td>0.430057</td>
      <td>0.393930</td>
      <td>-0.724426</td>
      <td>-0.515145</td>
    </tr>
    <tr>
      <th>S01013478</th>
      <td>-26.0</td>
      <td>-23.0</td>
      <td>473.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>699.0</td>
      <td>629.0</td>
      <td>202.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.527738</td>
      <td>0.122628</td>
      <td>-0.222140</td>
      <td>1.079797</td>
      <td>0.315971</td>
      <td>0.125031</td>
      <td>-0.149087</td>
      <td>-0.814744</td>
      <td>-0.201052</td>
    </tr>
    <tr>
      <th>S01013479</th>
      <td>-5.0</td>
      <td>-27.0</td>
      <td>-218.0</td>
      <td>-3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>215.5</td>
      <td>-346.0</td>
      <td>-571.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.164326</td>
      <td>0.313521</td>
      <td>-0.142780</td>
      <td>0.130326</td>
      <td>0.045423</td>
      <td>0.512698</td>
      <td>0.155121</td>
      <td>-0.293333</td>
      <td>0.212252</td>
    </tr>
    <tr>
      <th>S01013480</th>
      <td>14.0</td>
      <td>28.0</td>
      <td>570.0</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>69.5</td>
      <td>233.5</td>
      <td>799.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.151885</td>
      <td>0.525972</td>
      <td>0.000421</td>
      <td>0.341645</td>
      <td>0.027917</td>
      <td>0.326376</td>
      <td>1.976170</td>
      <td>0.509534</td>
      <td>0.728539</td>
    </tr>
    <tr>
      <th>S01013481</th>
      <td>-3.0</td>
      <td>9.0</td>
      <td>958.0</td>
      <td>14.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>922.5</td>
      <td>1981.0</td>
      <td>946.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.130493</td>
      <td>0.645421</td>
      <td>0.132700</td>
      <td>0.883480</td>
      <td>0.177007</td>
      <td>0.906607</td>
      <td>-0.030798</td>
      <td>-0.910249</td>
      <td>-0.784102</td>
    </tr>
  </tbody>
</table>
<p>6976 rows × 44 columns</p>
</div>




```python
simd_diff_2016_2020.columns
```




    Index(['Total_population', 'Working_Age_population', 'Rank', 'Percentile',
           'Vigintile', 'Decile', 'Quintile', 'Income_Domain_Rank',
           'Employment_Domain_Rank', 'Health_Domain_Rank', 'Education_Domain_Rank',
           'Access_Domain_Rank', 'Crime_Domain_Rank', 'Housing_Domain_Rank',
           'income_rate', 'income_count', 'employment_rate', 'employment_count',
           'CIF', 'ALCOHOL', 'DRUG', 'SMR', 'DEPRESS', 'LBWT', 'EMERG',
           'Attendance', 'Attainment', 'no_qualifications', 'not_participating',
           'University', 'crime_rate', 'overcrowded_count',
           'nocentralheating_count', 'overcrowded_rate', 'nocentralheating_rate',
           'drive_petrol', 'drive_GP', 'drive_post', 'drive_primary',
           'drive_retail', 'drive_secondary', 'PT_GP', 'PT_post', 'PT_retail'],
          dtype='object')




```python
# merge geospatial datazones
simd_diff_2016_2020_merged = datazones.merge(simd_diff_2016_2020, on='DataZone')
simd_diff_2016_2020_merged.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DataZone</th>
      <th>Name</th>
      <th>TotPop2011</th>
      <th>ResPop2011</th>
      <th>HHCnt2011</th>
      <th>StdAreaHa</th>
      <th>StdAreaKm2</th>
      <th>Shape_Leng</th>
      <th>Shape_Area</th>
      <th>geometry</th>
      <th>...</th>
      <th>nocentralheating_rate</th>
      <th>drive_petrol</th>
      <th>drive_GP</th>
      <th>drive_post</th>
      <th>drive_primary</th>
      <th>drive_retail</th>
      <th>drive_secondary</th>
      <th>PT_GP</th>
      <th>PT_post</th>
      <th>PT_retail</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S01006506</td>
      <td>Culter - 01</td>
      <td>872</td>
      <td>852</td>
      <td>424</td>
      <td>438.880218</td>
      <td>4.388801</td>
      <td>11801.872345</td>
      <td>4.388802e+06</td>
      <td>POLYGON ((-2.27748 57.09527, -2.27644 57.09521...</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.140103</td>
      <td>0.174295</td>
      <td>0.116239</td>
      <td>0.615747</td>
      <td>0.044260</td>
      <td>-0.869167</td>
      <td>0.463589</td>
      <td>-0.143865</td>
      <td>0.323406</td>
    </tr>
    <tr>
      <th>1</th>
      <td>S01006507</td>
      <td>Culter - 02</td>
      <td>836</td>
      <td>836</td>
      <td>364</td>
      <td>22.349739</td>
      <td>0.223498</td>
      <td>2900.406362</td>
      <td>2.217468e+05</td>
      <td>POLYGON ((-2.27354 57.10449, -2.27333 57.10449...</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.215072</td>
      <td>0.309812</td>
      <td>-0.144142</td>
      <td>0.546697</td>
      <td>0.149656</td>
      <td>-0.457184</td>
      <td>1.678272</td>
      <td>0.215000</td>
      <td>1.126029</td>
    </tr>
    <tr>
      <th>2</th>
      <td>S01006508</td>
      <td>Culter - 03</td>
      <td>643</td>
      <td>643</td>
      <td>340</td>
      <td>27.019476</td>
      <td>0.270194</td>
      <td>3468.761949</td>
      <td>2.701948e+05</td>
      <td>POLYGON ((-2.27443 57.10171, -2.27237 57.10046...</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.123025</td>
      <td>0.184549</td>
      <td>-0.659009</td>
      <td>0.547325</td>
      <td>0.162255</td>
      <td>-0.883232</td>
      <td>0.720700</td>
      <td>-1.478507</td>
      <td>0.470910</td>
    </tr>
    <tr>
      <th>3</th>
      <td>S01006509</td>
      <td>Culter - 04</td>
      <td>580</td>
      <td>580</td>
      <td>274</td>
      <td>9.625426</td>
      <td>0.096254</td>
      <td>1647.461389</td>
      <td>9.625426e+04</td>
      <td>POLYGON ((-2.26611 57.10133, -2.26599 57.10092...</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.122991</td>
      <td>0.278026</td>
      <td>0.620681</td>
      <td>0.536908</td>
      <td>-0.039858</td>
      <td>-0.763529</td>
      <td>0.535112</td>
      <td>0.133328</td>
      <td>-0.070181</td>
    </tr>
    <tr>
      <th>4</th>
      <td>S01006510</td>
      <td>Culter - 05</td>
      <td>644</td>
      <td>577</td>
      <td>256</td>
      <td>18.007657</td>
      <td>0.180076</td>
      <td>3026.111412</td>
      <td>1.800766e+05</td>
      <td>POLYGON ((-2.26013 57.10160, -2.26050 57.10134...</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.015004</td>
      <td>0.158335</td>
      <td>0.708416</td>
      <td>0.345672</td>
      <td>-0.115365</td>
      <td>-0.950000</td>
      <td>0.468964</td>
      <td>0.366429</td>
      <td>0.032609</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 54 columns</p>
</div>




```python
# save merged dataframe to a shapefile
simd_diff_2016_2020_merged.to_file('simd_diff_2016_2020_merged.shp')  
```

    /tmp/ipykernel_222/4260494113.py:2: UserWarning: Column names longer than 10 characters will be truncated when saved to ESRI Shapefile.
      simd_diff_2016_2020_merged.to_file('simd_diff_2016_2020_merged.shp')


### Visualizing the difference dataset in QGIS

![SIMD_percentile_change_2016_2020](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/2f3886a5-6ce3-48c5-8ef8-4663bd2c673f)


So here we can see in one graphic the change in the SIMD percentile for each datazone from 2016 to 2020. Dark green indicates the areas with the biggest improvement in percentile, whilst dark red represents the biggest drop in percentile.

Let me just check my datazone again - which was SI S01007821.

![dundee_change](https://github.com/Stephen137/Scottish-Index-of-Multiple-Deprivation/assets/97410145/a5da9189-08ef-4b4e-8958-9e6a67f650c7)


A change of -15. That's good! Remember that a fall in percentile is a positive thing as it means a lower "deprivation" score.




### Conclusion

First and foremost, this project was an opportunity for me to test my data cleansing and analysis skills within a geospatial context, using real data. I was also able put my recent QGIS and ArcGIS learning to practical use. It was a challenging, but rewarding project. I now have a better understanding of the data used to calculate the SIMD and the socio-economic geography of my homeland, and was able to visualize the changes in the 2020 index from 2016. The insights could provide a platform to help shape future resource planning.

I look forward to the publication of the 2024 index.

In conclusion, although the Scottish Index of Multiple Deprivation (SMID) is a useful indicator of deprivation levels throughout Scotland, like any index it must be interpreetd with caution. The choice of domains, indicators and weightings applied, are subjective and changes to these components could potentially paint a very different picture. Like any statistical measure, in order to make an informed decision based on the SMID, it is critical to perform a thorough analysis of the supporting data. 

### Improvements to my analysis

This was a first look at the index and subject to considerable time and resource constraints. Further improvements :

- interactive mapping
- creation of an end-to-end data pipeline
- deeper analysis of sub-domains
- create an alternative index based on different domains/indicators and/or weightings

