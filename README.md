# document_clustering

Performed exploratory data analysis, and implemented and evaluated various data clustering techniques, including:
Completed as part of a course in Applied Machine Learning.

**WordCloud:**

![image](https://github.com/user-attachments/assets/cbd7f553-8b9a-4c9a-b720-f74dd2fccd7c)

**KMeans** clusters tuned using Silhouette and distortion scores to find optimal number of clusters. 
For KMeans, I tested the elbow graph method and sampled using two different metrics, silhouette and distortion. Silhouette recommended k=2 clusters while distortion recommended k=8 clusters. There was no relationship at 2, so the model I chose was 8 clusters. 

![image](https://github.com/user-attachments/assets/9dffa0f7-87f6-49de-91ec-c45536e07801)

![image](https://github.com/user-attachments/assets/205ea46d-78c0-4916-a31a-2f2532b58144)

**DBScan** clusters tuned using k-Nearest Neighbours to find optimal clusters. 
DBScan was a challenge to tune. I used kNN as it is a common metric to tune DBScan. The first sharp increase is at around 0.4 distance according to the graph. I tried different min samples, and 0.4 was the trend for all. Next, I tried different values of min samples until I got a lower amount of clusters at 11.  

![image](https://github.com/user-attachments/assets/62141670-073e-4db3-badf-91a78b1f6fea)

![image](https://github.com/user-attachments/assets/ea9aa72a-f6e2-49dc-80c6-23c24ca18a90)

**Hierarchical** clusters tuned in combination of visual dendrogram and fcluster method. 
I pruned the dendrogram to the lastp = 12 clusters, which cut off clusters at the top 12 branches. I tried different values of fcluster, and tried to tune it to around the top branches of the dendrogram. By inspection, the value of 1.12 gave 21 clusters that seem more cohesive in semantics than a lower number of clusters. 

![image](https://github.com/user-attachments/assets/a94c207e-16a7-48fe-891b-d44b59cb4c52)

Hierarchical clustering was by far the most creative and entertaining to read. The clusters were very semantically nuanced and developed. To compare and contrast, the bigger picture indicates that KMeans is good for general/average patters, DBScan is good for finding very specific and nuanced, tight themes, while Hierarchical Clustering is good for finding cohesive semantic meaning. It is the most rich and developed model which is expected given that it has the most robust methods of finding clusters (iteratively, and using vector direction).

