---
layout: post
title: Examining Prediction Market Anomalies
comments: true
image: https://jonathanmann.github.io/public/img/prediction_markets.png
excerpt: Market inefficiencies can be difficult to identify and are usually competed away quickly but, prediction market trading restrictions can cause anomalies to occur frequently.
 
---

Market inefficiencies can be difficult to identify and are usually competed away quickly but, prediction market trading restrictions can cause anomalies to occur frequently.

![PredictionMarket](https://jonathanmann.github.io/public/img/prediction_markets.png)

### Loophole in the Betting Market?

The situation shown above should never exist in an efficient, competitive market. For example, ignoring transaction costs, if a speculator took a position against every candidate, it would be impossible to lose. 

On the prediction market [PredictIt](https://www.predictit.org/) however, this kind of anomaly is a regular occurrence.

This table shows the breakdown of how much would be earned in the case of each candidate winning (and consequentially, the other candidates losing) assuming that bets were placed against all the candidates shown.

<table>
  <thead>
    <tr>
      <th>Winner</th>
	  <th>Losses</th>
	  <th>Earnings</th>
	  <th>Net</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Hillary Clinton</td>
	  <td>8</td>
      <td>11</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Bernie Sanders</td>
	  <td>93</td>
      <td>96</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Joe Biden</td>
	  <td>97</td>
      <td>100</td>
	  <td>3</td>
    </tr>
    <tr>
      <td>Elizabeth Warren</td>
	  <td>99</td>
      <td>102</td>
	  <td>3</td>
    </tr>
  </tbody>
</table>

As an example for how these results are calculated, suppose that Hillary Clinton is the winner of the primary. In this case 8 cents would be paid out on each dollar wagered, but 11 cents would be earned from each dollar wagered (7 paid out from Bernie + 3 from Joe + 1 from Elizabeth). 

No matter the winner, the speculator makes a guaranteed 3 cents for every $4.00 wagered. This seems too good to be true, and sadly, it is.

### No Free Lunch...

When I first stumbled upon this, I thought I had found an inefficiency that could be exploited, but, after some careful reading it turns out that PredictIt charges a 10% fee on winnings and each winning is charged separately. So the actual table looks something like this:

<table>
  <thead>
    <tr>
      <th>Winner</th>
	  <th>Losses</th>
	  <th>Earnings</th>
	  <th>Net</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Hillary Clinton</td>
	  <td>8</td>
      <td>9.9</td>
      <td>1.9</td>
    </tr>
    <tr>
      <td>Bernie Sanders</td>
	  <td>93</td>
      <td>86.4</td>
      <td>-6.6</td>
    </tr>
    <tr>
      <td>Joe Biden</td>
	  <td>97</td>
      <td>90</td>
	  <td>-7</td>
    </tr>
    <tr>
      <td>Elizabeth Warren</td>
	  <td>99</td>
      <td>91.8</td>
	  <td>-7.2</td>
    </tr>
  </tbody>
</table>

### Conclusion

If PredictIt wants to be a tool for accurately estimating event probabilities, they should change their fee structure to act against a market (e.g. the result for the entire Democratic primary contest) rather than assessing fees bet by bet.
