# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 19.08.2025

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

# Load dataset
data = pd.read_excel("Data_Train.xlsx")

# Convert date column
data['Date_of_Journey'] = pd.to_datetime(data['Date_of_Journey'], format="%d/%m/%Y")

# Aggregate by daily average price (remove duplicate dates issue)
daily_data = data.groupby('Date_of_Journey')['Price'].mean()

# Create DataFrame with unique daily index
ts = pd.DataFrame(daily_data)

# Regular differencing
ts['price_diff'] = ts['Price'] - ts['Price'].shift(1)

# Seasonal decomposition (weekly seasonality = 7 days)
result = seasonal_decompose(ts['Price'], model='additive', period=7)
ts['price_seasonal_diff'] = result.resid

# Log transform
ts['price_log'] = np.log(ts['Price'])
ts['price_log_diff'] = ts['price_log'] - ts['price_log'].shift(1)

# Seasonal decomposition on log-differenced data
result = seasonal_decompose(ts['price_log_diff'].dropna(), model='additive', period=7)
ts['price_log_seasonal_diff'] = result.resid

# ---------------- Plotting ----------------
plt.figure(figsize=(16, 20))  # Taller figure

plt.subplot(6, 1, 1)
plt.plot(ts['Price'], label='Original')
plt.legend(loc='best')
plt.title('Original Data (Avg Flight Price per Day)')
plt.xlabel('Date')
plt.ylabel('Price')

plt.subplot(6, 1, 2)
plt.plot(ts['price_diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Date')
plt.ylabel('Price')

plt.subplot(6, 1, 3)
plt.plot(ts['price_seasonal_diff'], label='Seasonal Adjustment')
plt.legend(loc='best')
plt.title('Seasonal Adjustment')
plt.xlabel('Date')
plt.ylabel('Price')

plt.subplot(6, 1, 4)
plt.plot(ts['price_log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Date')
plt.ylabel('Log(Price)')

plt.subplot(6, 1, 5)
plt.plot(ts['price_log_diff'], label='Log + Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation + Regular Differencing')
plt.xlabel('Date')
plt.ylabel('RDiff(Log(Price))')

plt.subplot(6, 1, 6)
plt.plot(ts['price_log_seasonal_diff'], label='Log + Regular + Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log Transformation + Regular + Seasonal Differencing')
plt.xlabel('Date')
plt.ylabel('SDiff(RDiff(Log(Price)))')

# Add more space between subplots
plt.subplots_adjust(hspace=1.2)

plt.show()

# Final overall trend
ts['Price'].plot(kind='line', figsize=(12,6), title="Flight Price Trend (Daily Average)")
plt.show()


### OUTPUT:
<img width="1319" height="215" alt="Screenshot 2025-08-19 132839" src="https://github.com/user-attachments/assets/6a1777ef-cb1a-4b89-900e-be778702b01d" />


REGULAR DIFFERENCING:
<img width="1325" height="220" alt="Screenshot 2025-08-19 132848" src="https://github.com/user-attachments/assets/8a95785a-e016-4085-9ec0-f788bd4b90b8" />


SEASONAL ADJUSTMENT:
<img width="1314" height="208" alt="Screenshot 2025-08-19 132857" src="https://github.com/user-attachments/assets/596477b1-cee0-47ec-96d0-8608516889b4" />


LOG TRANSFORMATION:
<img width="1303" height="223" alt="Screenshot 2025-08-19 132903" src="https://github.com/user-attachments/assets/59d79002-3ec5-4bec-bd2b-a4fcf8567aa4" />

<img width="1306" height="202" alt="Screenshot 2025-08-19 132908" src="https://github.com/user-attachments/assets/85aa0a3c-0806-46fb-ade3-ff1a9bf1c017" />

<img width="1285" height="224" alt="Screenshot 2025-08-19 132918" src="https://github.com/user-attachments/assets/f267d06c-fad1-4b55-bc81-fb2ec43117c7" />


<img width="1068" height="535" alt="Screenshot 2025-08-19 132929" src="https://github.com/user-attachments/assets/e16d21ab-7726-48cf-b6f0-a76cfbcf0d06" />



### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
