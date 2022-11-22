---
layout: post
title: Modeling Verifiable Voting that Protects Privacy
comments: true
image: https://jonathanmann.github.io/public/img/ballot.png
excerpt: Technology has made it possible to design an election system that is both transparent and verifiable, but still protects the privacy of the ballot box.   
---

Technology has made it possible to design an election system that is both transparent and verifiable, but still protects the privacy of the ballot box. 

### Description

![ballot](https://jonathanmann.github.io/public/img/ballot.png)

The system works by generating a unique identifier for each of the voter's descisions. After the voting is complete, the voter receives a perforated printout with their unique identifiers on one side, and the candidates they selected on the other. When the results of the election are released, the each voter can indepently verify that the identifiers corresponding to their votes are matched to the appropriate candidates. 

### Demonstration

As a demonstration, I created a [PostgreSQL database](https://github.com/jonathanmann/blog_examples/blob/master/PostgreSQL/election_model/election_model.backup) to model a simplified version of this system and populated it with mock election data from an imaginary election between four parties : Neutral, Third, Libertarian, and Socialist. 

In this example, suppose a voter casts a vote for the Neutral party and receives a confirmation card with the following information :

2B182210-64CC-48AD-9022-DFA702A6C641 | NEUTRAL PARTY

Later, when the election results are released, the voter can look up the unique identifier from the confirmation card and verify that the vote was accounted for accurately.

Presumably, a trusted watchdog group would do the work of creating the [backend query](https://github.com/jonathanmann/blog_examples/blob/master/PostgreSQL/election_model/vote_verification.sql) and provide the voter with an interface to verify the accuracy of the vote simply by the voter entering the unique identifier from the confirmation card.

{% highlight sql %}
SELECT B.ID, T.DESCRIPTION
FROM BALLOT B
INNER JOIN TICKET T ON B.TICKET_ID = T.ID
WHERE B.ID = '2B182210-64CC-48AD-9022-DFA702A6C641'
{% endhighlight %}

![verification](https://jonathanmann.github.io/public/img/vote_verification.png)


With the free availablity of the election results database, anyone with basic SQL knowledge could [verify the results of the election](https://github.com/jonathanmann/blog_examples/blob/master/PostgreSQL/election_model/election_tally.sql) .

{% highlight sql %}
SELECT DESCRIPTION , COUNT(DESCRIPTION) CNT FROM
(
	SELECT B.ID, T.DESCRIPTION 
	FROM BALLOT B 
	INNER JOIN TICKET T ON B.TICKET_ID = T.ID
) RESULTS
GROUP BY DESCRIPTION 
ORDER BY CNT DESC
{% endhighlight %}

![election_results](https://jonathanmann.github.io/public/img/election_results.png)

Finally, anyone interested in viewing the raw results of the election could easily do so in [tabular form](https://github.com/jonathanmann/blog_examples/blob/master/PostgreSQL/election_model/election_data.sql).

{% highlight sql %}
SELECT B.ID, T.DESCRIPTION
FROM BALLOT B
INNER JOIN TICKET T ON B.TICKET_ID = T.ID
{% endhighlight %}

![election_data](https://jonathanmann.github.io/public/img/election_data.png)
