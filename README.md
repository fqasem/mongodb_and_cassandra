# Mongodb and Cassandra
Project showing Mongodb and Cassandra commands both from shell and python code.

## NoSQL Database Types and Use Cases
After completing this reading, you will be able to describe the characteristics of several NoSQL database types, list their use cases, and identify frequently mentioned vendors associated with each NoSQL database type.

As you review each use case, you'll see frequently used NoSQL database vendors mentioned in parentheses following the listed use case. This information is not an endorsement. This information is for market awareness.

Gain the insights you need to be able to explain some of the technical considerations associated the use of MongoDB with a content management system (CMS).

## Document store databases
Document-store databases, also known as document-oriented databases, store data in a document format, typically JSON or BSON (binary JSON), where each document contains key-value pairs or key-document pairs. These databases are schema-less, allowing flexibility in data structures within a collection.

# Characteristics
- Provides schema flexibility: Documents within collections can have varying structures, allowing for easy updates and accommodation of evolving data requirements.
- Performs efficient create, read, update, and delete (CRUD) operations: well-suited for read and write-intensive applications due to their ability to retrieve whole documents.
- Provides scalability: horizontal scalability by sharding data across clusters.

# Use cases
- Content management systems (CMS): CMS platforms like WordPress use document store databases for fast storage and access to content types such as articles, images, and user data. (MongoDB)
- E-commerce: E-commerce platforms need effective management of product catalogs with diverse attributes and hierarchies, accommodating the dynamic nature of e-commerce product listings. (Couchbase or Amazon DocumentDB, using MongoDB compatibility)

# Frequently mentioned vendors
- MongoDB
- Couchbase
- Amazon DocumentDB

## Expanded use case example: 

# Using MongoDB for a content management system (CMS)
Content management systems (CMS) intelligently collect, govern, manage, and enrich enterprise content, including HTML pages, images, articles, and more. Content management systems help companies deploy their content efficiently and securely across any cloud and within any application.

Good content management means that team members can quickly add, update, and remove content from the database and the associated pages that feature that content. Examples include pushing out breaking news, updating current news, including weather forecasts, pushing advertising content, updating college admission policies, launching new city services, and more.

For example, using MongoDB as a backend database for a content management system (CMS) is a practical choice when you need to manage and serve a variety of content types, especially in scenarios where you expect frequent schema changes or scaling requirements.

Next, let's check out some of the aspects of managing content using a content management system, specifically using MongoDB.

# Content structure using MongoDB
In MongoDB, you represent content as documents. Each document corresponds to a piece of content, such as an article, image, video, or page. You can use the subdocuments in the document to organize the content hierarchy and structure.

# Example of structuring: Storing a blog post
When storing a blog post, you will store core attributes like title, content, created at, and the image URL. Then, using an array field, you can store tags. The comments on that post are stored as an array of objects.


// Collection: posts
{
"\_id":1,
"title":"Sample Blog Post",
"content":"This is the content of the blog post...",
"author":{
"name":"John Doe",
"email":"john@example.com",
"bio":"A passionate blogger.",
"created\_at":"2023-09-20T00:00:00Z"
},
"created\_at":"2023-09-20T08:00:00Z",
"tags":["mongodb","blogging","example"],
"comments":[
{
"text":"Great post!",
"author":"Emily Johnson",
"created\_at":"2023-09-20T10:00:00Z"
},
{
"text":"Thanks for sharing!",
"author":"James Martin",
"created\_at":"2023-09-20T11:00:00Z"
}
]
}




# Metadata and indexing using MongoDB
You can use the indexing capabilities of MongoDB to optimize content retrieval. You can create indexes on fields commonly used for filtering or searching, such as keywords, publication date, or content type, or use MongoDB's text index support for text search queries on fields containing string content. Text indexes improve performance when searching for specific words or phrases within string content.

For example, you want to provide searching capability on the content of your blogs. You will first create a text index:

db.articles.createIndex( { subject: "text" } )

And then you can provide a query such as:

db.posts.find( { $text: { $search: "digital life" } } )

where MongoDB will look for stemmed versions of these words: digital or life

# Scaling your CMS using MongoDB
As your CMS grows, MongoDB can help you scale. You can use sharding for horizontal scaling or use zone-based sharding for global distribution.

# Using sharding for horizontal scaling (increased capacity)
Let's consider a company that currently has 100 million customers. This company expects to expand its customer base to 200 million customers. This increase in the number of customers means that the company will need to double its IT data storage hardware. The company can scale vertically, which can cost exponentially more as the hardware cost isn't linear with the performance. The following diagram shows that the company can scale horizontally and use sharding to manage the databases.



## Key-value stores
Key-value stores are the simplest NoSQL databases, storing data as a collection of key-value pairs, where the key is unique and directly points to its associated value.

# Characteristics
- Delivers high performance: efficient for read and write operations, optimized for speedy retrieval based on keys
- Provides scalability: easily scalable due to their simple structure and ability to distribute data across nodes
- Uses caching for fast access
- Provides session management
- Works with distributed systems

# Use cases:
- Enhanced web performance by caching frequently accessed data (Using Redis or Memcached)
- E-commerce platforms, software applications, including gaming: Amazon DynamoDB provides a highly scalable key-value store, facilitating distributed systems' seamless operation by handling high traffic and scaling dynamically.

# Frequently mentioned vendors
- Redis
- Memcached
- Amazon DynamoDB


## Column-family stores
Definition: Column-family stores NoSQL databases, also referred to as columnar databases, organize data in columns rather than rows. These databases store columns of data together, making them efficient for handling large data sets with dynamic schemas.

# Characteristics
- Uses column-oriented storage: Data is grouped by columns rather than rows, allowing for efficient retrieval of specific columns.
- Delivers scalability: Distributed architecture for high availability and scalability.

# Use cases
- IoT applications manage massive amounts of sensor data efficiently due to their ability to handle time-stamped data at scale, referred to as time-series data analysis. (Apache Cassandra)
- Applications that store and analyze user preferences and behaviors usually deliver personalization. (HBase, part of the Hadoop ecosystem)
- Large-scale data analysis.

# Frequently mentioned vendors
- Apache Cassandra
- HBase

## Graph databases:
Definition: Graph NoSQL databases are designed to manage highly interconnected data, representing relationships as first-class citizens alongside nodes and properties.

# Characteristics:
Analyzes the data using a graph data model: relationships are as important as the data itself, enabling efficient traversal and querying of complex relationships.
Fast performance for relationship queries: optimized for queries involving relationships, making them ideal for social networks, recommendation systems, and network analysis.

# Use cases:
- Social networks require efficient data management of relationships between users, posts, comments, and likes. (Neo4j)
- Recommendation systems: Organizations need a database structure that can create sophisticated recommendation engines, analyzing complex relationships between users, products, and behaviors for precise recommendations. (Amazon Neptune)

# Frequently mentioned vendors
- Neo4j
- Amazon Neptune
- ArangoDB Memcached


## Wide-column stores:
Wide-column store NoSQL databases organize data in tables, rows, and columns, like relational databases, but with a flexible schema.

# Characteristics:
Use columnar storage: Data is stored in columns, allowing for efficient retrieval of specific columns rather than entire rows.
Provide horizontal scalability and fault tolerance.

# Use cases:
- Analyzing big data: Efficiently handling large-scale data processing for real-time big data analytics. (Apache HBase used in conjunction with Hadoop)
- Managing enterprise content: Large organizations databases need to manage vast amounts of structured data like employee records or inventory due. (Cassandra)

# Frequently mentioned vendors
- Apache HBase
- Apache Cassandra


## Vector Databases

Vector databases, a newer NoSQL database, are rapidly becoming popular with the exponential increase in the use of Large Language Models (LLMs), such as OpenAI's GPT. But what is a vector database? Here's how IBM defines a vector database:

A vector database is designed to store, manage, and index massive quantities of high-dimensional vector data efficiently.

Source: https://www.ibm.com/topics/vector-database

Next, learn about data as vectors.

# Vectors
You can transform text, images, audio, and video into vectors, also known as vector data, using embedding functions based on various methods, including machine learning models, word embeddings, and feature extraction algorithms.

For example, the vector representation of dog is [2.1,-0.3, 7.2, 9.6, 6.1]

Similar words for dogs include the word canine or K9, so a vector database will identify both terms and include the same vector values.

The vectors of dog and k9 contain exactly the same values.
Important! When words have relationships or similar contexts, but the meaning is not identical, these words have vectors that are closer together within the database that help identify the relationship.

For example, you'll see the word animal represented as [1.9, -0.4, 7.2, 8.0, 6.3]

Each numeric value you see displayed inside of a vector is one dimension.

Vectors can have a few or thousands of dimensions, depending on the granularity and complexity of the classification required.

Returning to the example of a dog classifications, we know a dog is an animal, but not all animals are dogs. The relevance and relationships between the words "dog" and "animal" mean that these words will be much closer together as vector values and within the database itself. The following illustration shows the one dimension, dimension 7.2, that the word dog and animal share.

# Vector Database benefits
In contrast to conventional techniques that involve querying databases for exact matches or predefined criteria, a vector database empowers you to discover the most similar or related data by considering their semantic or contextual significance.

In other words, unlike other database types that require an exact term search, you can use a vector database to conduct similarity searches and retrieve data according to their vector distance or likeness.

For example, you can use a vector database to perform the following tasks:

- Recommend TV shows to watch based on your current viewing habits.
- Locate related products based on the first product's features and ratings when shopping online.

Because related vector data exists mathematically closer to each other within the database, search and data delivery times are faster. So rather than having to perform additional analysis techniques to retrieve related data, the trained model and vector database delivers relevant search results faster.

# Popular Vector Databases
Database offerings and their features are changing concurrently with the exponentially fast speed of AI development. Before selecting a vector database, you'll want to review its applicability to your data and LLM. Next, check out these currently popular vector databases:

# Chroma
Chroma is an open source embedding database with which you can perform the following tasks:

Store embeddings and their metadata
Embed documents and queries
Search embeddings

# Pinecone
Pinecone provides long-term memory for high-performance AI applications. Pinecone emphasizes the following capabilities and features:

Runs as a fully managed service
Provides high scalability
Provides real-time data ingestion
Delivers low-latency search

# Weaviate
Weaviate, an open-source vector database that stores data objects and vector embeddings from machine-learning models, is said to provide the following capabilities and features:

Provides efficient similarity searches
Scales to store and process billions of data objects
Runs the GraphQL API
Provides real-time updates