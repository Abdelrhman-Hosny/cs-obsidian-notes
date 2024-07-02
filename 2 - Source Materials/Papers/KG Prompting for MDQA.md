## Reference Information
- https://arxiv.org/html/2308.11730v3
Code Implementation: https://github.com/YuWVandy/KG-LLM-MDQA.

---
## KG Prompting for MDQA

Paper starts of showing questions where you **must have** graph RAG

![[questions-need-graph-1.png]]
Need to know who is the creator of the arrangement and then go to his info page to get the date in which he was born.


![[questions-need-graph-2.png]]
Question requires knowledge from two documents


![[questions-need-graph-3.png]]
Question requires knowing the structure of the document to retrieve the parts of the document asked about.

---
## Paper Proposal

The paper offers a **KG Construction** method, and a **KG Traversal Agent**

---
### KG Construction
![[construction.png]]
Nodes in this graph are:
1. Passages
2. Tables (Treated as markdowns, as LLMs can understand this format)
3. Pages
Edges represent either a:
- Logical relation
	- Common Keywords
	- Passage Semantic Similarity
		MDR is used, this is a strategy used for dealing with multiple passages)
	- Common Entity across passages (Can use Entity Linking for this)
- Structural relation
	- Passage contains table
	- Page contains passage
	N.B. You can consider adding other structural elements as needed.

****
## Graph Traversal Agent
Agent first checks if it is a logical or structural relation needed to answer the question.
![[traversal-algorithm.png]]
![[similarity-equation-graph-traversal.png]]
****
## Findings
- KNN-MDR achieves better performance than KNN-ST when the density of the two constructed KGs is the same.
- Using T5 was better than LLaMa-7b, authors think that this is because T5 is easier to finetune.
---
## Implementation Details
![[construction-methods-table.png]]

---
This paper has a lot of details, and needs to be read as it can't all be summarized.





