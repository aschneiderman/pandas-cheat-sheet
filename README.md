## Quick and Dirty Pandas Cheat Sheet
NOTE: right now it's just a notes dump; need to clean it up 



# Pandas Sample Code

```python
import pandas as pd

# Reading in a file
airports = pd.read_csv('data/airports.csv')
airports.info()  	# NOTE: any object field is essentially a str
```

```

# -----------  Replacing data ---------------------
# NOTE: they recommend using method chaining, but I think this old school approach is easier to read (and my datasets are small)

# For cleaning up a CSV file:

# Reminder:  numbers won't have been converted; you need to do it yourself

# If a number column has a comma or dollar sign in it
checking.fillna(0, inplace=True)
checking['Debit'] = checking['Debit'].str.replace(',', '').astype(float)
checking['Credit'] = checking['Credit'].replace('[\$,]', '', regex=True).astype(float)

# If one or more columns contain a date:
# (NOTE: If the file is loading slowly, Google for faster methods)
checking =  pd.read_csv(checking_file, parse_dates=['Date'])

# Convert all/part of a date to a string
all_data['Order Day new'] = all_data['Order Day new'].dt.strftime('%Y-%m-%d')

df.loc[(df['COUNCIL'] == 'LEEDS') & (df['POSTCODE'] == 'LS8'), ['CCG']] = 'LEEDS NORTH CCG'

tips.loc[tips['tip'] < 2, 'tip'] *= 2

For date format, see
https://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior


##### Merging data ######

result = pd.merge(left, right, on='key')
# needs to be in both

# for adding a category rollup
data = pd.merge(mint, rollup_categories, on=“category”, how=“right”)


# If you don’t want a pivot table just yet, you just want to aggregate/rollup the data:
groupby('Category').size()

# if you want to group by more than one column:
g1 = df1.groupby( [ "Name", "City"]).size().reset_index(name="count")

# Explaination
# size counts NaN values, count does not
# Why reset_index? Because if you group by more than one column, you get a dataframe
# with a “hierarchical index”, which is kinda like a pivot table — not what you wanted

# To aggregate by adding up:
df.groupby(['Fruit','Supplier'])[‘Quantity'].sum(). reset_index(name="total")

# To do: check out using a hierarchical index or flattened table and styling it as a better looking pivot table
https://pandas.pydata.org/pandas-docs/stable/style.html



########### Misc Code ##########
(harvest for more exs of how to do things)
games = (games.rename(columns=column_names)
.dropna(thresh=4)
[['date', 'away_team', 'away_points', 'home_team', 'home_points']]
.assign(date=lambda x: pd.to_datetime(x['date'], format='%a, %b %d, %Y'))
.set_index('date', append=True)
.rename_axis(["game_id", "date"])
.sort_index())
games.head()

[
](fp):
df = (pd.read_csv(fp)
.rename(columns=str.lower)
.drop('unnamed: 36', axis=1)
.pipe(extract_city_name)
.pipe(time_to_datetime, ['dep_time', 'arr_time', 'crs_arr_time', 'crs_dep_time'])
.assign(fl_date=lambda x: pd.to_datetime(x['fl_date']),
dest=lambda x: pd.Categorical(x['dest']),
origin=lambda x: pd.Categorical(x['origin']),
tail_num=lambda x: pd.Categorical(x['tail_num']),
unique_carrier=lambda x: pd.Categorical(x['unique_carrier']),
cancellation_code=lambda x: pd.Categorical(x['cancellation_code'])))
return df)

# A 2 Line if-then else with assignment to 2 columns
f.loc[df.AAA >= 5,['BBB','CCC']] = 555;
df.loc[df.AAA < 5,['BBB','CCC']] = 2000

airports.loc[airports['ident'] == 'KLAX', 'home_link'] = 'http://www.lawa.org/welcomelax.aspx'
lax_freq.drop(lax_freq[lax_freq.type == 'MISC'].index)

Sometimes, especially when you’re reading in a CSV with a bunch of numbers, some of the numbers will read in as strings instead of numeric values, or vice versa. Here’s a way you can fix that and normalize your data types:

data = pd.read_csv(‘movie_metadata.csv’, dtype={‘duration’: int})

# rename a column
data = data.rename(columns = {‘title_year’:’release_date’, ‘movie_facebook_likes’:’facebook_likes’})

requests['City'].str.upper().value_counts()

checking['Status'] = checking['Status'].str.upper()

#Similarly, to get rid of trailing whitespace:
data[‘movie_title’].str.strip()

# This tells Pandas that the column ‘duration’ needs to be an integer value. Similarly, if we want the release year to be a string and not a number, we can do the same kind of thing:
data = pd.read_csv(‘movie_metadata.csv’, dtype={title_year: str})

df['total_cost'] = df['price'] * df['quantity']

airports[airports.ident == 'KLAX'].id
airports.type.unique()
airports[(airports.iso_region == 'US-CA') & (airports.type == 'large_airport')][['ident', 'name', 'municipality']]
df2[df2['E'].isin(['two','four'])]
# pivot table
pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C'])

airports.groupby(['iso_country', 'type']).size()
airports.groupby(['iso_country', 'type']).size().to_frame('size').reset_index().sort_values(['iso_country', 'size'], ascending=[True, False])



# For more info:
# http://pandas.pydata.org/Pandas_Cheat_Sheet.pdf

```