# programming-project
pip install yfinance
import pandas as pd

# URL for the confirmed cases data
confirmed_cases_url = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv"

# Read the data into a DataFrame
confirmed_cases_df = pd.read_csv(confirmed_cases_url)

# URL for the deaths data
deaths_url = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv"

# Read the data into a DataFrame
deaths_df = pd.read_csv(deaths_url)

# URLs for the confirmed cases and deaths data
deaths_url = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv"

# Read the data into dataframes
deaths_df = pd.read_csv(deaths_url)

# Sum the confirmed cases and deaths globally
global_confirmed_cases = confirmed_cases_df.iloc[:, -1].sum()
global_deaths = deaths_df.iloc[:, -1].sum()

# Create a dataframe with the global totals
global_totals_df = pd.DataFrame({
    "Confirmed Cases": [global_confirmed_cases],
    "Deaths": [global_deaths]
})

# Print the dataframe
print(global_totals_df)
import yfinance as yf

# Overall American Market: S&P 500
spy = yf.Ticker("SPY")

# Overall Canadian Market: S&P/TSX Composite Index
xic = yf.Ticker("^GSPTSE")

# Travel sector: Delta Air Lines
dal = yf.Ticker("DAL")

# Real Estate sector: Simon Property Group
spg = yf.Ticker("SPG")

# Precious metals: Barrick Gold Corporation
gold = yf.Ticker("GOLD")

# Print some info about each stock
print("Overall American Market: S&P 500")
print(spy.info["longName"])
print()

print("Overall Canadian Market: S&P/TSX Composite Index")
print(xic.info["longName"])
print()

print("Travel sector: Delta Air Lines")
print(dal.info["longName"])
print()

print("Real Estate sector: Simon Property Group")
print(spg.info["longName"])
print()

print("Precious metals: Barrick Gold Corporation")
print(gold.info["longName"])
print()

import requests

# API endpoint and API key
endpoint = "https://www.alphavantage.co/query"
api_key = "YOUR_API_KEY_HERE"

# Stock symbols
symbols = ["SPY", "XIC.TO", "DAL", "SPG", "GOLD"]

# Empty list to hold the dataframes for each stock
dfs = []

# Loop through each stock symbol and get the daily high and low prices
for symbol in symbols:
    # API parameters
    params = {
        "function": "TIME_SERIES_DAILY",
        "symbol": symbol,
        "apikey": api_key
    }

    # Make a request to the API and get the response
    response = requests.get(endpoint, params=params)

    # Convert the response to a dataframe and select the high and low prices
    df = pd.read_json(response.text)["Time Series (Daily)"].transpose()[["2. high", "3. low"]]

    # Rename the columns to include the stock symbol
    df.columns = [f"{symbol} High", f"{symbol} Low"]

    # Append the dataframe to the list
    dfs.append(df)

# Combine the dataframes into a single dataframe
stock_prices_df = pd.concat(dfs, axis=1)

# Print the dataframe
print(stock_prices_df)
# Append the stock prices to the global totals dataframe
combined_df = pd.concat([global_totals_df, stock_prices_df], axis=1)

# Print the combined dataframe
print(combined_df)
import matplotlib.pyplot as plt

# Create a bar chart of the global confirmed cases and deaths
plt.bar(x=["Confirmed Cases", "Deaths"],
