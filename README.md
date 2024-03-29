import pandas as pd
import numpy as np
from datetime import datetime, timedelta

# Function to generate random data within a given range
def generate_random_data(low, high, size):
    return np.random.uniform(low, high, size)

# Generate dates from 4/1/2022 to 1/12/2023
start_date = datetime(2022, 4, 1)
end_date = datetime(2023, 12, 1)
date_range = [start_date + timedelta(days=x*30) for x in range(int((end_date - start_date).days / 30) + 1)]

# Generate city names
cities = ['CityA', 'CityB', 'CityC']

# Create an empty list to store DataFrames
dfs = []

# Generate data for each city and date
for city in cities:
    for date in date_range:
        ford_marketshare = np.random.uniform(5, 15)
        nissan_marketshare = np.random.uniform(5, 15)
        toyota_marketshare = np.random.uniform(5, 15)
        
        total_search_data = np.random.uniform(0, 1)
        ford_search_data = np.random.uniform(0, total_search_data)
        nissan_search_data = np.random.uniform(0, total_search_data - ford_search_data)
        toyota_search_data = total_search_data - ford_search_data - nissan_search_data
        
        data = {'Date': date, 'City': city,
                'Ford_marketshare': ford_marketshare,
                'Nissan_marketshare': nissan_marketshare,
                'Toyota_marketshare': toyota_marketshare,
                'Normalised_Ford_google_search_data': ford_search_data,
                'Normalised_Nissan_google_search_data': nissan_search_data,
                'Normalised_Toyota_google_search_data': toyota_search_data}
        
        # Append data to the list
        dfs.append(pd.DataFrame(data, index=[0]))

# Concatenate all DataFrames in the list
df = pd.concat(dfs, ignore_index=True)

# Sort the dataframe by city
df = df.sort_values(by=['City', 'Date']).reset_index(drop=True)
df

def add_lagged_columns(df, columns, lag_months=[1, 2, 3]):
    for lag in lag_months:
        for col in columns:
            new_col_name = f'{col}_lag{lag}'
            df[new_col_name] = df.groupby('City')[col].shift(lag)
    return df

# Test the function
# Assuming df is the dataframe generated in the previous step

lagged_df = add_lagged_columns(df, ['Ford_marketshare', 'Nissan_marketshare', 'Toyota_marketshare'])

def calculate_correlation(df, city, market_columns, google_columns, lag_months=[1, 2, 3]):
    city_df = df[df['City'] == city]
    correlations = {}
    for market_col in market_columns:
        for google_col in google_columns:
            for lag in lag_months:
                market_lag_col = f'{market_col}_lag{lag}'
                correlation = city_df[[market_lag_col, google_col]].corr().iloc[0, 1]
                key = f'{market_col}_lag{lag} vs {google_col}'
                correlations[key] = correlation
    return correlations

# Test the function
city = 'CityA'
market_columns = ['Ford_marketshare', 'Nissan_marketshare', 'Toyota_marketshare']
google_columns = ['Normalised_Ford_google_search_data', 'Normalised_Nissan_google_search_data', 'Normalised_Toyota_google_search_data']

correlation_results = calculate_correlation(lagged_df, city, market_columns, google_columns)
for key, value in correlation_results.items():
    print(f'{city}: {key} - Correlation: {value}')
    
    
cities = df['City'].unique()

for city in cities:
    correlation_results = calculate_correlation(lagged_df, city, market_columns, google_columns)
    print(f'Correlation results for {city}:')
    for key, value in correlation_results.items():
        print(f'{key} - Correlation: {value}')
    print()

