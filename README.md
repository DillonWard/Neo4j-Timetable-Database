# Neo4j Graph Database Timetable
###### *Dillon Ward - G00326756 - Graph Theory*
---
## Introduction
The following repository contains a Neo4j Database for a third-year undergraduate project for the module Graph Theory. The module is taught to undergraduate students at GMIT in the Department of Computer Science and Applied Physics. The lecturer is Ian McLoughlin.

## Project Overview
### Specifications
The purpose of this project is to design a Neo4j database that displays data taken from the [GMIT Timetable](https://timetable.gmit.ie/) such as student groups, classrooms, lecturers, and work hours for a timetabling system such as the one used in GMIT. The database should contain nodes with data properties and the relationships between them. 
### Neo4j
##### Getting Started
To begin using Neo4j, you need to download it from [here](https://neo4j.com/download/) and follow the installation instructions. After it has been installed, start your database from the launcher application and head over to `http://localhost:7474` in your browser to begin.
###### What is Neo4j?
[Neo4j](https://neo4j.com/) is an open source NoSQL graph database management system that allows users to create a graph database that uses Nodes to display and describe information and the relationship between them. Nodes are connected entities that represent Data, and can contain properties or Metadata - data that describes other data. For example, the Node `Person` could have a property `name: 'Dillon'`. Each node can also have one or many relationships with other nodes. When 2 nodes are related, their relationship will have a label which describes why they are related. Neo4j is used by querying the database using the query language Cypher.
`MATCH (a:Lecturer), (b:Module) RETURN a, b`
![alt tag](http://image.prntscr.com/image/2abf401380b54418a61a586953d68a8b.png)
*Here, we can see the Nodes Lecturer and Module being queried, as well as how they are related.*
##### Cypher
Cypher is a graph query language that allows efficient querying and updating. Cypher allows for complicated databases to be expressed and queried easily, making it a powerful language. Cypher mainly focuses on what to retrieve from a database, not how to retrieve. 
Cypher's syntax uses keywords, such as:
* `MATCH`: The graph pattern to match. This is the most common way to get data from the graph.
* `WHERE`: Adds constraints to a pattern, or filters the results of a query.
* `RETURN`: What to return from the database.

If you were to run a query on the database using these clauses to see what classes are on Monday, you'd use:
`MATCH (a:Day)-[r:START_TIME]->(b:Room)-[t:GROUP]->(c:Module)
WHERE a.day = "Monday"
RETURN a,b,c,r,t`

With this, you are returning the Day, Room, and Module with the Group and Start Time being the relationships.
![alt tag](http://image.prntscr.com/image/7717a6c553af4d9b8ecc66f1114b4cb8.png)
###### Why use Neo4j?
Neo4j uses graphs and labels to represent data and relationships. With these graphical representations of data, we can have several types of graphs. Including:
* Undirected Graphs: Nodes and relationships are interchangable and relationships can be interpreted any way.
* Directed Graphs: Nodes and relationships are not bidirectional.
* Graphs with Weight: Graphs with numerical assessment.
* Graphs with Labels: Graphs with vertices that are defined by their lables
* Property Graphs: A weighted graph with labels

Graph databases are significantly simpler and are more expressive than relational databases. They have many other benefits, such as:
1. Performance: Graph Databases perform better than relational databases. Even though data queries increase, the performance does not drop.
2. Responsiveness: Neo4j is very effecient at managing data.
3. Flexible/Scalable: Graph Databases are flexible and scalable in that a team could add to an existing graph without endangering its current functionality.

### Implementation
##### Planning
The data needed for this project included information to be extracted from our timetabling system. Rather than reading out the timetable and manually inputting each day, I scraped all the data from our timetable by viewing in a list view (to make it easier to extract), viewed the page source and copy/pasted all the information relevant. I only used the third year Software Development Semester 2 timetable, but other timetables can be remade into a Graph Database. Since they start with their Department and Course, other timetables are made by adding a Course or Department node, then similar Nodes and Relationships from the already existing Database can be used the same way, or new ones added. Eventually the nodes narrow down from Department and course, to semester and Modules, and finally rooms and days.
For the initial design of the nodes, I thought it would be best to start off as broad as possible, eventually narrowing down as the nodes went along, so that the nodes could be read almost like English.

`(software)-[SEMESTER]->(6)->[HAS_MODULE]->(Graph Theory)->[TAUGHT_BY]->(Ian McLoughlin)`


##### Design

##### Queries

### References