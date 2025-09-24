# Cassandra

Cassandra is column-family store NoSQL database, also referred to as columnar database. These databases organize data in columns rather than rows. These databases store columns of data together, making them efficient for handling large data sets with dynamic schemas.

# Key Features

- Apache Cassandra is an open source, distributed, decentralized, elastically scalable, highly available, fault-tolerant, tunable, and consistent database. 

- Apache Cassandra is best used by applications that require a database that is always available. 

- Data distribution and replication take place in one or more data center clusters.

- Cassandra stores data in tables that are grouped in keyspaces. 

- A clustering key specifies the order in which the data is arranged inside the partition (ascending or descending). 

- Dynamic table partitions grow dynamically with the number of entries. 

- **CQL** is the primary language for communicating with Apache Cassandra clusters. 

- **CQL queries** can be run programmatically using a licensed Cassandra client driver, or they can be run on the Python-based CQL shell client provided with Cassandra.

# Data Models in Cassandra

Cassandra stores data in tables whose schema defines the storage of the data at cluster and node level. Tables are grouped in key spaces.

A keyspace is a logical entity that contains one or more tables. A keyspace also defines a number of options thatapplies to all the tables it contains, most prominent of which is the replication strategy used by the key space.

It is generally encouraged to use one keyspace per application. So tables are the logical entities that organize data storage at cluster and node level. They contain rows of columns.

You can create, drop, and alter your tables without impacting the running updates on your data or the running queries.

In order to create a table, we need to declare using Cassandra query language a schema.

A table schema comprises at least a definition of the table's primary key and the regular columns of the table.

Table groups store information regarding several groups, such as groupid, group_name, and for each group, the username and age of their members.

You can see that the primary key is composed of two columns, groupid and username.

In Cassandra denomination, the groupid column is called the partition key and the username column is called the clustering key.

Let's talk a bit more about the primary key.

First of all, the primary key is basically a subset of the table's declared columns.

When creating a table, besides declaring the table's columns, it is mandatory to specify the primary key as well. The primary key, once defined, cannot be changed.

In Cassandra, the primary key has two roles.

The first role is to optimize the read performance of your queries.

Do not forget that the NoSQL systems are query driven data modeling systems, meaning that table definitions can start only after the queries you would like to answer are defined.

You should build your primary key based on your queries, and the second role is to provide uniqueness to the queries.

A primary key has two components. The mandatory component is called the partition key, and optionally you can have one or more clustering keys.

https://github.com/fqasem/mongodb_and_cassandra/blob/main/cassandra/images/logical%20entities%20-%20tables%20and%20keyspaces.png

When data is inserted into the cluster in a table, the data is grouped per partition key into partitions, and the first step is to apply a hash function to the partition key.

The partition key hash is used to determine what node and subsequent replicas will get the data.

In simpler terms, a partition key determines the data locality in the cluster.

You can see in the diagram and table that data is grouped according to the partition key, groupid, and that each partition is distributed to one of the cluster nodes.

The partition is the atom of storage in Cassandra, meaning that one partition's data will always be found on a node and its replicas in the case of a replication factor greater than one. So if we want to answer the query all users in group 12,then the query can address only  the fourth node and will get the answer.

In big clusters, those consisting of hundreds or thousands of nodes, limiting the number of nodes required to be contacted in order to answer the query is crucial for query performance.

Let's define two more concepts, **static and dynamic tables**.

When the table has a primary key that contains only the partition key, single or multiple columns, and no clustering key, the table is called a static table.

When the table has the primary key composed of both partition key or keys, and clustering key or keys, the table is called a dynamic table.

This user's table is a **static table**.

As you can see, the primary key consists only of the partition. This means that the number of distinct users we will have is the number of partitions we will have in our table.

In static tables, partitions have only one entry and thus are called static partitions.

![Logical Entities](images/logical%20entities%20-%20tables%20and%20keyspaces.png")

Let's move on with our Cassandra data modeling journey, having already introduced the primary key, the role of the partition key, and the definition of static tables.

After the following discussion, you will be able to explain what cluster string keys are, describe dynamic tables, and explain the basic guidelines for modeling your data.

In red, you can see that the second component of the primary key is called the clustering key. While the partition key is important for data locality, the clustering column specifies the order that the data is arranged in inside the partition that is, ascending or descending, and optimizes the retrieval of similar values column data inside a partition.

The clustering key can be a single or multiple column key. In our case, our clustering key contains only one column username, which means that the data inside the group partition is going to be stored by username by default in an ascending order. So when we query all users in a specific group, data will be ordered by default in ascending order by the user's username.

In this example, the clustering key was used for one main reason, to add uniqueness to each entry. But actually the clustering key is also very important for improving the read query performance.

The next part of this discussion will illustrate this point.

Let's assume we would like an answer to the query: Give me all users in group ID 12 that are aged 32.In this case, one of the possible data models for answering such a query is to have a table with group ID as the partition key and age and username as clustering keys.

Data inside each partition is first grouped and ordered by age, and inside each age group it is then grouped by username. So in this case, all users in a certain group that have the same age will be stored together. Thus, a query such as give me all users in a certain group by group ID that are aged 32 will just locate the node that contains the partition for
group 12 and read from that node just the two sequential records.

Reducing the amount of data to be read from a partition is crucial for query time, especially in the case of large partitions in which Cassandra would have to read hundreds of megabytes of data in order to provide an answer from just a few kilobytes of data.

Let's insert a new entry in a dynamic table like groups. Inserting new data in our table will just direct the write to the location of the partition and will increase the partition size from two to three entries.

In dynamic tables, partitions grow dynamically with the number of distinct entries due to the presence of the clustering key in the primary key. It's important to note that in this diagram, replication is not taken into account, so only one node holds the partition for group 45 for example.

Every write or read for group 45 will be routed to the second node of the cluster. As long as the cluster is not changed, this node will hold the data for group 45. A change such as nodes added or leaving would trigger a new token allocation and subsequently data distribution. What we just did previously building a primary key in order to answer the query in optimal time is the beginning of the data modeling process.

There's much more to Cassandra modeling than primary key definition, but to keep it simple we will refer back to this part, which is also the most important. When building a primary key for a table, you should take into consideration the following simple rules.

First, choose a partition key that starts answering your query, but that also spreads the data uniformly around the cluster. For example, group ID might be a good partition key if there are many groups. Many distinct values for group id and group sizes are similar. And secondly, build a primary key that allows you to minimize the number of partitions read in order to answer a certain query. 

Remember that data is distributed throughout the cluster. If we would need to read more partitions to answer the query, then we would need to potentially access several nodes in order to answer our query. This would affect the query time and even induce timeouts. Thus, the primary key needs to be designed sothat optimally we read one partition in order to answer our query. And one note before we summarize, besides the basic rules discussed here,make sure you build a clustering key that helps you reduce the amount of data that needs to be read even further by ordering your clustering key columns according to your query.

In this discussion, we learned that a clustering key specifies the order that the data is arranged inside the partition ascending or descending. A clustering key can be a single or multiple column key. A clustering key provides uniqueness to a primary key and
improves read query performance. Reducing the amount of data to be read from a partition is crucial for query time, especially in the case of large partitions.

In dynamic tables, partitions grow dynamically with the number of entries. Building a primary key in order to answer the query in optimal time isthe beginning of the data process. And to define a Cassandra table in order to get good read performance, you need to start with the queries you would like to answer, then build your primary key based on the query.
