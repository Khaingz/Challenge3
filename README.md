# Challenge4

# Analyzing Portfolio Risk and Return

## Instructions

### Import the Data

Use the ``risk_return_analysis.ipynb`` file to complete the following steps:

1. Import the required libraries and dependencies.

import pandas as pd
import numpy as np
from pathlib import Path
%matplotlib inline
import matplotlib.pyplot as plt


2. Use the `read_csv` function and the `Path` module to read the `whale_navs.csv` file into a Pandas DataFrame. Be sure to create a `DateTimeIndex`. 

whale_navs = pd.read_csv(
    Path('./Resources/whale_navs.csv'),
    index_col = "date",
    parse_dates=True,
    infer_datetime_format=True
Review the first five rows of the DataFrame by using the `head` function.   
    whale_navs.head()

3. Use the Pandas `pct_change` function together with `dropna` to create the daily returns DataFrame. Base this DataFrame on the NAV prices of the four portfolios and on the closing price of the S&P 500 Index. Review the first five rows of the daily returns DataFrame.

Used pct_change().dropna() to create the daily returns DataFrame

### Analyze the Performance

Analyze the data to determine if any of the portfolios outperform the broader stock market, which the S&P 500 represents. To do so, complete the following steps:

1. Use the default Pandas `plot` function to visualize the daily return data of the four fund portfolios and the S&P 500. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Used .plot(figsize = (10,7), title = "Whales Daily Returns")

2. Use the Pandas `cumprod` function to calculate the cumulative returns for the four fund portfolios and the S&P 500. Review the last five rows of the cumulative returns DataFrame by using the Pandas `tail` function.

Use the Pandas `cumprod` function and `tail` function.
cumulative_returns = (1+ whale_navs_daily_returns).cumprod()
cumulative_returns.tail()

3. Use the default Pandas `plot` to visualize the cumulative return values for the four funds and the S&P 500 over time. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Used .plot(figsize=(10,5), title="Whales Cumulative Return")

4. Answer the following question: Based on the cumulative return data and the visualization, do any of the four fund portfolios outperform the S&P 500 Index?

Based on the above cumulative return data and the visualization. None of the four fund portfolios outperform the S&P 500 Index. S&P 500 Index surpass the four fund portfolios.

### Analyze the Volatility

Analyze the volatility of each of the four fund portfolios and of the S&P 500 Index by using box plots. To do so, complete the following steps:

1. Use the Pandas `plot` function and the `kind="box"` parameter to visualize the daily return data for each of the four portfolios and for the S&P 500 in a box plot. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Used Plt function to adjust the figure size 
plt.rcParams["figure.figsize"] = (15,10)

Used `plot` function and the `kind="box"` parameter to visualize the daily return data 
whale_navs_daily_returns.plot(kind="box", title = "Whales Daily Returns")

2. Use the Pandas `drop` function to create a new DataFrame that contains the data for just the four fund portfolios by dropping the S&P 500 column. Visualize the daily return data for just the four fund portfolios by using another box plot. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Used the Pandas `drop` function to create a new DataFrame
Fund_Portfolios = whale_navs.drop(columns=['S&P 500'])

Used .plot(kind="box", title = "Four_Fund_Portfolios")

    > **Hint** Save this new DataFrame&mdash;the one that contains the data for just the four fund portfolios. You’ll use it throughout the analysis.

3. Answer the following question: Based on the box plot visualization of just the four fund portfolios, which fund was the most volatile (with the greatest spread) and which was the least volatile (with the smallest spread)?

Based on the box plot visualization of just the four fund portfolios, BERKSHIRE HATHAWAY INC was the most volatile and greatest spread and TIGER GLOBAL MANAGEMENT LLC was the least volatie and the smallest speread.

### Analyze the Risk

Evaluate the risk profile of each portfolio by using the standard deviation and the beta. To do so, complete the following steps:

1. Use the Pandas `std` function to calculate the standard deviation for each of the four portfolios and for the S&P 500. Review the standard deviation calculations, sorted from smallest to largest.

Used the Pandas `std` function to calculate the standard deviation
Used the 'sort_values'
standard_deviation = whale_navs_daily_returns.std()
standard_deviation.head()
standard_deviation_sorted = standard_deviation.sort_values()
standard_deviation_sorted

2. Calculate the annualized standard deviation for each of the four portfolios and for the S&P 500. To do that, multiply the standard deviation by the square root of the number of trading days. Use 252 for that number.

Set up annual trading day = 252
Used numpy as np
caculate the standard deviation by the square root of the number of trading days np.sqrt()
annualized_standard_deviation = standard_deviation * np.sqrt(252)
annualized_standard_deviation.sort_values()

3. Use the daily returns DataFrame and a 21-day rolling window to plot the rolling standard deviations of the four fund portfolios and of the S&P 500 index. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Caculated rolling deviations multiple a 21-day rolling window

rolling_Standard_deviations = standard_deviation* np.sqrt(21)

Used .plot(figsize = (15,10), title = "21-day Rolling Window")

4. Use the daily returns DataFrame and a 21-day rolling window to plot the rolling standard deviations of only the four fund portfolios. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Caculate only portfolios rolling deviations using 'std' and a np.sqrt (21 day) rolling window.

Fund_Portfolios_rolling_Standard_deviations = Fund_Portfolios_daily_returns.std() * np.sqrt(21)

Used .plot(figsize = (12,10), title = "21-day Fund_Portfolios Rolling Window")


5. Answer the following three questions:

* Based on the annualized standard deviation, which portfolios pose more risk than the S&P 500?

* Based on the annualized standard deviation, BERKSHIRE HATHAWAY INC pose more rish than the S&P 500.

* Based on the rolling metrics, does the risk of each portfolio increase at the same time that the risk of the S&P 500 increases?

* Yes, we can see that based on the rolling metrics, the risk of each portfolio did increase at the same time that the risk of the S&P 500 increases.

* Based on the rolling standard deviations of only the four fund portfolios, which portfolio poses the most risk? Does this change over time?

* Based on the rolling standard deviations of only the four fund portfolios, BERKSHIRE HATHAWAY INC poses the most risk, this is not change overtime.

### Analyze the Risk-Return Profile

To determine the overall risk of an asset or portfolio, quantitative analysts and investment managers consider not only its risk metrics but also its risk-return profile. After all, if you have two portfolios that each offer a 10% return but one has less risk, you’d probably invest in the smaller-risk portfolio. For this reason, you need to consider the Sharpe ratios for each portfolio. To do so, complete the following steps:

1. Use the daily return DataFrame to calculate the annualized average return data for the four fund portfolios and for the S&P 500. Use 252 for the number of trading days. Review the annualized average returns, sorted from lowest to highest.

Used trading days = 252 in a year.
Used 'mean' function to multiple trading days to get annual average return

Annual_Average_return = whale_navs_daily_returns.mean() * year_trading_days
Annual_Average_return.sort_values()

2. Calculate the Sharpe ratios for the four fund portfolios and for the S&P 500. To do that, divide the annualized average return by the annualized standard deviation for each. Review the resulting Sharpe ratios, sorted from lowest to highest.

To caculate the sharpe ratios : Annual_Average_return / annualized_standard_deviation

sharpe_ratios = Annual_Average_return / annualized_standard_deviation
sharpe_ratios.sort_values()

3. Visualize the Sharpe ratios for the four funds and for the S&P 500 in a bar chart. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Used .plot.bar(figsize=(10,7),title="Sharpe Ratios")

4. Answer the following question: Which of the four portfolios offers the best risk-return profile? Which offers the worst?

 Based on the both the Sharpe ratio I would have to suggest, BERKSHIRE HATHAWAY INC offers the best risk-return profile and PAULSON & CO.INC offers the worst.

#### Diversify the Portfolio

Your analysis is nearing completion. Now, you need to evaluate how the portfolios react relative to the broader market. Based on your analysis so far, choose two portfolios that you’re most likely to recommend as investment options. To start your analysis, complete the following step:

* Use the Pandas `var` function to calculate the variance of the S&P 500 by using a 60-day rolling window. Visualize the last five rows of the variance of the S&P 500.

Used the Pandas 'var' function to calculate the variance of the S&P 500 
Used  rolling(window=60) to get the variance

SP500_variance = whale_navs_daily_returns['S&P 500'].rolling(window=60).var()

Next, for each of the two portfolios that you chose, complete the following steps:

1. Using the 60-day rolling window, the daily return data, and the S&P 500 returns, calculate the covariance. Review the last five rows of the covariance of the portfolio.

Used the Tiger Blobal Management portfolio to get the 60-day rolling window daily return data and calculate the covariance.

Used the Pandas 'cov' function to caculate the Tiger Portfolio and S&P 500 returns.

tiger_rolling_60_covariance = whale_navs_daily_returns["TIGER GLOBAL MANAGEMENT LLC"].rolling(window=60).cov(whale_navs_daily_returns["S&P 500"])

2. Calculate the beta of the portfolio. To do that, divide the covariance of the portfolio by the variance of the S&P 500.

To calculate the beta portfolio , divide the tiger rolling 60 covariance divide by the variance of the S&P 500.

tiger_rolling_60_covariance / SP500_variance

3. Use the Pandas `mean` function to calculate the average value of the 60-day rolling beta of the portfolio.

Use the Pandas `mean` function to calculate "the tiger average value".
tiger_average = tiger_beta.mean()

4. Plot the 60-day rolling beta. Be sure to include the `title` parameter, and adjust the figure size if necessary.

Used .plot(figsize=(10,7),title =" TIGER GLOBAL 60-day Rolling Beta")

5. Calculate the others most likely to recommend as investment options.

Repate the same calculation steps using BERKSHIRE HATHAWAY INC return data.

Finally, answer the following two questions:

* Which of the two portfolios seem more sensitive to movements in the S&P 500?

PAULSON & CO.INC. and BERKSHIRE HATHAWAY INC seem more sensitive to movements in the S&P 500.

* Which of the two portfolios do you recommend for inclusion in your firm’s suite of fund offerings?

I would recommend BERKSHIRE HATHAWAY INC and TIGER GLOBAL MANAGEMENT LLC for fund offering. This two portfolios could be the investment choice if looking to invest in the most conservative of the others.