```python
 #  A Whale off the Port(folio)
 ---

 In this assignment, you'll get to use what you've learned this week to evaluate the performance among various algorithmic, hedge, and mutual fund portfolios and compare them against the S&P 500 Index.
```

# Initial imports
import pandas as pd
import numpy as np
import datetime as dt
from pathlib import Path

#!pip install quandl
import quandl

%matplotlib inline

# Data Cleaning

In this section, you will need to read the CSV files into DataFrames and perform any necessary data cleaning steps. After cleaning, combine all DataFrames into a single DataFrame.

Files:

* `whale_returns.csv`: Contains returns of some famous "whale" investors' portfolios.

* `algo_returns.csv`: Contains returns from the in-house trading algorithms from Harold's company.

* `sp500_history.csv`: Contains historical closing prices of the S&P 500 Index.

## Whale Returns

Read the Whale Portfolio daily returns and clean the data

#reading Whale Returns from
whale_returns = Path("Resources/whale_returns.csv")
whale_returns_data = pd.read_csv(whale_returns)
whale_returns_data.head()


# Count nulls
whale_returns_data.isnull()


```python
# Drop nulls
whale_returns_data.isnull().sum()

```


```python
## Algorithmic Daily Returns

Read the algorithmic daily returns and clean the data
```

# Reading algorithmic returns
#Section 3.3 Returns/Refreshers
algo_returns = Path("Resources/algo_returns.csv")
algo_returns = pd.read_csv(algo_returns)
algo_returns.head()

algo_returns.dtypes

# Count nulls
algo_returns.isnull()

# Drop nulls
algo_returns.isnull().sum()

## S&P 500 Returns

Read the S&P 500 historic closing prices and create a new daily returns DataFrame from the data. 

# Reading S&P 500 Closing Prices
sp500_history = Path("./resources/sp500_history.csv")
sp500_history = pd.read_csv(sp500_history)
sp500_history.head()

sp500_history.dtypes

# Check Data Types
sp500_history.count()

sp500_history.isnull()

sp500_history.isnull().sum()

# Fix Data Types
sp500_history.isnull().sum() / len(sp500_history) * 100

sp500_history

#Tutor Recomendation; research dropping dollar sign and doing percentage on the closing price.


# Calculate Daily Returns
sp500_history = sp500_history.pct_change()
sp500_history_returns

# Drop nulls
sp500_history.isnull().sum()

# Rename `Close` Column to be specific to this portfolio.
# Process noted in 4.2 "Portfolio Returns"

sp500_history.Close = ['Daily Returns']
sp500_history.head()



## Combine Whale, Algorithmic, and S&P 500 Returns

# Join Whale Returns, Algorithmic Returns, and the S&P 500 Returns into a single DataFrame with columns for each portfolio's returns.
# Using UNit 4.2 Concat 01 as my reference for this portion. 

column_appended_data = pd.concat([algo_returns, sp500_history, whale_returns_data], axis="columns", join="inner")
column_appended_data



---

# Conduct Quantitative Analysis

In this section, you will calculate and visualize performance and risk metrics for the portfolios.

## Performance Anlysis

#### Calculate and Plot the daily returns.

# Plot daily returns of all portfolios
# I am using unit 4.2 #13 Portfolio Returns for this section.

daily_returns = (sp500_history - sp500_history.shift(1)) / sp500_history.shift(1)
daily_returns.head()

daily_returns = sp500_history.csv.pct_change()
daily_returns.head()

daily_returns.plot(figsize=(10,5))




# Plot daily returns of all portfolios
# I am using unit 4.2 #13 Portfolio Returns for this section.

daily_returns = (algo_returns - algo_returns.shift(1)) / algo_returns.shift(1)
daily_returns.head()

daily_returns = algo_returns_history.csv.pct_change()
daily_returns.head()

daily_returns.plot(figsize=(10,5))





# Plot daily returns of all portfolios
# I am using unit 4.2 #13 Portfolio Returns for this section.

daily_returns = (whale_returns_data - whale_returns_data.shift(1)) / whale_returns_data.shift(1)
daily_returns.head()

daily_returns = whale_returns_history.csv.pct_change()
daily_returns.head()

daily_returns.plot(figsize=(10,5))





#### Calculate and Plot cumulative returns.

# Calculate cumulative returns of all portfolios

daily_returns = (whale_returns_data - whale_returns_data.shift(1)) / whale_returns_data.shift(1)
daily_returns.head()

cumulative_returns = (1 + daily_returns).cumprod()
cumulative_returns.head()
# Plot cumulative returns
cumulative_returns.plot(figsize=(10,5))

---

## Risk Analysis

Determine the _risk_ of each portfolio:

1. Create a box plot for each portfolio. 
2. Calculate the standard deviation for all portfolios
4. Determine which portfolios are riskier than the S&P 500
5. Calculate the Annualized Standard Deviation

### Create a box plot for each portfolio


# Box plot to visually show risk
# Using 4.2 #14 as my example.

whale_returns_data.plot()
algo_returns.plot()
sp500_history.plot()



### Calculate Standard Deviations

# Calculate the daily standard deviations of all portfolios
# I am using notes from section 3.3 #15

whale_returns = np.random.normal(scale=0.5, size=10000)
algo_returns = np.random.normal(scale=1.0, size=10000)
sp500_history = np.random.normal(scale=1.5, size=10000)

portfolio_std = pd.DataFrame({
    "0.5": whale_returns,
    "1.0": algo_returns,
    "1.5": sp500_history
})

portfolio_std.plot.hist(stacked=True, bins=100)



### Determine which portfolios are riskier than the S&P 500

# Calculate  the daily standard deviation of S&P 500
daily_std = sp500_history.std()
daily_std.head()

# Determine which portfolios are riskier than the S&P 500
daily_std = whale_returns.std()
daily_std.head()

daily_std = algo_returns.std()
daily_std.head()

### Calculate the Annualized Standard Deviation

# Calculate the annualized standard deviation (252 trading days)

annualized_std = sp500_history * np.sqrt(252)
annualized_std.head()

annualized_std = whale_returns * np.sqrt(252)
annualized_std.head()

annualized_std = algo_returns * np.sqrt(252)
annualized_std.head()

---

## Rolling Statistics

Risk changes over time. Analyze the rolling statistics for Risk and Beta. 

1. Calculate and plot the rolling standard deviation for all portfolios using a 21-day window
2. Calculate the correlation between each stock to determine which portfolios may mimick the S&P 500
3. Choose one portfolio, then calculate and plot the 60-day rolling beta between it and the S&P 500

### Calculate and plot rolling `std` for all portfolios with 21-day window

# Calculate the rolling standard deviation for all portfolios using a 21-day window
# Using 4.2 #9 as my example.  

# Plot the rolling standard deviation
whale_returns.rolling(window=21).std().plot()
algo_returns.rolling(window=21).std().plot()
sp500_history.rolling(window=21).std().plot()


### Calculate and plot the correlation

# Calculate the correlation
# I was unable to finish this portion.
# Display de correlation matrix
 

### Calculate and Plot Beta for a chosen portfolio and the S&P 500

# Calculate covariance of a single portfolio
covariance = daily_returns['whale_returns'].cov(daily_returns['SP500'])
covariance

# Calculate variance of S&P 500

variance = daily_returns['SP500'].var()
variance

# Computing beta
whale_returns = covariance / variance
whale_returns_beta

# Plot beta trend
rolling_beta = rolling_covariance / rolling_variance
rolling_beta.plot(figsize=(20, 10), title='Rolling 30-Day Beta of Whale Returns')


## Rolling Statistics Challenge: Exponentially Weighted Average 

An alternative way to calculate a rolling window is to take the exponentially weighted moving average. This is like a moving window average, but it assigns greater importance to more recent observations. Try calculating the [`ewm`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.ewm.html) with a 21-day half-life.

# Use `ewm` to calculate the rolling window
# I was unable to finish this portion.

---

# Sharpe Ratios
In reality, investment managers and thier institutional investors look at the ratio of return-to-risk, and not just returns alone. After all, if you could invest in one of two portfolios, and each offered the same 10% return, yet one offered lower risk, you'd take that one, right?

### Using the daily returns, calculate and visualize the Sharpe ratios using a bar plot

# Visualize the sharpe ratios as a bar plot
sharpe_ratios.plot(kind="bar", title="Sharpe Ratios")

### Determine whether the algorithmic strategies outperform both the market (S&P 500) and the whales portfolios.

At present I am unable to determinermine as I have not finished debugging my code in the alloted time. 

---

# Create Custom Portfolio

In this section, you will build your own portfolio of stocks, calculate the returns, and compare the results to the Whale Portfolios and the S&P 500. 

1. Choose 3-5 custom stocks with at last 1 year's worth of historic prices and create a DataFrame of the closing prices and dates for each stock.
2. Calculate the weighted returns for the portfolio assuming an equal number of shares for each stock
3. Join your portfolio returns to the DataFrame that contains all of the portfolio returns
4. Re-run the performance and risk analysis with your portfolio to see how it compares to the others
5. Include correlation analysis to determine which stocks (if any) are correlated

## Choose 3-5 custom stocks with at last 1 year's worth of historic prices and create a DataFrame of the closing prices and dates for each stock.

# Reading data from 1st stock

#This process was discussed on Pandas lecture: Video Time Stamp 02:20 
import pandas as pd
from pathlib import Path

csv_path = Path("../resources/Ford.csv")
csv_path = Path("../resources/GM.csv")
csv_path = Path("../resources/Toyota.csv")

Ford = pd.read_csv(csv_path, index_col='Date'), parse_dates=True, infer_datetime_format=True)
Ford_data.head()



#Download the data as CSV files and calculate the portfolio returns.
#Slice Data
Ford_slice = Ford_Data.loc(#Enter time frames here)
Ford_monthly_returns = Ford_slice.pct_change()

GM_slice = GM_Data.loc(#Enter time frames here)
GM_monthly_returns = GM_slice.pct_change()



# Reading data from 2nd stock
#Download the data as CSV files and calculate the portfolio returns.
#Slice Data
GM = pd.read_csv(csv_path, index_col='Date'), parse_dates=True, infer_datetime_format=True)
GM_data.head()

GM_slice = Toyota_Data.loc(#Enter time frames here)
GM_monthly_returns = Toyota_slice.pct_change()


# Reading data from 3rd stock
import pandas as pd
from pathlib import Path

csv_path = Path(Toyota)


#Download the data as CSV files and calculate the portfolio returns.
#Slice Data
Toyota = pd.read_csv(csv_path, index_col='Date'), parse_dates=True, infer_datetime_format=True)
Toyota_data.head()

Toyota_slice = Toyota_Data.loc(#Enter time frames here)
Toyota_monthly_returns = Toyota_slice.pct_change()


# Combine all stocks in a single DataFrame

Ford_data_path = Path('../Resources/Ford.csv')
GM_data_path = Path('../Resources/GM.csv')
Toyota_data_path = Path('../Resources/Toyota_products.csv')

joined_data_rows = pd.concat([Toyota_data_path, GM_data_path, Ford_data_path], axis="rows", join="inner")
joined_data_rows()

# Reset Date index
# I was unable to finish this portion.

# Reorganize portfolio data by having a column per symbol
# I was unable to finish this portion.

# Calculate daily returns
daily_returns = combined_df.pct_change()
daily_returns.head()

# Drop NAs
Ford_data_path_df.drop(columns=['NA: 0'], inplace=True)
Ford_data_path_df.head()

GM_df.drop(columns=['NA: 0'], inplace=True)
GM_df.head()

Toyota_df.drop(columns=['NA: 0'], inplace=True)
Toyota_df.head()

# Display sample data
products_data.head()

## Calculate the weighted returns for the portfolio assuming an equal number of shares for each stock

# Set weights
weights = [1/3, 1/3, 1/3]

# Calculate portfolio return
portfolio_returns = daily_returns.dot(weights)
portfolio_returns.head()

# Display sample data
portfolio_returns.plot()

## Join your portfolio returns to the DataFrame that contains all of the portfolio returns

# Join your returns DataFrame to the original returns DataFrame
# I was unable to finish this portion. 

# Only compare dates where return data exists for all the stocks (drop NaNs)
# I was unable to finish this portion.

## Re-run the risk analysis with your portfolio to see how it compares to the others

### Calculate the Annualized Standard Deviation

# Calculate the annualized `std`
annualized_std = daily_std * np.sqrt(252)
annualized_std.head()

### Calculate and plot rolling `std` with 21-day window

### Calculate and plot the correlation


```python
# Calculate and plot the correlation
# I was unable to finish this portion.
```

### Calculate and Plot Rolling 60-day Beta for Your Portfolio compared to the S&P 500

# Calculate and plot Beta
# I was unable to finish this portion. 

### Using the daily returns, calculate and visualize the Sharpe ratios using a bar plot

# Calculate Annualized Sharpe Ratios

all_portfolios_returns = pd.concat([whale_returns, algo_returns, sp500_history], axis='columns', join='inner')
all_portfolios_returns.head()

sharpe_ratios = ((all_portfolios_returns.mean()-all_portfolios_returns['rf_rate'].mean()) * 252) / (all_portfolios_returns.std() * np.sqrt(252))
sharpe_ratios




# Visualize the sharpe ratios as a bar plot
sharpe_ratios.plot(kind="bar", title="Sharpe Ratios")

### How does your portfolio do?

Write your answer here!

At present I am unable to determine this answer. 


```python

```
