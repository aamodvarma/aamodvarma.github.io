---
title: Rocket Neural Network
author_profile: true
permalink: /projects/stockanalysis
---
{% include base_path %}

The goal of this project was to use monte carlo simulations to analyse the volatility of stocks by taking into account the historical prices. I use the daily return of stocks over the course of their history to do the analysis.

# Data
First using the yahoo finance library in python I was able to mainly get access to the open and closing prices for each day from 1982 (around 15500 days).

Given the closing prices for each day I was able to calculate the daily return by using the below formula where \\(c_n\\) represents the closing price for a specific day, \\(c_{n-1}\\) represents the closing price of the previous day and $R$ represents the daily return,

$$DR = {c_n-c_{n-1} \over c_{n-1}} \times 100$$

# Decay 
In order to give more priority to recent stocks I employed exponential decay. Firstly the exponential weight \\(e_n\\) of a specific day is calculated as follows, 
$$e_n = e_{n-1} \times x$$ where \\(x\\) is the decay factor which ranges from 0 to 1. And considering \\(e_0\\) is equal to 1.
Or in other words,
$$e_n = x^n$$

Using this we know the the adjusted daily return \\(\hat{DR}\\) given the exponential weight \\(e_{n}\\).
$$\hat{DR} = DR \times e_n$$

## Decay Factor
The decay factor \\(x\\) plays a huge role in determining how much priority we need to give to recent stocks. A value for \\(x\\) closer to 1 would value much more of the history than a value closer to 0. To demonstrate this below are graphs of the simulated prices using both these decay parameters.

If \\(x = 0.01\\) , we know that \\(x\\) diminishes quickly, so very recent stocks will have a very high priority hence the following graph,

![Small Decay Parameter](/images/MSFT_small_decay_parameter.png)

Now changing \\(x\\) to \\(.99\\) we see the following,

![Large Decay Parameter](/images/MSFT_large_decay_parameter.png)

Between the two graphs we can see that the graph with \\(x = 0.01\\) appears to predict over a range from \\(401.6\\) to \\(402.4\\) which shows us that as it give more priority to recent prices the low values of the stock which might have been present decades back doesn't affect our analysis. On the other hand we see that the graph with \\(x = 0.99\\) the predicated prices range from \\(390\\) to \\(430\\) which is a much larger about 50 times more than the range of \\(x = 0.01$.

We can take this distinction as a positive thing as we recognize that stable state of a company changes over time and the prices from 40 years back will not help us in analyzing the risk of the stock into the future. 



# Simulation
Using \\(\hat{DR}\\) I was able to give more priority to recent stocks and calculated the mean and standard deviation based on this.

I ran 10 simulations which simulated over 5 years taking into account the adjusted daily return mean and std deviation and taking the initial price as the closing price of the last recorded day. In each simulation I iterated over the number of days over the 5 years and sampled the daily return from a normal distribution. Given the \\(DR_k\\) where \\(k\\) represents the daily return \\(k\\) days from the start of our simulation we can draw a price path by using the following formula,
$$P_k = P_{k-1} \times (1 + {DR_k\over100})$$
Where \\(P_k\\) is the simulated price given the daily return of a given day \\(k$.






