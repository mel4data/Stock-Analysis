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

### Plot each ticker's 'Volume' price

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


### Plot each ticker's 'Adj Close' price

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

### Moving Average for each ticker's 'Adj Close' price

Define the window periods
window_periods = [10, 20, 50]

Calculate moving averages for each ticker and window period
moving_averages = {}
for ticker in data.columns.levels[0]:
    ticker_data = data[ticker]
    moving_averages[ticker] = {}
    for column in ['Adj Close']:
        for period in window_periods:
            moving_averages[ticker][f'{column}_MA_{period}'] = ticker_data[column].rolling(window=period).mean()

Plot moving averages for each ticker separately for Adj Close
for ticker, ma_data in moving_averages.items():
    for column in ['Adj Close']:
        plt.figure(figsize=(10, 6))
        plt.title(f'{ticker} - {column} Moving Averages')
        # Plot Close
        for period in window_periods:
            ma_column = ma_data[f'{column}_MA_{period}']
            plt.plot(ma_column, label=f'{column} MA {period}')
        # Plot original data (light gray)
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




​


