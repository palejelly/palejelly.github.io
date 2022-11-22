---
layout: post
title: Visualizing Developer Income by State with D3
comments: true
image: https://jonathanmann.github.io/public/img/developer_income_by_state.png
excerpt: The average salary for developers varies widely across the United States from $129K/year in DC to $63K/year in Hawaii according to salary data from Indeed. Using D3 to create a choropleth map, we can quickly see which states have the most lucrartive tech opportunites.
 
---

The average salary for developers varies widely across the United States from $129K/year in DC to $63K/year in Hawaii according to salary data from Indeed. Using D3 to create a choropleth map, we can quickly see which states have the most lucrartive tech opportunites.

![HEAT_MAP_DEVELOPER_SALARY_BY_STATE](https://jonathanmann.github.io/public/img/developer_income_by_state.png)

### Live Implementation
You can view a live implementation of this visualization [here](https://cdn.rawgit.com/jonathanmann/blog_examples/master/Javascript/d3/developer_income_by_state/index.html). The source code is available [here](https://github.com/jonathanmann/blog_examples/tree/master/Javascript/d3/developer_income_by_state).

### Creating the Visualization
If you are provided with the [D3 javascript state map](https://github.com/jonathanmann/blog_examples/blob/master/Javascript/d3/developer_income_by_state/state_map.js) and [indeed salary data](https://github.com/jonathanmann/blog_examples/blob/master/Javascript/d3/developer_income_by_state/data/state_data.csv), creating the visualization is simple. 

First, create a tooltip to view the state's average developer salary on hover using the following snipit:
{% highlight js %}
function tooltipHtml(n, d){	
  return "<h4>"+n+"</h4><table>" +
    "<tr><td>Average</td><td>$" + 
    (d.avg) + "K</td></tr>" +
    "</table>"
}
{% endhighlight %}

Now use the code below to loop over the data in the provided csv to map it to the javascript state map.
{% highlight js %}
var sampleData = {}
var state_data = []
d3.csv("data/state_data.csv", function(data) {
  state_data = data
  for (var e in state_data){
    var z = state_data[e]
    var i = z["id"]
    var s = z["Salary"] 
    sampleData[i]={id:i, avg:s, color:d3.interpolate("steelblue","black")((s -70)/100)}
  }

  uStates.draw("#statesvg", sampleData, tooltipHtml)
});
{% endhighlight %}

### Conclusion
D3 provides an extraordinarily easy and powerful set of tools for visualizing state level data.


