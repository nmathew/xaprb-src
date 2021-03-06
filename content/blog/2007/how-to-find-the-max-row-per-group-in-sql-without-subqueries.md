---
title: How to find the max row per group in SQL without subqueries
date: "2007-03-14"
url: /blog/2007/03/14/how-to-find-the-max-row-per-group-in-sql-without-subqueries/
categories:
  - Databases
---
A while ago I wrote about how to select the [first, minimum, maximum or least row per group in SQL](/blog/2006/12/07/how-to-select-the-firstleastmax-row-per-group-in-sql/). This article shows how to solve this problem without subqueries.

Like many SQL problems, the key to understanding the solution is to *rephrase the English question* to make it easy to translate into SQL.

### My first try

This is exactly the same problem as in my earlier article on [how to select the first/least/max row per group in SQL](/blog/2006/12/07/how-to-select-the-firstleastmax-row-per-group-in-sql/). The only difference is subqueries are disallowed.

I love finding ways to do things without subqueries, even if I don't have to. My first article on this blog was about [how to write subqueries in the FROM clause without using subqueries](/blog/2005/09/21/subselects-in-mysql/). These tricks can occasionally be useful in very early versions of MySQL, which I still work with (I recently completed a consulting job where the only database available is MySQL 3.23).

My first thought on this problem was to use a [mutex table](/blog/2005/09/22/mutex-tables-in-sql/), another blast from the past. It would work, but it's not the best way to do it.

### The solution

Let's say you have a table of people, and you want to find the youngest of each gender. Here's the table:

| age | name  | gender |
|----:|-------|--------|
|   5 | david | m      |
|   8 | john  | m      |
|   9 | jane  | f      |
|   4 | kelly | f      |
|  11 | mary  | f      |
|  13 | kay   | f      |

The problem is easy if I rephrase it as "find all people where **there is no younger person** of the same gender." That's easy to write as a join and translate into an [exclusion join](/blog/2005/09/23/how-to-write-a-sql-exclusion-join/):

*   Find all people---easy.
*   And for each person, find all younger people of the same gender---okay, join on gender and "age less than."
*   Discard each row where there is a younger person---change the join to an exclusion join.

Here are the first two bullet points in SQL:

```sql
select young.*, younger.age
from person as young
   left outer join person as younger on younger.gender = young.gender
      and younger.age < young.age

+------+-------+--------+------+
| age  | name  | gender | age  |
+------+-------+--------+------+
|    5 | david | m      | NULL | 
|    8 | john  | m      |    5 |
|    9 | jane  | f      |    4 |
|    4 | kelly | f      | NULL |
|   11 | mary  | f      |    9 |
|   11 | mary  | f      |    4 |
|   13 | kay   | f      |    9 |
|   13 | kay   | f      |    4 |
|   13 | kay   | f      |   11 |
+------+-------+--------+------+
```

Look at the rightmost column. There are `NULL`s only in rows where there's **no younger person** of the same gender. Now it's easy to "cross out" the other rows with the `WHERE` clause, and we're done:

```sql
select young.*
from person as young
   left outer join person as younger on younger.gender = young.gender
      and younger.age < young.age
where younger.age is null;

+------+-------+--------+------+
| age  | name  | gender | age  |
+------+-------+--------+------+
|    5 | david | m      | NULL |
|    4 | kelly | f      | NULL |
+------+-------+--------+------+
```

### How efficient is it?

As long as you have appropriate indexes on the table, this might not be as inefficient as you'd think. It's theoretically a cross join, yes, but in reality if there's a good index it's only a repeated cross join on subsets of the data. In other words, you need a (gender, age) index on this table. Gender isn't a very good example to use for this, because it will never be very selective, but if you only have a few rows per group and you have a leftmost index on the grouping column, it should work fine.

### Conclusion

As with so many other SQL challenges, if you re-phrase the question, it's easy to select the maximum or minimum row per group without subqueries. The key is to understand what you want, and to be able to word the problem in a way that translates from English to SQL.
