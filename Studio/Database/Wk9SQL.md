Created: May 28 2025
Class: [[Advanced SQL]] 
- - -
## Part A
``` sql
SELECT DIRECTOR.DIRNAME, SUM(AWRD) FROM
MOVIE
JOIN DIRECTOR ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
GROUP BY DIRECTOR.DIRNAME;

Q. 2
SELECT DIRECTOR.DIRNAME, SUM(AWRD) FROM
MOVIE
JOIN DIRECTOR ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
GROUP BY DIRECTOR.DIRNAME
ORDER BY SUM(AWRD) DESC;

Q. 3
SELECT DIRECTOR.DIRNAME, SUM(AWRD) as awards FROM
DIRECTOR
LEFT OUTER JOIN MOVIE ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
GROUP BY DIRECTOR.DIRNAME
ORDER BY awards DESC;

Q. 4
SELECT DIRECTOR.DIRNAME, SUM(AWRD) as awards FROM
DIRECTOR
JOIN MOVIE ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
GROUP BY DIRECTOR.DIRNAME
UNION
SELECT DIRNAME, 0 as awards FROM
DIRECTOR
LEFT OUTER JOIN MOVIE ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
WHERE MOVIE.MVTITLE IS NULL
ORDER BY awards DESC;

Q. 5
SELECT DIRECTOR.DIRNAME, SUM(AWRD) as awards, SUM(NOMS) FROM
DIRECTOR
JOIN MOVIE ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
GROUP BY DIRECTOR.DIRNAME
HAVING SUM(MOVIE.NOMS) > (SELECT AVG(nms) FROM 
(SELECT SUM(NOMS) as nms FROM 
DIRECTOR
JOIN MOVIE ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
GROUP BY DIRECTOR.DIRNAME));

Q. 6
SELECT * FROM
MEMBER
GROUP BY mmbname
HAVING COUNT(*) > 1;

Q. 7
SELECT * FROM MOVIE
EXCEPT
SELECT * FROM MOVIE
WHERE MPAA = 'R'

Q. 8
SELECT MVTITLE FROM MOVIE
WHERE DIRNUMB = 1
INTERSECT
SELECT MVTITLE FROM MOVIE
JOIN MOVSTAR ON MOVIE.MVNUMB = MOVSTAR.MVNUMB
WHERE MOVSTAR.STARNUMB = 1;

Q. 9
SELECT MVTITLE FROM MOVIE
WHERE DIRNUMB = 1
EXCEPT
SELECT MVTITLE FROM MOVIE
JOIN MOVSTAR ON MOVIE.MVNUMB = MOVSTAR.MVNUMB
WHERE MOVSTAR.STARNUMB = 1;
```

## Part B
```sql
CREATE VIEW continent(name, NumCountries, Population) AS
SELECT c.Continent, count(*), sum(c.population)
FROM Country as c
GROUP BY c.Continent;
```

```sql
CREATE VIEW dir_awards(DirNumb, DirName, Awards, Nominations) AS
SELECT DIRNAME, DIRECTOR.DIRNUMB, SUM(AWRD), SUM(NOMS)
FROM DIRECTOR
JOIN MOVIE ON MOVIE.DIRNUMB = DIRECTOR.DIRNUMB
GROUP BY DIRECTOR.DIRNAME;
```

- Were you able to insert Steven Spielberg into dir_awards?
No
- Why do you think this is the case?
Its just a query, not the data itself

```sql
CREATE TABLE codes (

code TEXT PRIMARY KEY,

text TEXT

);

INSERT INTO codes values ('PG', 'Parental Guidance');

INSERT INTO codes values ('R','Restricted');

INSERT INTO codes values ('G', 'General');

INSERT INTO codes values ('NR', 'Not Rated');

  

  

CREATE VIEW Movie_Extended(MVNUMB, MVTITLE, YRMDE, MVTYPE, CRIT, MPAA, NOMS, AWRD, DIRDUMB) AS

SELECT MVNUMB, MVTITLE , YRMDE, MVTYPE, CRIT, text, NOMS, AWRD, DIRNUMB FROM MOVIE

JOIN codes ON codes.code = MOVIE.MPAA;
```

```sql
CREATE VIEW Movie_Extended(MVNUMB, MVTITLE, YRMDE, MVTYPE, CRIT, MPAA, NOMS, AWRD, DIRDUMB, SUCCESS) AS

SELECT MVNUMB, MVTITLE , YRMDE, MVTYPE, CRIT, text, NOMS, AWRD, DIRNUMB,

CASE

WHEN NOMS > 0 THEN CAST (AWRD AS FLOAT)/NOMS * 100

ELSE 0

END

FROM MOVIE

JOIN codes ON codes.code = MOVIE.MPAA;
```
