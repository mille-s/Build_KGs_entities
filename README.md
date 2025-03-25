# Build_KGs_entities

This repository contains code to produce Knowledge Graphs from DBpedia or Wikidata.

The .ipynb notebook contains the following sections:
- **Preliminary work: get info to build datasets**: a series of cells to get information from DBpedia, such as which properties are currently used, what are the specifications of the Subject (Domain) and Object (Range) of the properties, what kind of hypernyms are used, etc.
- **Build dataset**
  - **Get properties for list of entities**: given an input dictionary with category names (urls or name) as keys, and a list of entities (urls or names) as value, query DBpedia or Wikidata for each entity to get triples for which properties are covered in WebNLG. You can search for the entity as Subject of the property (e.g. country(Entity, Spain), or as object of the property (e.g. country(Barcelona, Entity), or for both cases. The process is time-consuming, so you can save the resulting data in a pickle file for later use.
  - **Build knowledge graphs #1: WebNLG format/distribution**: given a dictionary as produced by **Get properties for list of entities** described above (i.e. a dico_1 with category names as keys and a dico_2 as value, and where dico_2 should have entity_name as key, and a list of Triple Objects as value) and a list of triple configurations extracted from the WebNLG dataset, creates triple sets in separate XML files where for each entity, a set of triples that matches a WebNLG configuration is collected. For example, if in WebNLG an input of size 3 about an entity of class Person has been found that has the properties "birthPlace", "office" and "spouse", for every entity of that class Person in the pickle file for which "birthPlace", "office", and "spouse" (possibly among others) were found on DBpedia (see **Get properties for list of entities** above), an XML file is created with this content. Note that an entity can have several triples with the same property (e.g. a person can have had several "office" positions), but only one XML is created with the first property found. This is to avoid combinatorial explosion for entities that have multiple triples with the same property (for one single triple configuration, it is easy to find entities with 2k+ possible different intantiations).
  - **Build knowledge graphs #2:** ...
  - **Build knowledge graphs #3: free distribution**: given a dictionary as produced by **Get properties for list of entities** described above (i.e. a dico_1 with category names as keys and a dico_2 as value, and where dico_2 should have entity_name as key, and a list of Triple Objects as value) and a maximum number N of instances of a property, creates triple sets in separate XML files where for each entity, a set of triples with at most N instances of each property is collected. For instance, is a Person entity has 10 different instances of the property "office", only 3 will be added to the XML, alongside other properties found for that entity.
  - **Process output XMLs (group, etc.)**: Contains so far a cell to bring all XML files together. 
