
# Heroes of Pymoli Data Analysis

* Heroes of Pymoli is dominated by male players between 15 and 29 years old. Just over 80% of the player base is male and, additionally, they are slightly more profitable than female players. About 77% percent of the player base falls between 15 and 29 years old. Historically, this is the most common gaming demographic.
* The average purchase in Heroes of Pymoli is relatively low (2.93). This leads one to believe that the game is buyoed mostly by a large swath of microtransactions. In particular, if we analyze the top spenders, they don't even crack twenty dollars. While this does mean the top spender contributes about 6 times more than the average customer, the vast majority of the revenue comes from one and two time purchasers.
* The most profitable item, the Retribution Axe, has been purchased 9 times, which ties it in popularity for the third most popular item. This is clearly an item to focus on and one to examine further. If we can produce more items like the axe, then that could be an easy path to increasing profitability. Similarly, it seems like making items cost over $3.50 doesn't scare away the player base, as evidenced by the most profitable item list. Making some more desirable items slightly more expensive should similarly increase profits.


```python
# Import dependencies
import pandas as pd
```


```python
# Define the data path then read the data into a data frame
json_path = 'raw_data/purchase_data.json'

pymoli_df = pd.read_json(json_path)

pymoli_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



## Total Players


```python
# Find the total number of players by counting the number of screen names (SN)
# Note that we use value_counts because SN's are repeated in the dataset

total_players = len(pymoli_df["SN"].value_counts())

players_df = pd.DataFrame({"Total Players": total_players}, index = [0])

players_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing analysis (Total)


```python
# Unique items is the total distinct Item IDs, captured by value_counts
unique_items = len(pymoli_df["Item ID"].value_counts())
# Use mean on the price column to get average purchase amount
avg_purchase_total = pymoli_df["Price"].mean()
# Check the length of the Price series to get the total number of purchases
total_purchases = len(pymoli_df["Price"])
# Sum the Price series to get the total revenue
total_revenue = pymoli_df["Price"].sum()

purchasing_analysis_df = pd.DataFrame({"Number of unique items": unique_items,
                                       "Average price": avg_purchase_total,
                                       "Number of purchases": total_purchases,
                                       "Total revenue": total_revenue}, index = [0])

purchasing_analysis_df["Average price"] = purchasing_analysis_df["Average price"].map("${:.2f}".format)
purchasing_analysis_df["Total revenue"] = purchasing_analysis_df["Total revenue"].map("${:.2f}".format)

purchasing_analysis_df
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
      <th>Average price</th>
      <th>Number of purchases</th>
      <th>Number of unique items</th>
      <th>Total revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>183</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
# Group by Gender in order to extract summary data
gender_grouped = pymoli_df.groupby("Gender")
unique_members = gender_grouped["SN"].nunique()

# Create a data frame with the summary gender data
gender_demo = pd.DataFrame({"Total Counts": unique_members,
                           "Percentage of Players": 100*unique_members/total_players})

gender_demo["Percentage of Players"] = gender_demo["Percentage of Players"].map("{:.2f}".format)

gender_demo
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
      <th>Percentage of Players</th>
      <th>Total Counts</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)


```python
# Use methods on GroupedSeries objects to get relevant summary statistics
purchase_count = gender_grouped["Age"].count()
avg_purchase = gender_grouped["Price"].mean()
total_purchase_value = gender_grouped["Price"].sum()

# Create a summary data frame
purchasing_by_gender_df = pd.DataFrame({"Purchase count": purchase_count,
                                       "Average purchase price": avg_purchase,
                                       "Total purchase value": total_purchase_value,
                                       "Normalized Total": total_purchase_value/unique_members})

purchasing_by_gender_df["Average purchase price"] = purchasing_by_gender_df["Average purchase price"].map("${:.2f}".format)
purchasing_by_gender_df["Total purchase value"] = purchasing_by_gender_df["Total purchase value"].map("${:.2f}".format)
purchasing_by_gender_df["Normalized Total"] = purchasing_by_gender_df["Normalized Total"].map("${:.2f}".format)

purchasing_by_gender_df
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
      <th>Average purchase price</th>
      <th>Normalized Total</th>
      <th>Purchase count</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>$3.83</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$4.02</td>
      <td>633</td>
      <td>$1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$4.47</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>



## Age demographics


```python
# Use halves because ages are whole numbers and borders appear to be inclusive
# Made the last number very large to get any older folks included in the dataset
bins = [0, 9.5, 14.5, 19.5, 24.5, 29.5, 34.5, 39.5, 1000]

bin_labels = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

pymoli_df["Age group"] = pd.cut(pymoli_df["Age"], bins, labels = bin_labels)

age_grouped = pymoli_df.groupby("Age group")
unique_members = age_grouped["SN"].nunique()

age_demo = pd.DataFrame({"Total Counts": age_grouped["SN"].nunique(),
                           "Percentage of Players": 100*unique_members/total_players})

age_demo["Percentage of Players"] = age_demo["Percentage of Players"].map("{:.2f}".format)

age_demo
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
      <th>Percentage of Players</th>
      <th>Total Counts</th>
    </tr>
    <tr>
      <th>Age group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)


```python
# Extract the datapoints by performing operations on the relevant Grouped Series
purchase_count = age_grouped["Age"].count()
avg_purchase = age_grouped["Price"].mean()
total_purchase_value = age_grouped["Price"].sum()


purchasing_by_age_df = pd.DataFrame({"Purchase count": purchase_count,
                                       "Average purchase price": avg_purchase,
                                       "Total purchase value": total_purchase_value,
                                        "Normalized Total": total_purchase_value/unique_members})

purchasing_by_age_df["Average purchase price"] = purchasing_by_age_df["Average purchase price"].map("${:.2f}".format)
purchasing_by_age_df["Total purchase value"] = purchasing_by_age_df["Total purchase value"].map("${:.2f}".format)
purchasing_by_age_df["Normalized Total"] = purchasing_by_age_df["Normalized Total"].map("${:.2f}".format)


purchasing_by_age_df
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
      <th>Average purchase price</th>
      <th>Normalized Total</th>
      <th>Purchase count</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>Age group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>$2.98</td>
      <td>$4.39</td>
      <td>28</td>
      <td>$83.46</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>$2.77</td>
      <td>$4.22</td>
      <td>35</td>
      <td>$96.95</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>$2.91</td>
      <td>$3.86</td>
      <td>133</td>
      <td>$386.42</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>$2.91</td>
      <td>$3.78</td>
      <td>336</td>
      <td>$978.77</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>$2.96</td>
      <td>$4.26</td>
      <td>125</td>
      <td>$370.33</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>$3.08</td>
      <td>$4.20</td>
      <td>64</td>
      <td>$197.25</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>$2.84</td>
      <td>$4.42</td>
      <td>42</td>
      <td>$119.40</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>$3.16</td>
      <td>$4.89</td>
      <td>17</td>
      <td>$53.75</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
# In order to find top spenders, group by screen name
spending_grouped = pymoli_df.groupby("SN")

purchase_count = spending_grouped["Age"].count()
avg_purchase = spending_grouped["Price"].mean()
total_purchase_value = spending_grouped["Price"].sum()

top_purchasers_df = pd.DataFrame({"Purchase count": purchase_count,
                                       "Average purchase price": avg_purchase,
                                       "Total purchase value": total_purchase_value})

# Sort the data frame by "Total Purchase Value" descending
top_purchasers_df = top_purchasers_df.sort_values("Total purchase value", ascending = False)

# Format the money columns to appear as money
# Note: we must do this AFTER sorting because special characters mess with the sorting
top_purchasers_df["Average purchase price"] = top_purchasers_df["Average purchase price"].map("${:.2f}".format)
top_purchasers_df["Total purchase value"] = top_purchasers_df["Total purchase value"].map("${:.2f}".format)

# Select the top 5 rows of the sorted data frame
top_purchasers_df.iloc[0:5,:]
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
      <th>Average purchase price</th>
      <th>Purchase count</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>$3.41</td>
      <td>5</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$3.39</td>
      <td>4</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$3.18</td>
      <td>4</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$4.24</td>
      <td>3</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$3.86</td>
      <td>3</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
# Group by Item ID and Item Name
items_grouped = pymoli_df.groupby(["Item ID", "Item Name"])

purchase_count = items_grouped["Age"].count()
total_purchase_value = items_grouped["Price"].sum()
item_price = items_grouped["Price"].max() # this returns the item price because the max per each item = the min

top_items_df = pd.DataFrame({"Purchase count": purchase_count,
                                       "Item price": item_price,
                                       "Total purchase value": total_purchase_value})

# Sort the data frame by "Purchase count" descending
top_items_df = top_items_df.sort_values("Purchase count", ascending = False)

# Format the money columns to appear as money
top_items_df["Total purchase value"] = top_items_df["Total purchase value"].map("${:.2f}".format)
top_items_df["Item price"] = top_items_df["Item price"].map("${:.2f}".format)

# Select the top 5 rows of the sorted data frame
top_items_df.iloc[0:5,:]
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
      <th></th>
      <th>Item price</th>
      <th>Purchase count</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>$1.24</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
# We can use the same data structures from the most popular items, we just order differently
# We want to re-initialize the data frame in order to undo the formatting
top_items_df = pd.DataFrame({"Purchase count": purchase_count,
                                       "Item price": item_price,
                                       "Total purchase value": total_purchase_value})

# Sort the data frame by "Total Purchase Value" descending
top_items_df = top_items_df.sort_values("Total purchase value", ascending = False)

# Format the money columns to appear as money
# Note: we must do this AFTER sorting because special characters mess with the sorting
top_items_df["Total purchase value"] = top_items_df["Total purchase value"].map("${:.2f}".format)
top_items_df["Item price"] = top_items_df["Item price"].map("${:.2f}".format)

# Select the top 5 rows of the sorted data frame
top_items_df.iloc[0:5,:]
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
      <th></th>
      <th>Item price</th>
      <th>Purchase count</th>
      <th>Total purchase value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


