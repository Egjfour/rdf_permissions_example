# rdf_permissions_example
Using RDF, ontologies, and SPARQL to create an example of defining and displaying permissions in example "application" records

A small example of the RDF data structure and expected inference can be seen using the `data/example_data.ttl` file and `vocab/example_ontology.ttl` file

## Project Structure
### /data
- Contains the csv files (both raw and generated from the data prep notebook)
- When launching corese server, the `-v` flag points to the `data/server` folder which has a manually copied version of `data/graph_data.rdf`
### /src
- Contains the notebooks for processing and querying the data
    - `1-data_prep.ipynb`: Takes the orginal transaction dataset from Kaggle and reformats it for this use case as well as creates randomly generated sales reps and managers
    - `2-graph_setup.ipynb`: Applies the `morph_kgc` library to implement the CSV-LD protocol and generate RDF from the 3 CSV files created in the previous notebook
        - Also adds the ontology file to this graph
    - `3-query_graph-*.ipynb`: Different tests of implementations of applying OWL and RDFS reasoning to query results from Python
        - NOTE: currently none of these pass the test cases. `corese_python` and `kglab` take unacceptable amounts of time to process OWL rules and I have so far been unsuccessful getting `corese_server` to apply ontological inference
        - `corese_server` requires running the a [Corese server](https://hub.docker.com/r/wimmics/corese) locally at port `8080` with a volume mount of `data/server` which contains the output from notebook 2
### /vocab
- Contains metadata files for each of the three CSVs to load to express the mapping logic
- Additionally contains a separate ontology file to use for reasonining
    - The ontology MUST be loaded separately into RDFLib to be captured in the final graph from `/src/2-graph_setup.ipynb`

## TO DO
- Get OWL reasonining working for Corese server
- Setup a triple store using [Apache Jena](https://jena.apache.org/documentation/tdb/index.html) and use that as a [persistent data storage solution](https://github.com/Wimmics/corese/blob/master/docs/storage/Configuring%20and%20Connecting%20to%20Different%20Storage%20Systems%20in%20Corese.md) for the Corese server
- Add more complex rules such as negation to showcase more functionality
- Enhance the ontology to make the querying simpler and more efficient at run time
