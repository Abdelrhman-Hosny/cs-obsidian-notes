## Reference Information

- https://www.youtube.com/watch?v=LDh5MdR-CPQ
- https://www.llamaindex.ai/blog/introducing-the-property-graph-index-a-powerful-new-way-to-build-knowledge-graphs-with-llms

---
## Advanced Rag with Knowledge Graphs (Property Graphs)

Property graphs are graphs where nodes and edges of the graph have properties.
They also allow nodes and edges to have labels (i.e. types)
![[property-graphs-1.png]]
Instead of having the normal: Company -> HAS_CEO -> Amy Peters

---
## Property Graph Pipeline
![[property-graphs-pipeline.png]]

---
## Property Graph Construction
### ImplicitPathExtractor

![[Implicit Path Extractor]]

---
### SimpleLLMPathExtractor

![[Llama Index - Property Graph 2024-06-28 10.18.07.excalidraw]]

Uses an LLM to generate a graph representation of the document.
This extracts

Using Llama index implementation gives all nodes the same labels, but the step itself can be customized.

---
### SchemaLLMPathExtractor

![[SchemaLLMPathExtractor]]


---
### Entity Disambiguation

This is often an overlooked step and no official modules support that yet in Llama index as of 2024-06-28

---
## Property Graph Retrievers

- `LLMSynonymRetriever`: Text Matching
- `VectorContextRetriever`: Embeddings
- `Text2Cypher`: Given the user query and the schema of the graph, generate a Cypher (SQL equivalent for graphs) in order to retrieve relevant nodes.
---



