# MongoDB

MongoDB is a document-store database. Document-store databases store data in a document format, typically JSON or BSON (binary JSON), where each document contains key-value pairs or key-document pairs. These databases are schema-less, allowing flexibility in data structures within a collection.

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
