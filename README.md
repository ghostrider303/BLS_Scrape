import matplotlib.pyplot as plt

fig, axs = plt.subplots(3, 2, figsize=(14, 12))

for i, city in enumerate(df['City'].unique()):
    city_data = df[df['City'] == city]
    
    city_data_values = city_data[['Date', 'Ford_sales', 'Nissan_sales', 'Toyota_sales', 'Ford_google', 'Nissan_google', 'Toyota_google']].values
    
    axs[i, 0].plot(city_data_values[:, 0], city_data_values[:, 1:4], label=['Ford', 'Nissan', 'Toyota'])
    axs[i, 0].set_title(f'Sales for {city}')
    axs[i, 0].set_xlabel('Date')
    axs[i, 0].set_ylabel('Sales')
    axs[i, 0].legend()
    
    axs[i, 1].plot(city_data_values[:, 0], city_data_values[:, 4:], label=['Ford', 'Nissan', 'Toyota'])
    axs[i, 1].set_title(f'Google for {city}')
    axs[i, 1].set_xlabel('Date')
    axs[i, 1].set_ylabel('Google')
    axs[i, 1].legend()

plt.tight_layout()
plt.show()
