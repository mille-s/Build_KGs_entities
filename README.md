# Build_KGs_entities

The [DBpedia_dataset.ipynb](https://github.com/mille-s/Build_KGs_entities/blob/e3c0239909daeca78f37b8e8b950a4cf7174d2ea/DBpedia_dataset.ipynb) notebook contains code to produce Knowledge Graphs from DBpedia or Wikidata; it comprises the following sections:
- **Preliminary work: get info to build datasets**: a series of cells to get information from DBpedia, such as which properties are currently used, what are the specifications of the Subject (Domain) and Object (Range) of the properties, what kind of hypernyms are used, etc.
- **Build dataset**
  - **Get properties for list of entities**: given an input dictionary with category names (urls or name) as keys, and a list of entities (urls or names) as value, query DBpedia or Wikidata for each entity to get triples for which properties are covered in WebNLG. You can search for the entity as Subject of the property (e.g. country(Entity, Spain), or as object of the property (e.g. country(Barcelona, Entity), or for both cases. The process is time-consuming, so you can save the resulting data in a pickle file for later use.
  - **Build knowledge graphs #1: WebNLG format/distribution**: given a dictionary as produced by **Get properties for list of entities** described above (i.e. a dico_1 with category names as keys and a dico_2 as value, and where dico_2 should have entity_name as key, and a list of Triple Objects as value) and a list of triple configurations extracted from the WebNLG dataset, creates triple sets in separate XML files where for each entity, a set of triples that matches a WebNLG configuration is collected. For example, if in WebNLG an input of size 3 about an entity of class Person has been found that has the properties "birthPlace", "office" and "spouse", for every entity of that class Person in the pickle file for which "birthPlace", "office", and "spouse" (possibly among others) were found on DBpedia (see **Get properties for list of entities** above), an XML file is created with this content. Note that an entity can have several triples with the same property (e.g. a person can have had several "office" positions), but only one XML is created with the first property found. This is to avoid combinatorial explosion for entities that have multiple triples with the same property (for one single triple configuration, it is easy to find entities with 2k+ possible different intantiations).
  - **Build knowledge graphs #2:** ...
  - **Build knowledge graphs #3: free distribution**: given a dictionary as produced by **Get properties for list of entities** described above (i.e. a dico_1 with category names as keys and a dico_2 as value, and where dico_2 should have entity_name as key, and a list of Triple Objects as value) and a maximum number N of instances of a property, creates triple sets in separate XML files where for each entity, a set of triples with at most N instances of each property is collected. For instance, is a Person entity has 10 different instances of the property "office", only 3 will be added to the XML, alongside other properties found for that entity.
  - **Process output XMLs (group, etc.)**: Contains so far a cell to bring all XML files together.
 
The [LongInput_D2T_25_EvalLLM_analysis.ipynb](https://github.com/mille-s/Build_KGs_entities/blob/e3c0239909daeca78f37b8e8b950a4cf7174d2ea/LongInput_D2T_25_EvalLLM_analysis.ipynb) notebook contains the code used to compute the results and analyses of the 2025 *Scaling Up Data-to-Text Generation to Longer Sequences* INLG paper.

## Parameters for INLG 2025 paper:

Get properties:
- input_json_path = '/content/Build_KGs_entities/resources/GREC_NE.json'#@param{type:"string"}
- triple_source = 'Ontology'
- props_list_path = os.path.join('/content', 'DCU_TCD_FORGe_WebNLG23', 'code', 'sorted_properties.txt')
- ignore_properties = ','.join(json.loads(open('/content/WikipediaPage_Generator/resources/list_props_to_filter.json', 'r').read()))
- get_triples_where_entity_is_subj = True
- get_triples_where_entity_is_obj = True
- triple_Validation = "False" (added after paper publication)

Build knowledge graphs:
- min_num_of_instances_of_prop_desired = "3" (added after paper publication)
- max_num_of_instances_of_prop_desired = "3"
- min_input_size = "8"
- max_input_size = "100"
- properties_that_can_happen_once_only = json.loads(open('/content/WikipediaPage_Generator/resources/list_props_that_can_happen_once_only.json', 'r').read())
- entities_for_dev = ['Abu_Dhabi', 'Accra', 'Islamabad', 'Lagos', 'Aneurin_Bevan', 'Anthony_Giddens', 'Antoine_Lavoisier', 'Antonio_Negri']

