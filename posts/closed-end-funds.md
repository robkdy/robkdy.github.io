<!--
.. title: Closed-End Funds: Factors for Machine Learning
.. slug: closed-end-funds
.. date: 2019-07-21 14:58:43 UTC-05:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->


## **USING DISCOUNTS AND OTHER FACTORS TO  PREDICT FUTURE RETURNS**


## Introduction

Closed-End Funds (CEFs) are a particular type of investment vehicle with some interesting properties. They are like traditional open-ended mutual funds with unique properties like:

*   shares are not created or redeemed when traded
*   leverage is allowed
*   shares are traded throughout the day

These properties result in CEFs trading at a discount or premium to their net asset value (NAV). Whereas, stocks, ETFs, and mutual funds trade at or very close to their NAV.  The question is can you predict future returns based on the discount/premium, combined with other variables like leverage, expense ratio, etc.

The closed-end fund market is very small compared to other forms of investments. The entire landscape of the closed-end fund market is about 500 funds and around $250 billion in net assets. To put this in perspective, there are 16 companies in the S&P 500 with a greater market cap than the entire closed-end fund space! Mutual fund assets are around $18 trillion, and ETFs are around $5 trillion. This is probably why there is far less written about this space. The really big investors will probably ignore this space due to small net assets and lack of liquidity.


## Data

The Closed-End Funds (CEFs) data consisted of daily historical prices, NAVs, adjusted prices, volume, and distributions dating back to the year 2000. In addition, fundamental data consisting of category, market capitalization, net expense ratio, and distribution frequency, was available for each fund. 

The fundamental data for all the funds broke them down into 55 different categories. To simplify, I narrowed down to 5 categories. 



1. Equity US
2. Equity Foreign
3. Fixed Income (non- Municipal)
4. Municipal Bonds
5. Precious Metals

Precious Metals only had 4 funds, so that was deleted to simplify further. Removing funds without at least a 5-yr record reduced the total number of funds to 445 from the initial 499 number of funds. 


## Exploratory Data Analysis

In this section, I will explore correlations of certain factors (independent variables) to future returns (dependent variables). 


### Discount

Unlike mutual funds, ETFs, and stocks, closed-end funds can trade at a considerable discount to their net asset value. We can explore this uniqueness by plotting regressions that show correlations between discounts and forward 1, 3, 5, and 10 year returns across all four categories. The plots are below.


![](/images/cef-blog0.png "")

![](/images/cef-blog1.png "")

![](/images/cef-blog2.png "")

![](/images/cef-blog3.png "")


The pearson correlation between discounts and returns for categories can be summarized by the following table:



<table border="1">
  <tr>
    <td><p style="text-align: left">
<strong>Category</strong></p>

   </td>
   <td><p style="text-align: left">
<strong>1-yr return</strong></p>

   </td>
   <td><p style="text-align: left">
<strong>3-yr return</strong></p>

   </td>
   <td><p style="text-align: left">
<strong>5-yr return</strong></p>

   </td>
   <td><p style="text-align: left">
<strong>10-yr return</strong></p>

   </td>
  </tr>
 
  <tr>
   <td><p style="text-align: left">
<strong>Equity Foreign</strong></p>
   </td>

   <td><p style="text-align: right">
0.026912</p>
   </td>

   <td><p style="text-align: right">
-0.260944</p>

   </td>
   <td><p style="text-align: right">
-0.583268</p>

   </td>
   <td><p style="text-align: right">
-0.62627</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: left">
<strong>Equity US</strong></p>

   </td>
  
   <td><p style="text-align: right">
-0.235003</p>

   </td>
   <td><p style="text-align: right">
-0.06195</p>

   </td>
   <td><p style="text-align: right">
-0.404102</p>

   </td>
   <td><p style="text-align: right">
-0.176689</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: left">
<strong>Fixed Income</strong></p>

   </td>
   
   <td><p style="text-align: right">
-0.208146</p>

   </td>
   <td><p style="text-align: right">
-0.289813</p>

   </td>
   <td><p style="text-align: right">
-0.545451</p>

   </td>
   <td><p style="text-align: right">
-0.221148</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>Municipal Bonds</strong></p>

   </td>
 
   <td><p style="text-align: right">
-0.49617</p>

   </td>
   <td><p style="text-align: right">
-0.479254</p>

   </td>
   <td><p style="text-align: right">
-0.444536</p>

   </td>
   <td><p style="text-align: right">
-0.607414</p>

   </td>
  </tr>
</table>


The best correlation between Discount and 1 and 3-yr returns is in Municipal Bonds. For 5 and 10-yr returns it is Equity Foreign. For Equity Foreign and Muni Bonds, the best correlation is for 10-yr returns, for Equity US and Fixed Income the best correlation is for 5-yr returns. They tend to get better the longer the time horizon, Muni bonds appear to correlate the best, and none of the correlations are all that strong. All these correlations are for the mean values of all the funds in the category. If we go down to an individual fund, we might find it more interesting. For example, here is the discount to 1-yr return for the fund NID, which shows a much better correlation.









### Z-Score

There is another measure of discount that might correlate better with future returns, called the Z-score. Just like in statistics, the Z-score of closed-end funds is the difference of the discount to mean of the discount over a specified period, divided by the standard deviation of the discount over the same period.

 ![](/images/cef-blog18.png "")

 ![](/images/cef-blog19.png "")

![](/images/cef-blog20.png "")

![](/images/cef-blog21.png "")
![](/images/cef-blog22.png "")

Below are the correlation scatter plots of various periods of z-scores with the subsequent 1-yr return.





![](/images/cef-blog5.png "")






![](/images/cef-blog6.png "")


It appears that the Z-score is not very predictive of 1-year returns. However like discounts, perhaps at an individual fund level, there may be better correlations.


### Past Performance

Often, funds will state “past performance is no guarantee of future results”. Let’s see if they actually correlate. Here is the past 1-yr returns versus the forward 1-yr returns and the past 10-yr returns versus the forward 1-yr returns for the period ending on 03/05/2019.






![](/images/cef-blog7.png "")







![](/images/cef-blog8.png "")


The funds are correct to state that past performance doesn’t result in future results. All categories but fixed income don’t show much of any correlation. For fixed income, 10-yr return shows a better correlation than 1-yr returns. Maybe there is persistent skill in managing fixed income funds?


### The 200 Day Moving Average

The 200-day moving average (200dma) is a popular indicator as a smoothing function and longer term trend indicator. We can calculate how far as a percent from the 200dma each fund is and see if it correlates with future 1-yr returns for the period ending 03/05/19.






![](/images/cef-blog9.png "")


It looks like being below the 200dma is better for future returns, but not always. Again it would probably be better to drill down to individual funds and across different periods.


### Net Expense Ratio

Net expense ratio is how much the funds charge to manage the assets. The data set does not have historical net expense ratio so I can’t really do an accurate analysis. Assuming they were similar to a year ago, we can see what happened in the last year.






![](/images/cef-blog10.png "")


Nothing much there.

To summarize, we’ll take a closer look at the following to put in a model:



*   Discount
*   Past 10-yr return
*   Percent from 200 day moving average


## In-Depth Analysis

Specific funds may correlate better than the general market, so I will try to determine which funds to predict. I’m going to focus on predicting the 1-yr forward return. Eliminating funds without a past 10-yr return reduces the number of funds to 359 from 445.

Best discount to 1-yr forward return correlation:


<table border="1">
  <tr>
   <td><p style="text-align: right">
<strong>Ticker</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>1-yr forward return</strong></p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>GDL</strong></p>

   </td>
   <td><p style="text-align: right">
-0.889059</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>AOD</strong></p>

   </td>
   <td><p style="text-align: right">
-0.855981</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>FEO</strong></p>

   </td>
   <td><p style="text-align: right">
-0.847928</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>AGD</strong></p>

   </td>
   <td><p style="text-align: right">
-0.844202</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>BKT</strong></p>

   </td>
   <td><p style="text-align: right">
-0.843367</p>

   </td>
  </tr>
</table>
  

Best below 200 day moving average to 1-yr forward return correlation:


<table border="1">
  <tr>
   <td><p style="text-align: right">
<strong>Ticker</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>1-yr forward return</strong></p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>PKO</strong></p>

   </td>
   <td><p style="text-align: right">
-0.831584</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>MHI</strong></p>

   </td>
   <td><p style="text-align: right">
-0.78961</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>HTY</strong></p>

   </td>
   <td><p style="text-align: right">
-0.789423</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>FMY</strong></p>

   </td>
   <td><p style="text-align: right">
-0.768721</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>BTA</strong></p>

   </td>
   <td><p style="text-align: right">
-0.765692</p>

   </td>
  </tr>
</table>
  

Best past 10-yr return to 1-yr forward return correlation:


<table border="1">
  <tr>
   <td><p style="text-align: right">
<strong>Ticker</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>1-yr forward return</strong></p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>CAF</strong></p>

   </td>
   <td><p style="text-align: right">
0.759755</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>FGB</strong></p>

   </td>
   <td><p style="text-align: right">
0.385954</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>IRL</strong></p>

   </td>
   <td><p style="text-align: right">
0.304636</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>RIF</strong></p>

   </td>
   <td><p style="text-align: right">
0.275595</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>BKT</strong></p>

   </td>
   <td><p style="text-align: right">
0.249453</p>

   </td>
  </tr>
</table>


Graphically, here’s how some funds visually relates to a corresponding feature.






![](/images/cef-blog11.png "")







![](/images/cef-blog12.png "")







![](/images/cef-blog13.png "")


I’m going to pick the Linear Regression, and Random Forest Regression model to see if any predictions are possible. Running all the funds through each model results in the following funds with the best R2 score on the out of sample test data.

Linear Regression:


<table border="1">
  <tr>
   <td><p style="text-align: right">
<strong>Ticker</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>R2 Score</strong></p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>MFV</strong></p>

   </td>
   <td><p style="text-align: right">
0.676503</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>GCV</strong></p>

   </td>
   <td><p style="text-align: right">
0.605562</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>GRX</strong></p>

   </td>
   <td><p style="text-align: right">
0.598048</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>SRV</strong></p>

   </td>
   <td><p style="text-align: right">
0.546841</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>PNF</strong></p>

   </td>
   <td><p style="text-align: right">
0.538229</p>

   </td>
  </tr>
</table>
  

Random Forest Regression:


<table border="1">
  <tr>
   <td><p style="text-align: right">
<strong>Ticker</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>R2 Score</strong></p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>EGF</strong></p>

   </td>
   <td><p style="text-align: right">
0.811655</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>BTA</strong></p>

   </td>
   <td><p style="text-align: right">
0.797531</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>QQQX</strong></p>

   </td>
   <td><p style="text-align: right">
0.69472</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>ADX</strong></p>

   </td>
   <td><p style="text-align: right">
0.44853</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>RMT</strong></p>

   </td>
   <td><p style="text-align: right">
0.430567</p>

   </td>
  </tr>
</table>


Linear Regression for MFV and GCV:






![](/images/cef-blog14.png "")







![](/images/cef-blog15.png "")


Random Forest Regression for EGF






![](/images/cef-blog16.png "")







![](/images/cef-blog17.png "")



## Conclusions

Using the features of Discount to NAV, Past 10-yr returns, and Distance from 200 day moving average, and using the Linear Regression and Random Forest Regression models, at least some funds show some predictability through machine learning. This is just a starting point of exploring this space. Some other areas to dive deeper include:



*   The categories may have been oversimplified. If I break it down further, I could compare future returns versus the category to get outperformance.
*   Different future return periods. E.g. 1-month, or 10-years.
*   More feature engineering to get seasonal trends, slope of moving averages, etc.
*   Try more ML models

<!-- Docs to Markdown version 1.0β17 -->

