---
layout: post
title: Verifiable Election System Demo
comments: true
image: https://jonathanmann.github.io/public/img/voting_system_frontend.png
excerpt: In an earlier post, we looked at the mechanics behind creating an election system where the results could be verified by any third party while still protecting voter privacy. In this post, we'll look at a demo implementation of a user interface for this system.
---

In an [earlier post](https://jonathanmann.github.io/2015/08/29/transparent-verifialbe-private-elections/), we looked at the mechanics behind creating an election system where the results could be verified by any third party while still protecting voter privacy. In this post, we'll look at a [demo implementation](https://cdn.rawgit.com/jonathanmann/blog_examples/master/Javascript/election_system_demo/index.html) of a user interface for this system.

### Live Demo

You can view a live demo of this implementation [here](https://cdn.rawgit.com/jonathanmann/blog_examples/master/Javascript/election_system_demo/index.html). The source code for the demo is available [here](https://github.com/jonathanmann/blog_examples/tree/master/Javascript/election_system_demo).

### Voting Process Description

![ballot](https://jonathanmann.github.io/public/img/voting_system_frontend.png)

When the voter enters the booth, the ballot appears. The voter can select from the available options. When the voter touches an option, the selected option becomes highlighted. The voter can then confirm the selection with the bottom button. The system then routes to a confirmation screen where a unique identifier is presented along with the ticket the voter selected. A receipt matching the information on the confirmation screen would then print so that the voter could later use the information to verify the the vote was accounted for accurately.

The printed receipt might look like this :

2B182210-64CC-48AD-9022-DFA702A6C641 | NEUTRAL PARTY

Although the process would be over at this time, in the [live demo](https://cdn.rawgit.com/jonathanmann/blog_examples/master/Javascript/election_system_demo/index.html), I added a button to "View Election Result". 

### Result Verification Description

![result](https://jonathanmann.github.io/public/img/demo_result.png)

The [election results screen](https://cdn.rawgit.com/jonathanmann/blog_examples/master/Javascript/election_system_demo/index.html#/result), is a demo of what news agencies and watchdog organizations could put together from the election data after the election results were released. The top section contains a summary of the election results. The second section displays the raw data where the voters could look up the unique identifier from their confirmation cards and verify that their votes were accounted for accurately.

### Demo Limitations

For purposes of the demo, I decided to replace all database calls with static JSON files. This simplification allowed me to have the demo hosted on [rawgit](https://rawgit.com/). The downside of this, however, is that the election results won't change when new votes are cast. To make sure that the unique IDs from the receipts would match the results from the election data, I replaced the UUID generation for a ballot with a single hard-coded UUID for each ticket. The result of this is that, if you run the demo a second time and cast another vote for the same ticket as before, you will get the same unique identifier as the first time you voted. This brings up an interesting security issue: if a malevolent government wanted to rig the election, they could conceivably give two voters who voted for the same party the same unique identifier. When these voters checked the results, it would appear that their vote was appropriately accounted for, but the voters would have no way of knowing that the same vote was being shared by other voters supporting the same ticket. One possible solution for this would be to have voters bring in their own unique identifiers from trusted sources, but I will probably need another post to discuss this idea in detail.  
