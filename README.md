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

- [Working with dates](https://www.pgexercises.com/questions/basic/date.html)
```SQL
select memid,surname,firstname,joindate
	from cd.members
	where joindate >= '2012-09-01';
```

- [Removing duplicates, and ordering results](https://www.pgexercises.com/questions/basic/unique.html)
```SQL
select distinct surname 
	from cd.members 
		order by surname 
		limit 10;
```

- [Combining results from multiple queries](https://www.pgexercises.com/questions/basic/union.html)

```SQL
select surname 
	from cd.members
union
select name
	from cd.facilities;
```

- [Simple aggregation](https://www.pgexercises.com/questions/basic/agg.html)
```SQL
select joindate as "latest" 
	from cd.members 
	order by joindate desc 
	limit 1;
```

or

```SQL
select max(joindate) as latest
	from cd.members;
```

- [More aggregation](https://www.pgexercises.com/questions/basic/agg2.html)
```SQL
select firstname,surname,joindate
	from cd.members 
		order by joindate desc 
		limit 1;
```
or

```SQL
select firstname, surname, joindate
	from cd.membersmerge
		(select max(joindate) 
			from cd.members);
```


# Joins and Subqueries

- [Retrieve the start times of members' bookings](https://www.pgexercises.com/questions/joins/simplejoin.html)

```SQL
select bks.starttime 
	from cd.bookings bks
		inner join cd.members mems
		on mems.memid = bks.memid
			where mems.firstname='David' 
			and mems.surname='Farrell';
```

- [Work out the start times of bookings for tennis courts](https://www.pgexercises.com/questions/joins/simplejoin2.html)

```SQL
select bks.starttime as start,fac.name
	from
		cd.bookings bks
		inner join cd.facilities fac
		on bks.facid=fac.facid
	where
		fac.name like 'Tennis%'
		and bks.starttime >= '2012-09-21'
		and bks.starttime < '2012-09-22'
	order by bks.starttime;
```

-[Produce a list of all members who have recommended another member](https://www.pgexercises.com/questions/joins/self.html)

```SQL
select distinct mem.firstname, mem.surname
	from
		cd.members mem
		inner join cd.members mems
		on mems.recommendedby=mem.memid
	order by surname,firstname;
```

-[Produce a list of all members, along with their recommender](https://www.pgexercises.com/questions/joins/self2.html)

```SQL
select mem.firstname as memfname, mem.surname as memsname,
	   memr.firstname as recfname, memr.surname as recsname
	from
		cd.members mem
	left outer join cd.members memr
		on mem.recommendedby=memr.memid
	order by
		mem.surname, mem.firstname;
```

-[Produce a list of all members who have used a tennis court](https://www.pgexercises.com/questions/joins/threejoin.html)

```SQL
select distinct concat(mem.firstname, ' ', mem.surname) as member, fac.name as facility
	from
		cd.members mem
	inner join cd.bookings bks
		on mem.memid=bks.memid
	inner join cd.facilities fac
		on bks.facid=fac.facid
	where
		fac.name like 'Tennis%'
	order by
		member, facility;
```

OR,

```SQL
select distinct mems.firstname || ' ' || mems.surname as member, facs.name as facility
	from 
		cd.members mems
		inner join cd.bookings bks
			on mems.memid = bks.memid
		inner join cd.facilities facs
			on bks.facid = facs.facid
	where
		facs.name in ('Tennis Court 2','Tennis Court 1')
order by member, facility;
```

-[]()

```SQL
select concat(mem.firstname, ' ', mem.surname), fac.name as facility,
	case
		when
			mem.memid=0 then bks.slots*fac.guestcost
		else
			bks.slots*membercost
	end  as cost

	from
		cd.members mem
	inner join cd.bookings bks
		on mem.memid=bks.memid
	inner join cd.facilities fac
		on bks.facid=fac.facid
	
	where
		bks.starttime >= '2012-09-14' and
		bks.starttime < '2012-09-15' and
		(case
			when
				mem.memid=0 then bks.slots*fac.guestcost
			else
				bks.slots*membercost end) > 30
	order by cost desc;
```