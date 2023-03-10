# Recommender systems for a fast fashion company

## Team members
* Francesco Vitale (267091)
* Andrea Stella (270861)

## 1) Introduction
We impersonate the Data Science team at a fast fashion company. 

The platform already has a recommender system. Our job is to make it better.

We will test different models and select the most suitable one.

## 2) Methods
Our dataset is composed by 3 files:

* **Customers**, contaning details about customers. With columns:
  + customer_id
  + fashion_news
  + club_member
  + age
  
* **Articles**, containing details about articles. With columns:
  + article_id
  + prod_name
  + product_type
  + product_type_name
  + product_group_name
  + colour_group
  + colour_group_name
  + perceived_colour_value
  + perceived_colour_value_name
  + perceived_colour_master
  + perceived_colour_master_name
  + department
  + department_name
  + index
  + index_name
  + index_group
  + index_group_name
  + section
  + section_name
  + garment_group
  + garment_group_name
  
* **Transactions**, contaning data about customer purchases. With columns:
  + t_dat
  + customer_id
  + article_id
  

### Data Collection and Preparation

The pyhton libraries we used in order to analyze data were: 
* Numpy
* Pandas
* Sklearn
* Matplotlib
* Seaborn.

We began to analyse the dataset about articles since it has a complex structure and has a lot of columns, altough most attributes have 'code' and 'name' descriptions so we didn't need all of them. We looked for NaN values and luckily we didn't find them, despite this through a deeper analysis on all the attributes we discovered that some columns had values like '-1', 'Unknown' and 'undefined'. After having calculated the percentage of elements with these values we decided to delete these articles from the dataset as they couldn't help us in building an accurate model.

Talking about the data collection and preparation of 'Customers' dataset we found out that column 'age' was composed by 133 NaN values.
Now, we couldn't remove rows where NaN appeared, since we would have lost some customer ids. Therefore, we chose to:
* First, segment customers based on their subscriptions to the additional services (Fashion newsletter, Club).
* And then, compute the mode, i.e. the value that appears most often in a set of data values, for each segment and substitute it to the NaN.
We chose the mode over the mean because the mean is more sensitive to outliers and skewed data, and, as we can see below, 75% of customers' age is under 46, while the maximum age is 91. Then we noticed that column 'age' was represented as float and we decided to change it as an integer to have a better visualization.

Finally with the 'Transactions' dataset we noticed that it was connected to the other 2 datasets through 'article_id' and 'customer_id' so we needed to change it deleting the transactions made on articles that presented 'Unknown' features, in order to be coherent with the work done before.



### Exploratory Data Analysis
We then performed an EDA on the 3 datasets to understand the company data, to see undergoing trends and to get useful insight.

Articles: we grouped articles according to their features starting from the division in types. Plotting the results we saw that they were divided in 80 types and the type with more articles in the dataset was 'Trousers'. 

<img width="750" alt="product_type_name.png" src="https://user-images.githubusercontent.com/117635995/211692818-c3f5c04f-37a4-49a4-bb3e-572f8e5b357e.png">

Afterwards we did the same for 'product_group_name' noticing that the plot was less caothic and articles were divided in 11 groups where the group with most articles was 'Garment Upper Body'. 

<img width="750" alt="product_group_name.png" src="https://user-images.githubusercontent.com/117635995/211693334-a22196b2-e508-418f-8025-10ca0e2c17a8.png">

Then we looked at the different attributes regarding colours, where Black seems to dominate, and we have chosen 'perceived_colour_master_name' as the feature to include in our first recommender system as it was able to represent data better than 'colour_group_name' and 'perceived_colour_value_name'.

<img width="750" alt="perceived_colour_master_name.png" src="https://user-images.githubusercontent.com/117635995/211693560-85231bbc-0061-4104-a23a-8bd4a00084f4.png">

We simply followed the same process for each feature of the dataset till garment_group_name.

<img width="750" alt="garment.png" src="https://user-images.githubusercontent.com/117635995/211693854-1845561f-9806-4e25-b794-7de400a600f6.png">

Finally we chose the columns to take in consideration to build our Content-Based Filtering Recommender System.

Customers: we first took in consideration the column 'age' and plot how the different ages was spread over customers. To get an hint about the demography of the customer base we plotted the distribution of our customers' age:

<img width="750" alt="Screenshot 2023-01-09 alle 23 00 56" src="https://user-images.githubusercontent.com/96107340/211417060-44dc1754-861c-40e3-b1a1-5be656d2929f.png">

Something that we thought could be useful was to assign to each of them an age group under which he/she falls. This would make our data more interpretable and less caothic and would totally helps us building the user-based recommender system. We proceeded in this way creating age ranges of more or less 10 years until we reach 65 (since there are few customers we simply assigned the group '65+'). Afterwards we add the new column age_group to the dataset. After we counted how many customers of that specified age group were present in the datase and plotted our results, noticing that the groups with the highest number of customers are '15-24' and '25-34', this meant lots of young customers respect to old ones.

<img width="750" alt="age_group.png" src="https://user-images.githubusercontent.com/117635995/211693689-5f107556-dfdd-46d3-baf1-7b1e998d17d4.png">

We then worked with 'fashion news' and 'club member' columns creating different dataframes to see the choices of customer acoording to their age group and discovered that in general the majority of customers tend to be not subscribed to fashion news and tend not to be part of special clubs. Also here we plotted the results.

<img width="750" alt="fashion news.png" src="https://user-images.githubusercontent.com/117635995/211693802-08a862cf-fd9f-4123-80fe-d302b8312cb5.png">

<img width="750" alt="club members.png" src="https://user-images.githubusercontent.com/117635995/211693759-354bf37d-13cd-48bd-94f6-f677e220231e.png">


Transactions: some analysis has been made over the column 'date' where we found out that 2020-09-09 was the day with more transactions, although this was just for curiosity. 

<img width="750" alt="date.png" src="https://user-images.githubusercontent.com/117635995/211693921-f310b994-3eb9-446e-853f-08fe7a0bad8c.png">

These instead were some statistics from our dataset:

+ Number of transactions: 357535
+ Number of unique articles: 6328
+ Number of unique customers: 41238
+ Average number of transactions per customer: 8.67
+ Average number of transactions per article: 56.5

We created different dataframes useful for our model, like 'customer_transactions', 'customer' which stated the number of transactions made by each customer or also 'articles' that stated the number of transactions for each article. Concluding our EDA we plotted our results respect to the dataset 'new' to understand between age groups how transaction groups are spread.

<img width="750" alt="transactions per age_group.png" src="https://user-images.githubusercontent.com/117635995/211694126-40b5308e-d148-4361-a2d2-6f131383fc9b.png">




### RECOMMENDER SYSTEMS

In order to build a recommender system, three different recommendation systems were tested: 
* A Content-Based recommender system
* A Collaborative filtering recommender system
* A Neural Network-based content filtering system.

### Content-Based
The **Content-Based** recommender system uses the product information from the Articles dataset to make recommendations. 
We had to perform only EDA on the dataset of articles to built this model because we only needed to decide the features to take in consideration for our system. Finally we chose 'product_type_name', 'product_group_name', 'perceived_colour_master_name' and 'garment_group_name'. In order to build it we had to set up our dataset so that rows represent articles and columns represent the features we're interested in. To do that we had to manipulate these columns so that each of them was represented as a binary feature, so we create a one hot encoding for them. We obtained 4 tables were ID of articles represented rows while features represented columns, '1' means that article falls under that feature, '0' doesn't. After that we have merged all these features in a unique dataframe 'article_features'.

We used a similarity metric called cosine similarity to build our recommender system. Cosine similarity looks at the cosine angle between two vectors, the smaller the cosine angle, the higher the degree of similarity. The cosine similarity matrix has shape n_articles x n_articles (so in this case 6328 x 6328). With this cosine similarity matrix, we'll be able to extract articles that are most similar to the article of interest.

We used dictionaries to make our system complete, in this way was able to give information on articles that were recommended.

Then we have created a function that takes as input the article id and the number of similar items we desire to recommend. We take the row of the similarity matrix associated to our article and we sort the elements in decreasing order. Finally we can just take the items we are interested to by selecting n how we prefer.

<img width="750" alt="Content-based" src="https://user-images.githubusercontent.com/117635995/211694050-cf945cdd-4fba-40fe-b616-94fa10036ee6.png">

### Collaborative
The second type of recommendation system we have implemented is a Customer-based filtering recommender system. This system is based on the idea of using customers' opinions on the different products to suggest an article to the client based on other purchases of the client and purchases and opinions of customers that are similar to him. To do this, we worked initially with the dataset 'customer_transactions' that had columns 'customer_id','article_id' and 'transactions', which tell us how many times a customer has purchased that article.

The first step of collaborative filtering is to transform our data into a user-item matrix also known as a "utility" matrix. In this matrix, rows represent article_id and columns represent customer_id. 
Then we proceeded by creating dictionaries to map which row and column of the utility matrix corresponds to which article id, in particular 'article_mapper' maps article id to article index while 'article_inv_mapper' maps article indes to article id. 
We used the 'crs_matrix' function, which stores the data sparsely and calculated the sparsity.

However, the matrix had a very low sparsity, making it unreliable for predictions. To increase the sparsity, we decided to consider only a part of the dataset, discarding all the columns of the customers who bought less than a certain amount of products and some of the rows that corresponded to the products that have been bought less. To evaluate the similarity, we used the KNN algorithm from the 'sklearn' library and the cosine similarity to measure the distance and determine the ???closeness??? of instances.

As a result we obtained a sparsity of 0,5% that allowed us to proceed with the creation of our function 'find_similar_articles' that takes as input the id of the article of interest, X2 which is the new user-item utility matrix that we have built, k as the number of similar articles to recommend and metric as the metric to be used to calculate the distance for kNN calculations.

<img width="750" alt="Collaborative" src="https://user-images.githubusercontent.com/117635995/211693985-c27f54d7-3f39-4685-b8bd-7888896d079e.png">

### Neural Network-based
The **Neural Network-based** content filtering system uses product information from the Articles datasets to make recommendations. It uses a deep neural network to learn the customer and product features and then uses these features to make recommendations.

<br/><br/>

## Experiment design 

### Content-Based 
The Content-Based Filtering Recommender system seems to be very accurate, it is based on four important features of articles and resepect to the Collaborative Filtering Recommender system is composed by more articles and results are more precise, similar articles tend to belong either to the same perceived colour or to the same garment group or product group or sometimes to all of them. We would totally recommend articles using this system rather than using the Collaborative one.   

### Collaborative Filtering
The Collaborative Filtering Recommender system instead is not really reliable for our purpose. Our aim is to recommend articles to customers in order to increase revenues and to do that we need our model to be very accurate, it has to be able to recommend customers the right articles. Unfortunately to build this system some articles and customers were deleted from data, in this way the matrix sparsity increased arriving to 0,5% and was able to generate decent results. Despite this we lost some information deleting rows, even if we didn't proceed deleting random rows but only where the number of transactions per customer were less than 24. As the system is customer-based, past transactions of customers are really important to predict future purchases, from this we concluded that customers with a low number of transactions in the past are not so used to buy new articles, while customers that tend to buy more would rely their choices on the recommender system, in this way being more useful for the model. The problem here is that the percentage of customers with more than 24 transactions over the dataset is 2% which is absolutely insufficient to represent data.

### Neural Network Based: Two-tower retreival model
The features we decided to exploit were:
* index
* perceived_color_value
* garment_group
* colour_group

We chose these features for this specific model because these, in our opinion, were the features that would have made the model learn enough about customers tastes and similarities, but without getting too specific and, therefore, avoiding over-fitting or recommendations too similar to previous purchases.

The idea for the model is to construct a retrieval model composed by two components:

1. A query model which produces the query representation (commonly a fixed-dimensionality embedding vector) by means of query features.

2. A candidate model which yields the candidate representation (an equally-sized vector) by utilizing the candidate features.

The outputs from the two models are then multiplied to compute a query-candidate affinity score, with higher values indicating a more fitting match between the query and the candidate.

To build and train this two-tower system, we will predict a collection of items from the list of articles that the customer has purchased previously. This implies that each product a user buys is a positive example, and each item they have not bought is an implicit negative example.

To fit and evaluate the model, we need to split it into a training and evaluation set. We decided to split the data in chronological order, using older transactions to predict new ones.


#### Evaluation metric: factorized_top_k
Factorized_top_k is a metric used to measure the performance of a retrieval system. It is based on the idea of factorizing the user-item matrix into two matrices, one representing the user features and the other representing the item features. The metric then calculates the top k items for each user based on the dot product of the user and item feature matrices. 
This metric is useful for measuring the performance of a retrieval system because it takes into account both the user and item features, which can be used to make more accurate recommendations.

<img width="628" alt="Screenshot 2023-01-10 alle 00 06 09" src="https://user-images.githubusercontent.com/96107340/211426095-429dd113-bca2-4bda-ad87-318e48c4e40e.png">

We can see that the model performed very well on the test set. In fact, it has factorized top 100 categorical accuracy of over 96%, and of 93% for top 50. That's even higher than the performances of the training. This is probabably due to the fact that we reduced the articles features to avoid overfitting and to have a more generalized model. 


#### Results
After having constructed, trained and evaluated the model, we built the 'recSys()' function, that takes as input the id of a customer and prints 5 recommended articles, in a completely readable format. The function has a constraint to avoid any possibility of recommending already bought articles.

Here we can see its output for customer 9505:

<img width="480" alt="Screenshot 2023-01-10 alle 00 11 15" src="https://user-images.githubusercontent.com/96107340/211426701-cb97df42-4124-474e-a655-e265ce67e7fc.png">



## Conclusions 

Our job was to built an appropriate system that can predict customer preferences and recommend the right products to each customer. 
The results of our project have shown that the neural network based content filtering system has the highest potential for improving the recommender system. This system was able to make more accurate recommendations than the other two systems, and it could be further improved by exploiting customer data to increase accuracy. 
We have also seen that the collaborative filtering system and the content-based system have their own advantages and disadvantages, and they can be used in combination to create a more effective recommender system. 
All our systems provide good recommendations, although we think that if we had more information on customers' preferences and on their tastes we could expect even better results. However, in particular with the dataset related to articles, we were provided with sufficient data to develop and improve the recommender system.
In conclusion we hope we've helped our firm increasing revenues, but most important we hope they will thank us by increasing our salaries now.


