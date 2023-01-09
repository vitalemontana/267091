# Recommender systems for a fast fashion company

## Team members
* Francesco Vitale (267091)
* Andrea Stella (270861)

<br/><br/>
## 1) Introduction
We impersonate the Data Science team at a fast fashion company. 

The platform already has a recommender system. Our job is to make it better.

<br/><br/>
## 2) Methods
We had three datasets at our disposition:

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
  
<br/><br/>

### Exploratory Data Analysis
First we performed an EDA to understand the company data, to see undergoing trends, to get useful insight and to understand whether some data cleaning was needed.

(Spoiler: Yes)

There were some Null values in the 'age' column of the 'customers' dataset.

Now, we couldn't remove rows where Nulls appeared, since we would have lost some customer ids. Therefore, we chose to:
* First, segment customers based on their subscriptions to the additional services (Fashion newsletter, Club).
* And then, compute the mode, i.e. the value that appears most often in a set of data values, for each segment and substitute it to the Nulls.

We chose the mode over the mean because the mean is more sensitive to outliers and skewed data, and, as we can see below, 75% of customers' age is under 46, while the maximum age is 91.

<img width="207" alt="Screenshot 2023-01-09 alle 22 56 48" src="https://user-images.githubusercontent.com/96107340/211416575-3fe2ca4a-d371-4e76-a53e-fe5467be44fa.png">

<br/><br/>

To get an hint about the demography of the customer base we plotted the distribution of our customers' age:

<img width="1107" alt="Screenshot 2023-01-09 alle 23 00 56" src="https://user-images.githubusercontent.com/96107340/211417060-44dc1754-861c-40e3-b1a1-5be656d2929f.png">

<br/><br/>

The pyhton libraries we used in order to analyze data were: 
* Numpy
* Pandas
* Sklearn
* Matplotlib
* Seaborn.


<br/><br/>

In order to build a recommender system, three different recommendation systems were tested: 
* A collaborative filtering recommender system
* A content-based recommender system
* A neural network-based content filtering system.

The **collaborative filtering** recommender system uses the customer-item interaction data from the Transactions dataset to make recommendations. It uses a matrix factorization technique to identify the latent features of the customers and items and then uses these features to make recommendations.

The **content-based** recommender system uses the product information from the Articles dataset to make recommendations. It uses a vector representation of the products and then uses a similarity measure to make recommendations.

The **neural network-based** content filtering system uses product information from the Articles datasets to make recommendations. It uses a deep neural network to learn the customer and product features and then uses these features to make recommendations.

<br/><br/>

## Experiment design
### Collaborative filtering

#### Results

<br/>
### Content-based

#### Results

<br/>
### Neural Network Content Based: Two-tower retreival model
The idea is to contstruct a retrieval model are generally composed by two components:

1. A query model which produces the query representation (commonly a fixed-dimensionality embedding vector) by means of query features.

2. A candidate model which yields the candidate representation (an equally-sized vector) by utilizing the candidate features.

The outputs from the two models are then multiplied to compute a query-candidate affinity score, with higher values indicating a more fitting match between the query and the candidate.

To build and train this two-tower system, we will predict a collection of items from the list of articles that the customer has purchased previously. This implies that each product a user buys is a positive example, and each item they have not bought is an implicit negative example.


To fit and evaluate the model, we need to split it into a training and evaluation set. We decided to split the data in chronological order, using older transactions to predict new ones.

#### factorized_top_k
Factorized_top_k is a metric used to measure the performance of a retrieval system. It is based on the idea of factorizing the user-item matrix into two matrices, one representing the user features and the other representing the item features. The metric then calculates the top k items for each user based on the dot product of the user and item feature matrices. 
This metric is useful for measuring the performance of a retrieval system because it takes into account both the user and item features, which can be used to make more accurate recommendations.

