
# All Weather V1 - Performance Analysis (Brief)

**Marcus Williamson - 08/09/15**

---

The strategies in this analysis were previously optimised, please find the supplementary optimisation Notebooks [here](https://github.com/ArtificialInvestor/algo-optimisation/tree/master/All%20Weather%20V1).

Analysis has been trimmed for brevity, please find the full piece including all details and figures from backtesting [here](https://mw572.github.io/All%20Weather%20V1/All%20Weather%20V1%20Full%20Analysis.html)

##### Format:

1. Introduction
1. Backesting Optimised Strategies
1. Combining Optimised Strategies
1. Review of Results

---

## 1. Introduction

This Notebook highlights the backbone to the approach of the Artificial Investor ideology:

**Combining diversified alpha generators to deliver consistent returns**

We aim to show here that very basic quantitatively driven strategies that generate returns from different market behaviours when combined can provide a more *desirable* and relaiable return stream that can be leveraged to generate a superior return to the market, although we must be clear that this is **not** the objective.

Below we compare the different strategies alone vs when combined in any particular combination is somewhat inferior to the quality of returns stream generated when all are combined together.

---

**Desirable Returns:**

Desirable returns is understandably a very subjective and a weak term, here I shall briefly outline what we determine as reliable. Risk free rate is based on US Treasuries, see [here](https://research.stlouisfed.org/fred2/categories/116). An algorithm's returns ideally have:

* High [Sharpe Ratio](https://en.wikipedia.org/wiki/Sharpe_ratio) - Deviation risk measure (Reward to variability)
* High [Stability](https://en.wikipedia.org/wiki/Coefficient_of_determination) - R-squared of a linear fit to the returns
* High [Alpha](https://goo.gl/b3n4oy) - Active return of investment
* High [Calmar Ratio](https://en.wikipedia.org/wiki/Calmar_ratio) - Compound annual returns divided by Max Drawdown
* High [Sortino Ratio](https://en.wikipedia.org/wiki/Sortino_ratio) - Variation on Sharpe penilising downside volatility only
* High [Omega Ratio](https://en.wikipedia.org/wiki/Omega_ratio) - Probabilty weighted ratio of returns vs losses
* Low [Max Drawdown](https://goo.gl/cl1D0A) - Highest to lowest point since inception
* Low [Volatility](https://goo.gl/HPFERj) - Degree of variation in returns
* Low [Beta](https://goo.gl/WIWUY0) - Exposure to general market movements
* Low [Kurtois](https://en.wikipedia.org/wiki/Kurtosis) - "Peakedness" of returns vs Normal Distribution (High = Fat tails
* Positive [Skew](https://en.wikipedia.org/wiki/Skewness) - "Skewdness" of returns vs Normal Distribution


The explicit valuations depend entirely on circumstances such as strategy being tested, the current market regime it is tested over, and the slippage or market impact models being implemented. However one is able to compare apples to apples when looking at the same algorithms's strategies in various combinations over the same periods. 

Please note: the securities chosen to trade are simply for illustrative purpose, no thought has gone into what pair of stocks to trade, it has simply been taken from wikipedia.

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

In both cases the actual algorithm contains a more generous setup as compared to the backtesting environment, it is in line with the realistic costings as defined by the broker API however. 

---

##2. Backtesting Optimised Strategies

The original algorithm has been optimised over a total six year period in two three year segments, the parameters have been selected with a focus on consistency and stability of returns rather than maximal performance 

**In Sample Backtest:** 01/01/07 - 01/01/13

**Out-of-sample Backtest:** 01/01/13 - 01/01/15

---

### Mean Reversion

*Security: S&P 500 ETF (SPY)*

**Strategy Overview:**

* Based on the assumption that a stock's price will tend to move to the average price over time
* Stock price analysis involves both identifying the trading range for a stock and computing the average price using analytical techniques
* Trading logic:
    + When current market price is less than the average price, purchase on expectation of a return of the stock price to the average price
    + When current market price is greater than the average price, sell on expectation of a return of the stock price to the average price
  
**Considerations:**

* Simple forms of this strategy typically have little in the way of risk management, failing to mean revert due to exceptional circumstances, such as bankruptcy, can be catastrophic without human intervention or very advanced semantic interface
* Order feathering, absolute stop losses, and more advanced methods to detect suitable entry points would vastly improve strategy performance

Read more on Mean Reversion [here](https://en.wikipedia.org/wiki/Mean_reversion_(finance)).

I chose to include this strategy in my algorithm as consistent returns should theoretically be generated in QE tainted environments. My rationale for this lies in the theory that the stock market will revert to a support/resistance level proxy of Governemnt balance sheets, as they expand to support the market growth (prices roughly follow the expansion). I use the word 'theory' here as I have not personally performed significance testing on this, nor have seen any papers so far proving so.

![chart](Charts/balance.png)

The original PDF containing this chart can be found [here](http://www.raymondjames.com/pdfs/Technical_Chart_Book.pdf).

---

#### Backtest & Out-of-sample Performance

![graphic](Charts/1.png)

The strategy has a naturally contrarian approach, it appears to profit well from sudden drops, however its simple logic structure results in it giving back much of its gains in trending periods. These drawdowns could be reduced with some of the implmentations mentioned above.

In the out-of-sample period the strategy struggles with the bull run in 2013, considerable retuns over the Oct 15th 2014 [Treasury flash crash](http://ftalphaville.ft.com/2015/04/16/2126722/your-october-2014-flash-crash-charted/) event. Simialrity of returns are good, despite the actual consistence of returns being poor. 

---

### Momentum

*Security: Nasdaq 100 ETF (QQQ)*

**Strategy Overview:**

* Based on the empirically observed tendency fo rising asset prices to continue rising and falling asset prices to continue falling 
* Finance theory and the efficient-market hypothesis has difficulty in fully explaining the existence of this 'market anomaly'
* It is believed that the majority of this behaviour can be attributed to behavioural economics, cognititve biases and irrational investors!
* Trading logic:
    + When current market price is greater than the average price, purchase on expectation of a continued outperformance by the stock, rolling a stop up
    + When current market price is less than the rolling stop, close and wait for re-entry point
  
**Considerations:**

* This strategy has been implemented in a long only fashion, this is to ensure simplicity and to ensure only one security is traded
* Long-Short variations trading individual stocks would yield superior returns with a reduced market exposure
* The rolling stop utilised has not been optimised, and is not dynamically set over periods of volatility

Read more on Momentum [here](https://en.wikipedia.org/wiki/Momentum_(finance)).

I chose to include this strategy in my algorithm as it is the natural rival to Mean Reversion, which as previously discussed should theoretically operate well in QE tainted environments. Ideally the less correlated the strategies the better.

---

#### Backtest & Out-of-sample Performance

![graphic](Charts/2.png)

This strategy generates solid returns on bullish trends, its stoploss design prevented it from enduring a large drawdown over the subprime crisis. Over various events the returns are largely flat as the stoploss is hit and the algorithm is waiting for a re-entry signal.

---

### Pairs Trading (Statistical Arbitrage)

*Securities: Coca Cola (KO), Pepsi (PEP)*

**Strategy Overview:**

* Based on a market neutral convergence strategy where two historically correlated securitys are traded on the weakening / strengthening of their price correlation
* The trading of a pair's divergence / convergence requires good position sizing and market timing as opportunities are limited. The strategy has limited downside however
* Trading logic:
    + When pair of securities diverge (spread widens), algorithm goes long on one arm of the pair and short on the other
    + When spread narrows sufficiently positions are closed
  
**Considerations:**

* This strategy has been implemented across a single pair which is very well known to the industry, very few opportunities actually exist in this circumstance with trading costs
* Strategy can be vastly enhanced by using multiple pairs and using more advanced spread divergence signal generation such as [Kalman Filters](https://en.wikipedia.org/wiki/Kalman_filter)

Read more on Pairs trading [here](https://en.wikipedia.org/wiki/Pairs_trade).

I chose to include this strategy as it provides a non-correlated diverse returns stream to the portoflio, which theoretically should have the ability to generate returns in any circumstance due to market neutrality. Exposure here lies in company risk and violent market moves rapidly expanding / contracting spreads. I feel this strategy is very much not correlated with Mean Reversion returns.

---

#### Backtest & Out-of-sample Performance

![graphic](Charts/3.png)

The pairs trading strategy performs well over the total duration of the backtest and out-of-sample forward test. Sizeable drawdowns are realised however, despite the algorithm remaining relatively stable and not correlated to index returns. It appears that in extreme index moves the strategy experiences losses or is unable to generate returns, this is quite the oppposite to Reversion strategy where majority of returns are generated in these circumstances.

---

## 3. Combined Strategies

Here we examine the performance of the three strategies when combined in different combinations, some with and without the dynamic rebalancing. The advantage of stacking these strategies (as covered previously) are that the returns are diversified and stabilisied in aim of generate consistent returns over varying market behaviour.

---

#### Mean Reversion & Momentum - No Rebalance

Here we have combined the Mean Reversion and Momentum strategy, the portfolio control has now been shared equally between the two strategies.

![graphic](Charts/4.png)

The combination of Mean reversion and Momentum provides a complementary returns stream which delivers on violent drops and trending periods. We can still see the large moves generated by the Mean Reversion strategy, however the Momentum strategy has improved the cumulative return's line of fit coefficent.

---

#### Mean Reversion & Momentum - Dynamic Rebalancing

Here we are utilising the same strategies combined as above, but in this case we have enabled dynamic rebalancing as mentioned at the beginning of this analysis. The rebalancing results in an outperforming strategy gaining more control of the portfolio.

![graphic](Charts/5.png)

The rebalancing version of the strategy outperforms the non-rebalanced version, and has smoothed the returns stream nicely. This simplistic machine learning-esque modification has resulted in a superior performance even with very basic strateigies.

---

#### Momentum & Pairs Trading

Here we combine the Momentum strategy with the Pairs trading strategy and review its performance. We are unable to use rebalancing on the Pairs strategy due to the usage of two securities. This is something that will be fixed in my next project! 

![graphic](Charts/6.png)

Here we see a very steady and consistent returns stream with only one major sustained drawdown around 2009 which causes the strategy to dip into negative returns. 

---

#### Mean Reversion & Pairs Trading

Combining the Mean Reversion strategy and Pairs trading strategy.

![graphic](Charts/7.png)

Here we see a less stable cumulative return, however we have noticably avoided neative cumulative returns over the '09 period. The combination of Mean Reversion generating returns on large moves and Pairs generating returns on most occasions other than in violent market moves is a complimentary combination.

---

### All Strategies - Mean Reversion, Momentum, Pairs Trading - No Rebalance

For the grand finale we bring all three of our basic strategies together to review the performance firstly without rebalancing and then later with rebalancing.

![graphic](Charts/8.png)

Without rebalancing we see a returns stream with the qualities of the various strategies combined, we have not sustained negative cumulative returns or experience a large sustained drawdown in 2011. As a compromise (as there must always be one) we have sacrified absolute returns. The overall alorithms is steady and reliable with periods of underperformance followed by sharp increases in returns.

---

### All Strategies - Mean Reversion, Momentum, Pairs Trading - Dynamic Rebalancing

Turning now to the dynamically rebalanced version of the above, we are looking for performance improvements like seen with the Mean Reversion and Momentum strategies combination. Below I output a full backtest tearsheet like what can be found in the full analysis (see link at top).


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



![graphic](Charts/9.png)


Rebalanced we can see the overall strategy has generated superior returns are to be expected from prior backtests. We are seeing a slightly less stable returns stream, as you can noticably see by the dip into negative cumulative returns in '09 once again. Rebalancing has given us on face value a compromise in quality for quantity when it comes to returns, we shall have to examine the data later to see what the true impact has been.

---

**Further review of out-of-sample performance over each of the following periods:**

* 2013-2015 - Out-of-sample
* 2013-September'15 - Out-of-sample

In each case the algorithm has been run explicitly over these periods, this generated different returns compared to the overall period performance. The differences are due to a variety of factors not limited to:

* Backtesting environment calculations of performance
* Rebalancing warmup period
* Various calculations warmup period

I can confirm the sourcecode is identical and the time periods are accurate, please use the below as illustrative examples of how the strategy would behaved with different market behaviour.

---

**2013 - 2015**

![graphic](Charts/12.png)

We can see the strategy tracks the index fairly closely with reduced violent price movements. There is a period of underperformance around May and June 2014 which largely effects the overall cumulative returns. We have sacrificed returns for reduced risk and consistency of performance. Its not glamarous, but garnering risk chasing absolute unleveraged returns is unresonable.

---

**2013 - September**

![graphic](Charts/13.png)

This a repeat of the previous backtest but including the forming market top of 2015 and the "Black Monday" event that hit China and the rest of the world (1000pt drop in Dow Jones). It is very key to note the performance at the far right of the chart. 

Although in reality any position that may have been opened or closed on that day reaslistically would have been catastrophically affected by violent market moves, broker difficulties and slippage. However its clear to note the inital sharp drop was not felt severely by the algorithm, it only forms the second largest drawdown (around 4%). This is due to a diversified long and short holding with limited market exposure despite only trading three securities, two being major indicies. 

---

## 4. Review of Results

Here we review a selection of the strongest backtests side by side with commentary:

| Metric Annualised | Mean Reversion | Momentum | Pairs Trading | Mean & Pairs | Momentum & Pairs | Mean & Momentum | Mean & Mom Rebalanced | All Strategies | All Strategies Rebalanced |
|-------------------|----------------|----------|---------------|--------------|------------------|-----------------|-----------------------|----------------|---------------------------|
| Return            | 0.08           | 0.12     | 0.04          | 0.06         | 0.08             | 0.08            | 0.1                   | 0.07           | 0.08                      |
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

**To summarise the above table we have a few key points (moving from left to right):**

1. Reversion strategy returns are negatively correlated to market returns (as represented by Beta) and has lowest stability and Sharpe
2. Momentum strategy is highly correlated with market returns and has high stability and returns
3. Pairs strategy has no correlation to the market and has the lowest volatility and max drawdown
4. Combining the various stategies appears to have the effect of 'averaging out' weaknesses and strengths of the various strategies resulting in a more consistent performance

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

Although it seems a very close performance between Mean Reversion & Momentum Rebalanced and All Strategies Rebalanced, examining the data carefully shows we have sacrificed some Alpha and aboslute returns for stability and consistency. We are in search of **leveraged returns** which yields **superior returns for each unit of risk added**. I am not venturing into quantifying the extent to which one strategy is supeior over the other. The various ratio metrics highlight how returns are being generated on a better risk adjusted basis, we can see them as a proxy to this quantifying. 

**Overall Outcome:**

We have combined three relatively diversified and simplisitic strategies together, shared portfolio control with dynamic rebalancing between two of the three stratgies, this has lead to an **improved risk adjusted return** stream as measured by metrics in the table above.

---

### Performance Against Benchmark

It is of my opinion that comparing a multi strategy algorithm against an index is illogical, however this is industry standard. This is especially true for outside investors, who may rate the performance of a portfolio against a buy-and-hold return of the S&P 500. For this reason I have written up all of my above analysis against the standard benchmark. However in the chart below you can see the **unleveraged performance** against an ETF which tracks Multi-Strategy Hedge Fund performance, you can see more about this [here](https://uk.finance.yahoo.com/q/pr?s=QAI). This is by no means a perfect proxy, however it a sure step toward reflecting actual institutional performance. 


*As a side note: I personally disagree with the ideaology of "out performing the benchmark", this would be an unrealistic bonus (whilst unleveraged), rather than a target. Instead the intelligent investor will ideally seek consistent and reliable compounding of wealth above the risk free rate. Arguablly, this has been easy to achieve over the past 7/8 years due to experimental monetary policy and ZIRP (Zero Interest Rate Policy) specifically. Market regieme change is frequent and unpredictable, this is a blessing for Speculative Traders and a curse for Quantitiative Traders, however in my opinion uncertainty is most often than not an opportunity.*


#### Here is the backtest + out-of-sample period starting just after ETF inception:


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
    
    
![graphic](Charts/14.png)


Against the ETF benchmark we see a far superior unleveraged performance. It must be made clear that the hedge fund industry is becoming more crowded, and within the benchmark there will be some components that vastly out/under perform so representing an average performance. We can see that despite this algorithm being extremely basic in construction with very simplistic and "proof-of-concept" style design and strategies, it is not totally obsolete. Compounding at over **8%** per year with a Sharpe Ratio just under **1.5** in the out-of-sample period.
