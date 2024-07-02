## Reference Information
- https://arxiv.org/abs/2404.16130
Code Implementation: [aka.ms/graphrag](aka.ms/graphrag) says to be released "soon"
---
## From Local to Global- Graph Rag for Query-Focused Summarization

## TL;DR

One of the short comings of traditional RAG systems is that it **fails** on global questions directed at an **entire text corpus**. (e.g. What are the main themes in the dataset ?)

This question could be solved by the QFS task, but that fails to scale when the number of text ingested increases (can't handle as much text as RAG)

This paper aims to have a QFS system that uses Graph RAG in order to answer these types of questions.

### System Workflow

1. **Build Graph-Based Text Index**: 
	- Derive an **entity knowledge graph** from a group of documents
	- **pre-generate** community summaries for all groups of closely-related entities.

2. **Generate Final Response:**
	- Generate partial responses from each community.
	- Summarize all partial responses into a final answer.

### Key Difference

While other approaches utilize the **structured retrieval** and **graph traversal**, This approach focuses more on the **modularity**
i.e. using **community detection algorithms** to partition graphs into communities of **closely related nodes**.

---
## Pipeline
![[local-to-global-graph-rag-construction-pipeline.png]]

### Source Documents -> Text Chunks
The decision to be made here is about Chunk Size.
![[local-to-global-graph-rag-chunk-size.png]]


| **Larger Chunk Size** | **Smaller Chunk Size**                                                  |
| --------------------- | ----------------------------------------------------------------------- |
| Fewer LLM calls       | Better recall due to smaller size of text and context window limitation |

---
### Text Chunks -> Element Instances

1. **Entity and Relation Extraction**:
   - **LLM as NER + Relation Extractor**: Uses a large language model (LLM) to identify entities and relationships between them.
   - **Relationships Format**: Extracted in the form of `(srcEntity, targetEntity, relationDescription)`.

2. **Claim Extraction**:
   - **Secondary Prompt**: A separate prompt to extract claims.
   - **Claims Format**: Includes `(subject, object, type, description, source text span, start date, end date)`.

3. **Gleanings**:
   - **Entity Verification**: After initial extraction, the LLM is prompted to check if any entities were missed.
   - **Retry Mechanism**: If more entities are detected, the LLM is asked to extract them again.
---
### Element Instances -> Element Summaries

Use LLM to summarize the element instance and represent each element with a summary.

---
### Element Summaries -> Graph Communities

1. **Graph Creation**:
    
    - **Undirected, Weighted Graph**: Use extracted data to create a graph where nodes represent entities and edges represent relationships.
    - **Edge Weight**: Represents the normalized count of detected relationship instances.
2. **Community Detection**:
    
    - **Leiden Algorithm**: Chosen for its efficiency in recovering hierarchical community structures in large-scale graphs.
3. **Hierarchical Community Structure**:
    
    - **Community Partitioning**: Each level of the hierarchy provides a partition that is mutually exclusive and collectively exhaustive.
    - **Divide-and-Conquer Summarization**: This structure enables the system to perform global summarization by breaking down the graph into smaller, manageable communities.
#### Community Detection (Leiden's Algorithm)
![[From Local to Global - Graph Rag for Query-Focused Summarization 2024-06-28 17.38.23.excalidraw]]
![[From Local to Global - Graph Rag for Query-Focused Summarization 2024-06-28 17.43.16.excalidraw]]

---
### Graph Communities -> Community Summaries

Generate a summary for each community derived from the graph.

#### Summary Generation Strategy

##### Leaf Level Communities

These are the smallest communities that have not yet been summarized. The process involves:

1. **Loop on Edges**: Start with edges connected to the highest degree nodes.
2. **Add Information**:
    - Source Node - Target Node - Linked Claims (if extracted) - The Edge itself
3. **Token Limit**: Continue until the LLM's token limit is reached.
##### Higher-Level Communities

1. **Fit Within Token Limit**:
    - If possible, include all element summaries as with leaf nodes.
2. **Space-Saving Strategy**:
    - **Ranking Sub-Communities**: Rank based on total number of tokens (shorter summaries rank higher).
    - **Iterative Replacement**: Replace individual element summaries with shorter summaries of their corresponding sub-communities to ensure the overall summary fits within the context window.

---
### Community Summaries -> Community Answers -> Global Answer

1. **Intermediate Answer Generation**:
    
    - Use each community summary to generate an intermediate answer for the user's query.
2. **Score the Answers**:
    
    - Query the LLM to score each intermediate answer on a scale from 0 to 100.
    - Discard answers with a score of 0.
3. **Select Top Answers**:
    
    - Sort the remaining answers in descending order by score.
    - Add the top answers until the token limit is reached.
4. **Generate Final Answer**:
    
    - Use the selected top answers as context to generate the final answer to the user's query.

---

## Questions Asked

News articles
Example activity framing and generation of global sensemaking questions
User: A tech journalist looking for insights and trends in the tech industry
Task: Understanding how tech leaders view the role of policy and regulation
Questions:
1. Which episodes deal primarily with tech policy and government regulation?
2. How do guests perceive the impact of privacy laws on technology development?
3. Do any guests discuss the balance between innovation and ethical considerations?
4. What are the suggested changes to current policies mentioned by the guests?
5. Are collaborations between tech companies and governments discussed and how?
Dataset Podcast transcripts
User: Educator incorporating current affairs into curricula
Task: Teaching about health and wellness
Questions:
1. What current topics in health can be integrated into health education curricula?
2. How do news articles address the concepts of preventive medicine and wellness?
3. Are there examples of health articles that contradict each other, and if so, why?
4. What insights can be gleaned about public health priorities based on news coverage?
5. How can educators use the dataset to highlight the importance of health literacy?

---
## Results
Paper states that the community generated answers provided a small but consistent increase in score.

Communities were separated into 4 community hierarchies (C0 -> C3)
![[communities.png]]
C0 has the lowest number of communities at 34 in podcast dataset.

Paper states that the increase from going from C0 -> C1 (i.e. using more communities by going to a lower hierarchy) is not much but it is still better to use that than naive rag.
So they advise to use C0 as the number of requests needed will be much less.
C0 -> 34 Communities
C1 -> 367 Communities.

---
### Cost
![[cost-of-global-graph-rag.png]]

|           | RAG  | Graph RAG |
| --------- | ---- | --------- |
| # Tokens  | 4707 | 48606     |
| Time      | 8 s  | 71 s      |
| LLM Calls | 1    | 10        |
|           |      |           |
Source: https://www.youtube.com/watch?v=r09tJfON6kE

---
## Insights

- **Cloud-Based Dependency**: This approach is best suited for cloud-based vendors that operate on token-based pricing models due to intensive parallel processing during answer generation. For instance, generating 100 requests on GPT-4 may take the same time as handling 1 request.
    
- **Global Summarization Strength**: Particularly effective at providing global summaries of datasets, a task where many other models struggle to provide comprehensive answers.
    
- **Scaling Challenges**: Scaling becomes challenging with an increasing number of documents due to the growth of community hierarchies, which adds complexity and computational overhead.
    
- **Trade-off Between Global and Local Information**: While enabling global answers, this approach sacrifices some individual information at the local level, depending on the chosen hierarchy level.

