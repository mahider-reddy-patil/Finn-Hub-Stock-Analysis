# Finn-Hub-Stock-Analysis
Conducted comprehensive stock analysis by extracting and analyzing data from Finn Hub API. Employed Python and Databricks for data cleaning, statistical analysis, and generating insightful visualizations. Provided valuable insights into stock performance for informed decision-making.

Sure! Here are step-by-step instructions for beginners to run the above project:

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
      plt.figure(figsize=(12, 8))
      plt.scatter(avg_daily_returns, std_daily_returns, s=1000, alpha=0.5)
      plt.xlabel('Average Daily Return')
      plt.ylabel('Standard Deviation of Daily Returns')
      plt.title('Risk vs. Return')
      plt.grid(True)
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

11. Analyze the correlation between stock prices:
    ```python
    correlation = all_stocks_data[['AAPL', 'GOOG', 'MSFT', 'AMZN', 'FB']].corr()
    plt.figure(figsize=(10, 8))
    sns.heatmap(correlation, annot=True, cmap='coolwarm')
    plt.title('Correlation Between Stock Prices')
    plt.show()
    ```

12. Save the correlation matrix to an image file:
    ```python
    correlation_plot = sns.heatmap(correlation, annot=True, cmap='coolwarm')
    correlation_plot.figure.savefig('correlation_matrix.png')
    ```

13. Run the code and observe the generated visualizations and saved files.

Remember to replace 'YOUR_API_KEY' with your actual Finnhub API key in step 5. You can also modify the stock symbols and time period in step 6 to analyze different stocks and time ranges.

Feel free to explore and modify the code to further customize the analysis or add additional visualizations as per your requirements. Enjoy analyzing stock data with Python!
