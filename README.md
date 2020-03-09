# Movie Information Analysis

## Overview

The project is based on IMDb data from IMDb.com. The data warehouse is built for users who are interested in the movie data. The goal of the data warehouse is to show some statistical analysis about movie data from 2011 to 2018. The website will show this statistical analysis by various tables and graphs.
Three different databases are used which in this project based on MySQL, MongoDB and Neo4j. These three databases save same data in different architectures and use different database language to achieve some data warehouse query.
For the front-end structure, PHP is used to call data from MySQL, MongoDB and Neo4j, and html to do page design. 

## Example pages

### Index page  
The index page includes administrator log in, a navigation banner, some data description, and a copyright description.  
![image](https://github.com/YesiXie/MovieInformationAnalysis/blob/master/Images/Homepage.png)

  User can only see the home, ratings and Reference pages. If log in as administrator, we can check statistics information.  
  ![image](https://github.com/YesiXie/MovieInformationAnalysis/blob/master/Images/Login.png)

### Rating page  
This rating page shows the title of top 30 movies in 2011 which the number of votes larger than 100000 in descending order.  
![image](https://github.com/YesiXie/MovieInformationAnalysis/blob/master/Images/Ratings.png)

### Statistics page  
1. Line chart shows the number of votes of “Drama” movies in UK.  
![image](https://github.com/YesiXie/MovieInformationAnalysis/blob/master/Images/Statistics.png)

2. Bar chart shows the yield of “Drama” movies in 2012 in each country.  
![image](https://github.com/YesiXie/MovieInformationAnalysis/blob/master/Images/Bar.png)

3. Pie chart shows the yield of each genre of movies.  
![image](https://github.com/YesiXie/MovieInformationAnalysis/blob/master/Images/Pie.png)

## How to set up the project:

1. There are three sql files for mysql (IMDB.sql, bar.sql, line.sql)，import them into mysql database, please import IMDB.sql first.

2. There are one cypher file for neo4j (allCQL.cypher), import it into neo4j database. Then unwind data and build relationship of these tables:


  match (b:basic) unwind (b.genres) as bb create(s:newbasic{tconst:b.tconst,titleType:b.titleType,primaryTitle:b.primaryTitle,originalTitle:b.originalTitle,isAdult:b.isAdult,startYear:b.startYear,endYear:b.endYear,runtimeMinutes:b.runtimeMinutes,genres:bb})

  Match (r:ratings),(b:basic) where r.tconst=b.tconst create (r)-[re: evaluate_for]->(b) return re

  Match (a:akas),(b:basic) where a.titleId=b.tconst create (a)<-[r: display_in]-(b) return r

  Match (p:principals),(b:basic) where p.tconst = b.tconst create (p)-[r: act_in]->(b) return r

  Match (r:ratings),(b:newbasic) where r.tconst=b.tconst create (r)-[re:newevaluate_for]->(b) return re

  Match (a:akas),(b:newbasic) where a.titleId=b.tconst create (a)<-[r:newdisplay_in]-(b) return r

  Match (p:principals),(b:newbasic) where p.tconst = b.tconst create (p)-[r:newact_in]->(b) return r

  Match (n:name),(p:principals) where n.nconst=p.nconst create (p)<-[r:information_in]-(n) return r
  

  run these queries in order.


  3. import mergedALL.json into mongodb database.
