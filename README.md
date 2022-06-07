# MarketEye
CS180 Machine Project Requirement

The Machine Solution is in a Python Notebook. Each cell must be run in order.

For certain library imports, such as the statsmodels and pmdarima python libraries, please follow the versions specified in the requirements.txt file.

## Code Documentation

### Imports
The following Python libraries are used for this project:

- numpy
- pandas
- matplotlib
- statsmodels
- pmdarima
- warnings

### CSV Parsing

As explained in the wiki, Pandas' read_csv() function was used. This returns a DataFrame object that contains the
data set used by the machine.

### Training Data and Testing Data Separation

Using DataFrame's iloc (df.iloc[]), the training data and testing data could be spliced into two different arrays given an integer on where it should be spliced. In the case of this project, the dataset is spliced at the 26th index. Thus, training data stores the value df.iloc[:26], while testing data stores the value df.iloc[26:].

### RMSE Calculation

This cell contains the part of the code that calculates for RMSE. It takes in two parameters: forecast, and actual. forecast is the forecasted price values, while actual is the actual price values from the testing data. RMSE is calculated by the equation:

$$
RMSE = \frac{1}{n}\ \sum_{i = 0}^{n - 1} (forecast_i - actual_i) ^ 2
$$

### autoARIMA Code

As the name suggests, this is the cell that calculates for the best ARIMA model given the data. It takes in two parameters: training_data and n. training_data is the training data spliced previously, while n is the maximum AR (p) and MA (q) terms such that

$$
p_{max} = q_{max} = n
$$

The code will loop through $p \in \{0, 1, ... , n\}$ and $q \in \{0, 1, ..., n\}$ to find the optimal p and q values for reducing RMSE in the model. First, however, it finds the optimal differencing d for trend stationarity. This is done using pmdarima's ndiffs function, with the test selected being kpss and alpha = 0.05. After it finds the optimal differencing needed for the data, it will begin looping through every p and q combination EXCEPT for p = 1 and q = 0, which is reserved for AR(1), the comparison model. The function will then return the optimal p, d, and q values for model fitting.

### Main Function

This is the function that will be called for doing the training and testing of the model, called ModelAndForecast. It only has one parameter: fixed_query. This is the name of the vegetable whose price is being forecasted. Note that this is case sensitive and must follow the column name in the dataset. The process is commented in the code itself. However, a general rundown is that it will first call autoARIMA, and then plot a model based on the returned parameters. After which, the model and the compared model AR(1) both do their forecasts over the time period of the testing data, with the confidence intervals. Then, the performance metrics are printed out on the terminal, with the plot containing the training data, the testing data, the forecasts, and the model fit. 

### ARIMA Modelling

For each vegetable considered in the dataset, the ModelAndForecast function is called. Each cell prints out the results for each vegetable. These results are shown also in the wiki page.

### ARIMA Modelling with Query

Bonus part of the project, which is how it could be implemented with user inputs. The user will input the name of the vegetable into the input box, and the machine will print out the results of the forecast. 
