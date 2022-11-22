---
layout: post
title: Price Targets a Poor Predictor of Performance
comments: true
image: https://jonathanmann.github.io/public/img/current_v_target_pred.png
excerpt: In order to determine the relationship between consensus price targes and stock performance, I decided to measure the accuracy of predictions made from 2013 to the first two quarters of 2014 against the actual results from the matching quarter of the following year.  
---

In order to determine the relationship between consensus price targes and stock performance, I decided to measure the accuracy of predictions made from 2013 to the first two quarters of 2014 against the actual results from the matching quarter of the following year.

### Visualization 

![price_target_accuracy](https://jonathanmann.github.io/public/img/price_target_accuracy.png) 

The [code for the visualization in R](https://github.com/jonathanmann/blog_examples/blob/master/R/price_target_accuracy/price_target_accuracy.R) :

{% highlight r %}
require("ggplot2")
df <- read.csv(file="quarterly_stock_data.csv",head=TRUE,sep=",")
ggplot(df, aes(x=df$ProjectedGrowth, y=df$ActualGrowth)) + 
  geom_point(shape=1,size=.6,color="steelblue") +
  ylim(-1,2) + xlim(-1,2) +
  xlab("Projected") + ylab("Actual") + 
  theme(
    panel.background = element_rect(fill = "transparent",colour = NA),
    panel.grid.minor = element_blank(),
    panel.grid.major = element_blank(),
    plot.background = element_rect(fill = "transparent",colour = NA)
  )
{% endhighlight %}

### Analysis

Originally, I had expected to see a visible correlation between the predictions and the results, but the visualization of this relationship shows just how wrong my anticipation was. I decided to see if limiting the sample to stocks from the S&P500 with a narrow prediction window would improve the accuracy. I'll leave the final interpretation to you, but, to me, the results look pretty random.

![sp_target_accuracy](https://jonathanmann.github.io/public/img/sp_target_accuracy.png) 

{% highlight r %}
require("ggplot2")
dat2<- read.csv(file="quarterly_sp_data.csv",head=TRUE,sep=",")
ggplot(dat2, aes(x=dat2$ProjectedGrowth, y=dat2$ActualGrowth)) + geom_point(shape=1,size=.6,color="steelblue") + 
  #ylim(-.5, 1.5) + xlim(-.3,.7) + 
  ylim(-.01,.2) + xlim(-.01,.2) +
  xlab("Projected") + ylab("Actual") + theme(
    panel.background = element_rect(fill = "transparent",colour = NA), # or theme_blank()
    panel.grid.minor = element_blank(), 
    panel.grid.major = element_blank(),
    plot.background = element_rect(fill = "transparent",colour = NA)
  )
{% endhighlight %}

To satisfy my curiosity, I decided to compare the predictive power of the consensus price targets against a the hypothesis that the stock price would remain exactly the same as when the initial observation was made for stocks in the S&P 500.

![current_v_target](https://jonathanmann.github.io/public/img/current_v_target_pred.png) 

It appears that, for this time period, assuming no change whatsoever would have been a better prediction. My interpretation of these results is not that price targets are useless, but rather that external and unpredictable market forces have far more influence over the performance of a stock.

Here is a sample of the data I used to perform this analysis.

{% highlight sql %}
select t1.ticker, t1.yr, t1.qtr, t1.average_price , t1.average_target, t2.average_price, 
(t1.projected_growth - 1) projected_growth, round(t2.average_price/t1.average_price,3) - 1 actual_growth
from t_sum t1 inner join t_sum t2 on t1.ticker = t2.ticker 
and t1.qtr = t2.qtr and (t1.yr + 1) = (t2.yr)
where t1.ticker = 'AMD' and t1.yr = 2013
{% endhighlight %}


<table>
  <thead>
    <tr>
      <th>Ticker</th>
	  <th>Year</th>
	  <th>Quarter</th>
	  <th>Price</th>
      <th>Target</th>
      <th>Actual</th>
      <th>ProjectedGrowth</th>
      <th>ActualGrowth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AMD</td>
      <td>2013</td>
	  <td>1</td>
      <td>2.460</td>
	  <td>2.750</td>
	  <td>3.797</td>
	  <td>0.118</td>
	  <td>0.543</td>
    </tr>
     <tr>
      <td>AMD</td>
      <td>2013</td>
	  <td>2</td>
      <td>3.830</td>
	  <td>2.890</td>
	  <td>3.906</td>
	  <td>-0.245</td>
	  <td>0.020</td>
    </tr>  
     <tr>
      <td>AMD</td>
      <td>2013</td>
	  <td>3</td>
      <td>3.718</td>
	  <td>4.050</td>
	  <td>3.620</td>
	  <td>0.089</td>
	  <td>-0.026</td>
    </tr>
     <tr>
      <td>AMD</td>
      <td>2013</td>
	  <td>4</td>
      <td>3.478</td>
	  <td>4.200</td>
	  <td>2.741</td>
	  <td>0.208</td>
	  <td>-0.212</td>
    </tr> 
  </tbody>
</table>
