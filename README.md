# Finn-Hub-Stock-Analysis
Conducted comprehensive stock analysis by extracting and analyzing data from Finn Hub API. Employed Python and Databricks for data cleaning, statistical analysis, and generating insightful visualizations. Provided valuable insights into stock performance for informed decision-making.

Requirements:
1. Python installed on your system.
2. Jupyter Notebook or any Python IDE (Integrated Development Environment) installed.
3. Required packages: finnhub-python, pandas, matplotlib, seaborn.

Instructions:

1. Open your preferred Python IDE (e.g., Jupyter Notebook).
2. Create a new Python file or notebook.
3. Install the necessary packages by running the following command in your Python environment:
   ```
   pip install finnhub-python pandas matplotlib seaborn
   ```
4. Import the required libraries at the beginning of your code:
   ```python
   import pandas as pd
   import finnhub
   import matplotlib.pyplot as plt
   import seaborn as sns
   ```

5. Replace the API key with your own API key from Finnhub. You can sign up for a free API key at [Finnhub.io](https://finnhub.io/).
   ```python
   finnhub_client = finnhub.Client(api_key="YOUR_API_KEY")
   ```

6. Define the stock symbols and the time period you want to analyze:
   ```python
   symbols = ['AAPL', 'GOOG', 'MSFT', 'AMZN', 'FB', 'TSLA', 'NVDA', 'NFLX', 'PYPL', 'ADBE', 'AAL', 'USB', 'INTC', 'WFC', 'BAC', 'HPQ', 'UBER', 'F', 'NVDA', 'T', 'GOOGL', 'LYFT', 'META']
   start_date = '2000-01-01'
   end_date = '2023-05-01'
   ```

7. Run the code to fetch the stock data, process and transform it, and store it in a Pandas DataFrame.
   ```python
   stock_data = []
   for symbol in symbols:
       data = finnhub_client.stock_candles(symbol, 'D', int(pd.Timestamp(start_date).timestamp()), int(pd.Timestamp(end_date).timestamp()))
       df = pd.DataFrame(data)
        
       df['symbol'] = symbol
       df['date'] = pd.to_datetime(df['t'], unit='s').dt.date
       df['opening_price'] = df['o'].astype(float)
       df['highest_price'] = df['h'].astype(float)
       df['lowest_price'] = df['l'].astype(float)
       df['closing_price'] = df['c'].astype(float)
       df['status'] = df['s'].astype(str)
       df['volume'] = df['v'].astype(float)
       df = df[['symbol', 'date', 'opening_price', 'highest_price', 'lowest_price', 'closing_price', 'status', 'volume']]
       stock_data.append(df)
   all_stocks_data = pd.concat(stock_data)
   ```
8. Perform data cleaning and validation steps:
   - Drop any rows with missing values:
     ```python
     all_stocks_data.dropna(inplace=True)
     ```

   - Drop duplicate rows:
     ```python
     all_stocks_data.drop_duplicates(inplace=True)
     ```

   - Validate the data by removing outliers using the Interquartile Range (IQR) method:
     ```python
     q1 = all_stocks_data['closing_price'].quantile(0.25)
     q3 = all_stocks_data['closing_price'].quantile(0.75)
     iqr = q3 - q1
     lower_bound = q1 - 1.5 * iqr
     upper_bound = q3 + 1.5 * iqr
     all_stocks_data = all_stocks_data[(all_stocks_data['closing_price'] > lower_bound) & (all_stocks_data['closing_price'] < upper_bound)]
     ```

9. Save the cleaned and validated data to a CSV file:
   ```python
   file_path = 'stocks.csv'
   all_stocks_data.to_csv(file_path, index=False)
   ```

10. Visualize the data:
    - Plot the risk vs. return by calculating average daily returns and standard deviation of daily returns:
      ```python
      # Calculate daily returns
      all_stocks_data['daily_return'] = all_stocks_data.groupby('symbol')['closing_price'].pct_change()

      # Calculate the daily average percentage change in closing price for each stock
      avg_daily_returns = all_stocks_data.groupby('symbol')['daily_return'].mean()

      # Calculate the standard deviation of the daily percentage change in closing price for each stock
      std_daily_returns = all_stocks_data.groupby('symbol')['daily_return'].std()

      # Merge the average and standard deviation of returns into a single DataFrame
      risk_data = pd.concat([avg_daily_returns, std_daily_returns], axis=1)
      risk_data.columns = ['avg_daily_return', 'std_daily_return']

      plt.figure(figsize=(12, 8))
      plt.scatter(avg_daily_returns, std_daily_returns, s=1000, alpha=0.5)
      plt.xlabel('Average Daily Return')
      plt.ylabel('Standard Deviation of Daily Returns')
      plt.title('Risk vs. Return')
      plt.grid(True)
      
      # Label each point with the corresponding stock symbol
      for i, symbol in enumerate(symbols):
         plt.annotate(symbol, (avg_daily_returns[i], std_daily_returns[i]), xytext=(10,-10), textcoords='offset points', ha='center', va='center')
      plt.show()
      ```

    - Plot the total trading volume for each stock:
      ```python
      plt.figure(figsize=(12, 8))
      plt.bar(stocks_volume['symbol'], stocks_volume['volume']
      plt.xlabel('Stock Symbol')
      plt.ylabel('Trading Volume')
      plt.title('Total Trading Volume for Each Stock')
      plt.xticks(rotation=45)
      plt.grid(True)
      ```

11. Run the code from Jupyter notebook and observe the generated visualizations.

Remember to replace 'YOUR_API_KEY' with your actual Finnhub API key in step 5. You can also modify the stock symbols and time period in step 6 to analyze different stocks and time ranges.

Feel free to explore and modify the code to further customize the analysis or add additional visualizations as per your requirements. Enjoy analyzing stock data with Python!
