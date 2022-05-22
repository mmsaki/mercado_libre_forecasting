# Forecasting MercadoLibre using Prophet

## Table of Contents


1. Find unusual patterns in hourly Google search traffic.

2. Mine the search traffic data for seasonality.

3. Relate the search traffic to stock price patterns.

4. Create a time series model by using Prophet.

5. Forecast the revenue by using time series models.

6. Results

## Background

You’re a growth analyst at [MercadoLibre](http://investor.mercadolibre.com/investor-relations). With over 200 million users, MercadoLibre is the most popular e-commerce site in Latin America. You've been tasked with analyzing the company's financial and user data in clever ways to help the company grow. So, you want to find out if the ability to predict search traffic can translate into the ability to successfully trade the stock.

You’ll gain proficiency in the following tasks:

- Identifying patterns in time series data.

- Mining for patterns in seasonality by using visualizations.

- Building sales-forecast and user-interest predictive models.

## Part 1: Find Unusual Patterns in Hourly Google Search Traffic

- [x] Read the search data into a DataFrame, and then slice the data to just the month of May 2020. 
   - During this month, MercadoLibre released its quarterly financial results. 
   - Use hvPlot to visualize the results. Do any unusual patterns exist? 
      - **Answer:** Yes
   - Did the Google search traffic increase during the month that MercadoLibre released its financial results?
      - **Answer:** Yes, the median traffic search for may 2020 is higher than the median from accross all months.

   ![](./Images/01_2020-05_search_trends.png)
   
## Part 2: Mine the Search Traffic Data for Seasonality

- [x] Group the hourly search data to plot the average traffic by the day of the week for example, Monday vs. Friday.

![](./Images/02_avg_google_search_trends_grouped_by_dayofweek.png)

- [x] Using hvPlot, visualize this traffic as a heatmap, referencing `index.hour` for the x-axis and `index.dayofweek` for the y-axis. 
   - Does any day-of-week effect that you observe concentrate in just a few hours of that day? 
      - **Answer:** Yes
![](./Images/03_avg_traffic_during_hour_grouped_by_dayofweek.png)

- [x] Group the search data by the week of the year. 
   - Does the search traffic tend to increase during the winter holiday period (weeks 40 through 52)? 
      - **Answer:** Yes, Traffic increase during the winter holiday period.

![](./Images/04_avg_traffic_grouped_by_week_of_year.png)

## Part 3: Relate the Search Traffic to Stock Price Patterns

- [x] Read in and plot the stock price data. Concatenate the stock price data to the search data in a single DataFrame, use hvplot to plot historical closing price.

![](./Images/05_historical_mercado_closing_price.png)

- [x] Slice the data to just the first half of 2020, `2020-01` to `2020-06` in the DataFrame, and then use hvPlot to plot the data. 
   - Do both time series indicate a common trend that’s consistent with this narrative?
      - **Answer:** Yes, there is a trend consisent with the narative. Market events emerged during the year of 2020 that many companies found difficult. But, after the initial shock to global financial markets, new customers and revenue increased for e-commerce platforms.

![](./Images/06_search_trends_and_close_price.png)

3. Create a new column in the DataFrame named “Lagged Search Trends” that offsets, or shifts, the search traffic by one hour. Create two additional columns:

   - [x] “Stock Volatility”, which holds an exponentially weighted four-hour rolling average of the company’s stock volatility

   ```python
   # This column should calculate the standard deviation of the closing stock price return data over a 4 period rolling window
   mercado_stock_trends_df['Stock Volatility'] = mercado_stock_trends_df['close'].pct_change().rolling(4).std()
   ```

   ![](./Images/07_stock_volatility.png)

   - [x] “Hourly Stock Return”, which holds the percentage of change in the company stock price on an hourly basis
   ```python
   # This column should calculate hourly return percentage of the closing price
   mercado_stock_trends_df['Hourly Stock Return'] = mercado_stock_trends_df['close'].pct_change()
   ```

4. Review the time series correlation, and then answer the following question: Does a predictable relationship exist between the lagged search traffic and the stock volatility or between the lagged search traffic and the stock price returns?

**Answer:** The lagged search traffic has a negative correlation with stock volatility, but it has positive correlation with hourly stock returns.

![](./Images/lagged_trends_correlation.png)

## Part 4: Create a Time Series Model by Using Prophet

Now, you need to produce a time series model that analyzes and forecasts patterns in the hourly search data. To do so, complete the following steps:

1. Set up the Google search data for a Prophet forecasting model.

2. After estimating the model, plot the forecast. How's the near-term forecast for the popularity of MercadoLibre?

![](./Images/08_mercado_model_trend.png)

3. Plot the individual time series components of the model to answer the following questions:

![](./Images/10_mercado_trends_plot.png)

   - What time of day exhibits the greatest popularity?
   **Answer:** 00:00 Midnight

   - Which day of the week gets the most search traffic?
   **Answer:** # Tuesday

   - What's the lowest point for search traffic in the calendar year?
**Answer:** October - November

## Part 5 (Optional): Forecast the Revenue by Using Time Series Models

A few weeks after your initial analysis, the finance group follows up to find out if you can help them solve a different problem. Your fame as a growth analyst in the company continues to grow!

Specifically, the finance group wants a forecast of the total sales for the next quarter. This will dramatically increase their ability to both plan budgets and help guide expectations for the company investors.

To do so, complete the following steps:

1. Read in the daily historical sales (that is, revenue) figures, and then apply a Prophet model to the data.

![](./Images/11_mercado_daily_sales.png)

2. Interpret the model output to identify any seasonal patterns in the company revenue. For example, what are the peak revenue days? (Mondays? Fridays? Something else?)

**Answer:** Mondays and Wednesdays

![](./Images/12_mercado_sales_forecast.png)

3. Produce a sales forecast for the finance group. Give them a number for the expected total sales in the next quarter. Include the best- and worst-case scenarios to help them make better plans.

**Answer:** The most likely total sales forecast for next quater for the period 2020-07-01 to 2020-09-30 is 4974.633769. The Best Case total sales forecast is 5925.393263. The Worst Case total sales forecast is 4019.320500

![](./Images/13_mercado_forecast_plot.png)


## Results

**File:** [MercadoLibre Forecasting Analysis](./mercado_libre_forecast.ipynb)

**Resources:** [Resources](./Resources/)