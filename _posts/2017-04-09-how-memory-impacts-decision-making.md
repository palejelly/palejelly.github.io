---
layout: post
title: How Memory Impacts Decision Making
comments: true
image: https://jonathanmann.github.io/public/img/q_learning.gif
excerpt: Q-learning is a reinforcement learning technique that provides a model of how agents use memory to make decisions. In my implementation, the agent finds the optimal policy for a Markov decision process by transitioning to the state with the highest expected value.
---

Q-learning is a reinforcement learning technique that provides a model of how agents use memory to make decisions. In my implementation, the agent finds the optimal policy for a Markov decision process by transitioning to the state with the highest expected value.

![AGENT](https://jonathanmann.github.io/public/img/q_learning_steps.gif)

### Explanation
In the visualization above, the agent (represented by the letter "A") has the option to move North, South, East, or West from each position. From each state the agent picks the option with the highest expected value. If there are multiple options with the same highest value, it picks from these at random. Each time it moves, it updates its memory with the result of its decision and, should the agent ever find itself in the same position, the next time it will act based on the memory of what happened previously. Since there is a movement cost, the agent will be movtivated to try new paths if it doesn't get rewarded for a move. The agent moves around the space until it reaches a goal state. After it reaches the goal (represented by the black squre), it starts again, but now with a "memory" of the events that transpired in the previous iterations. As the agent repeats the journey a path begins to emerge, and, in the visualization, the squares leading to the goal state darken as their expected values increase. The iterations stop when the agent converges on a path.

### Implementation
The code is available [here](https://github.com/jonathanmann/q_learning). I'm still in the process of refining it, but, as a next step, I would like to pull out Q-Learning into its own class instead of having it rolled up in the Agent class.

### Motivation
Writing an implementation of Q-learning was suggested to me by one of my brothers as a lens to think about the way in which humans and animals might make decisions based on experience and memory.

### Visualization
After completing the implementation, I thought it would be interesting to provide a visualization. Here is the code:

{% highlight py %}
    def plot_path(self):
        fig = plt.figure()
        plt.clf()
        ax = fig.add_subplot(111)
        ax.set_aspect(1)
        ax.xaxis.tick_top()
        ax.annotate('A',xy=(self.state[1],self.state[0]),color='steelblue',horizontalalignment='center',verticalalignment='center')
        res = ax.imshow(self.space.grid,cmap=plt.cm.Greys,interpolation='nearest')
        s = (5 - len(str(self.step))) * '0' + str(self.step)
        plt.savefig(self.out_path + 'step' + s + '.png')
{% endhighlight %}

### Conclusion
Q-learning illustrates how an agent might arrive at an optimal policy through trial and error.
