# WNED - Walking Named Entity Disambiguation

WNED is an entity linking systems that accepts text with marked named entities and links them to their referent entities in a knowledge base (e.g. Wikipedia).

## Building
* mvn package

## System Setup
* export environment:
  export CP=target/wned-1.0-jar-with-dependencies.jar:lib/*:.

### Alias Dictionary
* Generate the alias dictionary
  * Generate the files (aliasOut.txt, redirectOut.txt, nameIDOut.txt).
    - java -cp $CP ca.ualberta.entitylinking.kb extract path-to-english-wikipedia-dump
  * Resolve the redirect
    - java -cp $CP ca.ualberta.entitylinking.kb redirect aliasOut.txt, redirectOut.txt
* Build the lucene index for alias.
  * java -cp $CP ca.ualberta.entitylinking.common.indexing.AliasLuceneIndex aliasOut.txt.new

### Index the Wikipedia articles
* Index using Lucene - for computing the context similarity between mention and candidates using tf-idf
  * java -cp $CP ca.ualberta.entitylinking.common.indexing.WikipediaIndex index_directory path-to-english-wikipedia-dump
  
### Graph generation
* Generate the directed and undirected graph files
    * java -cp $CP ca.ualberta.entitylinking.graph.extraction.WikiGraphExtractor extract path-to-english-wikipedia-dump
* Aggregate graph edges and generate edge weights
    * java -cp $CP ca.ualberta.entitylinking.graph.extraction.WikiGraphExtractor aggregate graph-file
* Store the graph using the WebGraph (need to use the graphfile.new).
    * DirectedGraph: java -cp $CP ca.ualberta.entitylinking.graph.DirectedGraph path-to-graph-directory directed-graph-file
    * UndirectedGraph: java -cp $CP ca.ualberta.entitylinking.graph.UndirectedGraph path-to-graph-directory undirected-graph-file

## Running
* Move all data files (alias index, graph files, etc.) under a common root directory.
* Update the el.config with the root directory.
* ./run.sh

## Input
See the benchmark dataset for example: http://dx.doi.org/10.7939/DVN/10968 

## Citing
If you use this code in your reseasrch, please acknowledge that by citing:

    @inproceedings{DBLP:conf/cikm/GuoB14,
      author    = {Zhaochen Guo and
                   Denilson Barbosa},
      title     = {Robust Entity Linking via Random Walks},
      booktitle = {Proceedings of the 23rd {ACM} International Conference on Conference
                   on Information and Knowledge Management, {CIKM} 2014, Shanghai, China,
                   November 3-7, 2014},
      pages     = {499--508},
      year      = {2014},
      url       = {http://doi.acm.org/10.1145/2661829.2661887},
      doi       = {10.1145/2661829.2661887}
    }
    
## People
* Zhaochen Guo
* [Denilson Barbosa](http://webdocs.cs.ualberta.ca/~denilson/)

## Acknowledgements
This work was funded by the NSERC Business Intelligence Network (BIN) and Alberta Innovates Technology Futures (AITF)
