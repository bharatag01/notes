**Drawbacks of SQl DB**
1) Normalized data- one data should be stored at one place. when we have to do sharding we have to use join whch is impossible 
 to do the join in multiple machines.
2) Fixed define structure - table column so we have to store the products data for flipkart so every product has diff attributes
   so most of teh fields will be null. so it s not a good design.

**Most sql DB doesnt support sharding**

**Types of NoSql**
1) key-value- store info like key-value. when you need complete value of a particular entity.
   For eg- score of a match
   match_id,score. Dynamo DB and redis are key value DB
2) Document DB- when you have to store not the fixed attributes like product in  ecommerce.EVery product ahs diff attributes
   Eg-Mongo DB.
3) Column DB- Instead of row by row it stores data column by column. EG-Cassandra.
Eg- when you have to fetch latest tweets of hastag or uber driver location history.


**Must Read**-https://blog.algomaster.io/p/sql-vs-nosql-7-key-differences,
https://blog.algomaster.io/p/15-types-of-databases
