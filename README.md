import statsmodels.api as sm

# Function to remove seasonality from sales data
def remove_seasonality(df, city):
    city_data = df[df['City'] == city]
    
    # Convert DataFrame to NumPy arrays
    city_data_values = city_data[['Date', 'Ford_sales', 'Nissan_sales', 'Toyota_sales']].values
    
    # Extract sales data
    dates = city_data_values[:, 0]
    sales_data = city_data_values[:, 1:4]
    
    # Perform seasonal decomposition
    decomposed_sales = {}
    for i, brand in enumerate(['Ford', 'Nissan', 'Toyota']):
        decomposition = sm.tsa.seasonal_decompose(sales_data[:, i], period=12)  # Assuming a yearly seasonality
        decomposed_sales[brand] = decomposition.resid
    
    # Plot the original and deseasonalized sales data
    fig, axs = plt.subplots(3, 2, figsize=(12, 10))
    
    for i, brand in enumerate(['Ford', 'Nissan', 'Toyota']):
        # Original sales plot
        axs[i, 0].plot(dates, sales_data[:, i], label='Original')
        axs[i, 0].set_title(f'Original Sales for {brand} in {city}')
        axs[i, 0].set_xlabel('Date')
        axs[i, 0].set_ylabel('Sales')
        axs[i, 0].legend()
        axs[i, 0].tick_params(axis='x', rotation=90)  # Rotate x-axis tick labels
        
        # Deseasonalized sales plot
        axs[i, 1].plot(dates, decomposed_sales[brand], label='Deseasonalized')
        axs[i, 1].set_title(f'Deseasonalized Sales for {brand} in {city}')
        axs[i, 1].set_xlabel('Date')
        axs[i, 1].set_ylabel('Sales')
        axs[i, 1].legend()
        axs[i, 1].tick_params(axis='x', rotation=90)  # Rotate x-axis tick labels
    
    plt.tight_layout()
    plt.show()

# Example usage: Remove seasonality for each city
for city in df['City'].unique():
    remove_seasonality(df, city)
