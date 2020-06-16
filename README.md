# StockMarketDataMining
China stock market data mining. Used stocks Data in 2018-2019. Explored several signals based on Moving Average and the idea of Order Flow. 

ShangHai-Shenzhen 300 Index is an Index that covers 300 stocks in China, whose total Market Cap is roughly 80% of the Shanghai-Shenzhen Market. 

To clarify, the list of stocks I'm using is 2020-6 HS300 stocks, which is available(the current list and weights of stocks). The weight as well. The HS300 stocks only changes twice a year and doesn't change too much. 

During 2018-2019, the HS300 Index in general experienced a decline, followed by a rise, then after April 2020, the Index experienced some tumbling. The experts claims that at the end of 2019, the market was ready to embrace a new round of rise, however the COVID-19 outbreak in Januaray,2020 changed that. During the time period, the behavior of the Index is quite typical. 

# Data Dependency
I used several open sources. 
TuShare http://tushare.org/index.html
BaoStock www.baostock.com
AKShare https://www.akshare.xyz/zh_CN/latest/index.html

# Data Processing Tools
I definitely made use of popular libraries such as pandas, numpy and matplotlib.
I also used some library popular for stock market data.
TA-lib http://mrjbq7.github.io/ta-lib/

# FPMiningSignals
I made used of severl data dimensions from TA-lib, including several types of signals(EMA, DEMA) based on moving averages using different combination of Parameters(fast: 4,6,9; slow: 12, 26); Momentem Signals such as MACD(which is actually also based on Moving Average),STOCH and SAR. 

Of course all of those signals are designed to capture some trends, and most of them only happens when a rise or bounce is already happening. But will those trends lasts for 5 or 20 days, producing enough profit that is significantly different from tumbling of the market? Are they robust against tumbling? 

I performed FPGrowth(A Frequent Pattern Mining algorithm) on Itemset of signals on signals that has a future 5 day profit larger than 8 percent(which accounts for 6 percent of total data); and Itemset of signals that has a future 20 day gain larger than 20 percent; and Itemset with no significant future gain in both period(That says, it includes those that have a lose). 

FPMining on NoGain Itemset is performed with a lower frequent threshold, 0.1, while on Gain5 or Gain20 Itemsets, 0.3. All of those Frequent Patterns(signals and combination of signals) in gain are Frequent on NoGain Itemsets. However, after I sorted the patterns using Precision, the top ten patterns improved accuracy by at least 2 percent(8%) than random guess(2%), which is bad, but not terrible if you assume the rest of the stocks are random walking. 

Precision is defined: 
<img src="http://latex.codecogs.com/gif.latex? Precision = \frac{TP}{TP+FP}" />

作者：Deep Reader
链接：https://www.zhihu.com/question/26887527/answer/43166739
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
Are they Random walking though? I later made a significant loss(5 day) itemset.
I find that all of the moving average signals from the pattern we mined above has a frequency difference larger than 5% in the loss5 itemset dataset than in the gain5 dataset. This apply to SAR as well, but not STOCH(both appeared in some combinations). The frequency difference is in favor of us if we use those signals to profit. 

This is not a complete justification but can give us some taste. It might suggests that we should make use of those signals and they might help us identify trends that will last for some period(5 days). 

Why is there such a high False Positive rate though? It might be tumbling triggers the signals while followed by some fall of stock prices: the trend is not Clear. 

# Signals that were mined useful, consider use them together on stocks with clear trends.

EMA4-EMA12; EMA6-EMA12; EMA9-EMA12;EMA9-EMA26;SAR

# ExamineOrderFlow

The Idea of Order Flow is examined. 
I can only Access to 2019 tick data so that's all I used. 
The plan was to make a plot using hs300 data throughout 2019(x-axis is the porportion of days in a month that has a positive comittee; while y-axis is the percentage of increase in the next month)

The function took forever to run so I manually interrupt it after it finished roughly 15 stocks. The graph was not satisfying: No correlation at all. It is just two independent normal distributions. 
I later tried the 15 stocks that is currently weighted the most in HS300, each of them have a weight larger than 1%. The pattern shown was the same. 
This Data Dimension will not help us to capture a one month trend in the future. How much people are willing to bid in this month will not affect how the price will behave in the future month.

Be aware that there's a sharp increase of the Index before April in 2019, while for the rest of year a tumbling. So that's the reason why the mean of increase is larger than 0. 

# Currently Working On...
I'm currently writing a strategy on JQQuant that will make use of those findings. 

# Claim
I'm not responsible for ANY loss caused by operation in any market according to any mined results that was mentioned here. 
This is a personal project a college student DID FOR FUN and NOT ANY guidance to ANY exchanger.

# License
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without rest riction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
