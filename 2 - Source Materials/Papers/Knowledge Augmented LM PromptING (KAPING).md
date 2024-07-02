## Reference Information

- https://aclanthology.org/2023.nlrse-1.7/
Code Implementation: https://github.com/jasmine95dn/kaping_prompt_zero-shot (not sure if official or not)
---
## Knowledge Augmented LM PromptING (KAPING)

## Problems with Graph RAG
- Most retrieved triplets are unrelated with the answer to the given user query
- 27% samples for the `WebQSP` dataset have more than 1k triplets
- This paper proposes filtering out irrelevant triplets using semantic search

---
## Implementation Details
### Entity Linking
- Paper uses reFind for entity linking
### Knowledge (Triplet) Verbalization
- It was found that **linear verbalization** worked well in prompting
- linear verbalization: concatenate subject, relation object texts in the triple 
```
(Lady Susan, written by, Jane Austen) is used as is: "(Lady Susan,
written by, Jane Austen)", for an LLM’s input.
```
### # of triplets

K = 10 was found to be the best value
and hops = 1
When we increase the hops for the relations, a lot of garbage was returned
### Model used for semantic search

MPNet was used
## Pipeline

1. Find the entities inside the user query
2. For the linked entities, perform semantic similarity between the **triplets of linked entities** and the **user query**
---
## Findings
### Effect of incorrect retrieval
 
When retrieved **triples contain answer entities**, performances of LLMs are **significantly improved,** compared to models without knowledge augmentation. 

However, when retrievers **fail**, performances are **lower than models of no knowledge augmentation**. 

These results suggest, when relevant knowledge is augmented, LLMs can contextualize and generate answers accurately. 

****
### Varying amount of triplets
most LLMs reach the somewhat **highest performance,** when the number of triples is **5 or 10.**
![[k-value-results.png]]

---
### Order of retrieved triples
It was shown that the order of triplets in the prompt is irrelevant and doesn't affect quality.
****
### Effectiveness of entity linking

Authors tried two different approaches, one using entity linking and the other was manually adding the correct entity.

![[entity-linking-effect.png]]

---
## Limitations

- Highly dependent on Entity Linking
- Doesn't do well on multi-hop questions (different from multi document question)
- If the answer is not included in the knowledge base, performance is worse.

---
