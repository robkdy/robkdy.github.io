<!--
.. title: Predicting Recessions
.. slug: predicting-recessions
.. date: 2019-08-27 20:10:17 UTC-05:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->





# USING MACHINE LEARNING TO PREDICT THE START OF RECESSIONS


## Problem

What indicators should you use to predict recessions? How about all of them! Since humans typically don't get any better at predictions after 5-10 pieces of relevant information (only more confident), this problem seems well-suited for the machines. As in, AI, machine learning, neural networks, deep learning, type of machines. Consider the Federal Reserve Bank of St. Louis Economic Data (FRED) website has 570,000 data series (up from 560,000 a couple of months ago), what chance do humans really have in processing that data. 570,000! One website! Really? I guess someone could be using the "Number of Identified Exporters to Greenland from Kansas" data series in their understanding of the economy. It's a real series, look it up.


## Data

Even 570,000 data series is a lot for machines (at least my machine). Ok, I didn’t really use ‘all of them’ data. I wanted to look at the last seven recessions, which means I needed data series that goes back to 1969. This, plus some series are discontinued, some series are only reported annually, and the FRED API only let me have about 100 series at a time, reduced the number of data series down to about 6000. Eliminating the data series that didn’t make sense to me, and adding three derivative series for each one resulted in a final of 1831 indicators to train the models. For each series from FRED, I created the following derivatives.



*   Difference from 1-year ago
*   Difference from 1-quarter ago
*   Difference from 10-month moving average

This to detrend the data, add momentum measures, and because...more data. 

All data series are down-sampled or up-sampled as necessary to result in a monthly data series. Consequently, all predictions are made on a monthly basis. Since data are often revised, only the first release is used so that the data would be the one available if one were to make predictions in real time. 


## Start of Recessions

I’m only interested in this project to predict the start of a recession. I define this as the period three months prior to three months after the official start of a recession. For example, if a recession starts in 12/2007, the recession start labels are from 09/2007 to 02/2008, inclusive. The data in the period of the recession but not at the start are then dropped. Recessions are based on the NBER (National Bureau of Economic Research) defined dates. And no, recessions are not “defined as two consecutive quarters of negative growth (GDP)”. Look at the 2001 recession, look at GDP during that time, use NBER dates.

Why only three months in advance? Because “predictions are hard, especially about the future”. Things change fast, I don’t think predicting a year out is practical. There’s this research group - described as “We are economists, statisticians, data scientists, segment specialists… In short, really smart people who like working together on tough challenges.” They have an AI based recession forecasting model that predicts ahead 24 months. Here’s from their report on July 3, 2018. “Probability of recession starting within: ...12 months:  92.2%...” So there was a 92.2% percent chance that a recession started by July of 2019. Ouch. That’s why I’m sticking with three months.


## Correlations

Here are some interesting indicators that are relatively highly correlated to the start of recessions - based on Pearson, Spearman, Kendall Tau correlations, and Granger causality.



*   10-Year Treasury Constant Maturity Minus Federal Funds Rate
*   New Private Housing Units Authorized by Building Permits, difference from 10-month MA
*   New Houses Sold by Stage of Construction, Total, difference from 1-year ago
*   Leading Indicators OECD: Leading indicators: CLI: Trend restored for the United States, difference from 10-month MA
*   Real Retail Sales, difference from 10-month MA
*   Smoothed U.S. Recession Probabilities


## Models

This is a binary classification problem predicting either ‘recession’ or ‘no recession’. I used the following models to make predictions:



1. Logistic Regression
2. Random Forests
3. Support Vector Machine
4. K-Nearest Neighbors
5. Neural Network

That’s a lot of models. I got carried away, it’s just more code, why not? A split of about 70% of the data was used as training data and the rest as test data. The entire date range has 7 recessions, with the training data having 6 recessions and the test data 1 recession.

Each of the first 4 models was tuned by doing a grid search on a 6-fold cross validation. Each fold was custom designed to start at the midpoint between two recessions and ending at the midpoint of the next two recessions. This makes each fold have one recession to test against. Of course, the first fold has the starting point at the beginning of the training data and the last fold has the ending point at the end of the training data.

The neural network was validated by splitting the training data into 70%/30% training/validation split. The scoring metric used was the log-loss scoring. The 30% validation data included the last of the six recessions in the entire training data. The number of layers, nodes per layer, and optimizer were manually manipulated to get a well performing model.


## Results

Here are the graphical results for each model in predicting the last two recessions. Remember, the first four models were trained and fitted to the first six of seven recessions, and the Neural Network was trained on the first five of seven recessions.






![alt_text](/images/recession-blog0.png "")






![alt_text](/images/recession-blog1.png "")






![alt_text](/images/recession-blog2.png "")







![alt_text](/images/recession-blog3.png "")





![alt_text](/images/recession-blog4.png "")


Here’s how the models scored across various metrics. For log loss, lower is better, all other scores is higher is better. The probability of recession  represents data for the month of July 2019. My custom scoring metric penalizes a model that missed a recession completely, i.e. no recession classification in the 6-months before or 3-months after the start of a recession, or a false positive recession that was outside that range. If either of those conditions were true, the model scores a zero, else it scores as the balanced accuracy metric. All of the models except for the Neural Network were tuned using the custom scoring metric.


<table border="1" cellspacing="3" cellpadding="3">
  <tr>
   <td>
   </td>
   <td><p style="text-align: right">
<strong>Log_Reg</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>Rand_For</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>Sup_Vec</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>K_Near</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>Neur_Net</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>Mean</strong></p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>log_loss</strong></p>

   </td>
   <td><p style="text-align: right">
0.077</p>

   </td>
   <td><p style="text-align: right">
0.107</p>

   </td>
   <td><p style="text-align: right">
0.044</p>

   </td>
   <td><p style="text-align: right">
0.645</p>

   </td>
   <td><p style="text-align: right">
0.040</p>

   </td>
   <td><p style="text-align: right">
0.182</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>f1_score</strong></p>

   </td>
   <td><p style="text-align: right">
0.800</p>

   </td>
   <td><p style="text-align: right">
0.800</p>

   </td>
   <td><p style="text-align: right">
0.800</p>

   </td>
   <td><p style="text-align: right">
0.667</p>

   </td>
   <td><p style="text-align: right">
1.000</p>

   </td>
   <td><p style="text-align: right">
0.813</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>roc_auc_score</strong></p>

   </td>
   <td><p style="text-align: right">
0.833</p>

   </td>
   <td><p style="text-align: right">
0.833</p>

   </td>
   <td><p style="text-align: right">
0.833</p>

   </td>
   <td><p style="text-align: right">
0.750</p>

   </td>
   <td><p style="text-align: right">
1.000</p>

   </td>
   <td><p style="text-align: right">
0.850</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>my_score</strong></p>

   </td>
   <td><p style="text-align: right">
0.833</p>

   </td>
   <td><p style="text-align: right">
0.833</p>

   </td>
   <td><p style="text-align: right">
0.833</p>

   </td>
   <td><p style="text-align: right">
0.750</p>

   </td>
   <td><p style="text-align: right">
1.000</p>

   </td>
   <td><p style="text-align: right">
0.850</p>

   </td>
  </tr>
  <tr>
   <td><p style="text-align: right">
<strong>prob_of_recession</strong></p>

   </td>
   <td><p style="text-align: right">
0.140</p>

   </td>
   <td><p style="text-align: right">
0.058</p>

   </td>
   <td><p style="text-align: right">
0.057</p>

   </td>
   <td><p style="text-align: right">
0.000</p>

   </td>
   <td><p style="text-align: right">
0.019</p>

   </td>
   <td><p style="text-align: right">
0.055</p>

   </td>
  </tr>
</table>


###
## Conclusion

As of data for the month of July 2019, the mean probability across all models of a recession start within three months is 5.5%, ranging from 0% to 14%. 

The two models that are most impressive are the Random Forest and the Neural Network. The Random Forest because it was the only model to nail all six recessions that were held out for cross validation, with no false positives. With predicting the seventh recession in the test data, that’s seven for seven in predicting recessions out of sample! Within three months! No false positives! The Neural Network because it has the best scores on the test data, and it was only trained on the first five recessions!

Comparing to the popular 10yr-2yr yield curve. In 2007, it was 22 months early, and in 2001, it was 33 months early. That would score a zero on my metric. So, why use a few indicators to make your recession calls, use all of them, or at least much more than our monkey brains can handle. The economy is complicated, leave it to the machines to predict recessions.


<!-- Docs to Markdown version 1.0β17 -->
