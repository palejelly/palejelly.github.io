---
layout: post
title: Basic Querying with PostgreSQL
comments: true
image: https://jonathanmann.github.io/public/img/vim_query.png
excerpt: In the previous post, we looked at data normalization in PostgreSQL. In this tutorial, we'll expand on our work to write queries that answer questions about the data. 
---

In the [previous post](https://jonathanmann.github.io/2015/10/23/sql-normalization-tutorial/), we looked at data normalization in PostgreSQL. In this tutorial, we'll expand on our work to write queries that answer questions about the data. 

![PostgreSQL](https://jonathanmann.github.io/public/img/vim_query.png)

This tutorial uses a database of famous people. I make no claims about the reliability of the data, however, the information in the database will be very useful for the purposes of this tutorial. The main table is called [person] which which depends on data in two ancillary tables, [occupation] and [nationality].

Before starting, please download and import [this practice database](https://github.com/jonathanmann/blog_examples/tree/master/PostgreSQL/famous_people/famous_people_wrangled.backup?raw=true) into PostgreSQL.

### Data Exploration

The first thing we should do is try to get a better picture of the data we will be working with. To do this, we can join the related tables and look at the first ten rows with the following query:

{% highlight sql %}
SELECT * FROM person p
INNER JOIN occupation o ON o.id= p.occupation_id
INNER JOIN nationality n ON n.id = p.nationality_id
ORDER BY estimated_iq_score desc
LIMIT 10
{% endhighlight %}

Because we know our data is highly normalized and complete, we can use an INNER JOIN without worrying about losing information. In many cases in the real world however, we would need to use a LEFT JOIN, RIGHT JOIN, or FULL OUTER JOIN depending on the situation. We will look at different JOIN types in another tutorial. 

Here are the results of our query:

<table>
  <thead>
    <tr>
      <th>id</th>
      <th>name</th>
      <th>n_id</th>
      <th>o_id</th>
      <th>e_iq</th>
      <th>o.id</th>
      <th>o.name</th>
      <th>n.id</th>
      <th>n.name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>60</td>
      <td>Leonardo da Vinci</td>
      <td>19</td>
      <td>6</td>
      <td>220</td>
      <td>6</td>
      <td>scientist</td>
      <td>19</td>
      <td>Italy</td>
    </tr>
    <tr>
      <td>50</td>
      <td>Johann Wolfgang von Goethe</td>
      <td>2</td>
      <td>9</td>
      <td>210</td>
      <td>9</td>
      <td>unknown</td>
      <td>2</td>
      <td>Germany</td>
    </tr>
    <tr>
      <td>81</td>
      <td>Emanuel Swedenborg</td>
      <td>18</td>
      <td>11</td>
      <td>205</td>
      <td>11</td>
      <td>philosopher</td>
      <td>18</td>
      <td>Sweden</td>
    </tr>
    <tr>
      <td>48</td>
      <td>Gottfried Wilhelm von Leibniz</td>
      <td>2</td>
      <td>11</td>
      <td>205</td>
      <td>11</td>
      <td>philosopher</td>
      <td>2</td>
      <td>Germany</td>
    </tr>
    <tr>
      <td>10</td>
      <td>John Stuart Mill</td>
      <td>1</td>
      <td>11</td>
      <td>200</td>
      <td>11</td>
      <td>philosopher</td>
      <td>1</td>
      <td>England</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Sir Francis Galton</td>
      <td>17</td>
      <td>6</td>
      <td>200</td>
      <td>6</td>
      <td>scientist</td>
      <td>17</td>
      <td>British</td>
    </tr>
    <tr>
      <td>65</td>
      <td>Kim Ung-Yong</td>
      <td>20</td>
      <td>9</td>
      <td>200</td>
      <td>9</td>
      <td>unknown</td>
      <td>20</td>
      <td>Korea</td>
    </tr>
    <tr>
      <td>21</td>
      <td>Thomas Wolsey</td>
      <td>1</td>
      <td>14</td>
      <td>200</td>
      <td>14</td>
      <td>politician</td>
      <td>1</td>
      <td>England</td>
    </tr>
    <tr>
      <td>90</td>
      <td>William James Sidis</td>
      <td>26</td>
      <td>9</td>
      <td>200</td>
      <td>9</td>
      <td>unknown</td>
      <td>26</td>
      <td>USA</td>
    </tr>
    <tr>
      <td>70</td>
      <td>Hugo Grotius</td>
      <td>9</td>
      <td>7</td>
      <td>200</td>
      <td>7</td>
      <td>jurist</td>
      <td>9</td>
      <td>Netherlands</td>
    </tr>
  </tbody>
</table>


This looks interesting, but it looks like there are a lot of extra identifier columns that will not be useful to us for answer questions. Let's cut them out and boil our query down to the essentials.

{% highlight sql %}
SELECT p.name,n.name,o.name,p.estimated_iq_score 
FROM person p
INNER JOIN occupation o ON o.id= p.occupation_id
INNER JOIN nationality n ON n.id = p.nationality_id
ORDER BY estimated_iq_score desc
LIMIT 10
{% endhighlight %}

<table>
  <thead>
    <tr>
      <th>name</th>
      <th>nationality</th>
      <th>occupation</th>
      <th>estimated_iq_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Leonardo da Vinci</td>
      <td>Italy</td>
      <td>scientist</td>
      <td>220</td>
    </tr>
    <tr>
      <td>Johann Wolfgang von Goethe</td>
      <td>Germany</td>
      <td>unknown</td>
      <td>210</td>
    </tr>
    <tr>
      <td>Emanuel Swedenborg</td>
      <td>Sweden</td>
      <td>philosopher</td>
      <td>205</td>
    </tr>
    <tr>
      <td>Gottfried Wilhelm von Leibniz</td>
      <td>Germany</td>
      <td>philosopher</td>
      <td>205</td>
    </tr>
    <tr>
      <td>John Stuart Mill</td>
      <td>England</td>
      <td>philosopher</td>
      <td>200</td>
    </tr>
    <tr>
      <td>Sir Francis Galton</td>
      <td>British</td>
      <td>scientist</td>
      <td>200</td>
    </tr>
    <tr>
      <td>Kim Ung-Yong</td>
      <td>Korea</td>
      <td>unknown</td>
      <td>200</td>
    </tr>
    <tr>
      <td>Thomas Wolsey</td>
      <td>England</td>
      <td>politician</td>
      <td>200</td>
    </tr>
    <tr>
      <td>William James Sidis</td>
      <td>USA</td>
      <td>unknown</td>
      <td>200</td>
    </tr>
    <tr>
      <td>Hugo Grotius</td>
      <td>Netherlands</td>
      <td>jurist</td>
      <td>200</td>
    </tr>
  </tbody>
</table>

### Creating a View

Now that we have our query in a form that we can use it to answer questions, let's go ahead and save it as a view since we'll be referring to it frequently. Note that in creating a view each column will need a distinct name, so we will need to alias the nationality and occupation columns as shown below. Additionally, we can remove the ORDER BY clause from our view since it adds unnecessary processing time to our view.

{% highlight sql %}
CREATE VIEW famous_people AS
SELECT p.name,n.name nationality,o.name occupation,p.estimated_iq_score 
FROM person p
INNER JOIN occupation o ON o.id= p.occupation_id
INNER JOIN nationality n ON n.id = p.nationality_id
{% endhighlight %}

### Answering Questions with SQL

Now that the view is created let's answer some questions using SQL. Using the view name in place of the table name. Use the following syntax to construct your queries:
 
{% highlight sql %}
SELECT [column name] FROM [table name] WHERE [column name] = [value]
{% endhighlight %}

##### How many people in the group are from the USA?

Consider the following query: 

{% highlight sql %}
SELECT * FROM famous_people  WHERE nationality = 'USA'
{% endhighlight %}

We could apply the query and count the result, but this would be impractical for larger data sets. Instead, let's apply the count function.

{% highlight sql %}
SELECT count(*) FROM famous_people  WHERE nationality = 'USA'
{% endhighlight %}

This immediately returns the answer we are looking for: 31.
 
##### What is the sum of the estimated IQ scores of people from Germany in the dataset?

We could write a query to list the all the people from Germany and add up their estimates manually, but using the sum function on the estimated_iq_score column will bring us the result we are looking for: 2769.

{% highlight sql %}
SELECT sum(estimated_iq_score) FROM famous_people  WHERE nationality = 'Germany'
{% endhighlight %}

I almost didn't want to include this question because we would almost never want to take the sum of IQ scores in real life unless we were trying to find an average (and there is already a function for average called avg), however, this is just for learning purposes and we don't have any other columns that are summable.
 
##### What is the average estimated IQ score for politicians in this dataset?

144.4

{% highlight sql %}
SELECT avg(estimated_iq_score) FROM famous_people  WHERE occupation = 'politician'
{% endhighlight %}

##### Which occupation in this dataset has the second highest estimated IQ score on average?

For this question, we will need to write a query to group the results together by occupation and apply the averge aggregator to the estimated_iq_score column. We will want to alias the column so that we can use it for sorting. 

{% highlight sql %}
SELECT occupation, avg(estimated_iq_score) e_avg_iq 
FROM famous_people
GROUP BY occupation
ORDER BY e_avg_iq DESC
{% endhighlight %}

<table>
  <thead>
    <tr>
      <th>Occupation</th>
      <th>Estimated Average IQ</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>jurist</td>
      <td>200</td>
    </tr>
    <tr>
      <td>bouncer</td>
      <td>195</td>
    </tr>
  </tbody>
</table> 

Interestingly, the occupation with the second highest average estimated IQ score in our dataset is bouncing. 

### Conclusion

Congratulations! This completes the tutorial. You are well on your way to becoming a master of SQL.
