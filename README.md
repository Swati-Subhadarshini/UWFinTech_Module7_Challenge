# UWFinTech_Module7_Challenge
Financial database and web application by using SQL, Python, and the Voilà library to analyze the performance of a hypothetical fintech ETF.

The etf_analyzer.ipynb notebook is used to complete your analysis of a fintech ETF that consists of four stocks: GOST, GS, PYPL, and SQ. 

Analyze the daily returns of the ETF stocks both individually and as a whole. Then deploy the visualizations to a web application by using the Voilà library.


### Analyze a single asset in the ETF

SQL SELECT statement by using an f-string that reads all the PYPL data from the database. By using the SQL SELECT statement, query is executed that reads the PYPL data from the database into a Pandas DataFrame.

Use the head and tail functions to review the first five and the last five rows of the DataFrame. 

Using hvPlot, an interactive visualization is created for the PYPL daily returns. The “time” column of the DataFrame is on the x-axis. 

Using hvPlot, an interactive visualization is created for the PYPL cumulative returns. The “time” column of the DataFrame is on the x-axis. 

### Optimize data access with advanced SQL queries

Closing prices for PYPL is accessed that are greater than 200:

```query = """SELECT time, close
           FROM PYPL
           WHERE close > 200.0"""
pypl_higher_than_200 = pd.read_sql_query(query, con=engine)
```

Find the top 10 daily returns for PYPL by completing the following steps:

```query = """SELECT time, daily_returns
           FROM PYPL
           ORDER BY daily_returns DESC
           LIMIT 10"""
```

### Analyze the ETF portfolio

SQL query to join each table in the portfolio into a single DataFrame.

Use a SQL inner join to join each table on the “time” column. Access the “time” column in the GDOT table via the GDOT.time syntax. Access the “time” columns from the other tables via similar syntax.

```query = """SELECT *
           FROM GDOT
           INNER JOIN PYPL ON
           GDOT.time = PYPL.time
           INNER JOIN GS ON
           GDOT.time = GS.time
           INNER JOIN SQ ON
           GDOT.time = SQ.time
           """
```

Using the SQL query, read the data from the database into a Pandas DataFrame, drop the duplicate columns, get the date from datetime column, set the index to time column.
```
etf_portfolio = pd.read_sql_query(query, con=engine)
etf_portfolio = etf_portfolio.T.drop_duplicates().T
etf_portfolio
etf_portfolio['time'] = pd.to_datetime(etf_portfolio['time']).dt.date
etf_portfolio = etf_portfolio.set_index('time')
print(etf_portfolio)
```
Create a DataFrame that averages the “daily_returns” columns for all four assets. Review the resulting DataFrame.

```etf_portfolio_returns = etf_portfolio['daily_returns'].mean(axis=1)```

Using the average daily returns in the etf_portfolio_returns DataFrame to calculate the annualized returns for the portfolio. Display the annualized return value of the ETF portfolio.

```annualized_etf_portfolio_returns =  (etf_portfolio_returns.mean() * 252)```

Using the average daily returns in the etf_portfolio_returns DataFrame to calculate the cumulative returns of the ETF portfolio.
```etf_cumulative_returns = (1+etf_portfolio_returns).cumprod()```

Using hvPlot, an interactive line plot is created that visualizes the cumulative return values of the ETF portfolio. Reflect the “time” column of the DataFrame on the x-axis. Make sure that you professionally style and format your visualization to enhance its readability.

```etf_cumulative_returns.hvplot(x="time",
                              xlabel="Time",
                              ylabel="Cumulative Return Value",
                              title="ETF Portfolio Cummulative Returns Plot 2016-2020",
                              height=500,
                              width=1000)
 ```

### Deploy the notebook as a web application

Using the Voilà library to deploy your notebook as a web application. You can deploy the web application locally on your computer.
use Git Bash CLI for deploying the notebook as a web application

$ voila etf_analyser.ipynb

Screenshot as examples:

![voila_screenshot](UWFinTech_Module7_Challenge/Images/Voila_screenshot.png)
![voila_screenshot2](UWFinTech_Module7_Challenge/Images/voila2_screenshot.png)

### Contributors

This project is designed by Swati Subhadarshini 
Emaid id: sereneswati@gmail.com
LinkedIn link: https://www.linkedin.com/in/swati-subhadarshini-89a8a538?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_contact_details%3BhUCLlUYCSc2jK4x4khPVUQ%3D%3D

---