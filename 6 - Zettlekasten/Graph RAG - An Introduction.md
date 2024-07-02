Tags: [[Graph RAG]] [[Artificial Intelligence]]
Status: #adult

---
## Graph RAG - An Introduction

### Why Graph RAG ?

- Graph RAG can answer questions that can't  be answered using Traditional RAG [[KG Prompting for MDQA]]

![[questions-need-graph-1.png]]
Need to know who is the creator of the arrangement and then go to his info page to get the date in which he was born.


![[questions-need-graph-2.png]]
Question requires knowledge from two documents


![[questions-need-graph-3.png]]
Question requires knowing the structure of the document to retrieve the parts of the document asked about.

- Traditional RAG doesn't utilize relationships between entities


---
## Graph Structure

<p style="font-size: 25px;font-weight: bold;">What will nodes and edges represent ?</p>

### Traditional Graph
Node: Entity
Edge: Relation
![[Llama Index - Property Graph 2024-06-28 10.18.07.excalidraw]]


### Passage Centered Graph

Node: Passage / Page / Table
Edge: Common Entity / Common Keyword / High Semantic Similarity Score

![[passage-doc-structure.png]]

### Community Graph (Microsoft Paper)

Graph has multiple levels.

Level 0: Nodes are entities and edges are relations
Level 1 -> N: 
Nodes of level `n` contains a sub graph from level `n-1`
The value of the node is the summary of the nodes from
the previous layer.
![[cluster-leiden.png]]
![[From Local to Global - Graph Rag for Query-Focused Summarization 2024-06-28 17.38.23.excalidraw]]
![[From Local to Global - Graph Rag for Query-Focused Summarization 2024-06-28 17.43.16.excalidraw]]

---
## Graph Retrieval

What is retrieved from each type ?

- Nodes, Edges with no attributes
- Entity Linking -> Filtered Relations [[Knowledge Augmented LM PromptING (KAPING)|KAPING]]
- Sub Graphs [[G Retriever]]
- Passages [[From Local to Global - Graph Rag for Query-Focused Summarization|MicrosoftGraphRAG]], [[KG Prompting for MDQA|MDQA]]
- Nodes, Edges with their attributes included [[Llama Index - Property Graph]]
---
## Graph Traversal

Research is starting to use LLMs to traverse graphs and check if the surrounding nodes can add useful context.
For the product use case it could be rule based at the beginning to avoid complexity.

![[Graph RAG - An Introduction 2024-07-01 11.23.51.excalidraw]]

---
## What about Neo4j and NebulaGraph ?

They only provide a store and good integration with LangChain and LlamaIndex.

But if we are going to have our own pipeline anyway, we don't necessarily have to use one of them.

---
## Why Graph RAG at Agolo ? (Product Support)
- Can utilize the nature of relations between products.
	- iPhone 15 has a standard version as well as 15 Plus, Pro, Pro Max.
	- Each one of those has different versions with different storage size.
	- This is not apple specific but that could apply to other products like laptops.

---
## Comments about EA
- Most versions start with an **Empty KG** which is something that we need to explore
- Semantic Search should get a higher priority, unless we plan to have different representations for the RAG and the Linking
- Linking the documents themselves, will be different from linking entities in the query.
- Should start using LLMs in Entity Extraction as documents won't necessarily be talking about POL
- Latency Constraints can be more relaxed, as linking in large amounts only once.
	- Linking could possibly be used to link entities in the user query.

