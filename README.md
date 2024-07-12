# Week3: ImmoEliza Dataset Analysis Make beautiful visualisation

Using pandas and data visualisation libraries (Matplotlib, Seaborn), let's establish conclusions about a dataset.

## This week's summary

**The week is**: Cleaning and doing a complete charts contains interesting information  

## Description

- This project focuses on cleaning and preparing a real estate dataset for analysis by addressing missing values and detecting outliers.

- The dataset contains various property attributes such as construction year, room count, and more.

- To ensure the data's accuracy and reliability for subsequent analysis, we use K-Nearest Neighbors (KNN) imputation to fill missing values and apply statistical methods to identify and handle outliers.


### Team member:  

<table style="width: 100%;" >
<tbody>
<tr>
<td style="border: 1px solid #ffffff00" width="90%">
Ezgi <img src="https://raw.githubusercontent.com/Joffreybvn/challenge-collecting-data/master/docs/arrow.svg" width="12"> https://github.com/ezgitandogan <br>
Mehmet <img src="https://raw.githubusercontent.com/Joffreybvn/challenge-collecting-data/master/docs/arrow.svg" width="12"> https://github.com/mehmetbatar35 <br>
Ziadoon <img src="https://raw.githubusercontent.com/Joffreybvn/challenge-collecting-data/master/docs/arrow.svg" width="12"> https://github.com/ziadoonAlobaidi/
</td>
<td>

</td>
</tr>
</tbody>
</table>

## Timeline

- **Day 1:** Data exploration and initial cleaning
- **Day 2-3:** Implement KNN imputation for missing values
- **Day 4:** Outlier detection and handling
- **Day 5:** Data Visualization and Analysis

### Objectives:
<pre>
How are variables correlated to each other? (Why?)
Which variables have the greatest influence on the price?
Which variables have the least influence on the price?
How many qualitative and quantitative variables are there? How would you transform these values into numerical values?
Percentage of missing values per column?
Which variables would you delete and why ?
Represent the number of properties according to their surface using a histogram.
In your opinion, which 5 variables are the most important and why?
What are the most expensive municipalities in Belgium? (Average price, median price, price per square meter)
What are the most expensive municipalities in Wallonia? (Average price, median price, price per square meter)
What are the most expensive municipalities in Flanders? (Average price, median price, price per square meter)
What are the less expensive municipalities in Belgium? (Average price, median price, price per square meter)
What are the less expensive municipalities in Wallonia? (Average price, median price, price per square meter)
What are the less expensive municipalities in Flanders? (Average price, median price, price per square meter)
</pre>


## File Information

- CSV File: `final_dataset.csv`
- Original JSON File: `final_dataset.json`

## Installation

- To work with this project, you need to have Python installed along with the necessary libraries. Follow these steps to set up the environment:

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/mehmetbatar35/ImmoEliza_EDA.git
   ```

2. **Navigate to the Project Directory:**

   ```bash
   cd ImmoEliza_EDA
   ```

3. **Install Required Libraries:**

   \*Install Required Libraries:\*\*

   It is recommended to use a virtual environment. Install the required libraries 


## Usage

1. **Load the Dataset:**

   Ensure you have the dataset file (`final_dataset.csv`) in the project directory.

   ```python
   import pandas as pd

   # Load the dataset
   df = pd.read_csv('final_dataset.csv')
   ```

2. **Data Cleaning:**

   - **Remove Duplicates:**

     ```python
     df = df.drop_duplicates()
     ```

   - **Convert Categorical Variables:**

     ```python
     df['stateofbuilding'] = df['stateofbuilding'].astype('category').cat.codes
     df['kitchen'] = df['kitchen'].astype('category').cat.codes
     df['floodingzone'] = df['floodingzone'].astype('category').cat.codes
     df['subtypeofproperty'] = df['subtypeofproperty'].astype('category').cat.codes
     df['typeofsale'] = df['typeofsale'].astype('category').cat.codes
     ```

3. **KNN Imputation:**

   ```python
   from sklearn.impute import KNNImputer

   # Columns to be imputed
   columns_for_imputation = ['roomcount', 'surfaceofplot', 'stateofbuilding', 'numberoffacades', 'livingarea', 'bedroomcount', 'bathroomcount', 'constructionyear', 'price', 'kitchen']

   # Select only the columns to impute
   df_impute = df[columns_to_impute].copy()

   # Convert categorical columns to numerical if necessary
   df_impute['stateofbuilding'] = df_impute['stateofbuilding'].astype('category').cat.codes
   df_impute['kitchen'] = df_impute['kitchen'].astype('category').cat.codes

   # Initialize the KNN Imputer
   knn_imputer = KNNImputer(n_neighbors=5)

   # Impute missing values
    df_imputed = pd.DataFrame(knn_imputer.fit_transform(df[columns_for_imputation]),
    columns=columns_for_imputation)


   # Round and map imputed categorical values
    df_imputed['stateofbuilding'] = np.round(df_imputed['stateofbuilding']).astype(int)
    df_imputed['stateofbuilding'] = df_imputed['stateofbuilding'].apply(lambda x: x if x in
    state_mapping.values() else np.nan)
    df['stateofbuilding'] = df_imputed['stateofbuilding'].values

    df_imputed['kitchen'] = np.round(df_imputed['kitchen']).astype(int)
    df_imputed['kitchen'] = df_imputed['kitchen'].apply(lambda x: x if x in
    kitchen_new_mapping.values() else np.nan)
    df['kitchen'] = df_imputed['kitchen'].values

   # Update original DataFrame with imputed values
    df['constructionyear'] = df_imputed['constructionyear'].values
    df['roomcount'] = df_imputed['roomcount'].values
    df['surfaceofplot'] = df_imputed['surfaceofplot'].values
    df['numberoffacades'] = df_imputed['numberoffacades'].values
    df['livingarea'] = df_imputed['livingarea'].values
   ```

4. **Outlier Detection and Handling:**

- Outliers can significantly skew analysis and models. The following steps demonstrate how to detect and handle outliers, focusing on the `price` column for `residential_sale` properties.

  - **Filter Data by Sale Type:**

    ```python
    df_sales = df[df['typeofsale'] == 'residential_sale']
    df_rent = df[df['typeofsale'] == 'residential_monthly_rent']
    ```

  - **Calculate Outlier Bounds for `price`:**

    ```python
    Q1 = df_sales['price'].quantile(0.25)
    Q3 = df_sales['price'].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    ```

  - **Filter Out Outliers:**

    ```python
    df_sales = df_sales[(df_sales['price'] > lower_bound) & (df_sales['price'] < upper_bound)]
    ```

  - **Save the Cleaned Dataset:**

    ```python
    df_sales.to_csv('cleaned_dataset.csv', index=False)
    ```

## Visuals

- Here are some example visualizations that can help in understanding the dataset and the effects of imputation and outlier removal:
1.**Heatmap showing the correlation between all data**
  <img src="https://github.com/ziadoonAlobaidi/immoMeZgZd/blob/main/heatmap_corr.png"/>
1. **Average price by state**
   <img src="https://github.com/ziadoonAlobaidi/immoMeZgZd/blob/main/image.png"/>
2.**Average construction year per zip code**
   Here is a quick introduction to our work: A map of all belgian's municipalities with their average construction year. 
  <img src="https://github.com/ziadoonAlobaidi/immoMeZgZd/blob/main/Untitled.png">
3.**Average price by construction year**
   <img src="https://github.com/ziadoonAlobaidi/immoMeZgZd/blob/main/22.png">
   
. **Missing Values Heatmap:**

. **Before and After Imputation Distribution:**


