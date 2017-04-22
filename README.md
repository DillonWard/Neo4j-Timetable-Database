# Neo4j Graph Database Timetable
###### *Dillon Ward - G00326756 - Graph Theory*
---
## Introduction
The following repository contains a Neo4j Database for a third-year undergraduate project for the module Graph Theory. The module is taught to undergraduate students at GMIT in the Department of Computer Science and Applied Physics. The lecturer is Ian McLoughlin.

## Project Overview
### Specifications
The purpose of this project is to design a Neo4j database that displays data taken from the [GMIT Timetable](https://timetable.gmit.ie/) such as student groups, classrooms, lecturers, and work hours for a timetabling system such as the one used in GMIT. The database should contain nodes with data properties and the relationships between them. 
## Neo4j
### Getting Started
To begin using Neo4j, you need to download it from [here](https://neo4j.com/download/) and follow the installation instructions. After it has been installed, start your database from the launcher application and head over to `http://localhost:7474` in your browser to begin.
### What is Neo4j?
[Neo4j](https://neo4j.com/) is an open source NoSQL graph database management system that allows users to create a graph database that uses Nodes to display and describe information and the relationship between them. Nodes are connected entities that represent Data, and can contain properties or Metadata - data that describes other data. For example, the Node `Person` could have a property `name: 'Dillon'`. Each node can also have one or many relationships with other nodes. When 2 nodes are related, their relationship will have a label which describes why they are related. Neo4j is used by querying the database using the query language Cypher.
`MATCH (a:Lecturer), (b:Module) RETURN a, b`
![alt tag](http://image.prntscr.com/image/2abf401380b54418a61a586953d68a8b.png)

*Here, we can see the Nodes Lecturer and Module being queried, as well as how they are related.*
### Cypher
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
### Why use Neo4j?
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

---
## Implementation
### Planning
The data needed for this project included information to be extracted from our timetabling system. Rather than reading out the timetable and manually inputting each day, I scraped all the data from our timetable by viewing in a list view (to make it easier to extract), viewed the page source and copy/pasted all the information relevant. I only used the third year Software Development Semester 2 timetable, but other timetables can be remade into a Graph Database. Since they start with their Department and Course, other timetables are made by adding a Course or Department node, then similar Nodes and Relationships from the already existing Database can be used the same way, or new ones added. Eventually the nodes narrow down from Department and course, to semester and Modules, and finally rooms and days.
For the initial design of the nodes, I thought it would be best to start off as broad as possible, eventually narrowing down as the nodes went along, so that the nodes could be read almost like English.
`(software)-[SEMESTER]->(6)->[HAS_MODULE]->(Graph Theory)->[TAUGHT_BY]->(Ian McLoughlin)`

### Design
#### Nodes
##### Department
The Department node is the root node. This node has the property `name: 'Science'`. Each course is part of a certain department, and each department has multiple courses in them. Another Node that is in the Science department can be connected to this Node to create another timetable.
##### Course
The course Node represents the course that the timetable is taken from. The timetable used is Semester 2, Year 3 of Software Development. The course node has the property `name: 'Software Development'`. Another of these nodes can be added if you wanted to add another timetable, presumably in the same department.
##### Semester
The semester Node represents the semester of the course. Each year has 2 semesters, so year 3 of software development semester 2 has the property `semester: '6'`. The semester node also has `Module` properties, containing an array of the modules that semester has, making it easier to create relationships with other nodes with 1 query.

`MATCH (a:Semester), (b:Module)
WHERE b.name IN a.modules
CREATE (a)-[HAS_MODULE]->(b)`
*This connectects each Node where the name of the Module is inside the array of Modules in the Semester Nodes*.

![alt tag](http://image.prntscr.com/image/1c6ae5b5da994afe913f574264c346af.png)

##### Module
In Semester 6, there are 6 Module Nodes. These represent the Modules/Subjects that are on that particular semester. Each Module node has a `name` and `lecturer` property. Some Modules may have more than 1 lecturer. The lecturer property is to connect all the Lecturers to their modules in 1 query.

`MATCH (a:Module), (b:Lecturer)
WHERE b.name IN a.lecturer
CREATE (a)-[TAUGHT_BY]->(b)`

*When the Module Lecturer property contains a lecturers name, create a relationship*
![alt tag](http://image.prntscr.com/image/579a8540adbb4853903fb4e19aa32978.png)

##### Lecturer
The Lecturer Node represents the Lecturers, and are connected to the module that they teach. Each lecturer has a `name` property.
##### Room
The Room nodes represent the rooms that each of the Modules are taught in. Each room is connected to a module and the day that room is in use. The rooms are connected to Modules, and their relationships are the representations of which group has that module at that time. Rooms are connected to the Day and their relationship represents the Timeslot on that day.

![alt tag](http://image.prntscr.com/image/7a577189b72145aab1e4e1d480e70da7.png)

##### Days
The days node represents the day that a room is in use. Each Day has a `day` property, and are connected to rooms. Their relationship the time the room is in use. 
`(On this Day)-[AT THIS TIME]->(This room is in use)`

#### Relationships
##### DEPARTMENT
The `DEPARTMENT` relationship connects the Department and Course. This relationship is a representaiton of how the course Software is in the Science department.
`(Software)-[DEPARTMENT]->(Science)`
##### SEMESTER
The `SEMESTER` relationship connects the semester and the course. The purpose of the `SEMESTER` relationship is to display which semester of which course is being displayed. I decided to make another node for the semester instead of it being a relationship so the student can specify which semester they want, or if they want to add another semester they can do so.
`(Software)-[SEMESTER]->(6)`

##### HAS_MODULE
The `HAS_MODULE` relationship shows which Modules are on a particular semester. If a module is on a particular semester, they are connected.
`(6)-[HAS_MODULE]->(Graph Theory)`

##### TAUGHT_BY
The `TAUGHT_BY` relationship is a connection between a Module Node and a Lecturer Node. These relationships show which modules are taught by which lecturers.
`(Graph Theory)-[TAUGHT_BY]->(Ian McLoughlin)`

##### GROUP
The `GROUP` relationship is a representation of which Group is in a Room and what Module they have. The relationship is a connection between Module and the room the module is in. The caption for the relationship is changed to the property `group` for clarity.
##### START_TIME
The `START_TIME` relationship is a representation of the time that a module is on in a particular room. The nodes connected are the day and room nodes. The caption for the relationship is changed to the property `time` for clarity.

---
## Queries
### CRUD
CRUD are the four basic functions of storage in a database. CRUD Operations allow for Creating, Retrieving, Updating, and Deleting of Nodes, Properties and Relationships.
#### Create
To create a single Node:
```
CREATE (n)
```
This creates a Node with no properties. You can create multiples of these Nodes at once. You can create a Node with Multiple labels:
```
CREATE (n:Person:Irish)
```
Now there are Nodes with multiple labels. If we wanted to create a Node with properties:
```
CREATE(n:Person {name: 'Dillon'})
```
This Node is now of Label `Person` with the property `name: 'Dillon'`. You can also create a Node with an array of properties:
```
CREATE(n:Semester {modules: ["Graph Theory", "Mobile Apps"]})
```
Now that we have Nodes, we want to make some relationships between them.
```
MATCH (a:Person), (b:Module)
CREATE (a)-[HAS]->(b)
```
This creates relationships between all the `Person` and `Module` nodes. We can also add conditions to the creation of properties.
```
MATCH (a:Person), (b:Module)
WHERE a.name = 'Dillon' AND b.module = 'Graph Theory'
CREATE (a)-[HAS]->(b)
```
This query creates relationships between all the Nodes with the properties `name: 'Dillon'` and `module: 'Graph Theory'`.
Relationships can also have properties. To create a relationship with properties:
```
MATCH (a:Room), (b:Module)
WHERE a.room = '0379' AND b.module = 'Graph Theory'
CREATE (a)-[START_TIME {time: '9:00'}]->(b)
```
![alt tag](http://image.prntscr.com/image/bd14a9e6d6e44f6f922209a7f16c98a1.png)

#### Retrieve
To retrieve everything:
```
MATCH (n)
RETURN n
```
You can retrieve specific nodes:
```
MATCH (n:Modules)
RETURN n
```
You can also return specific Nodes with specific properties:
```
MATCH (n:Modules {module: "Graph Theory"})
RETURN n
```
A user can retrieve the timetable for a specifc day too:
```
MATCH (a:Day{day: "Tuesday"})-[r:START_TIME]->(b:Room)-[t:GROUP]->(c:Module)
RETURN a,b,c,r,t
```
To return all the modules a group has on a specific day. The `All` is used if it is a lecture, in which all groups go to. Otherwise, it's a lab. To call everything group C has on a Wednesday:
```
MATCH (a:Day{day: "Wednesday"})-[t:START_TIME]->(b:Room)-[g:GROUP]->(c:Module)
WHERE g.group = "Group C" OR g.group = "All"
RETURN a,b,c,g,t
```
![alt tag](http://image.prntscr.com/image/c1bad5c63ba5446b92672fb5b3849ddc.png)

To return the only the labs a group has, remove `OR t.group = "All"`. :
```
MATCH (a:Day{day: "Wednesday"})-[t:START_TIME]->(b:Room)-[g:GROUP]->(c:Module)
WHERE g.group = "Group C"
RETURN a,b,c,g,t
```
![alt tag](http://image.prntscr.com/image/f797b7941f864af8abbd1c4c633c1774.png)

#### Update
You can update existing nodes or relationships with the `SET` keyword.
To change a node:
```
MATCH (a:Module {module: "Graph T"})
SET a.module = "Graph Theory"
RETURN a
```
To change a relationship:
```
MATCH (a:Day{day: "Wednesday"})-[t:START_TIME]->(b:Room)-[g:GROUP{group: 'Group C'}]->(c:Module)
SET g.group = 'Group A'
RETURN a,b,c,g,t
```

#### Delete
You can `DELETE` instead of returning. To delete Module nodes:
```
MATCH (a:Module) 
DELETE a
```
You can also delete module nodes with specific properties:
```
MATCH (a:Module{name: 'Graph Theory'}) 
DELETE a
```
If a node is in a relationship, you have to detach it before deleting it:
```
MATCH (a:Module) 
DETACH DELETE a
```
You can delete specific relationships too. To delete the `HAS_MODULE` relationship:
```
MATCH ()-[r:HAS_MODULE]-() 
DELETE r
```
You can also specify the Nodes in the relationship that you can to delete:
```
MATCH (:Semester)-[r:HAS_MODULE]-(Module) 
DELETE r
```
You can be even more specific. If you like, you can delete nodes with specific properties. To do so:
```
MATCH (:Semester {semester: '6'})-[r:HAS_MODULE]-(Module {module: 'Mobile Apps'}) 
DELETE r
```
This will delete the relationship between the Semester and Mobile Apps module only.

---
### References
[Why Graph Databases](https://neo4j.com/why-graph-databases/)

[Documentation](http://neo4j.com/docs/developer-manual/current/)

[GMIT Timetable](https://timetable.gmit.ie/)

[How to Delete Relationships](http://www.quackit.com/neo4j/tutorial/neo4j_delete_a_relationship_using_cypher.cfm)

[Delete Nodes using Cypher](http://www.quackit.com/neo4j/tutorial/neo4j_delete_a_node_using_cypher.cfm)
