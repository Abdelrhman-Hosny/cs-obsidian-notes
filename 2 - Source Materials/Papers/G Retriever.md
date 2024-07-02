## Reference Information
- https://arxiv.org/abs/2402.07630
- Code Implementation: https://github.com/XiaoxinHe/G-Retriever
---
## G Retriever

## Pipeline
![[step-1-and-2.png]]
### Indexing
- The paper skips how the graph it self is extracted from the documents (NER and Relation Extraction)
- After that step is done, we generate an embedding for each node and an embedding for each edge and store that inside a vector store.
### Retrieval
- Vector similarity between user query and nodes
- Vector similarity between user query and edges
We end up with $k$ nodes and $k$ edges

![[step-3-and-4.png]]

### Subgraph Construction

Uses an algorithm called **Prize-Collecting Steiner Tree**.

This uses the scores from the nodes and the edges to create a sub graph.

This approach removes irrelevant nodes and edges.

### Generation

Uses sub graph generated in step 3, and generates a graph embedding for it using a GNN.

feed subgraph embedding along with subgraph text into the LLM to generate answer.

N.B. The GNN is fine tuned not used as is, unlike the LLM.

```
query = ".."

query_embedding = sentence_tranformer(query)

retrieved_nodes, retrieved_edges = vector_store.search(query_embedding)

subgraph = prize_collecting_steiner_tree(retrieved_nodes, retrieved_edges)

subgraph_embedding = GNN(subgraph)
subgraph_embedding = projection_layer(subgraph_embedding)

subgraph_text = graph_to_text(subgraph)

final_query = generate_prompt(query, subgraph_text)

answer = LLM(final_query, subgraph_embedding)
```

---

## Limitations by Authors

Currently, G-Retriever employs a static retrieval component. Future developments could investigate more sophisticated RAG where the retrieval is trainable.

---
## Comments by Authors

![[13b-vs-7b.png]]

Better LLMs affect the performance significantly.