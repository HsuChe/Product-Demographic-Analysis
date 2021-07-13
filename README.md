
<br />
<p align="center">

  <h3 align="center">Heroes Of Pymoli</h3>

  <p align="center">
     Performing data analysis using pandas
    <br />
    <a href="https://github.com/HsuChe/pandas-challenge"><strong>Project Github URL »</strong></a>
    <br />
    <br />
  </p>
</p>



<!-- ABOUT THE PROJECT -->
## About The Project

![hero image](https://github.com/HsuChe/pandas-challenge/blob/d552ad43a81eb152fc208a1a150dde6396f97a4f/images/fantasy-3077928_1920.jpg)

Sales analysis is critical for business decisions. In this homework, we will be using pandas library to analyze in game purchase data from Heroes of Pymoli.

Features of the dataset:
* The in-game purchases dataset contains the following columns:
    * Purchase ID: **Unique ID for each in-game purchase**
    * SN: **The screen name of the customer**
    * Age: **The age of the custoemr**
    * Gender: **The gender of the customer**
    * Item ID: **Unique ID for the item in the transaction**
    * Item Name: **The name of the item in the transaction**
    * Prices: **The price of the transaction**

* The dataset is in the csv file format with delimiter of comma.

* Download Dataset click [HERE](https://github.com/HsuChe/pandas-challenge/blob/0f628f032da5f551f4000b80c5b4ccd4dd77c3ab/HeroesOfPymoli/Resources/purchase_data.csv)

The homework will present various business indicator to generate. The following information will be the focus for analysis.

* Unique Player Count.
* Overall purchasing analysis.
* Player and purchase analysis based on gender.
* Player and purchase analysis based on age.
* Top spender.
* Top performing item based on transaction quantity and total price.

<!-- GETTING STARTED -->
## Processing the csv 

Open the csv with pd.read_csv() and create the DataFrame with it. 

* generating DataFrame from the csv. 
  ```sh
  import pandas as pd
  file_to_load = "Resources/purchase_data.csv"
  purchase_data = pd.read_csv(file_to_load)
  ```

### Finding total unique players in the dataset.

After generating the DataFrame, we will calculate the total unique player count in the dataset.

* finding the total unique players in the dataset. 
  ```sh
    total_players = len(purchase_data['SN'].unique())
    total_players_df = pd.DataFrame({'Total Players':[total_players]})
    total_players_df
  ```

### Overall purchasing analysis

Next, we are tasked to find various business analysis on the dataset. 

* we are looking for unique items, average price, purchase count, and total revenue:
  ```sh
  item_unique = len(purchase_data['Item ID'].unique())
  average_price = purchase_data['Price'].mean()
  total_purchase = len(purchase_data['Purchase ID'])
  total_revenue = purchase_data['Price'].sum()
  ```

* we will generate a dictionary for formatting into a summary table DataFrame:
  ```sh
  df_dict = {
    'Number of Unique Items': item_unique,
    'Average Price': average_price,
    'Number of Purchases': total_purchase,
    'Total Revenue': total_revenue,
    }
  ```

* then we will create the DataFrame from the dictionary:
  ```sh
    purchase_analysis_df = pd.DataFrame(df_dict, index = [0])
    }
  ```

### Analysis based on gender.

We will analyze the data with gender as the focus. The first analysis we will perform are general analysis such as the number of players based on gender and percentage of players based on specific gender.

* Generating data using the groupby method
  ```sh
  total_count_gender = purchase_data.groupby('Gender').nunique()['SN']

  percentage_players = total_count_gender / total_players * 100

  gender_demographics = pd.DataFrame({"Total Count": total_count,"Percentage of Players": percentage_players.map("{:,.2f}%".format)}, index = ['Male','Female','Other / Non-Disclosed'])
  ```
Next we will calculate specific sales indicators based on gender. 

* calculate the indicators we are looking for. 
  ```sh
    gender_group = purchase_data.groupby('Gender')

    purchase_count = gender_group['Purchase ID'].count()
    avearge_purchase_price = gender_group['Price'].mean()
    total_purchase_value = gender_group['Price'].sum()

    avg_total_purchase_per = total_purchase_value/  total_count_gender
  ```

Next we will input all the calculations into a summary table. 

* create the summary table dataframe and format the numbers to currency with two decimal places.
  ```sh
    summary_table = pd.DataFrame()
    summary_table['Purchase Count'] = purchase_count

    summary_table['Average Purchase Price'] = avearge_purchase_price.map("${:,.2f}".format)
    summary_table['Total Purchase Value'] = total_purchase_value

    summary_table['Average Purchase Total per Person by Gender'] = avg_total_purchase_per.map("${:,.2f}".format)

  ```

### Analysis based on age.

Next we will be analyzing the dataset with focus on age. The best way to analyze ranges of number is to assign each range to a specic bin and groupby using the bins as indexes.

* generate the bins that we wish to analyze age with.
  ```sh
  bins = [0,9,14,19,24,29,34,39,150]
  bin_names = ["<10","10-14","15-19","20-24",'25-29','30-34','35-39','40+']
  ```
Next, create the column series with the bin that we just set. 

* create the series using the pd.cut method and add the series into the dataset as a column. 
  ``` sh
  bin_column = pd.cut(df_checkpoint['Age'], bins, labels = bin_names, include_lowest = True)
  
  purchase_data_bins = df_checkpoint.copy()
  purchase_data_bins['Age Bin'] = bin_column
  ```

Next we will do our calculations and put all the information into a summary table. 

* we can use the nunique() method on the series group object to calculate the count of unique SN based on our bins
  ```sh
    
    total_count_age = purchase_data_bins.groupby('Age Bin').nunique()['SN']
    percentage_players = total_count_age / total_players * 100

    age_demographics = pd.DataFrame({"Total Count": total_count_age,"Percentage of Players": percentage_players.map("{:,.2f}%".format)}, index = bin_names)

  ```

### Finding the top spenders

We will usd the dataset to generate the highest five spenders in terms of total purchases.

* generate the groupby object based on SN and then calculate the indicators we are looking for. 
  ```sh
    sn_group = purchase_data_bins.groupby("SN")

    total_purchase_value = sn_group['Price'].sum()
    average_purchase_price = sn_group['Price'].mean()
    purchase_count = sn_group['Price'].count()
  ```

Next we will create a new DataFrame with the indicators then sort the DataFrame descending based on total purchase. 

* generate the summary table and sort the data based on total purchases
  ``` sh
    summary_table = pd.DataFrame()
    summary_table['Purchase Count'] = purchase_count
    summary_table['Average Purchase Price'] = average_purchase_price
    summary_table['Total Purchase Value'] = total_purchase_value
    summary_table = summary_table.sort_values(['Total Purchase Value'], ascending = False).head()
  ```

Lastly, we will formate the currency cells and transform it into strings with two decimal places. 

* format the currency columns to two decimal places
  ```sh
    summary_table['Total Purchase Value'] = summary_table['Total Purchase Value'].map("${:,.2f}".format)
    
    summary_table['Average Purchase Price'] = summary_table['Average Purchase Price'].map("${:,.2f}".format)
  ```

### Finding the most popular item

We will use the dataset to find the top five most popular items based on purchase count.

* generate the groupby object based on item ID and Item name, then calculate the indicators we are looking for. 
  ```sh
    item_group = purchase_data_bins.groupby(['Item ID','Item Name'])

    total_purchase_value = item_group['Price'].sum()
    average_purchase_price = item_group['Price'].mean()
    purchase_count = item_group['Price'].count()
  ```

Next we will create a new DataFrame with the indicators then sort the DataFrame descending based on purchase count. 

* generate the summary table and sort the data based on total purchases descending
  ``` sh
    summary_table = pd.DataFrame()
    summary_table['Purchase Count'] = purchase_count
    summary_table['Average Purchase Price'] = average_purchase_price
    summary_table['Total Purchase Value'] = total_purchase_value

    summary_table_pop = summary_table.sort_values(['Purchase Count'], ascending = False).head()
  ```

Lastly, we will formate the currency cells and transform it into strings with two decimal places. 

* format the currency columns to two decimal places
  ```sh
    summary_table_pop['Total Purchase Value'] = summary_table_pop['Total Purchase Value'].map("${:,.2f}".format)
    summary_table_pop['Average Purchase Price'] = summary_table_pop['Average Purchase Price'].map("${:,.2f}".format)
  ```

### Finding the most profitable items

We will use the dataset to find the top five most profitable items based on total revenue.

* Using the summary table generated for items from the most popular, we will set new sorting parameters to Total Purchase Value. 
  ```sh
    summary_table = summary_table.sort_values(['Total Purchase Value'], ascending = False).head()

    summary_table['Total Purchase Value'] = summary_table['Total Purchase Value'].map("${:,.2f}".format)
    summary_table['Average Purchase Price'] = summary_table['Average Purchase Price'].map("${:,.2f}".format)
  ```

