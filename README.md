## Learning Equality - Curriculum Recommendations

### Final Score
- Private Score: 0.52985
- Rank: 121/1070, 11.3%
___
### Competition Summary

- To streamline the process of matching educational content to specific topics in a curriculum with in diverse languages, and cover a wide range of topics, particularly in STEM (Science, Technology, Engineering, and Mathematics).

___
## Strategy
#### **[Four Stage Architecture]**  

![Modeling Overview](/assets/modeling_overview.png)

**Stage 1. Extract Embedding by RetrieverLM**  
* **RetrieverLM**: sentence-transformers/paraphrase-multilingual-mpnet-base-v2  
* **Make Embedding:**  
    - topic embedding(**max_len==192**): title + desc + context(topic_tree)  
    - content Embedding(**max_len==64**): title + desc  
    - Multiple Negative Ranking Loss  

**Stage 2. Make Candidate for Re-RankerLM**  
* **Cluster topic & content by KNN**  
    - Cluster each topic & **Top 50** contents  
    - **Metric: Cosine Similarity**    

**Stage 3. Re-Rank Candidate**  
* **Re-RankerLM:** Fine-Tuned by Stage 1  
* **Input(max_len==256):** topic_embedding + [SEP] + content_embedding
* Loss: BCELoss  
* CV Score: F2-Score 


**Stage 4. Inference with Post-Processing**  
* **Blank** for topics that have **has_content==False**  
* Add Feature: **topic title==content title**  
* Remove result that has **different language** information to topic  

___
# 참고자료  
##### https://www.kaggle.com/competitions/learning-equality-curriculum-recommendations/overview
##### https://arxiv.org/abs/1908.10084
___