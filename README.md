# NLP - Document Clustering and Topic Modeling
*Completed as part of a course in Applied Machine Learning.

Analyzing 14,162 rows of text at first glance – the sheer volume of text can quickly become a tangled mess or a word soup; and can be challenging to navigate. Luckily there are a plethora of tools at our disposal created by seasoned data scientists and linguists – DBScan, KMeans, NearestNeighbours, SentenceTransformer, and Hierarchical Clustering.

**Initial EDA – Exploratory Data Analysis**

Kaggle’s Food.com recipe data set has a text corpus or body which was the ‘word soup’ de jour in which text ranged from the shortest title “atole” to the longest “baked brie stuffed with strawberry preserves and toasted almonds”. Using a WordCloud (seen below) for preliminary data exploration, it is apparent that the most common recipe words were “chicken”, “cake”, and ‘cookie’, so it is expected that some recipes would be centered around this. I used these aforementioned libraries to analyze Kaggle’s Food.com recipes corpus by topic modeling, or in other terms extracting general topics from the text data; in this case, titles. 

**WordCloud:**

![image](https://github.com/user-attachments/assets/cbd7f553-8b9a-4c9a-b720-f74dd2fccd7c)

**Sentence Embedding**

The most common method to compare words to each other is in numerical terms with similar numerical output analogous to similar sentence meaning. I used the "paraphrase-distilroberta-base-v1" model from Hugging Face’s SentenceTransformer library which maps sentences and paragraphs to a 768 dimensional dense vector space which transforms words to numerical representations which is then used for later processing in various NLP models. 

**K-Means Clustering** 

The first NLP model I used to process the numerical word representation or sentence embedding, divided the text data into evenly spaced ‘clusters’ or groups. I found that the model, KMeans Clustering, created 8 clusters with themes such as location-based (pork chops Madrid, vodka Gilligan island), salad, cake, chicken, cookies, etc. Clearly, KMeans is a good generalizer, with a lot of these themes also popping up in the WordCloud, which logically follows due to its average distance algorithm. The algorithm is based on average center points in order to derive major themes. The number of center points was decided beforehand (explained later under tuning). The model assigns each recipe title to the nearest center point based on the previously created numerical vector representations. The model iteratively adjusts these center points until 8 stable themes are found. See some examples below.

![image](https://github.com/user-attachments/assets/205ea46d-78c0-4916-a31a-2f2532b58144)

**K-Means Clustering – Hyperparameter Tuning**

I tuned the KMeans hyperparameters using Silhouette and distortion scores to find the optimal number of clusters. A Silhouette Score measures the cohesion of clusters by calculating average distance to other points both within its cluster and its next nearest cluster. The highest score was at k=2 clusters which was not fruitful. In hindsight, using Silhouette Score to optimize the number of clusters was not an ideal method because of the high dimensionality (768 features) of the embedded sentences. 

Distortion scores measure total squared distance between each point its closest cluster center, which finds tight clusters. The distortion score elbow plot below recommended k=8 clusters which did provide fruitful results. The key sweet spot is minizing the distortion while not creating too many clusters. Inherently, the distortion score makes more sense in this context for hyper parameter tuning of number of clusters due to it analyzing cluster denseness rather than cluster distance, which is more applicable to higher dimensionality/number of features. 

![image](https://github.com/user-attachments/assets/9dffa0f7-87f6-49de-91ec-c45536e07801)


**DBScan** 

Using a model based on the density of clusters rather than simply average distance between points creates very tight clusters with nuanced topics that other generalist models may not find. DBScan clustering uses two parameters for tuning, a defined radius (epsilon) and minimum samples per cluster. Since DBScan is more challenging to tune due to finding the best combination of these two parameters, a common metric, k-Nearest Neighbours, kNN, is used to define the best combination. I used this method to find the optimal number of clusters (see graph). The first sharp increase is at around 0.4 distance according to the graph and this holds true for different minimum samples. Next, I tried different values for minimum samples until I got a lower number of clusters at 11. The 11 clusters in DBScan had specific themes of dip, bourguignon, sidecar cocktails, weight watchers, etc (see below).

![image](https://github.com/user-attachments/assets/62141670-073e-4db3-badf-91a78b1f6fea)

![image](https://github.com/user-attachments/assets/ea9aa72a-f6e2-49dc-80c6-23c24ca18a90)

**Hierarchical Clustering** 

The final method used, Hierarchical clustering, was by far the most creative and entertaining to read. The clusters were very semantically nuanced and developed with themes of tart (dessert tarts, lemon flavoured items (semantically tart as well)), fantasy (dragons, unicorn, devil), alcoholic shots and shooters, location-based (Russian, Brussels, commonwealth, French Canadian), etc. (see example below).

![image](https://github.com/user-attachments/assets/8901b2dd-f749-446c-991d-2e0286008ce8)

Hierarchical clustering iteratively merges the most similar topics first and builds to create more complex groupings using those initial topics. The dendrogram or tree graph below displays the groupings that were ‘pruned’ or limited to the 12 most complex clusters.

![image](https://github.com/user-attachments/assets/a94c207e-16a7-48fe-891b-d44b59cb4c52)

To compare and contrast, the bigger picture indicates that KMeans is good for general/average patterns, DBScan is good for finding very specific and nuanced, tight themes, while Hierarchical Clustering is good for finding cohesive semantic meaning. It is the most rich and developed model which is expected given that it has the most robust methods of finding clusters (iteratively, and using vector direction).

**Caveats**

Cultural context may also be lost with this method of sentence embedding. All curries for instance, may be placed into one pile, while losing the nuance that they are part of different cuisines and are different recipes altogether.

The original Kaggle data set size was much larger than the present data set, which may create unintentional sampling bias. The size was reduced to keep computation time reasonably short. 

By clustering the data based on recipe name only, some nuance is lost. A more accurate, albeit more computationally expensive, method would be to also include ingredients or recipe body text to group the recipes in a more robust and semantically coherent way. 


