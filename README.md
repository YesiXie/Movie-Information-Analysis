# MovieInformationAnalysis

1.There are three sql files for mysql (IMDB.sql, bar.sql, line.sql)，import them into mysql database, please import IMDB.sql first.

2.There are one cypher file for neo4j (allCQL.cypher), import it into neo4j database. Then unwind data and build relationship of these tables:


  match (b:basic) unwind (b.genres) as bb create(s:newbasic{tconst:b.tconst,titleType:b.titleType,primaryTitle:b.primaryTitle,originalTitle:b.originalTitle,isAdult:b.isAdult,startYear:b.startYear,endYear:b.endYear,runtimeMinutes:b.runtimeMinutes,genres:bb})

  Match (r:ratings),(b:basic) where r.tconst=b.tconst create (r)-[re: evaluate_for]->(b) return re

  Match (a:akas),(b:basic) where a.titleId=b.tconst create (a)<-[r: display_in]-(b) return r

  Match (p:principals),(b:basic) where p.tconst = b.tconst create (p)-[r: act_in]->(b) return r

  Match (r:ratings),(b:newbasic) where r.tconst=b.tconst create (r)-[re:newevaluate_for]->(b) return re

  Match (a:akas),(b:newbasic) where a.titleId=b.tconst create (a)<-[r:newdisplay_in]-(b) return r

  Match (p:principals),(b:newbasic) where p.tconst = b.tconst create (p)-[r:newact_in]->(b) return r

  Match (n:name),(p:principals) where n.nconst=p.nconst create (p)<-[r:information_in]-(n) return r
  

  run these queries in order.


  3.import mergedALL.json into mongodb database.
