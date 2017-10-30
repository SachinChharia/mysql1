1. List all the columns of the Salespeople table..

mysql> desc salespeople;
+-------+--------------+------+-----+---------+-------+
| Field    | Type              | Null    | Key | Default   | Extra   |
+-------+--------------+------+-----+---------+-------+
| snum    | int(11)            | NO   | PRI  | NULL    |             |
| sname  | varchar(30)    | NO   |         | NULL    |             |
| city      | varchar(30)    | NO   |         | NULL    |             |
| comm  | decimal(4,2)   | NO   |         | NULL    |            |
+-------+--------------+------+-----+---------+-------+

2. List all customers with a rating of 100.

mysql> select * from customer where rating=100;
+------+---------+--------+--------+------+
| cnum  | cname    | city        | rating    | snum  |
+------+---------+--------+--------+------+
| 2001  | Hoffman | London |    100    | 1001 |
| 2006  | Clemens | London |    100    | 1001 |
| 2007  | Pereira   | Rome    |    100    | 1004 |
+------+---------+--------+--------+------+

3. Find all records in the Customer table with NULL values in the city column.

mysql> select * from customer where city=null;
Empty set

mysql> select * from customer where city is null;
Empty set

4. Find the largest order taken by each salesperson on each date.

mysql> select c.sname,max(a.amt),a.odate from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.sname,a.odate;
+---------+------------+------------+
| sname     | max(a.amt)  | odate          |
+---------+------------+------------+
| AxelRod |    1713.23   | 1996-10-04|
| Motika   |    1900.10   | 1996-10-10|
| Peel        |     767.19   | 1996-10-03|
| Peel       |    4723.00   | 1996-10-05|
| Peel       |    9891.88   | 1996-10-06|
| Rifkin     |    1098.16   | 1996-10-03|
| Serres    |    1309.95   | 1996-10-06|
| Serres    |    5160.45   | 1996-10-10|
+---------+------------+------------+
8 rows in set

5. Arrange the Orders table by descending customer number.

mysql> select * from orders order by cnum desc;
+------+---------+------------+------+
| onum  | amt        | odate           | cnum |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03| 2008 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
| 3002  | 1900.10 | 1996-10-10| 2007 |
| 3008  | 4723.00 | 1996-10-05| 2006 |
| 3011  | 9891.88 | 1996-10-06| 2006 |
| 3010  | 1309.95 | 1996-10-06| 2004 |
| 3005  | 5160.45 | 1996-10-10| 2003 |
| 3007  |   75.75   | 1996-10-04| 2002 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
| 3003  |  767.19 | 1996-10-03| 2001 |
+------+---------+------------+------+
10 rows in set

6. Find which salespeople currently have orders in the Orders table.

mysql> select c.sname,a.amt,a.odate from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum;
+---------+---------+------------+
| sname     | amt         | odate          |
+---------+---------+------------+
| Peel        |  767.19  | 1996-10-03|
| Peel        | 4723.00 | 1996-10-05|
| Peel        | 9891.88 | 1996-10-06|
| Serres    | 5160.45 | 1996-10-10|
| Serres    | 1309.95 | 1996-10-06|
| AxelRod |   75.75  | 1996-10-04|
| AxelRod | 1713.23| 1996-10-04|
| Motika   | 1900.10| 1996-10-10|
| Rifkin     |   18.69   | 1996-10-03|
| Rifkin     | 1098.16 | 1996-10-03|
+---------+---------+------------+
10 rows in set

7. List names of all customers matched with the salespeople serving them.

mysql> select b.cname,c.sname,b.snum,c.snum from customer b,salespeople c whereb.snum=c.snum;
+----------+---------+------+------+
| cname      | sname     | snum  | snum  |
+----------+---------+------+------+
| Hoffman   | Peel        | 1001 | 1001  |
| Clemens   | Peel        | 1001 | 1001  |
| Liu           | Serres     | 1002 | 1002  |
| Grass       | Serres     | 1002 | 1002  |
| Giovanni   | AxelRod | 1003 | 1003  |
| Pereira     | Motika    | 1004 | 1004  |
| Cisneros   | Rifkin      | 1007 | 1007  |
+----------+---------+------+------+
7 rows in set

8. Find the names and numbers of all salespeople who had more than one customer.

mysql> select c.sname,count(b.snum) from customer b,salespeople c where b.snum=c.snum group by c.sname having count(b.snum) > 1;
+--------+---------------+
| sname    | count(b.snum) |
+--------+---------------+
| Peel       |             2         |
| Serres   |             2          |
+--------+---------------+
2 rows in set

9. Count the orders of each of the salespeople and output the results in descending order.

mysql> select a.onum,c.sname,count(a.odate) as "total" from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.sname,a.odate order by total desc;
+------+---------+-------+
| onum  | sname    | total     |
+------+---------+-------+
| 3001   | Rifkin     |     2     |
| 3007   | AxelRod |     2    |
| 3008   | Peel       |     1     |
| 3002   | Motika  |     1     |
| 3010   | Serres   |     1     |
| 3011   | Peel      |     1      |
| 3003   | Peel     |     1      |
| 3005   | Serres  |     1      | 
+------+---------+-------+
8 rows in set

10. List the Customer table if and only if one or more of the customers in the Customer table are located in San Jose.

mysql> select cnum,cname,city,rating,snum from customer where city='San Jose' group by cnum;
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating    | snum  |
+------+----------+----------+--------+------+
| 2003  | Liu           | San Jose  |    200    | 1002 |
| 2008  | Cisneros   | San Jose |    300     | 1007 |
+------+----------+----------+--------+------+
2 rows in set

11. Match salespeople to customers according to what city they lived in.

mysql> select c.sname,b.cname,b.city as 'Customer city',c.city as 'Salespeople city' from customer b,salespeople c where b.snum=c.snum and b.city=c.city;
+--------+---------+---------------+------------------+
| sname    | cname    | Customer city  | Salespeople city   |
+--------+---------+---------------+------------------+
| Peel       | Hoffman | London           | London                |
| Serres    | Liu         | San Jose         | San Jose               |
| Peel       | Clemens | London           | London                |
+--------+---------+---------------+------------------+
3 rows in set

12. Find the largest order taken by each salesperson

mysql> select c.sname,max(a.amt) from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.sname;
+---------+------------+
| sname     | max(a.amt)  |
+---------+------------+
| AxelRod |    1713.23   |
| Motika   |    1900.10   |
| Peel       |    9891.88   |
| Rifkin     |    1098.16   |
| Serres   |    5160.45    |
+---------+------------+
5 rows in set

13. Find customers in San Jose who have a rating above 200.

mysql> select * from customer where city='San Jose' and rating > 200;
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating    | snum  |
+------+----------+----------+--------+------+
| 2008  | Cisneros  | San Jose  |    300    | 1007 |
+------+----------+----------+--------+------+
1 row in set

14. List the names and commissions of all salespeople in London.

mysql> select sname,comm from salespeople where city='London';
+--------+------+
| sname   | comm |
+--------+------+
| Peel      | 0.12   |
| Motika | 0.11    |
| Fran     | 0.26   |
+--------+------+
3 rows in set

15. List all the orders of salesperson Motika from the Orders table.

mysql> select a.onum,a.amt,a.odate,a.cnum from orders a,customer b,salespeople c  where a.cnum=b.cnum and b.snum=c.snum and c.sname='Motika';
+------+---------+------------+------+
| onum  | amt         | odate          | cnum |
+------+---------+------------+------+
| 3002  | 1900.10 | 1996-10-10| 2007 |
+------+---------+------------+------+
1 row in set

16. Find all customers with orders on October 3.

mysql> select b.cname,a.onum,a.amt,a.odate from orders a,customer b where a.cnum=b.cnum and a.odate='1996-10-03';
+----------+------+---------+------------+
| cname       | onum | amt        | odate          |
+----------+------+---------+------------+
| Cisneros   | 3001 |   18.69   | 1996-10-03|
| Hoffman   | 3003 |  767.19  | 1996-10-03|
| Cisneros   | 3006 | 1098.16 | 1996-10-03|
+----------+------+---------+------------+
3 rows in set

17. Give the sums of the amounts from the Orders table, grouped by date, eliminating all those dates where the SUM was not at least 2000.00 above the MAX amount.

mysql> select a.odate,sum(a.amt) as 'Sum' from orders a group by a.odate having sum(amt) > (select 2000.00 + max(b.amt) from orders b where a.odate=b.odate);
Empty set

18. Select all orders that had amounts that were greater than at least one of the orders from October 6.

mysql> select * from orders a where amt > (select min(b.amt) from orders b where  b.odate='1996-10-06') and a.odate!='1996-10-06';
+------+---------+------------+------+
| onum  | amt        | odate           | cnum |
+------+---------+------------+------+
| 3002  | 1900.10 | 1996-10-10| 2007 |
| 3005  | 5160.45 | 1996-10-10| 2003 |
| 3008  | 4723.00 | 1996-10-05| 2006 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
+------+---------+------------+------+
4 rows in set

19. Write a query that uses the EXISTS operator to extract all salespeople who have customers with a rating of 300.

mysql> select c.sname,c.snum from salespeople c where exists (select * from customer b where b.snum=c.snum and b.rating=300);
+--------+------+
| sname   | snum  |
+--------+------+
| Serres   | 1002  |
| Rifkin    | 1007  |
+--------+------+
2 rows in set

20. Find all pairs of customers having the same rating.

mysql> Select a.cname, b.cname,a.rating,b.rating from customer a, customer b where a.rating = b.rating and a.cnum != b.cnum;
+----------+----------+--------+--------+
| cname      | cname      | rating     | rating    |
+----------+----------+--------+--------+
| Clemens   | Hoffman   |    100   |    100    |
| Pereira     | Hoffman   |    100   |    100    |
| Liu           | Giovanni   |    200   |    200    |
| Giovanni  | Liu            |    200   |    200    |
| Cisneros  | Grass        |    300   |    300    |
| Hoffman  | Clemens    |    100   |    100    |
| Pereira    | Clemens    |    100   |    100    |
| Hoffman  | Pereira      |    100   |    100    |
| Clemens  | Pereira      |    100   |    100    |
| Grass      | Cisneros    |    300   |    300    |
+----------+----------+--------+--------+
10 rows in set

21. Find all customers whose CNUM is 1000 above the SNUM of Serres.

mysql> Select cnum, cname from customer b where b.cnum > ( select c.snum+1000 from salespeople c where c.sname = 'Serres');
+------+----------+
| cnum  | cname      |
+------+----------+
| 2003  | Liu           |
| 2004  | Grass       |
| 2006  | Clemens   |
| 2007  | Pereira     |
| 2008  | Cisneros  |
+------+----------+
5 rows in set

22. Give the salespeople’s commissions as percentages instead of decimal numbers.

mysql> select ((comm)*100) as commission from salespeople;
+------------+
| commission  |
+------------+
|      12.00     |
|      13.00     |
|      10.00     |
|      11.00     |
|      26.00    |
|      15.00    |
+------------+
6 rows in set

23. Find the largest order taken by each salesperson on each date, eliminating those MAX orders which are less than $3000.00 in value.

mysql> select c.sname,max(a.amt) as max,a.odate from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.sname,a.odate having max  > 3000;
+--------+---------+------------+
| sname   | max        | odate          |
+--------+---------+------------+
| Peel      | 4723.00 | 1996-10-05|
| Peel      | 9891.88 | 1996-10-06|
| Serres   | 5160.45 | 1996-10-10|
+--------+---------+------------+
3 rows in set

24. List the largest orders for October 3, for each salesperson.

mysql> select c.sname,max(a.amt) as max,a.odate from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.sname,a.odate having a.odate='1996-10-03';
+--------+---------+------------+
| sname    | max        | odate         |
+--------+---------+------------+
| Peel       |  767.19  | 1996-10-03|
| Rifkin     | 1098.16 | 1996-10-03|
+--------+---------+------------+
2 rows in set

25. Find all customers located in cities where Serres (SNUM 1002) has customers.

mysql> select b.* from customer b,salespeople c where b.snum=c.snum and c.sname='Serres';
+------+-------+----------+--------+------+
| cnum  | cname | city           | rating    | snum  |
+------+-------+----------+--------+------+
| 2003   | Liu      | San Jose  |    200   | 1002  |
| 2004   | Grass  | Berlin       |    300   | 1002  |
+------+-------+----------+--------+------+
2 rows in set

26. Select all customers with a rating above 200.00.

mysql> select * from customer where rating > 200;
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating    | snum  |
+------+----------+----------+--------+------+
| 2004  | Grass       | Berlin       |    300   | 1002  |
| 2008  | Cisneros  | San Jose   |    300   | 1007  |
+------+----------+----------+--------+------+
2 rows in set

27. Count the number of salespeople currently listing orders in the Orders table.

mysql> select count(distinct(b.snum)) as total from orders a,customer b where a.cnum=b.cnum;
+-------+
| total     |
+-------+
|     5     |
+-------+
1 row in set

28. Write a query that produces all customers serviced by salespeople with a commission above 12%. Output the customer’s name and the salesperson’s rate of commission.

mysql> select b.cname,c.comm from customer b,salespeople c where b.snum=c.snum and ((c.comm)*100) > 12.00;
+----------+------+
| cname      | comm |
+----------+------+
| Liu           | 0.13   |
| Grass       | 0.13   |
| Cisneros   | 0.15  |
+----------+------+
3 rows in set

29. Find salespeople who have multiple customers.

mysql> select c.sname,count(b.snum) as totalcustomers from customer b,salespeople c where  b.snum=c.snum group by b.snum having totalcustomers > 1;
+--------+----------------+
| sname    | totalcustomers  |
+--------+----------------+
| Peel      |              2          |
| Serres   |              2          |
+--------+----------------+
2 rows in set

30. Find salespeople with customers located in their city.

mysql> select c.sname,c.city,b.cname,b.city customers from customer b,salespeople c where  b.snum=c.snum and b.city=c.city;
+--------+----------+---------+-----------+
| sname    | city          | cname    | customers  |
+--------+----------+---------+-----------+
| Peel       | London    | Hoffman | London    |
| Serres    | San Jose  | Liu         | San Jose   |
| Peel       | London    | Clemens | London    |
+--------+----------+---------+-----------+
3 rows in set

31. Find all salespeople whose name starts with ‘P’ and the fourth character is ‘l’.

mysql> select * from salespeople where substring(sname,1,1)="P" and substring(sname,4,1)="l";
+------+-------+--------+------+
| snum  | sname  | city       | comm |
+------+-------+--------+------+
| 1001  | Peel     | London | 0.12  |
+------+-------+--------+------+
1 row in set 

32. Write a query that uses a subquery to obtain all orders for the customer named Cisneros. Assume you do not know his customer number.

mysql> select * from orders a where a.cnum=(select cnum from customer where cname='Cisneros');
+------+---------+------------+------+
| onum  | amt         | odate         | cnum  |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03| 2008 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
+------+---------+------------+------+
2 rows in set

OR

mysql> select * from orders a where a.cnum in (select cnum from customer where cname='Cisneros');
+------+---------+------------+------+
| onum  | amt         | odate          | cnum |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03| 2008 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
+------+---------+------------+------+
2 rows in set (0.00 sec)

33. Find the largest orders for Serres and Rifkin

mysql> select c.sname,max(a.amt) as max from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum and (c.sname='Serres' or c.sname='Rifkin') group by c.sname;
+--------+---------+
| sname    | max        |
+--------+---------+
| Rifkin     | 1098.16 |
| Serres    | 5160.45 |
+--------+---------+
2 rows in set

34. Extract the Salespeople table in the following order : SNUM, SNAME, COMMISSION, CITY.

mysql> select snum as SNUM,sname as SNAME,comm as COMMISSION,city as CITY from salespeople;
+-------+---------+-----------------+-----------+
| SNUM| SNAME| COMMISSION| CITY         |
+-------+---------+-----------------+-----------+
| 1001    | Peel       |       0.12             | London      |
| 1002    | Serres    |       0.13             | San Jose    |
| 1003    | AxelRod |       0.10            | New York  |
| 1004    | Motika   |       0.11             | London     |
| 1005    | Fran       |       0.26             | London     |
| 1007    | Rifkin     |       0.15             | Barcelona  |
+------+---------+------------+-----------+
6 rows in set

35. Select all customers whose names fall in between ‘A’ and ‘G’ alphabetical range

mysql> select * from customer where substring(cname,1,1) in ("A","B","C","D","E","F","G");
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating    | snum  |
+------+----------+----------+--------+------+
| 2002  | Giovanni  | Rome       |    200    | 1003 |
| 2004  | Grass      | Berlin        |    300    | 1002 |
| 2006  | Clemens  | London     |    100    | 1001 |
| 2008  | Cisneros | San Jose    |    300    | 1007 |
+------+----------+----------+--------+------+
4 rows in set

OR

mysql> select * from customer where substring(cname,1,1) between  "A" and "G";
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating     | snum |
+------+----------+----------+--------+------+
| 2002  | Giovanni  | Rome       |    200    | 1003 |
| 2004  | Grass       | Berlin       |    300    | 1002 |
| 2006  | Clemens  | London    |    100     | 1001 |
| 2008  | Cisneros  | San Jose  |    300     | 1007 |
+------+----------+----------+--------+------+
4 rows in set

36. Select all the possible combinations of customers that you can assign. (Not to be attempted as question is ambiguious)

37. Select all orders that are greater than the average for October 4.

mysql> select * from orders a where a.amt > (select avg(b.amt) from orders b where b.odate='1996-10-04');
+------+---------+------------+------+
| onum  | amt         | odate          | cnum |
+------+---------+------------+------+
| 3002  | 1900.10 | 1996-10-10| 2007 |
| 3005  | 5160.45 | 1996-10-10| 2003 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
| 3008  | 4723.00 | 1996-10-05| 2006 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
| 3010  | 1309.95 | 1996-10-06| 2004 |
| 3011  | 9891.88 | 1996-10-06| 2006 |
+------+---------+------------+------+
7 rows in set

38. Write a select command using a corelated subquery that selects the names and numbers of all customers with ratings equal to the maximum for their city.

mysql> select a.cname,a.cnum from customer a where a.rating=(select max(rating) from customer b where a.city=b.city group by b.city);
+----------+------+
| cname      | cnum  |
+----------+------+
| Hoffman   | 2001  |
| Giovanni   | 2002  |
| Grass       | 2004  |
| Clemens   | 2006  |
| Cisneros   | 2008  |
+----------+------+
5 rows in set

39. Write a query that totals the orders for each day and places the results in descending order.

mysql> select sum(amt) as total,odate from orders group by odate order by total desc;
+----------+------------+
| total          | odate          |
+----------+------------+
| 11201.83 | 1996-10-06|
|  7060.55 | 1996-10-10|
|  4723.00 | 1996-10-05|
|  1884.04 | 1996-10-03|
|  1788.98 | 1996-10-04|
+----------+------------+
5 rows in set

40. Write a select command that produces the rating followed by the name of each customer in San Jose.

mysql> select rating,cname from customer where city='San Jose';
+--------+----------+
| rating     | cname      |
+--------+----------+
|    200    | Liu           |
|    300    | Cisneros  |
+--------+----------+
2 rows in set

OR

mysql> select concat(rating,' ',cname) as customer_rating from customer where city='San Jose';
+-----------------+
| customer_rating   |
+-----------------+
| 200 Liu               |
| 300 Cisneros      |
+-----------------+
2 rows in set

41. Find all orders with amounts smaller than any amount for a customer in San Jose.

mysql> select * from orders a where a.amt < ANY (select amt from orders b,customer c where b.cnum=c.cnum and c.city='San Jose');
+------+---------+------------+------+
| onum  | amt        | odate          | cnum  |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03| 2008 |
| 3002  | 1900.10 | 1996-10-10| 2007 |
| 3003  |  767.19 | 1996-10-03| 2001 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
| 3007  |   75.75   | 1996-10-04| 2002 |
| 3008  | 4723.00 | 1996-10-05| 2006 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
| 3010  | 1309.95 | 1996-10-06| 2004 |
+------+---------+------------+------+
8 rows in set

OR

mysql> select * from orders a where a.amt < ALL (select amt from orders b,customer c where b.cnum=c.cnum and c.city='San Jose');
Empty set (0.00 sec)

42. Find all orders with above average amounts for their customers.

mysql> select a.onum,a.amt,b.cname from orders a,customer b where a.cnum=b.cnum and a.amt > (select avg(amt) from orders c where c.cnum=b.cnum) group by b.cnum;

+------+---------+----------+
| onum  | amt        | cname      |
+------+---------+----------+
| 3009  | 1713.23 | Giovanni  |
| 3011  | 9891.88 | Clemens  |
| 3006  | 1098.16 | Cisneros  |
+------+---------+----------+
3 rows in set

43. Write a query that selects the highest rating in each city.

mysql> select city,max(rating) from customer group by city;
+----------+-------------+
| city           | max(rating)   |
+----------+-------------+
| Berlin       |         300      |
| London    |         100      |
| Rome       |         200      |
| San Jose  |         300      |
+----------+-------------+
4 rows in set

44. Write a query that calculates the amount of the salesperson’s commission on each order by a customer with a rating above 100.00.

mysql> select c.sname,a.amt,c.comm,b.rating,(a.amt*c.comm) as sales_person_commission from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum and b.rating > 100 group by a.onum;
+---------+---------+------+--------+--------------------------+
| sname     | amt         | comm| rating    | sales_person_commission |
+---------+---------+------+--------+--------------------------+
| Rifkin      |   18.69   | 0.15   |    300   |                  2.8035            |
| Serres     | 5160.45 | 0.13   |    200   |                670.8585          |
| Rifkin      | 1098.16 | 0.15   |    300   |                164.7240          |
| AxelRod |   75.75   | 0.10   |    200    |                  7.5750           |
| AxelRod | 1713.23 | 0.10   |    200    |                171.3230         |
| Serres  | 1309.95    | 0.13  |    300    |                170.2935         |
+---------+---------+------+--------+--------------------------+
6 rows in set

45. Count the customers with ratings above San Jose’s average

mysql> select * from customer where rating > (select avg(rating) from customer where city='San Jose' group by city);
+------+----------+----------+--------+------+
| cnum   | cname     | city          | rating     | snum |
+------+----------+----------+--------+------+
| 2004  | Grass       | Berlin       |    300   | 1002  |
| 2008  | Cisneros  | San Jose   |    300   | 1007  |
+------+----------+----------+--------+------+
2 rows in set

46. Write a query that produces all pairs of salespeople with themselves as well as duplicate rows with the order reversed.

mysql> select a.sname, b.sname from salespeople a, salespeople b;
+---------+---------+
| sname     | sname     |
+---------+---------+
| Peel        | Peel        |
| Serres     | Peel        |
| AxelRod | Peel         |
| Motika   | Peel         |
| Fran       | Peel         |
| Rifkin     | Peel         |
| Peel       | Serres      |
| Serres    | Serres      |
| AxelRod | Serres      |
| Motika   | Serres      |
| Fran       | Serres      |
| Rifkin     | Serres      |
| Peel       | AxelRod   |
| Serres    | AxelRod   |
| AxelRod | AxelRod  |
| Motika   | AxelRod  |
| Fran       | AxelRod  |
| Rifkin     | AxelRod  |
| Peel       | Motika    |
| Serres    | Motika   |
| AxelRod | Motika  |
| Motika   | Motika  |
| Fran       | Motika  |
| Rifkin     | Motika  |
| Peel       | Fran      |
| Serres    | Fran     |
| AxelRod | Fran    |
| Motika   | Fran    |
| Fran       | Fran    |
| Rifkin     | Fran    |
| Peel       | Rifkin  |
| Serres    | Rifkin  |
| AxelRod | Rifkin  |
| Motika   | Rifkin  |
| Fran       | Rifkin  |
| Rifkin     | Rifkin  |
+---------+---------+
36 rows in set

47. Find all salespeople that are located in either Barcelona or London.

mysql> select * from salespeople where city='Barcelona' or city='London';
+------+--------+-----------+------+
| snum  | sname   | city            | comm |
+------+--------+-----------+------+
| 1001  | Peel      | London     | 0.12   |
| 1004  | Motika  | London     | 0.11   |
| 1005  | Fran      | London     | 0.26   |
| 1007  | Rifkin    | Barcelona  | 0.15   |
+------+--------+-----------+------+
4 rows in set

OR

mysql> select * from salespeople where city in ('Barcelona','London');
+------+--------+-----------+------+
| snum  | sname   | city            | comm |
+------+--------+-----------+------+
| 1001  | Peel      | London     | 0.12   |
| 1004  | Motika  | London     | 0.11   |
| 1005  | Fran     | London      | 0.26   |
| 1007  | Rifkin   | Barcelona   | 0.15  |
+------+--------+-----------+------+
4 rows in set

48. Find all salespeople with only one customer.

mysql> select c.sname,count(b.snum) from customer b,salespeople c where b.snum=c.snum group by b.snum having count(b.snum)=1;
+---------+---------------+
| sname     | count(b.snum)  |
+---------+---------------+
| AxelRod |             1         |
| Motika   |             1         |
| Rifkin     |             1         |
+---------+---------------+
3 rows in set

49. Write a query that joins the Customer table to itself to find all pairs of customers served by a single salesperson.

mysql> select a.cname,b.cname from customer a,customer b where a.cnum=b.cnum group by a.cnum having count(a.snum)=1;
+----------+----------+
| cname       | cname     |
+----------+----------+
| Hoffman   | Hoffman  |
| Giovanni   | Giovanni |
| Liu           | Liu          |
| Grass       | Grass      |
| Clemens   | Clemens  |
| Pereira     | Pereira    |
| Cisneros   | Cisneros |
+----------+----------+
7 rows in set

50. Write a query that will give you all orders for more than $1000.00

mysql> select * from orders where amt > 1000.00;
+------+---------+------------+------+
| onum  | amt        | odate          | cnum  |
+------+---------+------------+------+
| 3002  | 1900.10 | 1996-10-10| 2007 |
| 3005  | 5160.45 | 1996-10-10| 2003 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
| 3008  | 4723.00 | 1996-10-05| 2006 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
| 3010  | 1309.95 | 1996-10-06| 2004 |
| 3011  | 9891.88 | 1996-10-06| 2006 |
+------+---------+------------+------+
7 rows in set

51. Write a query that lists each order number followed by the name of the customer who made that order.

mysql> select a.onum,b.cname from orders a,customer b where a.cnum=b.cnum;
+------+----------+
| onum  | cname     |
+------+----------+
| 3003  | Hoffman  |
| 3007  | Giovanni |
| 3009  | Giovanni |
| 3005  | Liu         |
| 3010  | Grass     |
| 3008  | Clemens |
| 3011  | Clemens |
| 3002  | Pereira   |
| 3001  | Cisneros|
| 3006  | Cisneros|
+------+----------+
10 rows in set

52. Write 2 queries that select all salespeople (by name and number) who have customers in their cities who they do not service, one using a join and one a corelated subquery. Which solution
is more elegant?

Using Join
mysql> Select * from customer b, salespeople c where b.city = c.city and b.snum != c.snum;
+------+----------+----------+--------+------+------+--------+----------+------+

| cnum | cname    | city     | rating | snum | snum | sname  | city     | comm |

+------+----------+----------+--------+------+------+--------+----------+------+

| 2001  | Hoffman  | London    |    100    | 1001  | 1004 | Motika  | London   | 0.11   |

| 2001  | Hoffman  | London    |    100    | 1001  | 1005 | Fran      | London   | 0.26   |

| 2006  | Clemens  | London    |    100    | 1001  | 1004 | Motika  | London   | 0.11   |

| 2006  | Clemens  | London    |    100    | 1001  | 1005 | Fran      | London   | 0.26   |

| 2008  | Cisneros  | San Jose  |    300    | 1007  | 1002 | Serres   | San Jose | 0.13   |

+------+----------+----------+--------+------+------+--------+----------+------+

5 rows in set

Using corelated subquery

mysql> select * from salespeople c where city in ( select city from customer  b where b.snum != c.snum);
+------+--------+----------+------+
| snum   | sname  | city           | comm |
+------+--------+----------+------+
| 1004  | Motika  | London    | 0.11  |
| 1005  | Fran      | London    | 0.26  |
| 1002  | Serres   | San Jose   | 0.13  |
+------+--------+----------+------+
3 rows in set

53. Write a query that selects all customers whose ratings are equal to or greater than ANY (in the SQL sense) of Serres’?

mysql> Select cname, sname from customer, salespeople where rating >= any ( select rating from customer where snum = (select snum from salespeople where sname =
 'Serres')) and customer.snum=salespeople.snum and sname != 'Serres';
+----------+---------+
| cname       | sname    |
+----------+---------+
| Giovanni   | AxelRod |
| Cisneros   | Rifkin     |
+----------+---------+
2 rows in set

54. Write 2 queries that will produce all orders taken on October 3 or October 4.

Query 1
mysql> select * from orders where odate='1996-10-03' union  select * from orders where odate='1996-10-04';
+------+---------+------------+------+
| onum  | amt        | odate          | cnum  |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03| 2008 |
| 3003  |  767.19  | 1996-10-03| 2001 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
| 3007  |   75.75   | 1996-10-04| 2002 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
+------+---------+------------+------+
5 rows in set

Query 2

mysql> Select * from orders where odate between '1996-10-03' and '1996-10-04';
+------+---------+------------+------+
| onum  | amt        | odate          | cnum  |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03| 2008 |
| 3003  |  767.19  | 1996-10-03| 2001 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
| 3007  |   75.75   | 1996-10-04| 2002 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
+------+---------+------------+------+
5 rows in set

Query 3

mysql> Select * from orders where odate in ('1996-10-03','1996-10-04');
+------+---------+------------+------+
| onum  | amt         | odate          | cnum |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03| 2008 |
| 3003  |  767.19  | 1996-10-03| 2001 |
| 3006  | 1098.16 | 1996-10-03| 2008 |
| 3007  |   75.75   | 1996-10-04| 2002 |
| 3009  | 1713.23 | 1996-10-04| 2002 |
+------+---------+------------+------+
5 rows in set

55. Write a query that produces all pairs of orders by a given customer. Name that customer and eliminate duplicates.

mysql> Select c.cname, a.onum, b.onum from orders a, orders b, customer c where a.cnum = b.cnum and a.onum > b.onum and c.cnum = a.cnum;
+----------+------+------+
| cname      | onum  | onum |
+----------+------+------+
| Giovanni  | 3009  | 3007  |
| Clemens  | 3011  | 3008  |
| Cisneros  | 3006  | 3001  |
+----------+------+------+
3 rows in set

56. Find only those customers whose ratings are higher than every customer in Rome.

mysql> select * from customer a where a.rating > any (select b.rating from customer b where b.city='Rome') and a.city!='Rome';
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating    | snum  |
+------+----------+----------+--------+------+
| 2003  | Liu           | San Jose  |    200    | 1002 |
| 2004  | Grass       | Berlin       |    300    | 1002 |
| 2008  | Cisneros  | San Jose   |    300    | 1007 |
+------+----------+----------+--------+------+
3 rows in set

57. Write a query on the Customers table whose output will exclude all customers with a rating <= 100.00, unless they are located in Rome.

mysql> select * from customer where rating <=100 or city='Rome';
+------+----------+--------+--------+------+
| cnum  | cname      | city       | rating     | snum |
+------+----------+--------+--------+------+
| 2001  | Hoffman   | London |    100   | 1001 |
| 2002  | Giovanni   | Rome    |    200   | 1003 |
| 2006  | Clemens   | London |    100   | 1001 |
| 2007  | Pereira     | Rome    |    100   | 1004 |
+------+----------+--------+--------+------+
4 rows in set

58. Find all rows from the Customers table for which the salesperson number is 1001.

mysql> select * from customer where snum=1001;
+------+---------+--------+--------+------+
| cnum  | cname    | city        | rating    | snum  |
+------+---------+--------+--------+------+
| 2001  | Hoffman | London |    100   | 1001  |
| 2006  | Clemens | London |    100   | 1001  |
+------+---------+--------+--------+------+
2 rows in set

59. Find the total amount in Orders for each salesperson for whom this total is greater than the amount of the largest order in the table.

mysql> Select a.cnum,sum(amt) from orders a group by cnum having sum(amt) > ( select max(amt) from orders);
+------+----------+
| cnum  | sum(amt)  |
+------+----------+
| 2006  | 14614.88 |
+------+----------+
1 row in set

60. Write a query that selects all orders save those with zeroes or NULLs in the amount field.

mysql> Select * from orders where amt != 0 or amt is not null;
+------+---------+------------+------+
| onum  | amt        | odate          | cnum  |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03 | 2008 |
| 3002  | 1900.10 | 1996-10-10 | 2007 |
| 3003  |  767.19  | 1996-10-03 | 2001 |
| 3005  | 5160.45 | 1996-10-10 | 2003 |
| 3006  | 1098.16 | 1996-10-03 | 2008 |
| 3007  |   75.75   | 1996-10-04 | 2002 |
| 3008  | 4723.00 | 1996-10-05 | 2006 |
| 3009  | 1713.23 | 1996-10-04 | 2002 |
| 3010  | 1309.95 | 1996-10-06 | 2004 |
| 3011  | 9891.88 | 1996-10-06 | 2006 |
+------+---------+------------+------+
10 rows in set

61. Produce all combinations of salespeople and customer names such that the former precedes the latter alphabetically, and the latter has a rating of less than 200.

mysql> select c.sname,b.cname from customer b,salespeople c where b.snum=c.snum  and c.sname < b.cname and b.rating < 200;
+--------+---------+
| sname    | cname    |
+--------+---------+
| Motika  | Pereira   |
+--------+---------+
1 row in set

OR

mysql> select c.sname,b.cname from customer b,salespeople c where b.snum=c.snum  and b.cname < c.sname and b.rating < 200;
+-------+---------+
| sname  | cname    |
+-------+---------+
| Peel     | Hoffman |
| Peel     | Clemens |
+-------+---------+
2 rows in set

62. List all Salespeople’s names and the Commission they have earned.

mysql> select sname,comm from salespeople;
+---------+------+
| sname     | comm |
+---------+------+
| Peel        | 0.12   |
| Serres     | 0.13   |
| AxelRod | 0.10   |
| Motika   | 0.11   |
| Fran       | 0.26   |
| Rifkin     | 0.15   |
+---------+------+
6 rows in set

63. Write a query that produces the names and cities of all customers with the same rating as Hoffman. Write the query using Hoffman’s CNUM rather than his rating, so that it would still be
usable if his rating changed.

mysql> select * from customer a where rating = (select rating from customer b where b.cname='Hoffman') and cname!='Hoffman';
+------+---------+--------+--------+------+
| cnum  | cname    | city        | rating    | snum  |
+------+---------+--------+--------+------+
| 2006  | Clemens | London |    100    | 1001 |
| 2007  | Pereira   | Rome    |    100    | 1004 |
+------+---------+--------+--------+------+
2 rows in set (0.00 sec)

on Monday
mysql> select * from customer a where rating in (select rating from customer b where a.snum=b.snum and b.cnum in (select c.cnum  from customer c where c.cname='Hoffman')) and a.cname!='Hoffman';
+------+---------+--------+--------+------+
| cnum  | cname    | city        | rating    | snum  |
+------+---------+--------+--------+------+
| 2006  | Clemens | London |    100   | 1001 |
+------+---------+--------+--------+------+
1 row in set

64. Find all salespeople for whom there are customers that follow them in alphabetical order.

mysql> select * from salespeople where sname < any (select cname from customer where salespeople.snum = customer.snum);
+------+---------+----------+------+
| snum  | sname     | city          | comm |
+------+---------+----------+------+
| 1003  | AxelRod | New York| 0.10  |
| 1004  | Motika   | London    | 0.11   |
+------+---------+----------+------+
2 rows in set

65. Write a query that produces the names and ratings of all customers of all who have above average orders.

mysql> select b.cname,b.rating from orders a, customer b where a.cnum = b.cnum and a.amt > (select avg(amt) as avg_total from orders);
+---------+--------+
| cname     | rating    |
+---------+--------+
| Liu          |    200   |
| Clemens  |    100   |
| Clemens  |    100   |
+---------+--------+
3 rows in set

66. Find the SUM of all purchases from the Orders table.

mysql> select sum(amt) as Purchases from orders;
+-----------+
| Purchases |
+-----------+
|  26658.40 |
+-----------+
1 row in set

67. Write a SELECT command that produces the order number, amount and date for all rows in the order table.

mysql> select onum,amt,odate from orders;
+------+---------+-------------+
| onum  | amt        | odate            |
+------+---------+-------------+
| 3001  |   18.69   | 1996-10-03 |
| 3002  | 1900.10 | 1996-10-10 |
| 3003  |  767.19  | 1996-10-03 |
| 3005  | 5160.45 | 1996-10-10 |
| 3006  | 1098.16 | 1996-10-03 |
| 3007  |   75.75   | 1996-10-04 |
| 3008  | 4723.00 | 1996-10-05 |
| 3009  | 1713.23 | 1996-10-04 |
| 3010  | 1309.95 | 1996-10-06 |
| 3011  | 9891.88 | 1996-10-06 |
+------+---------+------------+
10 rows in set

68. Count the number of nonNULL rating fields in the Customers table (including repeats).

mysql> select count(rating) from customer where rating is not null;
+---------------+
| count(rating)     |
+---------------+
|             7          |
+---------------+
1 row in set

69. Write a query that gives the names of both the salesperson and the customer for each order after the order number.

mysql> select a.onum,c.sname,b.cname from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum;
+------+---------+----------+
| onum  | sname    | cname      |
+------+---------+----------+
| 3003  | Peel        | Hoffman  |
| 3007  | AxelRod | Giovanni  |
| 3009  | AxelRod | Giovanni |
| 3005  | Serres    | Liu          |
| 3010  | Serres    | Grass      |
| 3008  | Peel       | Clemens  |
| 3011  | Peel       | Clemens  |
| 3002  | Motika  | Pereira    |
| 3001  | Rifkin    | Cisneros  |
| 3006  | Rifkin    | Cisneros  |
+------+---------+----------+
10 rows in set

70. List the commissions of all salespeople servicing customers in London.

mysql> select comm from salespeople c where c.snum in (select b.snum from customer b where b.city = 'London');
+------+
| comm |
+------+
| 0.12   |
+------+
1 row in set

71. Write a query using ANY or ALL that will find all salespeople who have no customers located in their city.

Using ALL
mysql> select * from salespeople a,customer b where a.snum=b.snum and a.city != ALL ( select city from customer c where a.snum = c.snum);
+------+---------+-----------+------+------+----------+----------+--------+------+
| snum  | sname     | city            | comm | cnum | cname     | city           | rating    | snum  |
+------+---------+-----------+------+------+----------+----------+--------+------+
| 1003  | AxelRod | New York | 0.10   | 2002 | Giovanni  | Rome       |    200    | 1003 |
| 1004  | Motika   | London     | 0.11    | 2007 | Pereira    | Rome       |    100    | 1004 |
| 1007  | Rifkin     | Barcelona  | 0.15   | 2008  | Cisneros | San Jose   |    300    | 1007 |
+------+---------+-----------+------+------+----------+----------+--------+------+
3 rows in set

Using Any

mysql> select * from salespeople a,customer b where a.snum=b.snum and a.city != any ( select c.city from  customer c where a.snum = c.snum and c.city=b.city);
+------+---------+-----------+------+------+----------+----------+--------+------+
| snum  | sname     | city           | comm | cnum  | cname     | city           | rating    | snum  |
+------+---------+-----------+------+------+----------+----------+--------+------+
| 1003  | AxelRod | New York | 0.10   | 2002 | Giovanni   | Rome      |    200   | 1003  |
| 1002  | Serres    | San Jose    | 0.13   | 2004 | Grass       | Berlin       |    300   | 1002  |
| 1004  | Motika   | London     | 0.11   | 2007  | Pereira     | Rome      |    100   | 1004  |
| 1007  | Rifkin     | Barcelona  | 0.15  | 2008   | Cisneros  | San Jose  |    300  | 1007  |
+------+---------+-----------+------+------+----------+----------+--------+------+
4 rows in set

72. Write a query using the EXISTS operator that selects all salespeople with customers located in their cities who are not assigned to them.

mysql> Select distinct c.snum, c.sname from salespeople c,customer a where exists ( select cnum from customer b where c.city = b.city and c.snum != b.snum) and c.city = a.city and c.snum != a.snum;
+------+--------+
| snum  | sname   |
+------+--------+
| 1004  | Motika |
| 1005  | Fran     |
| 1002  | Serres  |
+------+--------+
3 rows in set

73. Write a query that selects all customers serviced by Peel or Motika. (Hint : The SNUM field relates the two tables to one another.)

mysql> select * from customer b,salespeople c where b.snum=c.snum and (c.sname='Peel' or c.sname='Motika');
+------+---------+--------+--------+------+------+--------+---------+------+
| cnum  | cname     | city       | rating    | snum  | snum  | sname   | city        | comm |
+------+---------+--------+--------+------+------+--------+---------+------+
| 2001  | Hoffman | London |    100    | 1001 | 1001 | Peel      | London  | 0.12  |
| 2006  | Clemens | London |    100    | 1001 | 1001 | Peel      | London  | 0.12  |
| 2007  | Pereira   | Rome    |    100    | 1004 | 1004 | Motika | London   | 0.11 |
+------+---------+--------+--------+------+------+--------+--------+------+
3 rows in set

74. Count the number of salespeople registering orders for each day. (If a salesperson has more than one order on a given day, he or she should be counted only once.)

mysql> select count(a.odate),c.sname from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.snum;
+----------------+---------+
| count(a.odate)   | sname     |
+----------------+---------+
|              3          | Peel        |
|              2          | Serres     |
|              2          | AxelRod |
|              1         | Motika    |
|              2         | Rifkin      |
+----------------+---------+
5 rows in set 

OR

mysql> select a.odate,count(distinct(c.sname)) from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by a.odate;
+------------+--------------------------+
| odate           | count(distinct(c.sname))   |
+------------+--------------------------+
| 1996-10-03 |                        2              |
| 1996-10-04 |                        1              |
| 1996-10-05 |                        1              |
| 1996-10-06 |                        2              |
| 1996-10-10 |                        2              |
+------------+--------------------------+
5 rows in set

75. Find all orders attributed to salespeople in London.

mysql> select a.onum,a.cnum from orders a where a.cnum in (select b.cnum from customer b where b.city='London');
+------+------+
| onum  | cnum |
+------+------+
| 3003  | 2001 |
| 3008  | 2006 |
| 3011  | 2006 |
+------+------+
3 rows in set

76. Find all orders by customers not located in the same cities as their salespeople.

mysql> select * from orders a where a.cnum in (select cnum from customer b,salespeople c where b.snum=c.snum and b.city!=c.city);
+------+---------+------------+-------+
| onum  | amt         | odate          | cnum  |
+------+---------+------------+-------+
| 3007  |   75.75   | 1996-10-04 | 2002 |
| 3009  | 1713.23 | 1996-10-04 | 2002 |
| 3010  | 1309.95 | 1996-10-06 | 2004 |
| 3002  | 1900.10 | 1996-10-10 | 2007 |
| 3001  |   18.69   | 1996-10-03 | 2008 |
| 3006  | 1098.16 | 1996-10-03 | 2008 |
+------+---------+------------+-------+
6 rows in set


77. Find all salespeople who have customers with more than one current order.

mysql> select a.cnum,count(a.cnum) from orders a where a.cnum in (select b.cnum from customer b where a.cnum=b.cnum and b.snum in(select c.snum from salespeople  c)) group by a.cnum having count(a.cnum) > 1;
+------+---------------+
| cnum  | count(a.cnum)  |
+------+---------------+
| 2002  |             2         |
| 2006  |             2         |
| 2008  |             2         |
+------+---------------+
3 rows in set

78. Write a query that extracts from the Customers table every customer assigned to a salesperson who currently has at least one other customer (besides the customer being selected) with orders in the Orders table.

mysql> select a.onum,max(b.cname) from orders a,customer b where a.cnum=b.cnum group by a.cnum having count(a.cnum) > 1;
+------+--------------+
| onum  | max(b.cname)|
+------+--------------+
| 3007  | Giovanni        |
| 3008  | Clemens        |
| 3001  | Cisneros        |
+------+--------------+
3 rows in set

79. Write a query that selects all customers whose names begin with ‘C’.

mysql> select * from customer where substring(cname,1,1)="C";
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating    | snum  |
+------+----------+----------+--------+------+
| 2006  | Clemens  | London    |    100    | 1001 |
| 2008  | Cisneros  | San Jose  |    300    | 1007 |
+------+----------+----------+--------+------+
2 rows in set

OR

mysql> select * from customer where cname like 'C%';
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating    | snum  |
+------+----------+----------+--------+------+
| 2006  | Clemens   | London   |    100    | 1001 |
| 2008  | Cisneros   | San Jose |    300    | 1007 |
+------+----------+----------+--------+------+
2 rows in set

80. Write a query on the Customers table that will find the highest rating in each city. Put the output in this form : for the city (city) the highest rating is : (rating).

mysql> select concat("For the city"," ",city," the highest rating"," ",max(rating) ) from customer group by city order by max(rating) desc;
+------------------------------------------------------------------------+
| concat("For the city"," ",city," the highest rating"," ",max(rating) )             |
+------------------------------------------------------------------------+
| For the city Berlin the highest rating 300                                                 |
| For the city San Jose the highest rating 300                                            |
| For the city Rome the highest rating 200                                                 |
| For the city London the highest rating 100                                              |
+------------------------------------------------------------------------+
4 rows in set

81. Write a query that will produce the SNUM values of all salespeople with orders currently in the Orders table (without any repeats).

mysql> select a.onum,c.snum from orders a,customer b,salespeople c where a.cnum=
b.cnum and b.snum=c.snum;
+------+------+
| onum  | snum |
+------+------+
| 3003  | 1001 |
| 3008  | 1001 |
| 3011  | 1001 |
| 3005  | 1002 |
| 3010  | 1002 |
| 3007  | 1003 |
| 3009  | 1003 |
| 3002  | 1004 |
| 3001  | 1007 |
| 3006  | 1007 |
+------+------+
10 rows in set

OR

mysql> select max(c.snum) from orders a,customer b,salespeople c where a.cnum=bcnum and b.snum=c.snum group by c.snum;
+-------------+
| max(c.snum) |
+-------------+
|        1001 |
|        1002 |
|        1003 |
|        1004 |
|        1007 |
+-------------+
5 rows in set

82. Write a query that lists customers in descending order of rating. Output the rating field first, followed by the customer’s names and numbers.

mysql> select rating,cname,cnum from customer order by rating desc;
+--------+----------+------+
| rating    | cname      | cnum  |
+--------+----------+------+
|    300   | Grass       | 2004  |
|    300   | Cisneros  | 2008  |
|    200   | Giovanni  | 2002  |
|    200   | Liu          | 2003  |
|    100   | Hoffman  | 2001  |
|    100   | Clemens  | 2006  |
|    100   | Pereira    | 2007  |
+--------+----------+------+
7 rows in set

83. Find the average commission for salespeople in London.

mysql> select avg(comm) from salespeople where city='London';
+-----------+
| avg(comm) |
+-----------+
|  0.163333 |
+-----------+
1 row in set

84. Find all orders credited to the same salesperson who services Hoffman (CNUM 2001).

mysql> select a.onum,c.sname,b.cname,a.amt from orders a,salespeople c,customer b where a.cnum=b.cnum and b.snum=c.snum and c.snum=(select snum from customer where cname ='Hoffman');
+------+-------+---------+---------+
| onum  | sname  | cname    | amt        |
+------+-------+---------+---------+
| 3003  | Peel     | Hoffman |  767.19  |
| 3008  | Peel     | Clemens | 4723.00 |
| 3011  | Peel     | Clemens | 9891.88 |
+------+-------+---------+---------+
3 rows in set

85. Find all salespeople whose commission is in between 0.10 and 0.12 (both inclusive).

mysql> select * from salespeople where comm between 0.10 and 0.12 order by comm;

+------+---------+-----------+-------+
| snum  | sname     | city           | comm  |
+------+---------+-----------+-------+
| 1003 | AxelRod  | New York | 0.10    |
| 1004 | Motika    | London     | 0.11    |
| 1001 | Peel        | London      | 0.12   |
+------+---------+----------+-------+
3 rows in set

86. Write a query that will give you the names and cities of all salespeople in London with a commission above 0.10.

mysql> select sname,city from salespeople where comm > 0.10 and city='London';
+--------+--------+
| sname    | city       |
+--------+--------+
| Peel      | London |
| Motika | London |
| Fran     | London |
+--------+--------+
3 rows in set

87. What will be the output from the following query?
mysql> SELECT * FROM ORDERS where (amt < 1000 OR NOT (odate = 10/03/1996 AND cnum > 2003));

+------+---------+------------+------+
| onum  | amt        | odate          | cnum  |
+------+---------+------------+------+
| 3001  |   18.69   | 1996-10-03 | 2008 |
| 3002  | 1900.10 | 1996-10-10 | 2007 |
| 3003  |  767.19 | 1996-10-03  | 2001 |
| 3005  | 5160.45 | 1996-10-10 | 2003 |
| 3006  | 1098.16 | 1996-10-03 | 2008 |
| 3007  |   75.75   | 1996-10-04 | 2002 |
| 3008  | 4723.00 | 1996-10-05 | 2006 |
| 3009  | 1713.23 | 1996-10-04 | 2002 |
| 3010  | 1309.95 | 1996-10-06 | 2004 |
| 3011  | 9891.88 | 1996-10-06 | 2006 |
+------+---------+------------+------+
10 rows in set (0.08 sec)

Display all rows from order except date is 03-oct 1990 with cnum >2003 or orders where amt is less then 1000

88. Write a query that selects each customer’s smallest order.

mysql> select a.cnum,min(a.amt) as 'Minimum_Order' from orders a group by a.cnum;
+------+------------------+
| cnum  | Minimum_Order  |
+------+------------------+
| 2001  |        767.19         |
| 2002  |         75.75          |
| 2003  |       5160.45        |
| 2004  |       1309.95        |
| 2006  |       4723.00        |
| 2007  |       1900.10        |
| 2008  |         18.69          |
+------+-----------------+
7 rows in set

89. Write a query that selects the first customer in alphabetical order whose name begins with G.

mysql> select * from customer where cname like 'G%' order by cname;
+------+----------+--------+--------+------+
| cnum  | cname      | city       | rating    | snum  |
+------+----------+--------+--------+------+
| 2002  | Giovanni  | Rome    |    200   | 1003  |
| 2004  | Grass       | Berlin    |    300   | 1002  |
+------+----------+--------+--------+------+
2 rows in set

90. Write a query that counts the number of different nonNULL city values in the Customers table.

mysql> select city,count(city) from customer where city is not null group by city;
+----------+-------------+
| city           | count(city)    |
+----------+-------------+
| Berlin       |           1        |
| London    |           2        |
| Rome       |           2       |
| San Jose  |           2        |
+----------+-------------+
4 rows in set

91. Find the average amount from the Orders table.

mysql> select avg(a.amt) 'Average Amount' from orders a;
+----------------+
| Average Amount |
+----------------+
|    2665.840000 |
+----------------+
1 row in set

92. What would be the output from the following query?

SELECT * FROM ORDERS WHERE NOT (odate = 10/03/96 OR snum > 1006) AND amt >= 1500;

ERROR 1054 (42S22): Unknown column 'snum' in 'where clause'

93. Find all customers who are not located in San Jose and whose rating is above 200.

mysql> select * from customer where rating > 200 and city!='San Jose';
+------+-------+--------+--------+------+
| cnum  | cname | city        | rating    | snum |
+------+-------+--------+--------+------+
| 2004  | Grass  | Berlin    |    300    | 1002 |
+------+-------+--------+--------+------+
1 row in set

94. Give a simpler way to write this query :
SELECT snum, sname city, comm FROM salespeople WHERE (comm >=0.12 OR comm < 0.14);

mysql> select * from salespeople a where comm < 0.14;
+------+---------+----------+------+
| snum  | sname     | city          | comm |
+------+---------+----------+------+
| 1001 | Peel        | London    | 0.12   |
| 1002 | Serres     | San Jose  | 0.13   |
| 1003 | AxelRod | New York | 0.10  |
| 1004 | Motika   | London    | 0.11   |
+------+---------+----------+------+
4 rows in set

95. Evaluate the following query :

mysql> SELECT * FROM orders WHERE NOT ((odate = 10/03/96 AND snum > 1002) OR amt  > 2000.00);
ERROR 1054 (42S22): Unknown column 'snum' in 'where clause'

96. Which salespersons attend to customers not in the city they have been assigned to?

mysql> select * from salespeople c,customer b where b.snum=c.snum and (c.city) != any (select city from customer a where a.snum=c.snum and a.city=b.city);
+------+---------+-----------+------+------+----------+----------+--------+------+
| snum  | sname     | city            | comm | cnum | cname     | city           | rating    | snum |
+------+---------+-----------+------+------+----------+----------+--------+------+
| 1003  | AxelRod  | New York | 0.10  | 2002 | Giovanni  | Rome       |    200   | 1003  |
| 1002  | Serres    | San Jose    | 0.13   | 2004 | Grass      | Berlin        |    300   | 1002 |
| 1004  | Motika  | London       | 0.11   | 2007 | Pereira    | Rome       |    100   | 1004 |
| 1007  | Rifkin    | Barcelona   | 0.15   | 2008 | Cisneros  | San Jose  |    300    | 1007 |
+------+---------+-----------+------+------+----------+----------+--------+------+
4 rows in set

97. Which salespeople get commission greater than 0.11 are serving customers rated less than 250?

mysql> select * from salespeople c,customer b where b.snum=c.snum and c.comm > 0.11 and b.rating < 250;
+------+--------+----------+------+------+---------+----------+--------+------+
| snum  | sname   | city          | comm | cnum | cname     | city          | rating    | snum  |
+------+--------+----------+------+------+---------+----------+--------+------+
| 1001 | Peel       | London   | 0.12    | 2001 | Hoffman | London    |    100    | 1001 |
| 1002 | Serres    | San Jose | 0.13    | 2003 | Liu         | San Jose  |    200    | 1002 |
| 1001 | Peel       | London   | 0.12    | 2006 | Clemens | London   |    100    | 1001 |
+------+--------+----------+------+------+---------+----------+--------+------+
3 rows in set

98. Which salespeople have been assigned to the same city but get different commission percentages?

mysql> select distinct c.sname,c.city 'Sales City',b.city 'Customer city' , c.comm 'Commission' from salespeople c,customer b where b.snum=c.snum and b.city=c.city;
+--------+------------+---------------+------------+
| sname   | Sales City    | Customer city  | Commission |
+--------+------------+---------------+------------+
| Peel      | London       | London            |       0.12     |
| Serres   | San Jose     | San Jose          |       0.13     |
+--------+------------+---------------+------------+
2 rows in set

99. Which salesperson has earned the most by way of commission?

mysql> select c.sname,a.amt,c.comm,sum(a.amt*c.comm) as max_earned from orders a ,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.snum order by max_earned  desc limit 1;
+-------+--------+------+-------------+
| sname  | amt       | comm | max_earned |
+-------+--------+------+------------+
| Peel     | 767.19 | 0.12   |  1845.8484 |
+-------+--------+------+------------+
1 row in set

100.Does the customer who has placed the maximum number of orders have the maximum rating?

mysql> select max(a.amt),b.rating,a.cnum,b.cnum from orders a,customer b where a.cnum=b.cnum;
+------------+--------+------+------+
| max(a.amt)  | rating    | cnum  | cnum |
+------------+--------+------+------+
|    9891.88   |    100    | 2001 | 2001 |
+------------+--------+------+------+
1 row in set

Nope customer with maximum order has lowest rating.

101.Has the customer who has spent the largest amount of money been given the highest rating?

mysql> select b.cnum,b.rating,a.amt,a.cnum from orders a,customer b where a.cnum=b.cnum order by a.amt desc;
+------+--------+---------+------+
| cnum  | rating    | amt         | cnum |
+------+--------+---------+------+
| 2006 |    100    | 9891.88 | 2006 |
| 2003 |    200    | 5160.45 | 2003 |
| 2006 |    100    | 4723.00 | 2006 |
| 2007 |    100    | 1900.10 | 2007 |
| 2002 |    200    | 1713.23 | 2002 |
| 2004 |    300    | 1309.95 | 2004 |
| 2008 |    300    | 1098.16 | 2008 |
| 2001 |    100    |  767.19  | 2001 |
| 2002 |    200    |   75.75   | 2002 |
| 2008 |    300    |   18.69   | 2008 |
+------+--------+---------+------+
10 rows in set

Nope customer who has spent the largest amount of money has lower rating.

102.List all customers in descending order of customer rating.

mysql> select * from customer order by rating desc;
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating     | snum |
+------+----------+----------+--------+------+
| 2004 | Grass        | Berlin      |    300    | 1002 |
| 2008 | Cisneros   | San Jose  |    300    | 1007 |
| 2002 | Giovanni   | Rome      |    200    | 1003 |
| 2003 | Liu           | San Jose  |    200    | 1002 |
| 2001 | Hoffman   | London   |    100     | 1001 |
| 2006 | Clemens   | London   |    100     | 1001 |
| 2007 | Pereira     | Rome      |    100     | 1004 |
+------+----------+----------+--------+------+
7 rows in set

103.On which days has Hoffman placed orders?

mysql> select day(a.odate) 'Day' from orders a,customer b where a.cnum=b.cnum and b.cname='Hoffman';
+------+
| Day    |
+------+
|    3     |
+------+
1 row in set

104.Do all salespeople have different commissions?

Yes all salespeople have different commissions

mysql> select * from salespeople;
+------+---------+-----------+------+
| snum  | sname     | city           | comm |
+------+---------+-----------+------+
| 1001 | Peel        | London    | 0.12    |
| 1002 | Serres     | San Jose  | 0.13    |
| 1003 | AxelRod | New York | 0.10  |
| 1004 | Motika   | London    | 0.11    |
| 1005 | Fran       | London    | 0.26   |
| 1007 | Rifkin     | Barcelona    | 0.15 |
+------+---------+-----------+------+
6 rows in set

105.Which salespeople have no orders between 10/03/1996 and 10/05/1996?

mysql> select c.sname,a.onum,a.odate from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum and odate not between '1996-10-03' and '1996-10-05';
+--------+------+------------+
| sname   | onum  | odate          |
+--------+------+------------+
| Peel     | 3011   | 1996-10-06 |
| Serres  | 3005  | 1996-10-10 |
| Serres  | 3010  | 1996-10-06 |
| Motika | 3002 | 1996-10-10 |
+--------+------+------------+
4 rows in set

106.How many salespersons have succeeded in getting orders?

Except Fran rest all of salespeople have succeeded in getting orders

mysql> select c.sname,a.onum,a.odate from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum;
+---------+------+-------------+
| sname     | onum  | odate           |
+---------+------+-------------+
| Peel        | 3003  | 1996-10-03 |
| Peel        | 3008  | 1996-10-05 |
| Peel        | 3011  | 1996-10-06 |
| Serres    | 3005  | 1996-10-10 |
| Serres    | 3010  | 1996-10-06 |
| AxelRod | 3007 | 1996-10-04 |
| AxelRod | 3009 | 1996-10-04 |
| Motika   | 3002 | 1996-10-10 |
| Rifkin     | 3001 | 1996-10-03 |
| Rifkin     | 3006 | 1996-10-03 |
+---------+------+------------+
10 rows in set

107.How many customers have placed orders?

All Customers have placed orders

mysql> select b.cname,a.onum,a.odate from orders a,customer b where a.cnum=b.cnum;
+----------+------+-------------+
| cname      | onum  | odate           |
+----------+------+-------------+
| Hoffman  | 3003  | 1996-10-03 |
| Giovanni  | 3007  | 1996-10-04 |
| Giovanni | 3009   | 1996-10-04 |
| Liu          | 3005  | 1996-10-10 |
| Grass      | 3010  | 1996-10-06 |
| Clemens  | 3008 | 1996-10-05  |
| Clemens  | 3011 | 1996-10-06  |
| Pereira    | 3002 | 1996-10-10  |
| Cisneros | 3001 | 1996-10-03  |
| Cisneros | 3006 | 1996-10-03  |
+----------+------+------------+
10 rows in set

OR

mysql> select b.cname,count(*) from orders a,customer b where a.cnum=b.cnum group by a.cnum;
+----------+----------+
| cname      | count(*)   |
+----------+----------+
| Hoffman   |        1      |
| Giovanni   |        2      |
| Liu           |        1      |
| Grass       |        1      |
| Clemens   |        2     |
| Pereira     |        1     |
| Cisneros  |        2      |
+----------+---------+
7 rows in set

108.On which date has each salesperson booked an order of maximum value?

mysql> select a.odate,max(a.amt),c.sname from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by a.odate,c.snum;
+------------+------------+---------+
| odate           | max(a.amt) | sname    |
+------------+------------+---------+
| 1996-10-03 |     767.19 | Peel         |
| 1996-10-03 |    1098.16 | Rifkin     |
| 1996-10-04 |    1713.23 | AxelRod |
| 1996-10-05 |    4723.00 | Peel       |
| 1996-10-06 |    9891.88 | Peel       | 
| 1996-10-06 |    1309.95 | Serres   |
| 1996-10-10 |    5160.45 | Serres   |
| 1996-10-10 |    1900.10 | Motika  |
+------------+------------+---------+
8 rows in set

109.Who is the most successful salesperson?

mysql> select c.sname,max(amt) from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum;
+-------+----------+
| sname  | max(amt) |
+-------+----------+
| Peel     |  9891.88 |
+-------+----------+
1 row in set

110.Who is the worst customer with respect to the company?

mysql> select b.cname,min(a.amt) from orders a,customer b where a.cnum=b.cnum;
+---------+------------+
| cname     | min(a.amt)  |
+---------+------------+
| Hoffman  |      18.69    |
+---------+------------+
1 row in set

111.Are all customers not having placed orders greater than 200 totally been serviced by salespersons Peel or Serres?

mysql> select * from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum and (c.sname='Peel' or c.sname='Serres') and amt > 200;
+------+---------+------------+------+------+---------+----------+--------+--------+-------+------+-------+------+
| onum | amt         | odate          | cnum  | cnum  | cname    | city          | rating    | snum     | snum   | sname | city     | comm |
+------+---------+------------+------+------+---------+----------+--------+--------+-------+------+-------+-------+
| 3003 |  767.19  | 1996-10-03 | 2001 | 2001 | Hoffman  | London   |    100    | 1001    | 1001   | Peel    | London | 0.12 |
| 3005 | 5160.45 | 1996-10-10 | 2003 | 2003 | Liu          | San Jose |    200    | 1002    | 1002   | Serres | San Jose| 0.13 |
| 3008 | 4723.00 | 1996-10-05 | 2006 | 2006 | Clemens  | London   |    100    | 1001    | 1001   | Peel    | London  | 0.12 |
| 3010 | 1309.95 | 1996-10-06 | 2004 | 2004 | Grass      | Berlin     |    300     | 1002    | 1002   | Serres | San Jose | 0.13 |
| 3011 | 9891.88 | 1996-10-06 | 2006 | 2006 | Clemens  | London   |    100    | 1001    | 1001   | Peel    | London   | 0.12 |
+------+---------+------------+------+------+---------+----------+--------+------+------+--------+-----------+-----+
5 rows in set

112.Which customers have the same rating?

mysql> select b.cname,b.rating from customer b,salespeople c where b.snum=c.snum  group by b.rating,b.cnum;
+----------+--------+
| cname      | rating    |
+----------+--------+
| Hoffman  |    100    |
| Clemens  |    100    |
| Pereira    |    100    |
| Giovanni  |    200   |
| Liu          |    200   |
| Grass      |    300   |
| Cisneros  |    300  |
+----------+--------+
7 rows in set

113.Find all orders greater than the average for October 4th.

mysql> select * from orders a where a.amt > (select avg(amt) from orders where odate='1996-10-04');
+------+---------+------------+------+
| onum  | amt        | odate           | cnum |
+------+---------+------------+------+
| 3002  | 1900.10 | 1996-10-10 | 2007 |
| 3005  | 5160.45 | 1996-10-10 | 2003 |
| 3006  | 1098.16 | 1996-10-03 | 2008 |
| 3008  | 4723.00 | 1996-10-05 | 2006 |
| 3009  | 1713.23 | 1996-10-04 | 2002 |
| 3010  | 1309.95 | 1996-10-06 | 2004 |
| 3011 |  9891.88 | 1996-10-06 | 2006 |
+------+---------+------------+------+
7 rows in set

114.Which customers have above average orders?

mysql> select b.cname,a.amt from orders a,customer b where a.cnum=b.cnum and a.amt > (select avg(amt) from orders);
+---------+---------+
| cname     | amt        |
+---------+---------+
| Liu         | 5160.45 |
| Clemens | 4723.00 |
| Clemens | 9891.88 |
+---------+---------+
3 rows in set

115.List all customers with ratings above San Jose’s average.

mysql> select * from customer where rating > (select avg(rating) from customer where city='San Jose');
+------+----------+----------+--------+------+
| cnum  | cname      | city          | rating     | snum |
+------+----------+----------+--------+------+
| 2004  | Grass       | Berlin      |    300    | 1002 |
| 2008  | Cisneros   | San Jose |    300    | 1007 |
+------+----------+----------+--------+------+
2 rows in set

116.Select the total amount in orders for each salesperson for whom the total is greater than the amount of the largest order in the table.

mysql> select c.sname,b.snum,sum(a.amt) as total from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by b.snum having total > (select max(c.amt) from orders c);
+-------+------+----------+
| sname  | snum  | total         |
+-------+------+----------+
| Peel     | 1001 | 15382.07 |
+-------+------+----------+
1 row in set

117.Give names and numbers of all salespersons who have more than one customer.

mysql> select c.sname,b.snum,count(b.snum) as total from salespeople c,customer b where b.snum=c.snum group by b.snum having total > 1;
+--------+------+-------+
| sname    | snum | total     |
+--------+------+-------+
| Peel      | 1001  |     2     |
| Serres   | 1002  |     2     |
+--------+------+-------+
2 rows in set

118.Select all salespersons by name and number who have customers in their city whom they don’t service.

mysql> select distinct(b.sname),b.snum,a.city as Customer_City,b.city as Salesperson_City from customer a, salespeople b where a.city = b.city and a.snum != b.snum;
+--------+------+---------------+------------------+
| sname   | snum  | Customer_City | Salesperson_City |
+--------+------+---------------+------------------+
| Motika  | 1004 | London            | London                |
| Fran      | 1005 | London            | London                |
| Serres   | 1002 | San Jose          | San Jose               |
+--------+------+---------------+------------------+
3 rows in set

mysql> Select * from salespeople b where b.city in ( select city from customer a  where a.snum != b.snum );
+------+--------+----------+------+
| snum  | sname   | city           | comm |
+------+--------+----------+------+
| 1004 | Motika  | London    | 0.11   |
| 1005 | Fran      | London    | 0.26   |
| 1002 | Serres   | San Jose    | 0.13 |
+------+--------+----------+------+
3 rows in set

119.Which customers’ rating should be lowered?

mysql> select min(a.amt),b.cname from orders a,customer b where a.cnum=b.cnum;
+------------+---------+
| min(a.amt)   | cname    |
+------------+---------+
|      18.69     | Hoffman |
+------------+---------+
1 row in set

Hoffman has lowest order so his rating should be lowered.

120.Is there a case for assigning a salesperson to Berlin?

mysql> select * from salespeople c,customer b where b.snum=c.snum and b.city='Berlin';
+------+--------+----------+------+------+-------+--------+--------+------+
| snum   | sname   | city          | comm | cnum | cname | city       | rating    | snum  |
+------+--------+----------+------+------+-------+--------+--------+------+
| 1002  | Serres   | San Jose  | 0.13   | 2004 | Grass  | Berlin    |  300      | 1002 |
+------+--------+----------+------+------+-------+--------+--------+------+
1 row in set

121.Is there any evidence linking the performance of a salesperson to the commission that he or she is being paid?

mysql> select c.sname,a.amt,c.comm,sum(a.amt*c.comm) as max_earned from orders a ,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.snum order by max_earned;
+---------+---------+------+------------+
| sname     | amt        | comm | max_earned |
+---------+---------+------+------------+
| Rifkin     |   18.69   | 0.15   |   167.5275  |
| AxelRod |   75.75   | 0.10  |   178.8980  |
| Motika   | 1900.10 | 0.11  |   209.0110   |
| Serres    | 5160.45 | 0.13  |   841.1520   |
| Peel       |  767.19  | 0.12  |  1845.8484  |
+---------+---------+------+------------+
5 rows in set

122.Does the total amount in orders by customer in Rome and London exceed the commission paid to salespersons in London and New York by more than 5 times?

mysql> select sum(a.amt) from orders a,customer b where a.cnum=b.cnum and (b.city='Rome' or b.city='London');
+------------+
| sum(a.amt) |
+------------+
|   19071.15 |
+------------+
1 row in set

mysql> select distinct (select sum(total) as grand_total from (select sum(a.amt*c.comm) as total from orders a, customer b,salespeople c where a.cnum = b.cnum and b.snum=c.snum group by a.cnum)sumtot)  sum_tot from orders a1 ,customer b1,salespeople c1 where a1.cnum=b1.cnum and b1.snum=c1.snum and (c1.city='London' or c1.city='New York') order by sum_tot;
+-----------+
| sum_tot   |
+-----------+
| 3242.4369 |
+-----------+
1 row in set

The total amount in orders by customer in Rome and London exceeds the commission paid to salespersons in London and New York by more than 5 times

123.Which is the date, order number, amt and city for each salesperson (by name) for the maximum order he has obtained?

mysql> select a.odate,a.onum,max(a.amt),c.city,c.sname from orders a,customer b,salespeople c where a.cnum=b.cnum and b.snum=c.snum group by c.sname;
+------------+------+------------+-----------+---------+
| odate           | onum | max(a.amt) | city            | sname     |
+------------+------+------------+-----------+---------+
| 1996-10-04 | 3007 |    1713.23 | New York  | AxelRod |
| 1996-10-10 | 3002 |    1900.10 | London      | Motika   |
| 1996-10-03 | 3003 |    9891.88 | London      | Peel       |
| 1996-10-03 | 3001 |    1098.16 | Barcelona   | Rifkin    |
| 1996-10-10 | 3005 |    5160.45 | San Jose    | Serres    |
+------------+------+------------+-----------+---------+
5 rows in set

124.Which salesperson(s) should be fired? 

mysql> select * from orders where cnum in (select cnum from customer where snum in (select snum from salespeople where sname='Fran'));
Empty set

Fran should be fired as he has neither customer nor orders placed for any customer.

Rifkin and Axelrod can be fired as they are having least order for their customers;

125.What is the total income for the company?

mysql> select sum(amt) as total_income from orders;
+--------------+
| total_income |
+--------------+
|     26658.40 |
+--------------+
1 row in set
