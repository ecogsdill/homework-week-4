

```python
#dependencies
import pandas as pd
```


```python
#read CSV
json_path='purchase_data.json'

df = pd.read_json(json_path)
df.head()
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




```python
#**Player Count**

#Total Number of Players
total_players=len(df["SN"].value_counts())
print(f"Total players: {total_players}")

```

    Total players: 573



```python
#**Purchasing Analysis

#Number of Unique Items
unique_items_count=len(df['Item ID'].value_counts())

#Average Purchase Price
avg_purchase_price=df["Price"].mean()

# Total Number of Purchases
total_purchases=df["Price"].count()

# Total Revenue
total_revenue=df["Price"].sum()

print(f"Total Unique Items: {unique_items_count}")
print(f"Avg. Purchase Price: ${round(avg_purchase_price,2)}")
print(f"Total Purchases: {total_purchases}")
print(f"Total Revenue: ${round(total_revenue,2)}")
```

    Total Unique Items: 183
    Avg. Purchase Price: $2.93
    Total Purchases: 780
    Total Revenue: $2286.33



```python
#**Gender Demographics**

#* Percentage and Count of Male Players
#* Percentage and Count of Female Players
#* Percentage and Count of Other / Non-Disclosed

# Calculate percentage of respondents belonging to each gender



total_males=len(df.loc[df["Gender"]=="Male",]["SN"].value_counts())
total_females=len(df.loc[df["Gender"]=="Female",]["SN"].value_counts())
total_other = total_players - total_males - total_females

pct_male = round(total_males/total_players*100,2)
pct_female = round(total_females/total_players*100,2)
pct_other = round(total_other/total_players*100,2)
#pct_no_gender = round(total_no_gender/total_gender*100,2)

print(f"Males:               {total_males} ({pct_male}%)")
print(f"Females:             {total_females} ({pct_female}%)")
print(f"Other/Non-Disclosed: {total_other}   ({pct_other}%)")
#print(f"% No Response: {pct_no_gender}%")
```

    Males:               465 (81.15%)
    Females:             100 (17.45%)
    Other/Non-Disclosed: 8   (1.4%)



```python
#**Purchasing Analysis (Gender)** 

#* The below each broken by gender
 # * Purchase Count
 # * Average Purchase Price
 # * Total Purchase Value
 # * Normalized Totals

 # * Purchase Count
gender_group=df.groupby("Gender")
purchase_count_by_gender=pd.DataFrame(gender_group["Price"].count())
purchase_count_by_gender=purchase_count_by_gender.rename(columns={"Price":"Purchase Count"}).reset_index()
#purchase_count_by_gender

 # * Average Purchase Price
avg_purchase_price_by_gender=pd.DataFrame(gender_group["Price"].mean())
avg_purchase_price_by_gender=avg_purchase_price_by_gender.rename(columns={"Price":"Avg. Purchase Price"}).reset_index()
#avg_purchase_price_by_gender    

 # * Total Purchase Value
total_purchase_value_by_gender=pd.DataFrame(gender_group["Price"].sum())
total_purchase_value_by_gender=total_purchase_value_by_gender.rename(columns={"Price":"Total Purchase Value"}).reset_index()
#total_purchase_value_by_gender 

#summarize all stats by gender

gender_stats = pd.DataFrame({"Gender": ["Female","Male","Other / Non-Disclosed"],
              "Total Users": [total_females, total_males, total_other]})

gender_stats=gender_stats.merge(total_purchase_value_by_gender, on="Gender")

#Normalized Totals
gender_stats["Normalized Totals"]=gender_stats["Total Purchase Value"]/gender_stats["Total Users"]
gender_stats=gender_stats.merge(purchase_count_by_gender, on="Gender")
gender_stats=gender_stats.merge(avg_purchase_price_by_gender, on="Gender")
gender_stats=gender_stats[["Gender","Purchase Count","Avg. Purchase Price","Total Purchase Value","Normalized Totals"]]
gender_stats
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
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Avg. Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>136</td>
      <td>2.815515</td>
      <td>382.91</td>
      <td>3.829100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>633</td>
      <td>2.950521</td>
      <td>1867.68</td>
      <td>4.016516</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>11</td>
      <td>3.249091</td>
      <td>35.74</td>
      <td>4.467500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Top Spenders**

#* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
#  * SN
#  * Purchase Count
#  * Average Purchase Price
# * Total Purchase Value

groupby_user = df.groupby("SN")

#top 5 spenders
groupby_user_sorted = df.groupby("SN").sum().sort_values("Price", ascending=False)
top_5_spenders=groupby_user_sorted.iloc[0:5,:].reset_index()[["SN"]]

#add Purchase Counts per user
purchase_counts_per_user=pd.DataFrame(groupby_user["Price"].count())
purchase_counts_per_user=purchase_counts_per_user.reset_index()
top_5_spenders=top_5_spenders.merge(purchase_counts_per_user, on="SN").rename(columns={"Price":"Purchase Count"})

#add Avg Purchase Price per user
avg_purchase_price_per_user=pd.DataFrame(groupby_user["Price"].mean())
avg_purchase_price_per_user=avg_purchase_price_per_user.reset_index()
top_5_spenders=top_5_spenders.merge(avg_purchase_price_per_user, on="SN").rename(columns={"Price":"Avg Purchase Price"})

#add Total Purchase Value per user
total_purchase_value_per_user=pd.DataFrame(groupby_user["Price"].sum())
total_purchase_value_per_user=total_purchase_value_per_user.reset_index()
top_5_spenders=top_5_spenders.merge(total_purchase_value_per_user, on="SN").rename(columns={"Price":"Total Purchase Value"})

#display summary table
top_5_spenders


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
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Avg Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>5</td>
      <td>3.412000</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Saedue76</td>
      <td>4</td>
      <td>3.390000</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mindimnya67</td>
      <td>4</td>
      <td>3.185000</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Haellysu29</td>
      <td>3</td>
      <td>4.243333</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eoda93</td>
      <td>3</td>
      <td>3.860000</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Most Popular Items**

#* Identify the 5 most popular items by purchase count, then list (in a table):
#  * Item ID
#  * Item Name
#  * Purchase Count
#  * Item Price
#  * Total Purchase Value

groupby_item = df.groupby("Item ID")

#top 5 items by purchase count
groupby_item_sorted = df.groupby("Item ID").count().sort_values("Price", ascending=False)
top_5_items=groupby_item_sorted.iloc[0:5,:].reset_index()[["Item ID"]]

#add item names
top_5_items=pd.DataFrame(top_5_items.merge(df[["Item ID","Item Name"]].drop_duplicates(), on="Item ID"))

#add Purchase Counts per item
purchase_counts_per_item=pd.DataFrame(groupby_item["Price"].count())
purchase_counts_per_item=purchase_counts_per_item.reset_index()
top_5_items=top_5_items.merge(purchase_counts_per_item, on="Item ID").rename(columns={"Price":"Purchase Count"})

#add Avg Purchase Price per item
avg_purchase_price_per_item=pd.DataFrame(groupby_item["Price"].mean())
avg_purchase_price_per_item=avg_purchase_price_per_item.reset_index()
top_5_items=top_5_items.merge(avg_purchase_price_per_item, on="Item ID").rename(columns={"Price":"Avg Purchase Price"})

#add Total Purchase Value per item
total_purchase_value_per_item=pd.DataFrame(groupby_item["Price"].sum())
total_purchase_value_per_item=total_purchase_value_per_item.reset_index()
top_5_items=top_5_items.merge(total_purchase_value_per_item, on="Item ID").rename(columns={"Price":"Total Purchase Value"})

#display summary table
top_5_items
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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Avg Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31</td>
      <td>Trickster</td>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>3</th>
      <td>175</td>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>Serenity</td>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Most Profitable Items**

#* Identify the 5 most profitable items by total purchase value, then list (in a table):
#  * Item ID
#  * Item Name
#  * Purchase Count
#  * Item Price
#  * Total Purchase Value


#top 5 items by total purchase value
groupby_item_total_value_sorted = df.groupby("Item ID").sum().sort_values("Price", ascending=False)
top_5_items_total_value=groupby_item_total_value_sorted.iloc[0:5,:].reset_index()[["Item ID"]]

#add item names
top_5_items_total_value=pd.DataFrame(top_5_items_total_value.merge(df[["Item ID","Item Name"]].drop_duplicates(), on="Item ID"))

#add Purchase Counts per item
top_5_items_total_value=top_5_items_total_value.merge(purchase_counts_per_item, on="Item ID").rename(columns={"Price":"Purchase Count"})

#add Avg Purchase Price per item
top_5_items_total_value=top_5_items_total_value.merge(avg_purchase_price_per_item, on="Item ID").rename(columns={"Price":"Avg Purchase Price"})

#add Total Purchase Value per item
top_5_items_total_value=top_5_items_total_value.merge(total_purchase_value_per_item, on="Item ID").rename(columns={"Price":"Total Purchase Value"})

#display summary table
top_5_items

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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Avg Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>3</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>






    '\n#display summary table\ntop_5_items'




```python
#* You must include a written description of three observable trends based on the data. 


#1.) There are both a lot more males than females who make purchases in this game 
#(although we don't know what percentage they represent of all PLAYERS in the game, assuming it is free-to-play)
```
