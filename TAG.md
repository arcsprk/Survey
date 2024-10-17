## Background

- We find that real business users’ questions often require sophisticated combinations of **domain knowledge**, **world knowledge**, **exact computation**, and **semantic reasoning**
- Database: a source of **domain knowledge** through the up-to-date data they store, as well as **exact computation** at scale (which LMs are bad at)
- LMs possess **semantic reasoning** capabilities as well as **world knowledge** (e.g., "what are the **QoQ trends** for the "retail" vertical?")
- Aim to combine language models' (LMs) reasoning capabilities with database systems' computational power

## Limitations of Existing Approaches

## Text2SQL

- Focuses only on questions expressible in relational algebra
- Covers only a subset of real-world user queries

## Retrieval-Augmented Generation (RAG)

- Limited to queries answerable by simple point lookups
- Not able to handle complex data manipulations

## Proposed Paradigm: TAG (Table-Augmented Generation)

![[Pasted image 20241018020615.png]]

- Comprehensive approach for answering natural language questions about databases
- Represents diverse interactions between LMs and databases
- Explores previously uncharted territories in AI-database integration

## Benchmark Development to study TAG problem

- The BIRD benchmark defines fundamental query types, including **match-based**, **comparison**, **ranking**, and **aggregation** queries. 
- Modify subset of BIRD benchmark and modify them to require either **world knowledge** or **semantic reasoning** for the model to answer
- Final benchmark consists of 80 modified queries, 
	- 40 requiring parametric knowledge
	- 40 requiring reasoning with 20 of each of the 4 chosen BIRD query types.
- 
#### Examples


### Evaluation of Standard Methods

- Tested existing approaches against the new benchmark

## Experimental Results

#### Evaluation metrics 

- accuracy: the percentage of exact matches as compared to the labeled correct answer for the match-based, comparison, and ranking query types
	- For aggregation queries, we provide qualitative analysis on results using each baseline
- execution time in seconds for each query

#### Experimental setup

- LM: Llama-3.1-70B-Instruct model for both Text2SQL and final output generation
- Database API: 
	- SQLite3 for baselines involving SQL
	- E5 base embedding model for RAG baseline
- Llama-3.1-70B-Instruct with vLLM on 8 A100 (80GB) GPUs

#### Baselines

1. **Text2SQL**
    - Language Model (LM) generates SQL code which is executed to obtain answers
    - Uses prompts containing table schema information
    - Evaluated by executing in SQLite3 and measuring accuracy
    
2. **Retrieval Augmented Generation (RAG)**
    - Uses row-level embeddings for table retrieval
    - Utilizes FAISS index to retrieve 10 relevant rows as context
    
3. **Retrieval + LM Rank**
    - Extends RAG by using an LM to rerank retrieved rows
    - Employs Llama-3.1-70B-Instruct model for reranking
    
4. **Text2SQL + LM**
    - Generates SQL to retrieve relevant rows, then uses these as context
    - Distinct from basic Text2SQL as it doesn't directly generate answer SQL
    
5. **Hand-written TAG**
    - Implements manual TAG pipelines using expert knowledge
    - Uses LOTUS API to combine relational and semantic operators
    - Leverages optimized semantic query execution engine

#### Performance of Existing Methods

- Standard methods accurately answered less than 20% of queries
- Highlights significant room for improvement




## Conclusion

- TAG presents a new paradigm for natural language database querying
- Potential for revolutionary changes in database query processing
- Open-sourced benchmark code to facilitate further research

## Impact and Implications

- Bridges the gap between AI and database research communities
- Potential applications in business intelligence, data analysis, and user-friendly database interfaces
- Challenges traditional database query optimization techniques

