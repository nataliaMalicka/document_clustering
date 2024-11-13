# document_clustering

Explored data clustering techniques including:

**WordCloud:**

**KMeans** clusters tuned using Silhouette and distortion scores to find optimal number of clusters. 
For KMeans, I tested the elbow graph method and sampled using two different metrics, silhouette and distortion. Silhouette recommended k=2 clusters while distortion recommended k=8 clusters. There was no relationship at 2, so the model I chose was 8 clusters. 

**DBScan** clusters tuned using k-Nearest Neighbours to find optimal clusters. 
DBScan was a challenge to tune. I used kNN as it is a common metric to tune DBScan. The first sharp increase is at around 0.4 distance according to the graph. I tried different min samples, and 0.4 was the trend for all. Next, I tried different values of min samples until I got a lower amount of clusters at 11.  

**Hierarchical** clusters tuned in combination of visual dendrogram and fcluster method. 
I pruned the dendrogram to the lastp = 12 clusters, which cut off clusters at the top 12 branches. I tried different values of fcluster, and tried to tune it to around the top branches of the dendrogram. By inspection, the value of 1.12 gave 21 clusters that seem more cohesive in semantics than a lower number of clusters. 

