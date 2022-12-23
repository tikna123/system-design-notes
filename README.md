# This repo contains all the system design related notes or docs

## System Design tips
* Web server and cache server should be close to each other. So that it should not take much time for cache request.
* We can put the cache in memory to the server(if results are small). What if this server fails?
Redis: distributed cache(few boxes). Data consistency, availability). We can scale the redis cache.(global cache)

* Decide whether the system is read heavy or write heavy(mostly it is read heavy in case of url shortener or pastebin). If it is read heavy then need to decide  about the cache. It is good to store top  20% of the request in the cache.

* All images and text stores in blob storage or object storage(like amazon S3 or google cloud storage(GCS)). In the database we can store image link or text link(external link) in s3. We can also store some part of object like preview and show it to user for better user experience(bcz image download from s3 will take more time).

* During capacity estimation, keep extra 20-30%(buffer) for storage or bandwidth etc.

* Based on read heavy or write heavy you can also decide cache updation policy(write through or write back)

* Mem-cache is multi threaded as compared to redis.

* In case of url shortener or pastebin, keep key generation service as a separate.

* 100GB can be stored in Redis.

* Elastic search vs mongo db vs cassandra vs dynamodb?

* Reddis vs memcache?

* Nosql vs relational database?
* System design talking points:
    * Non functional requirements: availability,reliability,consistency
    * Storage
        * Which database to use relational or nosql
        * data partitioning (based on what keys: hashing or region or user)
            * While doing data partitioning, keep in mind that for search complexity and write complexity(time take to read and write tradeoff..which is more important for the current application)
        * Elastic search: if text search is needed
        * Blob storage: if need to store images or binaries.
    * Cache
        * read heavy or write heavy
        * memcache or reddis
        * CDN(how it works?)
    * Load balancer: consistent hashing
    * Security(if needed in like chat messenger)
    * Logging: (monitoriing ELK kibana, grafana). There always be analytics component or monitoring
    * Think to create different microservices(for each task)
    * How data flow or flow in architecture in runtime?
    * Read facebook engineering blog?
    * Talk about ranking(in case of uber drivers,yelp restaurants,newfeed ranking)
    * Kafka for real time ingestion(logging activity from user): kafka -> hadoop
    * Online updates(primary&secondary server): updates secondary while primary serving the traffic. Update secondary, do AB testing and then make secondary as primary & update primary offline.
    * If need to aggregate results from multiple application server, then add aggregation server between load balancer & applications servers. In case of search, we have different recall strategy so need to merge them.
    * Instagram, twitter: eventual consistent, stock price show more immediate consistency need.
    * Zookeeper is used to coordinated between different nodes in the cluster. Whenever we have big cluster use zookeeper.
    * Two layer sharding if data is too much: 1st level by user id and 2nd level by tweet id.
    * While doing the data partition keep in mind the search complexity when searching a word. For ex: in search inverted indexing(twitter), if we shard by word(by using some kind of hashing)  then for search query word we need to check only a single shard..where as we shard by tweet id then we need to check all the shards for a search query.(check typeahed section too)
    * When writing API function..Also mention rate limiter component if it is a critical API.
    * Facebook possible questions
        * Security permission changes
        * Design "stories" (like Instagram and Whatsapp). Stories disappear after friends view them, and are only available for 24 hours after publishing. 
        * Design facebook newsfeed
        * Design facebook search
        * Also explore fb marketplace
    * Multi level load balancing: netflix/youtube
    * Self upload consistency is needed: netflix/youtube 
    * For web crawler check tech dummies
    * You can use object storage(amazon s3) to store videos/images.
    * Use rdms and nosql both in case of book my show.
    * Dropbox scales MYSQL to stores metadata information about 600 million files.
    * By logging user activity or any other activities can help us to do analytics. It will also help us to know about how much we want to scale in the future.(metrics & kibana)
    * Using transaction in MYSQL, you can ensure concurrent access to the data.
    * It’s better to use data service to interact with the databases..It abstracts the database from the real application & changing the database won’t affect much the real application.
    * For Sharding we can also have extra k-v system(etcd/zookeeper), that basically stores key and value is the shard no(for the key)(important)
    * We can use pub-sub(each group will be 1 topic) model to introduce group chat in messenger.
    * RDBMS can handle 1 million entries
    * Etcd vs zookeeper vs dynamodb
    * We can use Spark after kafka for some transformation of the data.

* Row major format vs column major format:
    * Overall, row-major formats are better when you have to do a lot of writes, whereas column-major ones are better when you have to do a lot of column-based reads.
    * If u want to read lot of features then column based format is better.
* Normalization leads to different relatation and access is slower.
* Declarative vs Imperative language:
    * The most important thing to note about SQL is that it’s a declarative language, as opposed to Python which is an imperative language. In the imperative paradigm, you specify the steps needed for an action and the computer executes these steps to return the outputs. In the declarative paradigm, you specify the outputs you want, and the computer figures out the steps needed to get you the queried outputs.
* From declarative data systems to declarative ML systems
    * AutoML and Ludwig is an example of declarative ML systems.
* NoSQL
    * A collection of documents could be considered analogous to a table in a relational database, and a document analogous to a row
    * The document model doesn’t enforce a schema, it’s often referred to as schemaless.
    * The document model has better locality than the relational model. All the information  related to one object can be stored in single document while in relational database you may have to query multiple tables to accumulate all the information of an object.
    * For analytical usecases NOSQLs DBs are better.



