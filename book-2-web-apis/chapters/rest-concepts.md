# RESTful APIs
1. Read [this article](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design) from Microsoft. You might not yet have enough context for some of the higher-level explanations to be relevant, but pay attention especially to [this section](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design#organize-the-api-design-around-resources) and the sections that follow on naming conventions for the URIs for your endpoints. There is a _lot_ of good advice in these sections, and the rest of this chapter will focus on that material,  as it is the most practical information for this stage of your learning about API development. 
1. It is important to note, and this is made clear in the article, that most APIs _do not perfectly conform_ to REST principles, and most tools you will use to build APIs won't enforce the principles on you. You decide how much to conform to them, and when you might need to break the rules for your specific use case.

## Design an API for a blogging site
Assume that the blogging site has the following entities:
1. Blog
1. Article
1. Comment
1. Author

Assume the following relationships:

1. A Blog can have many Authors, and an Author may participate in many Blogs. (Blog Articles are published by the whole group of Authors, individual Authors are not named on Articles)
1. A Blog may have many Articles, but an Article appears in only one Blog 
1. A Comment is made on a single Article by a single Author, but an Article may have many Comments on it. 

### Create the ERD for this system

Once you have completed the ERD, share it with an instructor or compare with a colleague's.

### Define the endpoints for this system
For each of the following, define a URI and an HTTP method for each: 

1. Create a new Blog
1. Retrieve all Blogs
1. Retrieve the Articles for a Blog
1. Create a Comment on a specific Article
1. Update an Article
1. Delete a Comment
1. Retrieve the Comments by a particular Author
1. Retrieve the Blogs that include a particular Author
1. Delete all Articles for a Blog
1. Retrieve recently posted articles (challenge)
1. Add an Author to a Blog (challenge)
1. Update all Articles in a Blog
1. Get all Comments on Articles on a Blog (challenge)