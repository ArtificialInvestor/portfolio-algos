
#All Weather V1 - Performance Analysis (Full)

**Marcus Williamson - 08/09/15**

---
Please find the supplementary optimisation Notebooks [here](https://github.com/ArtificialInvestor/algo-optimisation/tree/master/All%20Weather%20V1)

#####Format:

1. Introduction
1. Backesting Optimised Strategies
1. Combining Optimised Strategies
1. Review of Results

##1. Introduction

This Notebook highlights the backbone to the approach of the Artificial Investor ideology:

**Combining diversified alpha generators to deliver consistent returns**

We aim to show here that very basic quantitatively driven strategies that generate returns from different market behaviours when combined can provide a more *desirable* and relaiable return stream that can be leveraged to generate a superior return to the market, although we must be clear that this is **not** the objective.

Below we compare the different strategies alone vs when combined in any particular combination is somewhat inferior to the quality of returns stream generated when all are combined together.

---
**Desirable Returns:**

Desirable returns is understandably a very subjective and a weak term, here I shall briefly outline what we determine as reliable. Risk free rate is based on US Treasuries, see [here](https://research.stlouisfed.org/fred2/categories/116). An algorithm's returns ideally have:

* High [Sharpe Ratio](https://en.wikipedia.org/wiki/Sharpe_ratio) - Deviation risk measure (Reward to variability)
* High [Stability](https://en.wikipedia.org/wiki/Coefficient_of_determination) - R-squared of a linear fit to the returns
* High [Alpha][] - Active return of investment
* High [Calmar Ratio](https://en.wikipedia.org/wiki/Calmar_ratio) - Compound annual returns divided by Max Drawdowb
* High [Sortino Ratio](https://en.wikipedia.org/wiki/Sortino_ratio) - Variation on Sharpe penilising downside volatility only
* High [Omega Ratio](https://en.wikipedia.org/wiki/Omega_ratio) - Probabilty weighted ratio of returns vs losses
* Low [Max Drawdown][] - Highest to lowest point since inception
* Low [Volatility][] - Degree of variation in returns
* Low [Beta][] - Exposure to general market movements
* Low [Kurtois](https://en.wikipedia.org/wiki/Kurtosis) - "Peakedness" of returns vs Normal Distribution (High = Fat tails
* Positive [Skew](https://en.wikipedia.org/wiki/Skewness) - "Skewdness" of returns vs Normal Distribution


The explicit valuations depend entirely on circumstances such as strategy being tested, the current market regime it is tested over, and the slippage or market impact models being implemented. However one is able to compare apples to apples when looking at the same algorithms's strategies in various combinations over the same periods. 

Please note: the securities / ETF's chosen to trade are simply for illustrative purpose, no thought has gone into what pair of stocks to trade, it has simply been taken from wikipedia.

---
**Dynamic Rebalancing**

A dynamic monthly rebalancing has been implemented for the first two strategies in this algorithm, the frequency was chosen after reviewing the peformance. This is based closely on the Kelly Criterion, looking at the covariance of returns, see more [here](https://www.quantstart.com/articles/Money-Management-via-the-Kelly-Criterion). 

In the backtests I compare the peformance of the strategies with and without this dynamic rebalancing. There has been no independent leverage control implemente which could theoretically (and should in practice) force a strategy to zero leverage if it consistently underperforms. This is something I hope to implement in my next project, along with fixing two potential problems, which I cover below.

---
**Known Issues**

* Initial investigation presents the potential issue that the daily returns may be being incorrectly calculated for the strategies which hold some positions over night. 
* Leverage control ensures the algorithm does not exceed a leverage of 1.05, however on three occasions over 8 years this leverage is excedeeded momentarily, I believe this is a bug in the timing of orders vs leverage calculation.

Although both these issues arguably have little affect to my performance it is of the upmost importance that quantitatively driven strategies have failsafe watertight architecture that **does not have bugs**. Flaws in the programming present the greatest danger to quantiatively driven models.

---
**Transaction Costs & Slippage**

* Applicable fees can be found [here](https://www.interactivebrokers.com/en/index.php?f=commission&p=stocks1&ns=T), the minimum order charge is 1 USD per order and 0.005 USD per share for fixed tier pricing
* Slippage model implemented can be read about [here](https://www.quantopian.com/help#ide-slippage)

In both cases the actual algorithm contains more conservative parameters for costs and slippage (per share costs is 200% above fixed rate), in order to create a realistic performance in live foward test with funding. 

[Alpha]:https://en.wikipedia.org/wiki/Alpha_(finance)
[Max Drawdown]:https://en.wikipedia.org/wiki/Drawdown_(economics)
[Volatility]:https://en.wikipedia.org/wiki/Volatility_(finance)
[Beta]:https://en.wikipedia.org/wiki/Beta_(finance)

##2. Backtesting Optimised Strategies

The original algorithm has been optimised over a total six year period in two three year segments, the parameters have been selected with a focus on consistency and stability of returns rather than maximal performance 

**In Sample Backtest:** 01/01/07 - 01/01/13

**Out-of-sample Backtest:** 01/01/13 - 01/01/15

###Mean Reversion

Designed to generate returns in QE tainted environments under the assumption that a stock's price will tend to move to the average price over time. See more [here](https://en.wikipedia.org/wiki/Mean_reversion_(finance).


    #2007-2013 In-sample
    mean1 = get_backtest('55f7eb5cb8d4ac0e0cdb1d15')
    mean1.create_full_tear_sheet()

    100% Time: 0:00:11|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.08
    annual_volatility      0.19
    sharpe_ratio           0.45
    calmar_ratio           0.25
    stability              0.43
    max_drawdown          -0.33
    omega_ratio            1.08
    sortino_ratio          0.73
    skewness               0.43
    kurtosis               3.54
    alpha                  0.09
    beta                  -0.12
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              33.45  2009-03-23 00:00:00+00:00  2011-04-19 00:00:00+00:00   
    4              14.72  2008-01-24 00:00:00+00:00  2008-09-16 00:00:00+00:00   
    1              13.46  2011-10-12 00:00:00+00:00  2012-03-02 00:00:00+00:00   
    2              13.43  2008-11-06 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    3              12.02  2009-01-14 00:00:00+00:00  2009-01-23 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-10-10 00:00:00+00:00      666  
    4  2008-10-09 00:00:00+00:00      186  
    1                        NaN      NaN  
    2  2009-01-02 00:00:00+00:00       42  
    3  2009-03-18 00:00:00+00:00       46  
    
    
    2-sigma returns daily    -0.023
    2-sigma returns weekly   -0.045
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.001 -0.031  0.045
    US downgrade/European Debt Crisis  0.007 -0.033  0.053
    Fukushima                         -0.001 -0.022  0.018
    EZB IR Event                      -0.000 -0.009  0.016
    Aug07                              0.000 -0.034  0.015
    Sept08                            -0.002 -0.031  0.045
    2009Q1                             0.000 -0.048  0.034
    2009Q2                             0.000 -0.027  0.044
    Flash Crash                        0.005 -0.031  0.033
    
    
    Top 10 long positions of all time (and max%)
    [u'SPY']
    [ 1.]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY']
    [-0.337]
    
    
    Top 10 positions of all time (and max%)
    [u'SPY']
    [ 1.]
    
    
    All positions ever held
    [u'SPY']
    [ 1.]
    
    


    /usr/local/lib/python2.7/dist-packages/matplotlib/cbook.py:133: MatplotlibDeprecationWarning: The "loc" positional argument to legend is deprecated. Please use the "loc" keyword instead.
      warnings.warn(message, mplDeprecation, stacklevel=1)



![png](output_4_2.png)



![png](output_4_3.png)



![png](output_4_4.png)



![png](output_4_5.png)


The strategy has a naturally contrarian approach, it appears to profit well from sudden drops, however gives back much of its gains in trending periods. This could clearly be enhanced with a more sophisticated reversion model along with absolute stoplosses and order feathering.


    #2007-2015 In-sample + Out-of-sample
    mean2 = get_backtest('55f7eb7148636b0df3a8c61f')
    mean2.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:14|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.08           0.05         0.08
    annual_volatility      0.19           0.10         0.17
    sharpe_ratio           0.45           0.51         0.45
    calmar_ratio           0.25           0.55         0.23
    stability              0.43           0.02         0.66
    max_drawdown          -0.33          -0.09        -0.33
    omega_ratio            1.08           1.09         1.08
    sortino_ratio          0.73           0.96         0.72
    skewness               0.43           0.62         0.47
    kurtosis               3.54           1.83         4.38
    alpha                  0.09           0.07         0.08
    beta                  -0.12          -0.09        -0.12
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              33.45  2009-03-23 00:00:00+00:00  2011-04-19 00:00:00+00:00   
    4              14.72  2008-01-24 00:00:00+00:00  2008-09-16 00:00:00+00:00   
    1              13.46  2011-10-12 00:00:00+00:00  2012-03-02 00:00:00+00:00   
    2              13.43  2008-11-06 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    3              12.02  2009-01-14 00:00:00+00:00  2009-01-23 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-10-10 00:00:00+00:00      666  
    4  2008-10-09 00:00:00+00:00      186  
    1  2013-01-25 00:00:00+00:00      338  
    2  2009-01-02 00:00:00+00:00       42  
    3  2009-03-18 00:00:00+00:00       46  
    
    
    2-sigma returns daily    -0.021
    2-sigma returns weekly   -0.041
    dtype: float64



![png](output_6_1.png)


The strategy struggles with the bull run in 2013, however generates considerable retuns over the October 15 Treasury flash crash event. Simialrity of returns are good, we can see the distribution curves fitting one another fairly well with tighter tails and more peak suggesting a generally improved performance over the Out-of-sample period. 

###Momentum

Generates returns from market trends, it works especially well under sustained periods of market growth. Read more [here](https://en.wikipedia.org/wiki/Momentum_investing).


    #2007-2013 In-sample
    momo1 = get_backtest('55f7eb89237b0c0e09e64a91')
    momo1.create_full_tear_sheet()

    100% Time: 0:00:07|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.12
    annual_volatility      0.17
    sharpe_ratio           0.70
    calmar_ratio           0.43
    stability              0.85
    max_drawdown          -0.27
    omega_ratio            1.15
    sortino_ratio          0.96
    skewness               0.07
    kurtosis               3.12
    alpha                  0.10
    beta                   0.41
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              27.46  2007-10-31 00:00:00+00:00  2009-01-30 00:00:00+00:00   
    1              11.69  2011-02-16 00:00:00+00:00  2011-06-24 00:00:00+00:00   
    4               9.75  2011-10-28 00:00:00+00:00  2011-12-21 00:00:00+00:00   
    2               9.46  2012-04-02 00:00:00+00:00  2012-07-25 00:00:00+00:00   
    3               8.98  2012-09-19 00:00:00+00:00  2012-12-28 00:00:00+00:00   
    
                   recovery date duration  
    0  2009-07-23 00:00:00+00:00      452  
    1  2011-10-14 00:00:00+00:00      173  
    4  2012-02-03 00:00:00+00:00       71  
    2  2012-09-06 00:00:00+00:00      114  
    3                        NaN      NaN  
    
    
    2-sigma returns daily    -0.02
    2-sigma returns weekly   -0.04
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.000 -0.025  0.035
    US downgrade/European Debt Crisis  0.001 -0.022  0.029
    Fukushima                          0.000 -0.006  0.012
    EZB IR Event                      -0.001 -0.014  0.014
    Aug07                              0.000 -0.022  0.019
    Sept08                            -0.001 -0.025  0.000
    2009Q1                            -0.001 -0.038  0.043
    2009Q2                             0.004 -0.033  0.060
    Flash Crash                        0.000  0.000  0.000
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ']
    [ 1.]
    
    
    Top 10 short positions of all time (and max%)
    []
    []
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ']
    [ 1.]
    
    
    All positions ever held
    [u'QQQ']
    [ 1.]
    
    



![png](output_9_1.png)



![png](output_9_2.png)



![png](output_9_3.png)



![png](output_9_4.png)


This strategy monopolises on bullish trends, its stoploss design prevented it from experiencing a large drawdown over the subprime crisis. Over various events the returns are largely flat as the stoploss is hit to prevent losses.


    #2007-2015 In-sample + Out-of-sample
    momo2 = get_backtest('55f7ebc622161c0e00989816')
    momo2.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:09|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.12           0.14         0.12
    annual_volatility      0.17           0.11         0.15
    sharpe_ratio           0.70           1.25         0.79
    calmar_ratio           0.43           1.67         0.45
    stability              0.85           0.89         0.93
    max_drawdown          -0.27          -0.08        -0.27
    omega_ratio            1.15           1.24         1.16
    sortino_ratio          0.96           1.76         1.08
    skewness               0.07          -0.36         0.03
    kurtosis               3.12           1.45         3.45
    alpha                  0.10          -0.01         0.09
    beta                   0.41           0.81         0.43
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              27.46  2007-10-31 00:00:00+00:00  2009-01-30 00:00:00+00:00   
    2              11.69  2011-02-16 00:00:00+00:00  2011-06-24 00:00:00+00:00   
    3               9.46  2012-04-02 00:00:00+00:00  2012-07-25 00:00:00+00:00   
    1               8.43  2014-09-18 00:00:00+00:00  2014-10-10 00:00:00+00:00   
    4               7.73  2014-03-05 00:00:00+00:00  2014-05-08 00:00:00+00:00   
    
                   recovery date duration  
    0  2009-07-23 00:00:00+00:00      452  
    2  2011-10-14 00:00:00+00:00      173  
    3  2012-09-06 00:00:00+00:00      114  
    1  2014-11-24 00:00:00+00:00       48  
    4  2014-06-27 00:00:00+00:00       83  
    
    
    2-sigma returns daily    -0.019
    2-sigma returns weekly   -0.037
    dtype: float64



![png](output_11_1.png)


The momentum strategy performs better than estimated over the out-of-sample period, as it can be seen filling the upper half of the cone.

###Pairs Trading

Trading the converging relationship between two correlated stocks in attempt to hedge market and sector risk (market neutral position). This provides a return stream without any major trend / reversion taking place. Read more [here](https://en.wikipedia.org/wiki/Pairs_trade)


    #2007-2013 In-sample
    pair1 = get_backtest('55f7ebfbb8d4ac0e0cdb1d23')
    pair1.create_full_tear_sheet()

    100% Time: 0:00:06|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.04
    annual_volatility      0.07
    sharpe_ratio           0.52
    calmar_ratio           0.25
    stability              0.68
    max_drawdown          -0.14
    omega_ratio            1.11
    sortino_ratio          0.69
    skewness               0.36
    kurtosis               6.83
    alpha                  0.04
    beta                  -0.01
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              14.25  2010-10-14 00:00:00+00:00  2011-07-26 00:00:00+00:00   
    1               8.01  2008-06-30 00:00:00+00:00  2008-10-16 00:00:00+00:00   
    3               5.00  2007-03-05 00:00:00+00:00  2007-06-29 00:00:00+00:00   
    2               4.86  2009-08-31 00:00:00+00:00  2010-03-17 00:00:00+00:00   
    4               4.58  2007-10-11 00:00:00+00:00  2008-01-31 00:00:00+00:00   
    
                   recovery date duration  
    0                        NaN      NaN  
    1  2009-02-12 00:00:00+00:00      164  
    3  2007-09-11 00:00:00+00:00      137  
    2  2010-10-07 00:00:00+00:00      289  
    4  2008-04-21 00:00:00+00:00      138  
    
    
    2-sigma returns daily    -0.008
    2-sigma returns weekly   -0.017
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.000 -0.011  0.009
    US downgrade/European Debt Crisis  0.001 -0.002  0.005
    Fukushima                          0.001 -0.006  0.011
    EZB IR Event                      -0.000 -0.005  0.004
    Aug07                              0.001 -0.006  0.013
    Sept08                            -0.001 -0.011  0.007
    2009Q1                             0.001 -0.008  0.024
    2009Q2                             0.001 -0.013  0.022
    Flash Crash                        0.000 -0.006  0.008
    
    
    Top 10 long positions of all time (and max%)
    [u'PEP' u'KO']
    [ 0.268  0.266]
    
    
    Top 10 short positions of all time (and max%)
    [u'KO' u'PEP']
    [-0.269 -0.264]
    
    
    Top 10 positions of all time (and max%)
    [u'KO' u'PEP']
    [ 0.269  0.268]
    
    
    All positions ever held
    [u'KO' u'PEP']
    [ 0.269  0.268]
    
    



![png](output_14_1.png)



![png](output_14_2.png)



![png](output_14_3.png)



![png](output_14_4.png)


The pairs trading strategy performs well over the duration of the backtest, it experiences one major drawdown, otherwise it is not considerably effeted by general marktet direction.


    #2007-2015 In-sample + Out-of-sample
    pair2 = get_backtest('55f7ec07b8d4ac0e0cdb1d26')
    pair2.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:08|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.04           0.07         0.04
    annual_volatility      0.07           0.06         0.07
    sharpe_ratio           0.52           1.17         0.67
    calmar_ratio           0.25           1.33         0.31
    stability              0.68           0.45         0.84
    max_drawdown          -0.14          -0.05        -0.14
    omega_ratio            1.11           1.29         1.15
    sortino_ratio          0.69           1.86         0.91
    skewness               0.36           2.01         0.67
    kurtosis               6.83          15.68         8.51
    alpha                  0.04           0.06         0.04
    beta                  -0.01           0.06        -0.00
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              14.25  2010-10-14 00:00:00+00:00  2011-07-26 00:00:00+00:00   
    1               8.01  2008-06-30 00:00:00+00:00  2008-10-16 00:00:00+00:00   
    2               5.36  2013-09-03 00:00:00+00:00  2014-10-08 00:00:00+00:00   
    4               5.00  2007-03-05 00:00:00+00:00  2007-06-29 00:00:00+00:00   
    3               4.86  2009-08-31 00:00:00+00:00  2010-03-17 00:00:00+00:00   
    
                   recovery date duration  
    0  2013-01-08 00:00:00+00:00      584  
    1  2009-02-12 00:00:00+00:00      164  
    2  2014-11-11 00:00:00+00:00      311  
    4  2007-09-11 00:00:00+00:00      137  
    3  2010-10-07 00:00:00+00:00      289  
    
    
    2-sigma returns daily    -0.008
    2-sigma returns weekly   -0.016
    dtype: float64



![png](output_16_1.png)


Pairs strategy, like the momentum strategy, outperforms its expectations in the out of sample period. The returns distribution show some heavy tails for backtest and out-of-sample, this is not ideal, but the skew is positive.

##3. Combined Strategies

Here we examine the performance of the strategies when combined with and without the dynamic rebalancing. The advantage of stacking these strategies, as covered previously, are that the returns are diversified and stabilisied with the ability to generate consistent returns over varying market behaviour.

####Mean Reversion & Momentum - No Rebalance


    #2007-2013 In-sample
    meanmomo1 = get_backtest('55f7ec202c02340df43a11dc')
    meanmomo1.create_full_tear_sheet()

    100% Time: 0:00:10|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.09
    annual_volatility      0.12
    sharpe_ratio           0.71
    calmar_ratio           0.65
    stability              0.90
    max_drawdown          -0.13
    omega_ratio            1.14
    sortino_ratio          1.21
    skewness               0.64
    kurtosis               3.48
    alpha                  0.08
    beta                   0.10
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              13.32  2008-01-24 00:00:00+00:00  2008-09-16 00:00:00+00:00   
    1               9.14  2010-09-24 00:00:00+00:00  2011-06-23 00:00:00+00:00   
    2               8.87  2008-11-04 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    4               6.71  2009-11-17 00:00:00+00:00  2010-06-09 00:00:00+00:00   
    3               5.71  2011-10-14 00:00:00+00:00  2011-11-16 00:00:00+00:00   
    
                   recovery date duration  
    0  2008-10-31 00:00:00+00:00      202  
    1  2011-08-08 00:00:00+00:00      227  
    2  2009-01-06 00:00:00+00:00       46  
    4  2011-08-15 00:00:00+00:00      455  
    3  2012-02-09 00:00:00+00:00       85  
    
    
    2-sigma returns daily    -0.015
    2-sigma returns weekly   -0.028
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.000 -0.016  0.023
    US downgrade/European Debt Crisis  0.004 -0.018  0.028
    Fukushima                         -0.000 -0.011  0.011
    EZB IR Event                      -0.000 -0.006  0.013
    Aug07                             -0.000 -0.017  0.013
    Sept08                            -0.002 -0.016  0.023
    2009Q1                            -0.000 -0.024  0.037
    2009Q2                             0.002 -0.019  0.038
    Flash Crash                        0.002 -0.016  0.017
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ' u'SPY']
    [ 0.568  0.515]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY']
    [-0.252]
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ' u'SPY']
    [ 0.568  0.515]
    
    
    All positions ever held
    [u'QQQ' u'SPY']
    [ 0.568  0.515]
    
    



![png](output_20_1.png)



![png](output_20_2.png)



![png](output_20_3.png)



![png](output_20_4.png)


The combination of Mean reversion and Momentum provides a complementary returns stream which delivers on violent drops and trending periods.


    #2007-2015 In-sample + Out-of-sample
    meanmomo2 = get_backtest('55f7ec30237b0c0e09e64a94')
    meanmomo2.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:13|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.09           0.08         0.08
    annual_volatility      0.12           0.07         0.11
    sharpe_ratio           0.71           1.14         0.76
    calmar_ratio           0.65           1.24         0.63
    stability              0.90           0.73         0.95
    max_drawdown          -0.13          -0.06        -0.13
    omega_ratio            1.14           1.21         1.15
    sortino_ratio          1.21           2.19         1.28
    skewness               0.64           0.78         0.68
    kurtosis               3.48           2.59         4.26
    alpha                  0.08           0.02         0.08
    beta                   0.10           0.29         0.12
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              13.32  2008-01-24 00:00:00+00:00  2008-09-16 00:00:00+00:00   
    1               9.14  2010-09-24 00:00:00+00:00  2011-06-23 00:00:00+00:00   
    3               8.87  2008-11-04 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    2               6.33  2014-02-18 00:00:00+00:00  2014-05-09 00:00:00+00:00   
    4               5.71  2011-10-14 00:00:00+00:00  2011-11-16 00:00:00+00:00   
    
                   recovery date duration  
    0  2008-10-31 00:00:00+00:00      202  
    1  2011-08-08 00:00:00+00:00      227  
    3  2009-01-06 00:00:00+00:00       46  
    2  2014-10-31 00:00:00+00:00      184  
    4  2012-02-09 00:00:00+00:00       85  
    
    
    2-sigma returns daily    -0.014
    2-sigma returns weekly   -0.026
    dtype: float64



![png](output_22_1.png)


Out-of-sample has a slightly wider distribution of daily returns, this is not too much of a problem as when viewed with variance normalisation.

####Mean Reversion & Momentum - Dynamic Rebalancing


    #2007-2013 In-sample
    meanmomo3 = get_backtest('55f7ec62c9cc280e05fd1687')
    meanmomo3.create_full_tear_sheet()

    100% Time: 0:00:11|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.07
    annual_volatility      0.14
    sharpe_ratio           0.53
    calmar_ratio           0.39
    stability              0.71
    max_drawdown          -0.19
    omega_ratio            1.10
    sortino_ratio          0.91
    skewness               0.51
    kurtosis               2.13
    alpha                  0.07
    beta                   0.05
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              18.92  2009-11-11 00:00:00+00:00  2011-06-23 00:00:00+00:00   
    1              14.97  2009-03-23 00:00:00+00:00  2009-07-06 00:00:00+00:00   
    3               9.73  2008-01-24 00:00:00+00:00  2008-03-20 00:00:00+00:00   
    2               7.62  2011-10-12 00:00:00+00:00  2011-11-16 00:00:00+00:00   
    4               4.61  2011-10-11 00:00:00+00:00  2012-05-03 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-09-22 00:00:00+00:00      487  
    1  2009-11-09 00:00:00+00:00      166  
    3  2008-10-30 00:00:00+00:00      201  
    2  2011-12-05 00:00:00+00:00       39  
    4  2012-07-03 00:00:00+00:00      191  
    
    
    2-sigma returns daily    -0.017
    2-sigma returns weekly   -0.033
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.000 -0.015  0.018
    US downgrade/European Debt Crisis  0.005 -0.018  0.037
    Fukushima                         -0.000 -0.016  0.013
    EZB IR Event                      -0.000 -0.006  0.012
    Aug07                              0.000 -0.024  0.013
    Sept08                            -0.002 -0.015  0.018
    2009Q1                            -0.000 -0.026  0.031
    2009Q2                             0.001 -0.026  0.042
    Flash Crash                        0.004 -0.023  0.026
    
    
    Top 10 long positions of all time (and max%)
    [u'SPY' u'QQQ' u'PEP' u'KO']
    [ 0.852  0.506  0.225  0.203]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.328 -0.23  -0.207]
    
    
    Top 10 positions of all time (and max%)
    [u'SPY' u'QQQ' u'KO' u'PEP']
    [ 0.852  0.506  0.23   0.225]
    
    
    All positions ever held
    [u'SPY' u'QQQ' u'KO' u'PEP']
    [ 0.852  0.506  0.23   0.225]
    
    


    /usr/local/lib/python2.7/dist-packages/pyfolio/plotting.py:833: UserWarning: Negative cash, taking absolute for area plot.
      warnings.warn('Negative cash, taking absolute for area plot.')



![png](output_25_2.png)



![png](output_25_3.png)



![png](output_25_4.png)



![png](output_25_5.png)


The rebalancing version of the Mean Reversion & Momentum strategy acutally underperforms the non rebalanced strategy. This may be due to the optimisation taking place without dynamic rebalancing in effect.


    #2007-2015 In-sample + Out-of-sample
    meanmomo4 = get_backtest('55f7ec7bae2b840e1335d773')
    meanmomo4.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:14|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.10           0.11         0.10
    annual_volatility      0.11           0.07         0.10
    sharpe_ratio           0.92           1.57         1.02
    calmar_ratio           0.81           1.60         0.84
    stability              0.89           0.64         0.94
    max_drawdown          -0.12          -0.07        -0.12
    omega_ratio            1.19           1.34         1.22
    sortino_ratio          1.41           3.08         1.58
    skewness               0.45           2.06         0.60
    kurtosis               7.24          14.33         8.54
    alpha                  0.10           0.06         0.10
    beta                   0.06           0.25         0.07
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              12.24  2010-09-24 00:00:00+00:00  2011-07-25 00:00:00+00:00   
    3               9.61  2007-12-10 00:00:00+00:00  2008-03-20 00:00:00+00:00   
    2               9.50  2008-11-06 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    1               6.91  2014-07-03 00:00:00+00:00  2014-10-10 00:00:00+00:00   
    4               4.46  2013-11-14 00:00:00+00:00  2014-02-05 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-09-07 00:00:00+00:00      249  
    3  2008-10-09 00:00:00+00:00      219  
    2  2009-01-02 00:00:00+00:00       42  
    1  2014-10-30 00:00:00+00:00       86  
    4  2014-10-31 00:00:00+00:00      252  
    
    
    2-sigma returns daily    -0.012
    2-sigma returns weekly   -0.024
    dtype: float64



![png](output_27_1.png)


A solid performance is seen here with the algorithm underperforming around Oct 15th as the only major issue.

####Momentum & Pairs Trading


    #2007-2013 In-sample
    momopair1 = get_backtest('55f7eca18e304c0e0dde2558')
    momopair1.create_full_tear_sheet()

    100% Time: 0:00:07|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.08
    annual_volatility      0.09
    sharpe_ratio           0.85
    calmar_ratio           0.64
    stability              0.91
    max_drawdown          -0.12
    omega_ratio            1.16
    sortino_ratio          1.24
    skewness               0.04
    kurtosis               2.23
    alpha                  0.07
    beta                   0.20
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              11.84  2007-12-10 00:00:00+00:00  2009-01-30 00:00:00+00:00   
    1               6.63  2011-02-16 00:00:00+00:00  2011-08-03 00:00:00+00:00   
    2               5.55  2012-04-02 00:00:00+00:00  2012-07-25 00:00:00+00:00   
    3               4.08  2010-04-15 00:00:00+00:00  2010-08-19 00:00:00+00:00   
    4               3.67  2010-11-04 00:00:00+00:00  2010-11-30 00:00:00+00:00   
    
                   recovery date duration  
    0  2009-04-17 00:00:00+00:00      355  
    1  2011-10-14 00:00:00+00:00      173  
    2                        NaN      NaN  
    3  2010-09-17 00:00:00+00:00      112  
    4  2011-02-08 00:00:00+00:00       69  
    
    
    2-sigma returns daily    -0.011
    2-sigma returns weekly   -0.021
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.000 -0.014  0.016
    US downgrade/European Debt Crisis  0.001 -0.010  0.015
    Fukushima                          0.001 -0.005  0.008
    EZB IR Event                      -0.001 -0.007  0.007
    Aug07                              0.001 -0.010  0.008
    Sept08                            -0.001 -0.014  0.004
    2009Q1                            -0.000 -0.020  0.019
    2009Q2                             0.002 -0.020  0.029
    Flash Crash                        0.000 -0.003  0.004
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ' u'KO' u'PEP']
    [ 0.531  0.181  0.176]
    
    
    Top 10 short positions of all time (and max%)
    [u'KO' u'PEP']
    [-0.177 -0.177]
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ' u'KO' u'PEP']
    [ 0.531  0.181  0.177]
    
    
    All positions ever held
    [u'QQQ' u'KO' u'PEP']
    [ 0.531  0.181  0.177]
    
    



![png](output_30_1.png)



![png](output_30_2.png)



![png](output_30_3.png)



![png](output_30_4.png)


Again an enhanced performance as a result of combining the two diversified strategies, in this case the Pairs strategy helps to make the returns stream more consistent


    #2007-2015 In-sample + Out-of-sample
    momopair2 = get_backtest('55f7ecc4b8d4ac0e0cdb1d32')
    momopair2.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:09|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.08           0.11         0.08
    annual_volatility      0.09           0.07         0.08
    sharpe_ratio           0.85           1.61         0.99
    calmar_ratio           0.64           1.92         0.71
    stability              0.91           0.91         0.96
    max_drawdown          -0.12          -0.06        -0.12
    omega_ratio            1.16           1.32         1.19
    sortino_ratio          1.24           2.43         1.44
    skewness               0.04           0.11         0.04
    kurtosis               2.23           2.77         2.54
    alpha                  0.07           0.02         0.07
    beta                   0.20           0.44         0.22
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              11.84  2007-12-10 00:00:00+00:00  2009-01-30 00:00:00+00:00   
    2               6.63  2011-02-16 00:00:00+00:00  2011-08-03 00:00:00+00:00   
    1               5.55  2014-09-18 00:00:00+00:00  2014-10-15 00:00:00+00:00   
    3               5.55  2012-04-02 00:00:00+00:00  2012-07-25 00:00:00+00:00   
    4               3.36  2014-03-05 00:00:00+00:00  2014-04-25 00:00:00+00:00   
    
                   recovery date duration  
    0  2009-04-17 00:00:00+00:00      355  
    2  2011-10-14 00:00:00+00:00      173  
    1  2014-11-10 00:00:00+00:00       38  
    3  2013-02-12 00:00:00+00:00      227  
    4  2014-06-05 00:00:00+00:00       67  
    
    
    2-sigma returns daily    -0.01
    2-sigma returns weekly   -0.02
    dtype: float64



![png](output_32_1.png)


The combined strategy occupies the upper half of the cone as it continues to perform well in the bull market stretch.

####Mean Reversion & Pairs Trading


    #2007-2013 In-sample
    meanpair1 = get_backtest('55f7ecd822161c0e0098982d')
    meanpair1.create_full_tear_sheet()

    100% Time: 0:00:10|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.06
    annual_volatility      0.10
    sharpe_ratio           0.60
    calmar_ratio           0.34
    stability              0.64
    max_drawdown          -0.17
    omega_ratio            1.11
    sortino_ratio          0.97
    skewness               0.39
    kurtosis               3.32
    alpha                  0.06
    beta                  -0.06
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              17.16  2009-11-11 00:00:00+00:00  2011-06-23 00:00:00+00:00   
    1               8.50  2009-03-18 00:00:00+00:00  2009-07-06 00:00:00+00:00   
    3               6.94  2008-11-06 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    2               6.19  2011-12-05 00:00:00+00:00  2012-03-02 00:00:00+00:00   
    4               6.19  2009-01-14 00:00:00+00:00  2009-01-23 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-10-03 00:00:00+00:00      494  
    1  2011-10-06 00:00:00+00:00      667  
    3  2009-01-02 00:00:00+00:00       42  
    2  2012-08-02 00:00:00+00:00      174  
    4  2009-03-12 00:00:00+00:00       42  
    
    
    2-sigma returns daily    -0.012
    2-sigma returns weekly   -0.024
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.000 -0.015  0.018
    US downgrade/European Debt Crisis  0.004 -0.017  0.027
    Fukushima                         -0.000 -0.011  0.007
    EZB IR Event                      -0.000 -0.007  0.008
    Aug07                              0.000 -0.017  0.010
    Sept08                            -0.001 -0.015  0.018
    2009Q1                             0.001 -0.024  0.017
    2009Q2                             0.000 -0.014  0.022
    Flash Crash                        0.003 -0.016  0.021
    
    
    Top 10 long positions of all time (and max%)
    [u'SPY' u'PEP' u'KO']
    [ 0.528  0.178  0.173]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.252 -0.183 -0.175]
    
    
    Top 10 positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [ 0.528  0.183  0.178]
    
    
    All positions ever held
    [u'SPY' u'KO' u'PEP']
    [ 0.528  0.183  0.178]
    
    



![png](output_35_1.png)



![png](output_35_2.png)



![png](output_35_3.png)



![png](output_35_4.png)


The combination of Mean reversion and Pairs strategy is less stable with larger swings, however it provides a negatively correlated returns stream as reflected with its negative Beta


    #2007-2015 In-sample + Out-of-sample
    meanpair2 = get_backtest('55f7ecf148636b0df3a8c632')
    meanpair2.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:13|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.06           0.06         0.06
    annual_volatility      0.10           0.06         0.09
    sharpe_ratio           0.60           1.00         0.65
    calmar_ratio           0.34           0.99         0.34
    stability              0.64           0.27         0.81
    max_drawdown          -0.17          -0.06        -0.17
    omega_ratio            1.11           1.19         1.12
    sortino_ratio          0.97           2.01         1.06
    skewness               0.39           1.16         0.46
    kurtosis               3.32           5.01         4.09
    alpha                  0.06           0.06         0.06
    beta                  -0.06          -0.02        -0.06
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              17.16  2009-11-11 00:00:00+00:00  2011-06-23 00:00:00+00:00   
    1               8.50  2009-03-18 00:00:00+00:00  2009-07-06 00:00:00+00:00   
    4               6.94  2008-11-06 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    3               6.19  2011-12-05 00:00:00+00:00  2012-03-02 00:00:00+00:00   
    2               6.02  2013-07-15 00:00:00+00:00  2014-10-08 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-10-03 00:00:00+00:00      494  
    1  2011-10-06 00:00:00+00:00      667  
    4  2009-01-02 00:00:00+00:00       42  
    3  2012-08-02 00:00:00+00:00      174  
    2  2014-10-30 00:00:00+00:00      339  
    
    
    2-sigma returns daily    -0.011
    2-sigma returns weekly   -0.022
    dtype: float64



![png](output_37_1.png)


Performance is average over the out-of-sample period, there is somewhat of a fat-tail to the upside.

###All Strategies - Mean Reversion, Momentum, Pairs Trading - No Rebalance


    #2007-2013 In-sample
    stack1 = get_backtest('55f7ed5348636b0df3a8c637')
    stack1.create_full_tear_sheet()

    100% Time: 0:00:10|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 71
                       Backtest
    annual_return          0.07
    annual_volatility      0.08
    sharpe_ratio           0.84
    calmar_ratio           0.89
    stability              0.92
    max_drawdown          -0.08
    omega_ratio            1.16
    sortino_ratio          1.40
    skewness               0.53
    kurtosis               3.05
    alpha                  0.07
    beta                   0.07
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0               7.96  2010-09-24 00:00:00+00:00  2011-06-23 00:00:00+00:00   
    1               7.60  2008-01-04 00:00:00+00:00  2008-03-20 00:00:00+00:00   
    2               6.08  2008-11-04 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    3               5.03  2009-05-06 00:00:00+00:00  2009-07-06 00:00:00+00:00   
    4               4.63  2009-11-17 00:00:00+00:00  2010-02-12 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-08-29 00:00:00+00:00      242  
    1  2008-10-28 00:00:00+00:00      213  
    2  2009-01-06 00:00:00+00:00       46  
    3  2009-07-20 00:00:00+00:00       54  
    4  2010-09-20 00:00:00+00:00      220  
    
    
    2-sigma returns daily    -0.01
    2-sigma returns weekly   -0.02
    dtype: float64
    
    Stress Events
                                        mean    min    max
    Lehmann                           -0.000 -0.010  0.012
    US downgrade/European Debt Crisis  0.003 -0.012  0.019
    Fukushima                          0.000 -0.007  0.008
    EZB IR Event                      -0.000 -0.005  0.009
    Aug07                              0.000 -0.011  0.008
    Sept08                            -0.001 -0.010  0.012
    2009Q1                             0.000 -0.016  0.022
    2009Q2                             0.001 -0.016  0.023
    Flash Crash                        0.002 -0.011  0.014
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ' u'SPY' u'PEP' u'KO']
    [ 0.361  0.353  0.132  0.129]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.201 -0.135 -0.131]
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.361  0.353  0.135  0.132]
    
    
    All positions ever held
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.361  0.353  0.135  0.132]
    
    



![png](output_40_1.png)



![png](output_40_2.png)



![png](output_40_3.png)



![png](output_40_4.png)


Without rebalancing we see the combination of all three strategies yeilding a smoother returns stream apart from the vigerous returns over mid 2011, where the effects of the mean reversion have contributed.


    #2007-2015 In-sample + Out-of-sample
    stack2 = get_backtest('55f7ed708e304c0e0dde256a')
    stack2.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:14|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.07           0.08         0.07
    annual_volatility      0.08           0.05         0.08
    sharpe_ratio           0.84           1.50         0.93
    calmar_ratio           0.89           1.80         0.91
    stability              0.92           0.77         0.96
    max_drawdown          -0.08          -0.04        -0.08
    omega_ratio            1.16           1.30         1.19
    sortino_ratio          1.40           2.97         1.56
    skewness               0.53           1.15         0.60
    kurtosis               3.05           5.15         3.77
    alpha                  0.07           0.04         0.07
    beta                   0.07           0.23         0.08
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0               7.96  2010-09-24 00:00:00+00:00  2011-06-23 00:00:00+00:00   
    1               7.60  2008-01-04 00:00:00+00:00  2008-03-20 00:00:00+00:00   
    3               6.08  2008-11-04 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    4               5.03  2009-05-06 00:00:00+00:00  2009-07-06 00:00:00+00:00   
    2               4.35  2014-07-03 00:00:00+00:00  2014-10-09 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-08-29 00:00:00+00:00      242  
    1  2008-10-28 00:00:00+00:00      213  
    3  2009-01-06 00:00:00+00:00       46  
    4  2009-07-20 00:00:00+00:00       54  
    2  2014-10-24 00:00:00+00:00       82  
    
    
    2-sigma returns daily    -0.010
    2-sigma returns weekly   -0.018
    dtype: float64



![png](output_42_1.png)


###All Strategies - Mean Reversion, Momentum, Pairs Trading - Dynamic Rebalancing


    #2007-2015 In-sample + Out-of-sample
    stack4 = get_backtest('55f7edaaae2b840e1335d784')
    stack4.create_returns_tear_sheet(live_start_date='2013-1-1')

    100% Time: 0:00:13|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Out-of-Sample Months: 24
    Backtest Months: 71
                       Backtest  Out_of_Sample  All_History
    annual_return          0.08           0.09         0.08
    annual_volatility      0.08           0.05         0.08
    sharpe_ratio           0.95           1.64         1.06
    calmar_ratio           0.92           1.73         0.95
    stability              0.91           0.75         0.95
    max_drawdown          -0.09          -0.05        -0.09
    omega_ratio            1.19           1.35         1.22
    sortino_ratio          1.46           3.10         1.64
    skewness               0.42           1.60         0.53
    kurtosis               5.49          10.57         6.53
    alpha                  0.08           0.04         0.07
    beta                   0.08           0.25         0.09
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    1               8.50  2007-12-10 00:00:00+00:00  2008-03-20 00:00:00+00:00   
    0               7.42  2010-09-24 00:00:00+00:00  2011-07-25 00:00:00+00:00   
    3               6.94  2008-11-04 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    2               5.09  2014-07-03 00:00:00+00:00  2014-10-10 00:00:00+00:00   
    4               3.08  2013-11-29 00:00:00+00:00  2014-02-05 00:00:00+00:00   
    
                   recovery date duration  
    1  2008-10-27 00:00:00+00:00      231  
    0  2011-08-31 00:00:00+00:00      244  
    3  2009-01-02 00:00:00+00:00       44  
    2  2014-10-30 00:00:00+00:00       86  
    4  2014-07-01 00:00:00+00:00      153  
    
    
    2-sigma returns daily    -0.009
    2-sigma returns weekly   -0.018
    dtype: float64



![png](output_44_1.png)


We can clearly see a strong performance from the three strategies combined, returns are steady and consistent. Backtest vs out-of-sample returns are very similar which is a good confirmation of consistent strategy logic.


    #Bayesian tear sheet
    stack4.create_bayesian_tear_sheet(live_start_date='2013-1-1')

     [-----------------100%-----------------] 2000 of 2000 complete in 7.1 sec

    /usr/local/lib/python2.7/dist-packages/theano/scan_module/scan_perform_ext.py:135: RuntimeWarning: numpy.ndarray size changed, may indicate binary incompatibility
      from scan_perform.scan_perform import *
    /usr/local/lib/python2.7/dist-packages/matplotlib/axes/_axes.py:475: UserWarning: No labelled objects found. Use label='...' kwarg on individual plots.
      warnings.warn("No labelled objects found. "


     [-----------------100%-----------------] 2000 of 2000 complete in 3.7 sec


![png](output_46_3.png)


Bayesian analysis reveals some more insightful similarities and in some cases unsimilarities between the returns in the backtest and out-of-sample period. We can see that the out-of-sample period was well suited to the strategies optimisations. Considering the strategies were optimised over the subprime crisis and the bull market following, which is two quite diverse periods, this is good news.

**Further review of performance over each of the following periods:**

* 2007-2010 - In-sample
* 2010-2013 - In-sample
* 2013-2015 - Out-of-sample
* 2013-September'15 - Out-of-sample


    #2007-2010 In-sample
    stack5 = get_backtest('55f7ee68ae2b840e1335d78c')
    stack5.create_full_tear_sheet()

    100% Time: 0:00:05|###########################################################|
    Entire data start date: 2007-01-03 00:00:00+00:00
    Entire data end date: 2009-12-31 00:00:00+00:00
    
    
    Backtest Months: 36
                       Backtest
    annual_return          0.05
    annual_volatility      0.10
    sharpe_ratio           0.43
    calmar_ratio           0.43
    stability              0.86
    max_drawdown          -0.10
    omega_ratio            1.08
    sortino_ratio          0.71
    skewness               0.36
    kurtosis               2.05
    alpha                  0.05
    beta                   0.03
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0              10.40  2009-03-23 00:00:00+00:00  2009-07-06 00:00:00+00:00   
    1               6.82  2008-01-24 00:00:00+00:00  2008-03-19 00:00:00+00:00   
    2               6.13  2008-11-04 00:00:00+00:00  2008-11-18 00:00:00+00:00   
    3               2.75  2008-01-04 00:00:00+00:00  2008-01-15 00:00:00+00:00   
    4               2.62  2007-05-07 00:00:00+00:00  2007-06-29 00:00:00+00:00   
    
                   recovery date duration  
    0                        NaN      NaN  
    1  2008-10-27 00:00:00+00:00      198  
    2  2009-03-12 00:00:00+00:00       93  
    3  2008-10-28 00:00:00+00:00      213  
    4  2007-07-27 00:00:00+00:00       60  
    
    
    2-sigma returns daily    -0.013
    2-sigma returns weekly   -0.025
    dtype: float64
    
    Stress Events
              mean    min    max
    Lehmann -0.000 -0.010  0.012
    Aug07    0.000 -0.016  0.010
    Sept08  -0.001 -0.010  0.012
    2009Q1   0.000 -0.017  0.023
    2009Q2   0.000 -0.017  0.024
    
    
    Top 10 long positions of all time (and max%)
    [u'SPY' u'QQQ' u'PEP' u'KO']
    [ 0.642  0.469  0.132  0.129]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.278 -0.134 -0.131]
    
    
    Top 10 positions of all time (and max%)
    [u'SPY' u'QQQ' u'KO' u'PEP']
    [ 0.642  0.469  0.134  0.132]
    
    
    All positions ever held
    [u'SPY' u'QQQ' u'KO' u'PEP']
    [ 0.642  0.469  0.134  0.132]
    
    



![png](output_49_1.png)



![png](output_49_2.png)



![png](output_49_3.png)



![png](output_49_4.png)


This was the first period that the individual strategies were optimised over. There is a strong performance considering the bear market of the subprime crisis, however the true value of this optimisation will be seen over similar violent drops in the market.


    #2010-2013 In-sample
    stack6 = get_backtest('55f7ee762c02340df43a1204')
    stack6.create_full_tear_sheet()

    100% Time: 0:00:04|###########################################################|
    Entire data start date: 2010-01-04 00:00:00+00:00
    Entire data end date: 2012-12-31 00:00:00+00:00
    
    
    Backtest Months: 35
                       Backtest
    annual_return          0.07
    annual_volatility      0.08
    sharpe_ratio           0.89
    calmar_ratio           0.90
    stability              0.80
    max_drawdown          -0.08
    omega_ratio            1.17
    sortino_ratio          1.41
    skewness               0.43
    kurtosis               2.40
    alpha                  0.05
    beta                   0.19
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0               7.70  2010-09-24 00:00:00+00:00  2011-06-24 00:00:00+00:00   
    1               3.90  2012-09-14 00:00:00+00:00  2012-11-13 00:00:00+00:00   
    2               3.54  2011-12-05 00:00:00+00:00  2011-12-21 00:00:00+00:00   
    3               3.50  2010-03-23 00:00:00+00:00  2010-08-19 00:00:00+00:00   
    4               2.77  2012-06-19 00:00:00+00:00  2012-07-25 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-08-26 00:00:00+00:00      241  
    1                        NaN      NaN  
    2  2012-01-18 00:00:00+00:00       33  
    3  2010-09-14 00:00:00+00:00      126  
    4  2012-08-03 00:00:00+00:00       34  
    
    
    2-sigma returns daily    -0.010
    2-sigma returns weekly   -0.019
    dtype: float64
    
    Stress Events
                                        mean    min    max
    US downgrade/European Debt Crisis  0.002 -0.014  0.022
    Fukushima                          0.000 -0.004  0.009
    EZB IR Event                      -0.000 -0.008  0.012
    Flash Crash                        0.001 -0.006  0.009
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.562  0.541  0.127  0.127]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.26  -0.135 -0.132]
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.562  0.541  0.135  0.132]
    
    
    All positions ever held
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.562  0.541  0.135  0.132]
    
    



![png](output_51_1.png)



![png](output_51_2.png)



![png](output_51_3.png)



![png](output_51_4.png)


The strategies fail to take advantage of the bull trend in 2011 however 2012 proves much more fruitful.


    #2013-2015 Out-of-sample
    stack7 = get_backtest('55f7eeb78e304c0e0dde257d')
    stack7.create_full_tear_sheet()

    100% Time: 0:00:03|###########################################################|
    Entire data start date: 2013-01-02 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Backtest Months: 24
                       Backtest
    annual_return          0.08
    annual_volatility      0.06
    sharpe_ratio           1.48
    calmar_ratio           1.67
    stability              0.82
    max_drawdown          -0.05
    omega_ratio            1.29
    sortino_ratio          2.49
    skewness               0.40
    kurtosis               2.50
    alpha                  0.03
    beta                   0.31
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0               5.08  2014-09-02 00:00:00+00:00  2014-10-10 00:00:00+00:00   
    1               3.88  2014-03-05 00:00:00+00:00  2014-05-09 00:00:00+00:00   
    2               3.85  2013-05-17 00:00:00+00:00  2013-06-28 00:00:00+00:00   
    3               3.00  2013-12-31 00:00:00+00:00  2014-02-05 00:00:00+00:00   
    4               1.95  2013-03-11 00:00:00+00:00  2013-04-05 00:00:00+00:00   
    
                   recovery date duration  
    0  2014-10-31 00:00:00+00:00       44  
    1  2014-08-22 00:00:00+00:00      123  
    2  2013-08-01 00:00:00+00:00       55  
    3  2014-02-18 00:00:00+00:00       36  
    4  2013-04-23 00:00:00+00:00       32  
    
    
    2-sigma returns daily    -0.007
    2-sigma returns weekly   -0.015
    dtype: float64
    
    Stress Events
            mean    min    max
    Apr14 -0.000 -0.011  0.004
    Oct14  0.001 -0.009  0.020
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.4    0.13   0.129]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.238 -0.135 -0.128]
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.4    0.135  0.129]
    
    
    All positions ever held
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.4    0.135  0.129]
    
    



![png](output_53_1.png)



![png](output_53_2.png)



![png](output_53_3.png)



![png](output_53_4.png)


Moving onto the out-of-sample period we see a very strong performance, consistent stable returns. Sharp drops have been dulled whilst returns have been generated at most opportunities.


    #2013-September'15 Out-of-sample 
    stack8 = get_backtest('55f7eee22c02340df43a120c')
    stack8.create_full_tear_sheet()

    100% Time: 0:00:05|###########################################################|
    Entire data start date: 2013-01-02 00:00:00+00:00
    Entire data end date: 2015-09-14 00:00:00+00:00
    
    
    Backtest Months: 32
                       Backtest
    annual_return          0.06
    annual_volatility      0.06
    sharpe_ratio           1.06
    calmar_ratio           1.21
    stability              0.90
    max_drawdown          -0.05
    omega_ratio            1.20
    sortino_ratio          1.82
    skewness               0.42
    kurtosis               1.95
    alpha                  0.03
    beta                   0.22
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0               5.08  2014-09-02 00:00:00+00:00  2014-10-10 00:00:00+00:00   
    1               4.44  2015-04-24 00:00:00+00:00  2015-09-14 00:00:00+00:00   
    2               3.88  2014-03-05 00:00:00+00:00  2014-05-09 00:00:00+00:00   
    3               3.85  2013-05-17 00:00:00+00:00  2013-06-28 00:00:00+00:00   
    4               3.00  2013-12-31 00:00:00+00:00  2014-02-05 00:00:00+00:00   
    
                   recovery date duration  
    0  2014-10-31 00:00:00+00:00       44  
    1                        NaN      NaN  
    2  2014-08-22 00:00:00+00:00      123  
    3  2013-08-01 00:00:00+00:00       55  
    4  2014-02-18 00:00:00+00:00       36  
    
    
    2-sigma returns daily    -0.007
    2-sigma returns weekly   -0.016
    dtype: float64
    
    Stress Events
            mean    min    max
    Apr14 -0.000 -0.011  0.004
    Oct14  0.001 -0.009  0.020
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.5    0.13   0.129]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.238 -0.135 -0.128]
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.5    0.135  0.129]
    
    
    All positions ever held
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.5    0.135  0.129]
    
    


    /usr/local/lib/python2.7/dist-packages/pyfolio/utils.py:140: UserWarning: Could not update cache /usr/local/lib/python2.7/dist-packages/pyfolio/data/factors.csv.Exception: [Errno 13] Permission denied: '/usr/local/lib/python2.7/dist-packages/pyfolio/data/factors.csv'
      UserWarning)



![png](output_55_2.png)



![png](output_55_3.png)



![png](output_55_4.png)



![png](output_55_5.png)


This a repeat of the previous backtest but including the market top of 2015 and the "Black Monday" event that hit China and the rest of the world (1000pt drop in Dow Jones). It is very key to not the performance at the far right of the chart. 

Although in reality any position that may have been opened or closed on that day reaslistically would have been affected by market moves and slippage would have taken the toll. However its clear to note the inital sharp drop was not felt severely by the algorithm, it only forms the second largest drawdown (around 4%).

##4. Review of Results

Here we review a selection of the strongest backtests side by side with commentary:

| Metric Annualised | Mean Reversion | Momentum | Pairs Trading | Mean & Pairs | Momentum & Pairs | Mean & Momentum | Mean & Mom Rebalanced | All Strategies | All Strategies Rebalanced |
|-------------------|----------------|----------|---------------|--------------|------------------|-----------------|-----------------------|----------------|---------------------------|
| Return            | 0.08           | 0.12     | 0.04          | 0.06         | 0.08             | 0.08            | 0.10                  | 0.07           | 0.08                      |
| Volatility        | 0.17           | 0.15     | 0.07          | 0.09         | 0.08             | 0.11            | 0.1                   | 0.08           | 0.08                      |
| Sharpe            | 0.45           | 0.79     | 0.63          | 0.65         | 0.99             | 0.76            | 1.02                  | 0.93           | 1.06                      |
| Calmar            | 0.23           | 0.45     | 0.3           | 0.34         | 0.7              | 0.63            | 0.84                  | 0.91           | 0.95                      |
| Stability         | 0.66           | 0.93     | 0.77          | 0.81         | 0.96             | 0.95            | 0.94                  | 0.96           | 0.95                      |
| Max Drawdown      | 0.33           | 0.27     | 0.14          | 0.17         | 0.12             | 0.13            | 0.12                  | 0.08           | 0.09                      |
| Omega Ratio       | 1.08           | 1.16     | 1.14          | 1.12         | 1.19             | 1.15            | 1.22                  | 1.19           | 1.22                      |
| Sortino Ratio     | 0.72           | 1.08     | 0.85          | 1.06         | 1.43             | 1.28            | 1.58                  | 1.56           | 1.64                      |
| Skewness          | 0.47           | 0.03     | 0.5           | 0.46         | 0.04             | 0.68            | 0.6                   | 0.6            | 0.53                      |
| Kurtosis          | 4.38           | 3.45     | 7.44          | 4.09         | 2.52             | 4.26            | 8.54                  | 3.77           | 6.53                      |
| Alpha             | 0.08           | 0.07     | 0.04          | 0.06         | 0.07             | 0.08            | 0.1                   | 0.07           | 0.07                      |
| Beta              | -0.12          | 0.43     | 0.0           | -0.06        | 0.22             | 0.12            | 0.07                  | 0.08           | 0.09                      |

**To summarise the above table we have a few key points, moving from left to right:**

1. Reversion strategy returns are negatively correlatd to market returns and has lowest stability and Sharpe
2. Momentum strategy is highly correlated with market returns and has high stability and returns
3. Pairs strategy has no correlation to the market and has the lowest volatility and max drawdown
4. Combining the various stategies appears to average out weaknesses and strengths resulting in a more consistent performance

---
**We find there are three top performing strategies:**

* Momentum & Pairs
* Mean & Momentum Rebalanced
* All Strategies Rebalanced


1. Comparing Momentum & Pairs vs other two strategies, we see the following key points:
    * Joint largest Max Drawdown 
    * Lowest Calmar, Sharpe, Sortino, Omega ratio
    * Highest Stability
2. Comparing Mean Revesion & Momentum Rebalanced vs other two strategies, we see the following key points:
    * Joint largest Max Drawdown
    * Largest Alpha and lowest Beta
    * Highest Volatility
    * Lowest Stability
    * Largest Kurtosis of returns, but the most positive Skew
3. Comparing All Strategies Rebalanced vs other two strategies, we see the following key points:
    * Joint lowest Volatility
    * Highest Sharpe, Calmar, Sortino
    * Joint highest Omega
    * Lowest Max Drawdown

**Conclusion:**

Although it seems extremely close between Mean Reversion & Momentum Rebalanced and All Strategies Rebalanced, to examine the data carefully shows we have sacrificed Alpha and returns for stability and consistency. We are in search of leveraged returns which yields superior returns for each unit of risk added. The various ratio metrics highlight how returns are being generated on a better risk adjusted basis. 

---
###Overall:

We have combined relatively diversified strategies together and shared the portfolio control equally between the three with dynamic rebalancing between the Mean Reversion and Momentum strategy, this has lead to an **improved risk adjusted returns** stream as measured by Stability, Sharpe, and Max Drawdown along with the other metrics in the table above.

###Yearly Breakdown of Performance

Under the column "Yearly Cont." we have the **actual performance** of the strategy when run over the total time period, this is the true reflection of the metrics. 

However breaking down the algorithm over one year periods causes it to vastly underperform with this "stop-start" style operation, this would not be done like this in reality (this is not what the algorithm is designed for) however it is useful to see relative performance and underperformance in an illustrative fashion. External testing outside of this notebook has proven that the dynamic rebalancing without warmup compounds this underperformance potentially due to the warmup period for the rebalancing.

These metrics could possibly be seen as the worst case (Big 'O') scenario for the algorithm. In this table the returns compound to roughly 55% with inferior metrics whilst correct algorithm operation compounds to over 86% with superior quality of returns.

| Metric Annualised | Yearly Cont. | 07-08 | 08-09 | 09-10 | 10-11 | 11-12 | 12-13 | 13-14 | 14-15 | 15-"China's Black Monday" |
|-------------------|--------------|-------|-------|-------|-------|-------|-------|-------|-------|---------------------------|
| Return            | 0.08         | 0.07  | 0.04  | 0.01  | 0.01  | 0.16  | 0.05  | 0.11  | 0.04  | -0.03                     |
| Volatility        | 0.08         | 0.06  | 0.11  | 0.12  | 0.14  | 0.10  | 0.06  | 0.06  | 0.05  | 0.06                      |
| Sharpe            | 1.06         | 1.16  | 0.35  | 0.07  | 0.07  | 1.67  | 0.79  | 1.97  | 0.78  | -0.55                     |
| Calmar            | 0.95         | 2.83  | 0.61  | 0.07  | 0.17  | 2.98  | 1.37  | 2.89  | 0.85  | -0.74                     |
| Stability         | 0.95         | 0.74  | 0.23  | 0.02  | 0.07  | 0.65  | 0.52  | 0.90  | 0.30  | 0.02                      |
| Max Drawdown      | 0.09         | 0.03  | 0.06  | 0.10  | 0.05  | 0.06  | 0.04  | 0.04  | 0.05  | 0.04                      |
| Omega Ratio       | 1.22         | 1.21  | 1.06  | 1.01  | 1.03  | 1.34  | 1.14  | 1.39  | 1.15  | 0.91                      |
| Sortino Ratio     | 1.64         | 2.00  | 0.55  | 0.11  | 0.20  | 3.33  | 1.48  | 3.22  | 1.68  | -0.93                     |
| Skewness          | 0.53         | 0.07  | 0.19  | 0.55  | 0.08  | 0.8   | 0.41  | -0.04 | 1.61  | 0.52                      |
| Kurtosis          | 6.53         | 0.97  | 2.84  | 1.10  | 1.59  | 2.28  | 0.92  | 1.02  | 7.49  | 1.03                      |
| Alpha             | 0.07         | 0.07  | 0.04  | 0.02  | -0.01 | 0.16  | 0.03  | 0.01  | 0.02  | -0.03                     |
| Beta              | 0.09         | 0.01  | 0.01  | 0.12  | 0.17  | -0.01 | 0.11  | 0.36  | 0.15  | 0.05                      |

###Performance Against Benchmark

It is of my opinion that comparing a multi strategy algorithm against an index is illogical, however this is industry standard, especially for outside investors, who may rate the performance of a portfolio against a buy-and-hold return of the S&P 500. For this reason I have written up all of my analysis against the standard benchmark. However in the image below you can see the **unleveraged performance** against an ETF which tracks Multi-Strategy Hedge Fund performance, you can see more about this [here](https://uk.finance.yahoo.com/q/pr?s=QAI). This is by no means perfect, however it a step toward reflecting the "institutional players" performance. 

![Benchmark Returns]()

*As a side note: I personally disagree with the ideaology of "out performing the benchmark", this would be an unrealistic bonus, whilst unleveraged, rather than a target. Instead the intelligent investor will ideally seek consistent and reliable compounding of wealth above the risk free rate. Arguablly this has been easy to achieve over the past 7/8 years due to experimental monetary policy and ZIRP specifically. It seems market regieme change is frequent and considerable, this is a blessing for Speculative Traders and a curse for Quantitiative Traders, however in my opinion uncertainty is opportunity.*


    # 2010 - 2015 - Since ETF Inception 
    test = get_backtest('55f81c0359cf920df908961f')
    test.create_full_tear_sheet()

    100% Time: 0:00:10|###########################################################|
    Entire data start date: 2010-01-04 00:00:00+00:00
    Entire data end date: 2014-12-31 00:00:00+00:00
    
    
    Backtest Months: 59
                       Backtest
    annual_return          0.08
    annual_volatility      0.07
    sharpe_ratio           1.10
    calmar_ratio           1.03
    stability              0.94
    max_drawdown          -0.08
    omega_ratio            1.22
    sortino_ratio          1.77
    skewness               0.51
    kurtosis               3.15
    alpha                  0.07
    beta                   0.42
    
    Worst Drawdown Periods
       net drawdown in %                  peak date                valley date  \
    0               7.70  2010-09-24 00:00:00+00:00  2011-06-24 00:00:00+00:00   
    1               5.08  2014-09-02 00:00:00+00:00  2014-10-10 00:00:00+00:00   
    4               3.90  2012-09-14 00:00:00+00:00  2012-11-13 00:00:00+00:00   
    2               3.88  2014-03-05 00:00:00+00:00  2014-05-09 00:00:00+00:00   
    3               3.75  2013-05-17 00:00:00+00:00  2013-06-28 00:00:00+00:00   
    
                   recovery date duration  
    0  2011-08-26 00:00:00+00:00      241  
    1  2014-10-31 00:00:00+00:00       44  
    4  2013-01-02 00:00:00+00:00       79  
    2  2014-08-22 00:00:00+00:00      123  
    3  2013-08-01 00:00:00+00:00       55  
    
    
    2-sigma returns daily    -0.009
    2-sigma returns weekly   -0.018
    dtype: float64
    
    Stress Events
                                        mean    min    max
    US downgrade/European Debt Crisis  0.002 -0.014  0.022
    Fukushima                          0.000 -0.004  0.009
    EZB IR Event                      -0.000 -0.008  0.012
    Flash Crash                        0.001 -0.006  0.009
    Apr14                             -0.000 -0.011  0.004
    Oct14                              0.001 -0.009  0.020
    
    
    Top 10 long positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.541  0.13   0.129]
    
    
    Top 10 short positions of all time (and max%)
    [u'SPY' u'KO' u'PEP']
    [-0.26  -0.135 -0.132]
    
    
    Top 10 positions of all time (and max%)
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.541  0.135  0.132]
    
    
    All positions ever held
    [u'QQQ' u'SPY' u'KO' u'PEP']
    [ 0.582  0.541  0.135  0.132]
    
    



![png](output_62_1.png)



![png](output_62_2.png)



![png](output_62_3.png)



![png](output_62_4.png)


Against the ETF benchmark we see a far superior unleveraged performance. It must be made clear that the hedge fund industry is becoming more crowded, and within the benchmark there will be some components that vastly out/under perform so representing an average performance. We can see that despite this algorithm being extremely basic in construction with very simplistic and "proof-of-concept" style design and strategies, it is not without purpose. Compounding at over 8% per year with a Sharpe Ratio just under 1.5 in the out-of-sample period.
