> Graph databases have their own query language, GQL, an ISO standard for graph databases.
> Cypher is Neo4j’s implementation of GQL. Cypher is a declarative language, meaning the database is responsible for
> finding the most optimal way of executing that query.


What gives Neo4j its advantage?
- Neo4j is a native graph database designed specifically for graph traversal.
- Where Joins between tables are computed at read-time, this information is saved in a way that allows for quick pointer-chasing in memory
- Queries in Graph Databases are proportional to the amount of data touched during a query, not the size of data overall.


- `MATCH` to read data
- `MERGE` to write data

`
MATCH (n:Person)
WHERE n.name = 'Tom Hanks'
RETURN n
`

Run the following Cypher statement to find the movie 'Toy Story' and the people who acted in the movie. The query uses the [ACTED_IN] relationship to find Person nodes who have a connection to the Movie node.

`
MATCH (m:Movie)<-[r:ACTED_IN]-(p:Person)  
WHERE m.title = 'Toy Story'  
RETURN m, r, p
`

![cypher-1.png](img/cypher-1.png)


`MATCH (m:Movie)-[r:IN_GENRE]->(g:Genre)
WHERE m.title = 'Toy Story'
RETURN m, r, g`

You can return tabular data by including the properties of the nodes.

`MATCH (m:Movie)-[r:IN_GENRE]->(g:Genre)
WHERE m.title = 'Toy Story'
RETURN m.title, g.name`

![tabular-1.png](img/tabular-1.png)


`MATCH (m:Movie)<-[r:ACTED_IN]-(p:Person)
WHERE p.name = 'Emma Stone'
RETURN m.title, m.released`


The statement returns all the movies that have been rated by the user "Mr. Jason Love".

`MATCH (u:User)-[r:RATED]->(m:Movie)
WHERE u.name = "Mr. Jason Love"
RETURN u, r, m`

`MATCH (u:User)-[r:RATED]->(m:Movie)
WHERE u.name = "Mr. Jason Love"
RETURN u.name, r.rating, m.title`

![tabular-3.png](img/tabular-3.png)

`MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE p.name = 'Tom Hanks'
RETURN p.name AS person, m.title AS title, r.role AS role
`

> The MATCH clause is used to find patterns in the data.
> Patterns can be as simple as a single node, or contain multiple relationships.

`MATCH (p:Person)-[:ACTED_IN]->(m:Movie)<-[r:ACTED_IN]-(p2:Person)
WHERE p.name = 'Tom Hanks'
RETURN p.name as main_actor, p2.name AS actor, m.title AS movie, r.role AS role`

> The MERGE Clause
> To create nodes and relationships in the database, you use the MERGE clause.
> You can use the MERGE clause to create a pattern in the database. MERGE will only create the pattern if it doesn’t already exist.

Run this Cypher statement, that uses MERGE to create a new Movie node:

`MERGE (m:Movie {title: "Arthur the King"})
SET m.year = 2024
RETURN m`

Run this Cypher statement, that creates Movie and User nodes and a RATED relationship between them.

`MERGE (m:Movie {title: "Arthur the King"})
MERGE (u:User {name: "Adam"})
MERGE (u)-[r:RATED {rating: 5}]->(m)
RETURN u, r, m`

Run this query to return movies order by the most recent release date:

`MATCH (m:Movie)
WHERE m.released IS NOT NULL
RETURN m.title AS title, m.url AS url, m.released AS released
ORDER BY released DESC LIMIT 5`

Modify this query to add your favourite movie and a user rating

`MERGE (m:Movie {title: "Java"})
MERGE (u:User {name: "Tuna"})
MERGE (u)-[r:RATED {rating: 1}]->(m)
RETURN u, r, m`

`MATCH (u:User)-[r:RATED]->(m:Movie)
WHERE m.title = 'Java'
RETURN m.title, u.name, r.rating`
