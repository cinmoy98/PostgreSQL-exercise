# PostgreSQL-exercise
You will find all the solutions of the problems of "PostgreSQL Exercise" website.

This is the main site where you can find problems.[PostgreSQL Exercises](https://www.pgexercises.com/)

[Database introduction](https://www.pgexercises.com/gettingstarted.html) Here you can understand the basic structure of the database on which you will be solving problems.

[PostgreSQL Documentation](https://www.postgresql.org/docs/current/index.html)

## Let's Get Started !!!

This Documentation includes:
- [Basic](#basic)
- [Joins and Subqueries](#joins-and-subqueries)
- [Modifying Data](#modifying-data)
- [Aggregates](#aggregates)
- [Date](#date)
- [String](#string)
- [Recursive](#recursive)


# Basic
- [Retrieve everything from a table](https://www.pgexercises.com/questions/basic/selectall.html)

```SQL
SELECT * FROM cd.facilities;
```

- [Retrieve specific columns from a table](https://www.pgexercises.com/questions/basic/selectspecific.html)

```SQL
SELECT name, membercost FROM cd.facilities;
```

- [Control which rows are retrieved](https://www.pgexercises.com/questions/basic/where.html)

```SQL
SELECT * FROM cd.facilities WHERE cd.membercost != 0;
```

- [Control which rows are retrieved - part 2](https://www.pgexercises.com/questions/basic/where2.html)

```SQL
SELECT facid,name,membercost,monthlymaintenance
	FROM cd.facilities 
	WHERE 
		membercost>0 AND 
		membercost<(monthlymaintenance/50);
```

- [Basic string searches](https://www.pgexercises.com/questions/basic/where3.html)
 ```SQL
 SELECT * 
 	FROM cd.facilities
 	WHERE
 		name LIKE '%Tennis%';
 ```

- [Matching against multiple possible values](https://www.pgexercises.com/questions/basic/where4.html)
```SQL
SELECT * 
	FROM cd.facilities
	WHERE facid IN (1,5);
```

- [Classify results into buckets](https://www.pgexercises.com/questions/basic/classify.html)
```SQL
SELECT name,
	case when (monthlymaintenance > 100) then
		'expensive'
	else
		'cheap'
	end as cost

	FROM cd.facilities; 
```