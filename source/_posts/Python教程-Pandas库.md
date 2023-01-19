---
title: Python教程--Pandas Library
tags:
  - Python
  - Pandas
categories:
  - 技术杂谈
  - Python
date: 2023-01-17 18:14:22
---
Pandas是一个建立在 Python 之上的一个高效的，简单易用的数据结构和分析工具。 Pandas 的核心就是一个高效易用的数据类型：DataFrame。这个数据类型有点类似 R 语言的数据框 (Data Frame)，也有点类似于 Excel 表格，但是比这两种更加适合在 Python 的语言环境内操作数据。在这个数据结构之下，我们可以轻松的对数据进行清洗，整理，归纳总结，合并，转换，计算等等。

<!-- more -->
* * *

# 从CSV文件中读取并输出数据 -- Reading in a CSV File
+ 读取数据--pd.read_csv("data.csv")
+ 输出整个dataframe--print(df)
+ 输出前五行数据----print(df.head())
+ 输出最后五行数据--print(df.tail())

Example:
```
import pandas as pd

def test _run():
    """Function called by Test Run."""
    df = pd.read_csv("./data/AAPL.csv")
    # Quiz: Print last 5 rows of the data frame
    # print df				# prints entire data set (dataframe)
    print (df.head())		# prints first five records
    print (df.tail())			# prints last five records

if __name__ = "__main__":
	test_run()
```

# 选择特定行的数据--Select data in specific rows
+ 输出第10行到第20行之间的所有数据--print(df[10:21])

Example:
```
import pandas as pd

def test_run():
    """Function called by Test Run."""
    df = pd.read_csv("data/AAPL.csv")
    print (df[10:21])		# print rows between index 10 and 20 inclusive
	
if __name__ == "__main__":
    test_run()
```

# 输出某列数据的最大值、平均值--Find maximum\mean closing value for stock
+ 输出Close列的最大值--df['Close'].max()
+ 输出Close列的平均--df['Close'].mean()

Example:
```
import pandas as pd

def get_max_close(symbol):
	"""Return the maximum closing value for stock indicated by symbol.

	Note: Data for a stock is stored in file: data/<symbol>.csv
	"""
	df = pd.read_csv("data/{}.csv".format(symbol)) #read in data
	return df['close'].max() #compute and return max

def test _run():
	"""Function called by Test Run."""
	for symbol in ['APPL', 'IBM']:
		print "Max close"
		print symbol, get_max_close(symbol)

if __name__ = "__main__":
	test_run()
```

# 配合matplotlib将数据绘制成折线图--Plotting
+ 绘图--df.plot() 以及 plt.show()

Example 1--输出单列数据:
```
import pandas as pd
import matplotlib.pyplot as plt

def test_run():
	"""Plot a single column."""
	df = pd.read_csv("data/AAPL.csv")
	print df['Ajf Close']
	df['Adf Close'].plot()
	plt.show()

if __name__ = "__main__":
	test_run()
```

Example 2 -- 输出两列数据:
```
import pandas as pd
import matplotlib.pyplot as plt
		
def test_run():
    df = pd.read_csv("data/IBM.csv")
    df[['Close', 'Adj Close']].plot()
    plt.show()  # must be called to show plots
	
if __name__ == "__main__":
    test_run()

```
# 创建和合并DataFrame--Build a DataFrame in Pandas
+ 创建pandas格式日期区间--pd.date_range(start_date, end_date)
+ 以日期为index创建空的DataFrame--pd.DataFrame(index=dates)
+ 将数据与创建的空白模板合并--df1.join(dfSPY) 注：default how='left'只保留左侧表格的所有数据
+ 合并表格并保留两个表中公共部分信息--df1.join(dfSPY, how='inner') 注：另外outer是保留两个表格所有信息，right是只保留右侧表格所有数据
+ 剔除空白数据行--df1.dropna()

Example:
```
def test_run():
	# Define data range
	start_date = '2010-01-22'
	end_date = '2010-01-26';
	dates=pd.date_range(start_date,end_date)
	
	print (dates)
	print (dates[0])  # get first element of list
	
	# Create an empty dataframe
	df1=pd.DataFrame(index=dates)  # define empty dataframe with these dates as index

	print (df1)
	
	# Read SPY data into temporary dataframe
	# dfSPY = pd.read_csv("data/SPY.csv") # will result in no data because this has index of integers
	# dfSPY = pd.read_csv("data/SPY.csv", index_col="Date", parse_dates=True)
	dfSPY = pd.read_csv("data/SPY.csv", index_col="Date",
						parse_dates=True, usecols=['Date','Adj Close'],
						na_values=['nan'])
	print (dfSPY)
	
	# Join the two dataframes using DataFram.join()
	df1=df1.join(dfSPY)
	print (df1)
	
	# Drop NaN Values
	df1 = df1.dropna()
	print (df1)

if __name__ == "__main__":
    test_run()
```
# 读取多组数据
1. 将需要读取的文件名组成列表
2. 将列表中的string做循环，并读取
3. 将读取的数据与现有表格合并

Example:
```
import pandas as pd

def test_run():
    # Define data range
    start_date = '2010-01-22'
    end_date = '2010-01-26';
    dates = pd.date_range(start_date, end_date)

    # Create an empty dataframe
    df1 = pd.DataFrame(index=dates)  # define empty dataframe with these dates as index

    # Read SPY data into temporary dataframe
    dfSPY = pd.read_csv("data/SPY.csv", index_col="Date",
                        parse_dates=True, usecols=['Date', 'Adj Close'],
                        na_values=['nan'])

    # Rename 'Adj Close' column to 'SPY' to prevent clash
    dfSPY = dfSPY.rename(columns={'Adj Close': 'SPY'})

    # Join the two dataframes using DataFram.join()
    df1 = df1.join(dfSPY, how='inner')

    # Read in more stocks
    symbols = ['GOOG', 'IBM', 'GLD']
    for symbol in symbols:
        df_temp = pd.read_csv("data/{}.csv".format(symbol, index_col='Date',
                                                   parse_dates=True, usecols=['Date', 'Adj Close'],
                                                   na_values=['nan']))
        df = df1.join(df_temp)  # use default how='left'

    print (df1)

if __name__ == "__main__":
    test_run()
```
# 常用函数--Utility functions
+ 文件路径生成函数
+ 从CSV文件中读取特定文件特定列的数据

```
import os
import pandas as pd

def symbol_to_path(symbol, base_dir="data"):
    """Return CSV file path given ticker symbol."""
    return os.path.join(base_dir, "{}.csv".format(str(symbol)))

def get_data(symbols, dates):
    """Read stock data (adjusted close) for given symbols from CSV files."""
    df = pd.DataFrame(index=dates)
    if 'SPY' not in symbols:  # add SPY for reference, if absent
        symbols.insert(0, 'SPY')

    for symbol in symbols:
		# Read and join data for each symbol
        df_temp = pd.read_csv(symbol_to_path(symbol), index_col='Date',
                parse_dates=True, usecols=['Date', 'Adj Close'], na_values=['nan'])
        df_temp = df_temp.rename(columns={'Adj Close': symbol})
        df = df.join(df_temp)
        if symbol == 'SPY':  # drop dates SPY did not trade
            df = df.dropna(subset=["SPY"])

    return df
```

# 数据截取--Slicing
+ 根据行的区间来截取数据--df.ix['2010-01-01':'2010-01-31'] 注：新版本中使用loc代替ix
+ 根据列的区间来截取数据--df[['IBM', 'GLD']]
+ 行和列同时截取--df.ix['2010-03-01':'2010-03-15', ['SPY', 'IBM']]

Example:
```
import os
import pandas as pd

def symbol_to_path(symbol, base_dir="data"):
    """Return CSV file path given ticker symbol."""
    return os.path.join(base_dir, "{}.csv".format(str(symbol)))

def get_data(symbols, dates):
    """Read stock data (adjusted close) for given symbols from CSV files."""
    df = pd.DataFrame(index=dates)
    if 'SPY' not in symbols:  # add SPY for reference, if absent
        symbols.insert(0, 'SPY')

    for symbol in symbols:
		# Quiz: Read and join data for each symbol
        df_temp = pd.read_csv(symbol_to_path(symbol), index_col='Date',
                parse_dates=True, usecols=['Date', 'Adj Close'], na_values=['nan'])
        df_temp = df_temp.rename(columns={'Adj Close': symbol})
        df = df.join(df_temp)
        if symbol == 'SPY':  # drop dates SPY did not trade
            df = df.dropna(subset=["SPY"])
    return df

def test_run():
    # Define a date range
    dates = pd.date_range('2010-01-01', '2010-12-31')

    # Choose stock symbols to read
    symbols = ['GOOG', 'IBM', 'GLD']  # SPY will be added in get_data()

    # Get stock data
    df = get_data(symbols, dates)

	# Slice by row range (dates) using DataFram.ix[] selector
    print (df.ix['2010-01-01':'2010-01-31'])  # the month of January

    #  Slice by column (symbols)
    print (df['GOOG']) # a single label selects a single column
    print (df[['IBM', 'GLD']]) # a list of labels selects multiple columns

    # Slice by row and column
    print (df.ix['2010-03-01':'2010-03-15', ['SPY', 'IBM']])


if __name__ == "__main__":
    test_run()
```
# 绘制多张图表--Plotting Multiple Stocks
+ 设置坐标轴-- ax.set_xlabel("x label"); ax.set_ylabel("y label")
+ 生成所需DataFrame之后使用df.plot()命令即可在同一张图中生成多个曲线

```
import os
import pandas as pd
import matplotlib.pyplot as plt


def symbol_to_path(symbol, base_dir="data"):
    """Return CSV file path given ticker symbol."""
    return os.path.join(base_dir, "{}.csv".format(str(symbol)))

def get_data(symbols, dates):
    """Read stock data (adjusted close) for given symbols from CSV files."""
    df = pd.DataFrame(index=dates)
    if 'SPY' not in symbols:  # add SPY for reference, if absent
        symbols.insert(0, 'SPY')

    for symbol in symbols:
        # Quiz: Read and join data for each symbol
        df_temp = pd.read_csv(symbol_to_path(symbol), index_col='Date',
                parse_dates=True, usecols=['Date', 'Adj Close'], na_values=['nan'])
        df_temp = df_temp.rename(columns={'Adj Close': symbol})
        df = df.join(df_temp)
        if symbol == 'SPY':  # drop dates SPY did not trade
            df = df.dropna(subset=["SPY"])

    return df

def plot_data(df, title="Stock prices"):
    """Plot stock prices with a custom title and meaningful axis labels."""
    ax = df.plot(title=title, fontsize=12)
    ax.set_xlabel("Date")
    ax.set_ylabel("Price")
    plt.show()
    
def test_run():
    # Define a date range
    dates = pd.date_range('2010-01-01', '2010-12-31')

    # Choose stock symbols to read
    symbols = ['GOOG', 'IBM', 'GLD']  # SPY will be added in get_data()
    
    # Get stock data
    df = get_data(symbols, dates)

    # Plot
    plot_data(df)

if __name__ == "__main__":
    test_run()
```
# 截取特定数据并生成图标--Slice and plot
+ 数据归一化--df/df.ix[0,:]
+ 截取数据并画图--plot_data(df.ix[start_index:end_index,columns]) 注：新版本中使用loc代替ix

Example:
```
import os
import pandas as pd
import matplotlib.pyplot as plt


def symbol_to_path(symbol, base_dir="data"):
    """Return CSV file path given ticker symbol."""
    return os.path.join(base_dir, "{}.csv".format(str(symbol)))

def get_data(symbols, dates):
    """Read stock data (adjusted close) for given symbols from CSV files."""
    df = pd.DataFrame(index=dates)
    if 'SPY' not in symbols:  # add SPY for reference, if absent
        symbols.insert(0, 'SPY')

    for symbol in symbols:
		# Quiz: Read and join data for each symbol
        df_temp = pd.read_csv(symbol_to_path(symbol), index_col='Date',
                parse_dates=True, usecols=['Date', 'Adj Close'], na_values=['nan'])
        df_temp = df_temp.rename(columns={'Adj Close': symbol})
        df = df.join(df_temp)
        if symbol == 'SPY':  # drop dates SPY did not trade
            df = df.dropna(subset=["SPY"])

    return df	

def normalize_data(df):
	"""Normalize stock prices using the first row of the dataframe."""
	return df/ df.ix[0,:] 
	
def plot_selected(df, columns, start_index, end_index):
    """Plot the desired columns over index values in the given range."""
    # Quiz: Your code here
    plot_data(df.ix[start_index:end_index,columns], title="Selected data")
	
def plot_data(df, title="Stock prices"):
    """Plot stock prices with a custom title and meaningful axis labels."""
    ax = df.plot(title=title, fontsize=12)
    ax.set_xlabel("Date")
    ax.set_ylabel("Price")
    plt.show()


def test_run():
    # Define a date range
    dates = pd.date_range('2010-01-01', '2010-12-31')

    # Choose stock symbols to read
    symbols = ['GOOG', 'IBM', 'GLD']  # SPY will be added in get_data()
    
    # Get stock data
    df = get_data(symbols, dates)
	
    print (df)

    # Slice and plot
    plot_selected(df, ['SPY', 'IBM'], '2010-03-01', '2010-04-01')


if __name__ == "__main__":
    test_run()
```