# Stock-Analysis (using Python)

## Python Script

import pandas as pd

import numpy as np

import yfinance as yf

import matplotlib.pyplot as plt

import seaborn as sns

from datetime import datetime

### download data from yfinance

data = yf.download (['AAPL', 'GOOG', 'MSFT'], start='2010-1-1', end='2024-4-30', group_by='ticker')

### EDA

print(data.head(10)) #show top 10 of dataset (by default - 5)

data.tail() #show end of dataset (by default - 5)

data.describe() # descriptive information about the dataset

data.info() # gives information on data types, non-null values and memory usage

data.shape

data.dtypes

data.isna()

data.duplicated().sum() # check for duplicate rows

data.isnull().sum()

### Plot each company's 'Volume' price

fig, ax = plt.subplots(figsize=(10, 6))

Plotting for MSFT
data['MSFT']['Volume'].plot(label='MSFT', ax=ax)

Plotting for AAPL
data['AAPL']['Volume'].plot(label='AAPL', ax=ax)

Plotting for GOOG
data['GOOG']['Volume'].plot(label='GOOG', ax=ax)

Add labels and title
plt.xlabel('Date')
plt.ylabel('Volume')
plt.title('Stock Volume for Apple, Google, and Microsoft')
Add a legend
plt.legend()
Show the plot
plt.show()

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/7c9e7c6b-f4d8-45c1-aeae-7d9187f0d364)


### Plot each company's 'Adj Close' price

fig, ax = plt.subplots(figsize=(10, 6))

Plotting for MSFT
data['MSFT']['Adj Close'].plot(label='MSFT', ax=ax)

Plotting for AAPL
data['AAPL']['Adj Close'].plot(label='AAPL', ax=ax)

Plotting for GOOG
data['GOOG']['Adj Close'].plot(label='GOOG', ax=ax)

Add labels and title
plt.xlabel('Date')
plt.ylabel('Adjusted Close Price')
plt.title('Adjusted Closing Prices for Apple, Google, and Microsoft')

Add a legend
plt.legend()

Show the plot
plt.show()

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/e52b4493-4937-4462-800f-55ccd3c66000)

### Moving Average for each company's 'Adj Close' price

Define the window periods

window_periods = [10, 20, 50]

Calculate moving averages for each company and window period

moving_averages = {}
for ticker in data.columns.levels[0]:
    ticker_data = data[ticker]
    moving_averages[ticker] = {}
    for column in ['Adj Close']:
        for period in window_periods:
            moving_averages[ticker][f'{column}_MA_{period}'] = ticker_data[column].rolling(window=period).mean()

Plot moving averages for each company separately for Adj Close

for ticker, ma_data in moving_averages.items():
    for column in ['Adj Close']:
        plt.figure(figsize=(10, 6))
        plt.title(f'{ticker} - {column} Moving Averages')
        Plot Close
        for period in window_periods:
            ma_column = ma_data[f'{column}_MA_{period}']
            plt.plot(ma_column, label=f'{column} MA {period}')
            
        Plot original data (light gray)
        plt.plot(data[ticker][column], label=column, color='lightgray')
        plt.legend()
        plt.grid(True)
        plt.xlabel('Date')
        plt.ylabel('Value')
        plt.tight_layout()
        plt.show()

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/c076e991-81ab-4828-8793-ffdb15d2dc49)

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/0d5f2ddd-8ed1-4272-a259-a0ed9bdae94f)

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/f5213ef3-759c-4dc3-887b-26f17d8cabbb)


### Daily Returns for each company's 'Adj Close' price

Calculate daily returns for 'Adj Close' for each ticker
returns_df = pd.DataFrame()

for ticker in data.columns.levels[0]:
    for column in ['Adj Close']:
        returns_df[(ticker, column + ' Daily Return')] = data[ticker][column].pct_change()

Drop NaN values resulting from the calculation
returns_df = returns_df.dropna()

Display the resulting DataFrame
print(returns_df)

Plot Returns

for column in returns_df.columns:
    plt.figure(figsize=(10, 6))
    plt.plot(returns_df.index, returns_df[column])
    plt.title(f'{column}')
    plt.xlabel('Date')
    plt.ylabel('Daily Return')
    plt.grid(False)
    plt.show()

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/4d569bb6-7f25-4853-82df-66ff61cb7757)

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/e70ba350-fa13-4750-8f16-ad97dd1e8677)

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/f4ea637e-ac57-4a15-989f-01bede1cf79e)


### Check for Correlation

Calculate correlation matrix

correlation_matrix = returns_df.corr()

Plot heatmap

plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', fmt=".2f", linewidths=0.5)
plt.title('Correlation Heatmap of Daily Returns')
plt.show()
​
![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/6c3afae6-e6ab-446b-93a9-c6828cb0452b)


### Volatility of each company

Calculate volatility (standard deviation of returns)

volatility = returns_df.std()

Plot daily volatility

for ticker in volatility.index:
    plt.figure(figsize=(10, 6))
    plt.plot(returns_df.index, returns_df[ticker], label=ticker)
    plt.title(f'Daily Volatility for {ticker}')
    plt.xlabel('Date')
    plt.ylabel('Volatility (Standard Deviation)')
    plt.legend()
    plt.grid(False)
    plt.show()

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/2484feef-2388-4b4f-a981-fcb4745369ce)

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/8b860559-7dd9-4298-bf81-42156cfcd48f)

![image](https://github.com/mel4data/Stock-Analysis/assets/170362474/85cc4420-3ef8-45b5-9958-1e4fde219503)



