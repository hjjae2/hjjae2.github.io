---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

# What is OSM?
The OSM is OpenStreetMap which creates and distributes free geographic data for the world.

# Elements
Elements are the basic components of OSM’s conceptual data model of the physical world.
There are three types of elements:
* nodes (defining points in space)
* ways (defining linear features and area boundaries)
* relations (defining how other elements work together)

# A Type Of Element: Node
A node represents a specific point on the earth’s surface defined by its latitude and longitude. Each node comprises at least (1)an id number and (2)a pair of coordinates.

Nodes
1. can be used to define standalone point features.
2. are used to define the shape of a way.
   A node
1. can be included as a member of a relation.

# A Type Of Element: Way
A way is an oredred list of between 1 and 2,000 nodes.

Ways
1. are used to represent linear features such as rivers and roads.
2. can represent the boundaries of areas such as buildings or forests.
    1. In this case, the way’s first and last node will be the same. This is called a “closed way”.
    2. Note that “closed ways” occasionally represent loops, such as roundabouts on highways, rather than solid areas. This is usually inferred from tags on the way.  However, some real-life objects can have both (1)a linear or an areal representation, and (2)the tag which can be used to avoid ambiguity or misinterpretation.

# A Type Of Element: Relation
A relation is a multi-purpose data structure that documents a relationship between two or more data elements(nodes, ways, relations).

Examples include:
* A route relation, which lists the ways that form a major highway, a cycle route, or a bus route.
* A turn restriction that says you can’t turn from one way into another way.
* A multipolygon that describes an area(whose boundary is the ‘outer way’) with holes(the ‘inner ways’).

The relation
1. is an ordered list of nodes, ways, other relations.
2. ’s meaning is defined by its tags. A relation must have at least the ‘type’ tag.

# Tag
All types of data elements(nodes, ways, relations), as well as changesets, can have tags. Tags describe the meaning of the particular element to which they are attached.

A tag consists of two free-format text fields; a ‘key’ and a ‘value’.

# References
* https://wiki.openstreetmap.org/wiki/Elements
