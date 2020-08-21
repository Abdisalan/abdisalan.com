---
title: "SQL Practice Question: Newest Dog & Owner"
date: "2020-08-21T23:00:48.015Z"
template: "post"
draft: false
slug: "/posts/sql-practice-question-dogs"
description: "It seems simple, then it's challenging, then it's easy!"
category: "Tutorial"
tags:
  - "SQL"
socialImage: "https://dev-to-uploads.s3.amazonaws.com/i/h93o768yc4f0ytk47pp8.jpg"
---
![Photo of a cute puppy labrador with a red collar](media/20200816-puppy.jpg)

<center><i><span>Photo by <a href="https://unsplash.com/@berkaygumustekin?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText" target="_blank">Berkay Gumustekin</a> on <a href="https://unsplash.com/s/photos/puppy?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText" target="_blank">Unsplash</a></span></i></center>

In the 50+ programming interviews I've done, I've only been asked two SQL questions.

**I failed both of those questions.**

While I won't give the questions away, I will give you a problem to practice so that you can succeed where I failed!

This question combines many principles you'll need to quickly solve SQL interview problems.

Hopefully you'll be better prepared than I was 😅

## The Dog Database

Imagine you're running a dog shelter and you have a database of dogs and owners. Every dog has one owner, but owners can have many dogs.

Here's the owner and dogs table written in PostgreSQL.

### Owner Table

```sql
CREATE TABLE owners (
  id SERIAL PRIMARY KEY,
  name VARCHAR(256)
);
```

### Dogs Table

```sql
CREATE TABLE dogs (
  id SERIAL PRIMARY KEY,
  owner_id INTEGER REFERENCES owners(id),
  breed VARCHAR(256),
  adopted_on TIMESTAMP
);
```

Note that there is a one to many relationship between the owners and dogs table. There is one owner id tied to each dog and enforced by a foreign key contraint i.e. you can only use owner ids that actually exist in the owners table.

## The Question

Now, that the tables are setup, we can get to the real question.

_Write an SQL query that gives the latest dog each owner adopted along with the name of the owner._

Pretty simple right?

Here's some example data to help you out.

```sql
owners
 id |  name
----+--------
  1 | PersonA
  2 | PersonB

dogs
 id | owner_id |   breed   | adopted_on
----+----------+-----------+--------------
  1 |        1 | chow chow | 2019-02-03
  2 |        2 | dalmation | 2019-03-07
  3 |        2 | beagle    | 2020-09-21
  4 |        1 | pit bull  | 2020-08-01
```

The answer to the question should give you a result that looks like this.

```sql
✅ RESULT
name      |   breed  | adopted_on
----------+----------+------------
PersonB   | beagle   | 2020-09-21
PersonA   | pit bull | 2020-08-01
```

Try this out for yourself first, then I'll go over the answer below. Don't worry about setting this up on your computer! Here's an SQL Fiddle (like CodePen but for SQL) for you test your answer!

http://sqlfiddle.com/#!17/5059f/10

## Final Answer

Let's go through this step by step. There's probably a few other ways of doing this but this is mine.

### Part 1: Getting the newest dogs

First we find each newest adoption date by each owner.
To do this, I use the max function on the `adopted_on` column after grouping by owners. I make sure to also get the `owner_id`, that way we can use it to join on another table.

```sql
SELECT owner_id, max(adopted_on) FROM dogs GROUP BY owner_id
```
```sql
RESULT
owner_id |	   max
---------+--------------
       1 |	2020-09-21
       2 |	2020-08-01
```

### Part 2: Getting the breed of the newest dogs

Next, we join the last query with the dogs table (itself) to get the breed of the dog and match by the adoption date as well as the owner.

```sql
SELECT dogs.breed, dogs.adopted_on FROM dogs
JOIN (
  SELECT owner_id, max(adopted_on) FROM dogs GROUP BY owner_id
) AS newest_dogs
ON
	dogs.owner_id = newest_dogs.owner_id AND
	dogs.adopted_on = newest_dogs.max;
```
```sql
RESULT
  breed    |  adopted_on 
-----------+--------------
  beagle   |  2020-09-21
  pit bull |  2020-08-01
```

### Final: Get the names of the owners

Lastly, we join the result of the last query on the owners table to get their name.

```sql
SELECT owners.name, dogs.breed, dogs.adopted_on FROM dogs
JOIN (
  SELECT owner_id, max(adopted_on) FROM dogs GROUP BY owner_id
) AS newest_dogs
ON
  dogs.owner_id = newest_dogs.owner_id AND
  dogs.adopted_on = newest_dogs.max
JOIN
owners ON dogs.owner_id = owners.id;
```
```sql
✅ FINAL RESULT
name      | breed    | adopted_on
----------+----------+------------
PersonB   | beagle   | 2020-09-21
PersonA   | pit bull | 2020-08-01
```

## Conclusion
Although the question was simple, there were a few tricky queries we had to make! We needed to join tables two times and find the max aggregate on one of the tables.

I hope you learned something from this exercise! If you want to experiment with my final answer, I've also included a SQL Fiddle with the final answer below.

http://sqlfiddle.com/#!17/5059f/9


Thanks for reading! If you want more content, [follow me on twitter!](https://twitter.com/abdisalan_js)

✌️
