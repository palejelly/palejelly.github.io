---
layout: post
title: Processing Big Data with Elastic MapReduce
comments: true
image: https://jonathanmann.github.io/public/img/emr_cluster.png
excerpt: A few years ago, setting up a Hadoop cluster was a painstaking challenge, especially with ever shifting dependencies and documentation that was usually not quite up-to-date. Today things are easier, but, even now, setting up a cluster is no small task. Recently, however, I ran some experiments with Amazon's Elastic MapReduce (EMR) and the entire process was swift and painless.
---

A few years ago, setting up a Hadoop cluster was a painstaking challenge, especially with ever shifting dependencies and documentation that was usually not quite up-to-date. Today things are easier, but, even now, setting up a cluster is no small task. Recently, however, I ran some experiments with [Amazon's Elastic MapReduce (EMR)](https://aws.amazon.com/elasticmapreduce/), and the entire process was swift and painless. 

![EMR](https://jonathanmann.github.io/public/img/emr_cluster.png)

### Data Wrangling Challenge

Recently, a friend and I were discussing a problem he was working on where he had written a Python parser to go through every file in a giant text collection to build a tally of how many times each word was used as a means of collecting data for some language learning software he had been working on. The code worked, but it needed to be run in batches, and the process was taking hours to run. We discussed the possiblity of using Hadoop, but, both having played with it previously, agreed that it would probably be more trouble than it was worth. The conversation made me curious though. 

### Discovering Elastic MapReduce
 
A few days later, I decided to try running Hadoop on DigitalOcean. As I was setting up the first droplet, I decided to check and see if Amazon had any solutions (I run instances with both Amazon and DigitalOcean, but I usually keep my EC2 instances offline when I'm not using them since they are more expensive than DigitalOcean droplets). What I discovered was Elastic MapReduce: a prepackaged solution that was so easy to set up and run that I was actually able to finish running a job using EMR with a master node and two core nodes before I had even finished setting up my first DigitalOcean droplet. 

### Running a Job

Running a job with EMR is amazingly simple. Just upload your mapper and reducer scripts to S3 and run the job (for this job, the built in aggregate function is perfect as a reducer so there is no need for a reducer script):

{% highlight sh %}
hadoop-streaming -files s3://elasticmapreduce/samples/wordcount/wordSplitter.py -mapper wordSplitter.py -reducer aggregate -input s3://elasticmapreduce/samples/wordcount/input -output "s3://hadoop-mapreduce-data/wordcount/output/2015-11-09/20-37-31 (UTC-5)"
{% endhighlight %}

### Conclusion

I really like DigitalOcean and will continue using it for most of my IaaS needs, but EMR allows me to focus on the parts of MapReduce that I actually want to work on: writing the mapper and reducer. Interestingly, one of the sample jobs provided by AWS was almost exactly what my friend needed. EMR is my new goto option for running big data jobs.
