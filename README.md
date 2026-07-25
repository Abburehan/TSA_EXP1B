# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 25-07-2026

### AIM:
To develop a Python program to perform Regular Differencing, Seasonal Adjustment, and Log Transformation on the International Airline Passenger dataset to convert non-stationary data into stationary data.

### ALGORITHM:
1. Import the required libraries such as Pandas, NumPy, and Matplotlib.
2. Read the International Airline Passenger dataset using the Pandas library.
3. Convert the date column into datetime format and set it as the index.
Perform Regular Differencing to remove trends from the data.
Perform Seasonal Adjustment by subtracting the seasonal difference.
Apply Log Transformation to stabilize the variance of the data.
Plot the original data and the transformed data for comparison.
Display the results.
### PROGRAM:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

data = pd.read_csv("AirPassengers.csv")
data['Month'] = pd.to_datetime(data['Month'])
data.set_index('Month', inplace=True)

data = data.dropna(subset=['#Passengers'])
data['passenger_diff'] = data['#Passengers'].diff()

result = seasonal_decompose(
    data['#Passengers'],
    model='additive',
    period=12
)

data['passenger_seasonal_diff'] = result.resid
data['passenger_log'] = np.log(data['#Passengers'])

# Log + Regular Difference
data['passenger_log_diff'] = (
    data['passenger_log'] -
    data['passenger_log'].shift(1)
)

# Log + Seasonal Difference
result = seasonal_decompose(
    data['passenger_log_diff'].dropna(),
    model='additive',
    period=12
)

data['passenger_log_seasonal_diff'] = result.resid
plt.figure(figsize=(16,16))

# Original Data
plt.subplot(6,1,1)
plt.plot(data['#Passengers'], label='Original')
plt.legend(loc='best')
plt.title('Original Passenger Data')
plt.xlabel('Year')
plt.ylabel('Passengers')

# Regular Difference
plt.subplot(6,1,2)
plt.plot(data['passenger_diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Year')
plt.ylabel('Differenced Passengers')

# Seasonal Adjustment
plt.subplot(6,1,3)
plt.plot(data['passenger_seasonal_diff'],
         label='Seasonal Adjustment')
plt.legend(loc='best')
plt.title('Seasonal Adjustment')
plt.xlabel('Year')
plt.ylabel('Seasonally Adjusted Passengers')

# Log Transformation
plt.subplot(6,1,4)
plt.plot(data['passenger_log'],
         label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Year')
plt.ylabel('Log(Passengers)')

# Log + Difference
plt.subplot(6,1,5)
plt.plot(data['passenger_log_diff'],
         label='Log + Difference')
plt.legend(loc='best')
plt.title('Log Transformation + Regular Difference')
plt.xlabel('Year')
plt.ylabel('Diff(Log(Passengers))')

# Log + Seasonal Difference
plt.subplot(6,1,6)
plt.plot(data['passenger_log_seasonal_diff'],
         label='Log + Regular + Seasonal Difference')
plt.legend(loc='best')
plt.title('Log + Regular + Seasonal Difference')
plt.xlabel('Year')
plt.ylabel('Seasonally Differenced Log(Passengers)')

plt.tight_layout()
plt.show()

# Plot all transformed columns
data.plot(figsize=(12,6))
plt.show()
```

### OUTPUT:
ORIGINAL DATA:
<img width="1442" height="231" alt="image" src="https://github.com/user-attachments/assets/6d8738ca-e619-4f2e-be01-df069beba34a" />


REGULAR DIFFERENCING:
<img width="1433" height="231" alt="image" src="https://github.com/user-attachments/assets/e27fc6b2-ab32-4c79-b206-801d8bbe40a9" />


SEASONAL ADJUSTMENT:
<img width="1430" height="235" alt="image" src="https://github.com/user-attachments/assets/119dbc35-cc8b-4432-99a9-75bcac4d1a45" />


LOG TRANSFORMATION:
<img width="1422" height="225" alt="image" src="https://github.com/user-attachments/assets/fe7b86fb-9651-4d19-b895-f79a70040c75" />


LOG TRANSFORMATION + REGULAR DIFFERENCING:
<img width="1428" height="225" alt="image" src="https://github.com/user-attachments/assets/dc742677-1691-44c5-baf5-c321a9bb6739" />


LOG TRANSFORMATION + REGULAR DIFFERENCE + SEASONAL DIFFERENCE:
<img width="1432" height="260" alt="image" src="https://github.com/user-attachments/assets/8f44d021-2f94-4991-911e-93906eb43408" />


### RESULT:
Thus, the Python program for the conversion of non-stationary data into stationary data using Regular Differencing, Seasonal Adjustment, and Log Transformation on the International Airline Passenger dataset was successfully developed and executed.
