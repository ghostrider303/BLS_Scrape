# BLS_Scrape

import matplotlib.pyplot as plt

# Create subplots for each city
fig, axs = plt.subplots(3, 2, figsize=(14, 12))

# Filter data for each city and plot
for i, city in enumerate(df['City'].unique()):
    city_data = df[df['City'] == city]
    
    # Sales plot
    axs[i, 0].plot(city_data['Date'].values, city_data['Ford_sales'].values, label='Ford')
    axs[i, 0].plot(city_data['Date'].values, city_data['Nissan_sales'].values, label='Nissan')
    axs[i, 0].plot(city_data['Date'].values, city_data['Toyota_sales'].values, label='Toyota')
    axs[i, 0].set_title(f'Sales for {city}')
    axs[i, 0].set_xlabel('Date')
    axs[i, 0].set_ylabel('Sales')
    axs[i, 0].legend()
    
    # Google plot
    axs[i, 1].plot(city_data['Date'].values, city_data['Ford_google'].values, label='Ford')
    axs[i, 1].plot(city_data['Date'].values, city_data['Nissan_google'].values, label='Nissan')
    axs[i, 1].plot(city_data['Date'].values, city_data['Toyota_google'].values, label='Toyota')
    axs[i, 1].set_title(f'Google for {city}')
    axs[i, 1].set_xlabel('Date')
    axs[i, 1].set_ylabel('Google')
    axs[i, 1].legend()

plt.tight_layout()
plt.show()
