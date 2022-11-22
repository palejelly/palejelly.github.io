---
layout: post
title: Modeling the Monty Hall Problem with Python
comments: true
image: https://jonathanmann.github.io/public/img/monty_hall.png
excerpt: At first glance, the Monty Hall problem can seem counterintuitive, but, when viewed from the correct perspective, the appropriate strategy becomes abundantly apparent.  
---

At first glance, the Monty Hall problem can seem counterintuitive, but, when viewed from the correct perspective, the appropriate strategy becomes abundantly apparent.

![monty_hall](https://jonathanmann.github.io/public/img/monty_hall.png)

Although there are many excellent explanations for why the it is always a [better strategy to change doors](https://www.youtube.com/watch?v=ugbWqWCcxrg), after discussing the solution with one of the interns at the office, I thought it might be a handy excercise to write a simulator to model the outcomes for different strategies.

### Simulation 

The code for the [simulation in Python](https://github.com/jonathanmann/blog_examples/blob/master/Python/monty_hall/monty_hall.py) :

{% highlight py %}
from random import randint

class MontyHall:
    """
    The Monty Hall Problem
    """
    def __init__(self,doors=3,change=False):
        """
        Initialize game

        @param doors : an int used to generate the doors list from one up to the input number (must be at least 2).
        @param change : a boolean to determine whether to stay with the original door pick
        """
        self.doors = [x for x in xrange(1,doors + 1)]
        self.change = change

    def play(self):
        """
        Play game

        @return : a boolean representing whether the correct door was choosen
        """

        #copy the doors list
        doors = self.doors + []
        
        #randomly assign the prize to one of the doors
        prize = doors[randint(0,len(doors) - 1)]

        #randomly assign a guess for which door the prize is behind
        choice = doors.pop(randint(0,len(doors) - 1))

        #open all unchosen doors except for one and call it the "alternative"
        #let the player select between the chosen door and the alternative
        #if the prize was not selected, make the alternative door the prize
        #otherwise, pick a door at random for the alternative
        if prize in doors:
            alternative = prize
        else:
            alternative = doors.pop(randint(0,len(doors) - 1))
    
        #if change is True, pick the alternative door
        #otherwise stay with the choosen door
        if self.change:
            choice = alternative
    
        #return True if the correct door was selected
        return prize == choice

    def test(self,iterations=1000):
        """
        Test strategy

        @param iterations : 
        @return : a float representing the success rate for the chosen strategy
        """
        score = 0
        i = 0
        while i < iterations:
            if self.play():
                score += 1
            i+=1
        return float(score) / iterations


m = MontyHall()
print m.test()

#change the default strategy from False to True
m = MontyHall(change=True)
print m.test()
{% endhighlight %}

### Results

Just as expected, over the course of 1000 trials, the simulator shows that the winning strategy is to always change doors.

<table>
  <thead>
    <tr>
      <th>Doors</th>
	  <th>Strategy</th>
	  <th>Trials</th>
	  <th>Wins</th>
      <th>Losses</th>
      <th>Success Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
	  <th>Stay</th>
	  <th>1000</th>
	  <th>344</th>
      <th>656</th>
      <th>34.4%</th>
    </tr>
     <tr>
      <th>3</th>
	  <th>Change</th>
	  <th>1000</th>
	  <th>662</th>
      <th>338</th>
      <th>66.2%</th>
    </tr>
    <tr>
      <th>5</th>
	  <th>Stay</th>
	  <th>1000</th>
	  <th>224</th>
      <th>776</th>
      <th>22.4%</th>
    </tr>
     <tr>
      <th>5</th>
	  <th>Change</th>
	  <th>1000</th>
	  <th>796</th>
      <th>204</th>
      <th>79.6%</th>
    </tr> 
  </tbody>
</table>
