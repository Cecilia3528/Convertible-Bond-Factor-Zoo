# Convertible-Bond-Strategy
A sum-up about my research on Convertible Bond Strategy, mainly on Limit-up/Limit-down/Price _interval/Market_Capacity features.

## 1. Common Stock Limit-Up and Limit-Down Factors

- **Limit-Up Factor**
    - There is a 96.7% probability of a price increase, with an average maximum increase of around 5.9% and a significant maximum drawdown of 5.3%.
    - After the limit-up, enthusiasm and volatility decline (halved), but this is mainly because the effect of this signal fades quickly; if we ignore whether the limit is lifted and only consider the next day, the effect is already halved.
    
- **Limit-Down Factor**
    - A clear reversal effect: There is a 97.7% probability of a price increase, with an average increase of around 5.7% and a very low maximum drawdown of 2.9% (compared to a 3-day maximum drawdown of 1.9% across the market).
    - Similarly, after the limit-down, enthusiasm and volatility decline, again due to the rapid fading of the signal's effect; ignoring whether the limit is lifted and only looking at the next day, the effect has already halved.
    - **Strategy Construction:**
        - For a more conservative approach, buy convertible bonds at the close on the day of the common stock limit-up (Day 0) and sell at the open on Day 1, achieving an average annualized return of 20% with a drawdown of only 8.6% and a Sharpe ratio of 2.3.
        - For a more aggressive approach, buy convertible bonds at the close on Day 0 and sell at the open on Day 4, achieving an average annualized return of 76% with a drawdown of about 29.3% and a Sharpe ratio of 2.59.
- Avoid ST stocks.
- The optimal strategy is 1 open/0 close, achieving an average annualized return of 70% with a drawdown of only 19.8% and a Sharpe ratio as high as 3.6. The win rate is 51%, with an average holding return of 0.33%.
- **Main Method:** Adjust n and open/close, calculating the combination where n_msg/0 close yields the highest return (win rate, average return, net value).

## 2. Price Range Factor

- The number of low-priced convertible bonds has surged in recent years (due to price declines, economic downturns, etc.).
- The largest market volume is below 95 and in the 130-135 range; higher-priced bonds tend to have better quality, while lower-priced bonds are cheaper.
- **Main Method:** Statistics on the distribution of trading volume and transaction value in different price ranges (divided by 5).

## 3. Market Capacity Factor

- Construct market capacity: total trading volume over 5/10/30 minutes (5/10/30 TC), with a larger sum indicating greater market capacity.
- Conduct return attribution:
    - Is there an issue of "insufficient counterparty" for buy and sell orders?
        - Fortunately, 70% have 10 TC accounting for less than 50%.
    - Returns under different market capacities for buy and sell orders:
        - Methods: (1) 10 TC share; (2) statistics on individual bonds: average market capacity in the month prior to purchase.
        - The pattern shows that lower share percentages yield higher returns (points can be bought, points can be sold).
        - When the share is high (60%-80% quantile), market characteristics indicate a rebound in returns.
    - The correlation between market capacity and returns is very high, reaching 30%.

- **Market Research:**
    - **Main Method:** Group backtesting to observe average returns and net value, finding the windows with the best predictive effect.
    - Group backtesting shows that 5 TC leads, showing some monotonicity. 10 TC has better predictive performance.

## 4. Volatility Factor

- Average market IC: Monthly volatility factor: around 4% (others about 3.5%).
- Group average returns: The 60%-80% quantile (high volatility but not the maximum) has the highest average returns; the 5D and 10D volatility factors perform best.
    - Group backtesting net value: Not very obvious, but the highest two quantiles (80%-100%) have consistently lagged, showing a certain U-shaped effect.
- Real market pure volatility factor performs poorly: consistently incurs drawdowns [and outputs the worst-performing securities for mentor review of strategy logic].
- **[Migration]** Construct a "volatility and return correlation" factor: similar to volume-price, separating out divergence situations. Hypothesis: divergence reflects the views of market leaders (low volatility but high return: expected to rise; high volatility but low return: speculation, risk).
    - IC is around 3%, which is quite effective, and it is more effective in return attribution, with an IC close to 10%.
- **Main Method:** IC, group backtesting returns, group net value.

## 5. Market Situation Classification

- Classify daily market conditions based on the rise and fall of the convertible bond index: unilateral rise / volatile rise / significant fluctuation.
    - Unilateral rise: pct > 0.01; unilateral fall: pct < -0.01
    - Volatile rise: 0 < pct < 0.01 & volat > 0.015; volatile fall: -0.01 < pct < 0 & volat > 0.015
    - Significant fluctuation: -0.01 < pct < 0.01 & volat < 1.5% & (pct) - volat > 0.5%; slight fluctuation: -0.01 < pct < 0.01 & volat < 1.5% & (pct) - volat < 0.5%.
- **Net Value Return Attribution:**
    - Classifying daily market conditions based on the rise and fall of the convertible bond index: unilateral rise / volatile rise / significant fluctuation, examining trading gains and losses. Conclusion: As expected, volatile rises yield the most profit (0.8%); significant and slight fluctuations occur the most but yield little profit (less than 0.1%); unilateral falls and volatile falls both incur a loss of 0.5%.
    - Strategies during volatile falls can be further improved.

## 6. Market Research

- **Factors:** Trading volume, transaction value, and volatility for the past 5, 10, and 20 days, as well as the maximum and minimum returns over the next 3 days.
- **Conclusion:** The 5-day trading volume, transaction value, and volatility perform best. The volatility factor has the best effect, but also incurs significant drawdown. IC is as high as nearly 20%.
- However, the current strategy seems unable to capture the returns brought by volatility (as observed from market situation classification).
- **Main Method:** IC, average returns, regression analysis beta, group returns.

## 7. Return Attribution

- **Order Matching:**
    - Develop strategies for buy/sell signals, matching the strategy's buys and sells to calculate strategy returns; unmatched buy orders are matched with nearby sell orders, calculating strategy effects.
    - Develop strategies for buy/sell signals, matching the strategy's buys and sells to calculate strategy returns; unmatched buy orders are matched with sell orders within the next 5 minutes, calculating strategy effects.
    
- **Market Capacity Factor Return Attribution:**
    - Is there an issue of "insufficient counterparty" for buy and sell orders? No.
    - Returns under different market capacities for buy and sell orders? The higher, the better, but along with the market, with a rebound in the 60%-80% segment.
    
- **Volatility Factor Attribution:**
    - Real market pure volatility factor performs poorly: consistently incurs drawdowns [and outputs the worst-performing securities for mentor review of strategy logic].
    - Construct a "volatility and return correlation" factor: similar to volume-price, separating out divergence situations. Hypothesis: divergence reflects the views of market leaders (low volatility but high return: expected to rise; high volatility but low return: speculation, risk).
        - More effective in return attribution, with IC close to 10%.
        
- **Market Situation Attribution (unilateral rise / volatile rise / significant fluctuation):**
    - How do different situations perform? Volatile rises yield the most profit (0.8%); significant and slight fluctuations occur the most but yield little profit (less than 0.1%); unilateral falls and volatile falls both incur a loss of 0.5%.
    - Strategies during volatile falls can be further improved.
