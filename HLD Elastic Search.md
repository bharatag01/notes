Design a system that allows to efficiently find relevant content.
for eg- log system like splunk.

**Functional**

1) store conent which we are searching.
2) find relevant conent based on query.

**Non functional**
   
consistency vs availability. here consistent means whatever we have wrtten just now it should also come via search. but we are looking for highly available system.
search results should come.

consistecy vs low latency- looking for low latency.

**Back of Envelope calculation**

Daily linkedin users =100M
% people making post- 1%=1M post/day
post per second= 10^6/10^5=10 post/sec

data size= 5(years)*400(days)*1*10^6(post per day)*500(500 bytes per post)= 2*10^12 bytes= 1TB

mostly data will not fit in 1 machine so we need to do sharding.

searches per day= 100 M(1 user search 1 daily average)=100 M searches per day
searches per second= 100*10^6/10^5=1000 searches per seocnd

so it is read heavy.

**API**

1. search(query)
2. putContent(content)

when a user create post so it put on post service and and post service put the content on serach service.

**Deep Dive**

how will serach service will find the data at very low atency with high availability.

suppose if we use sql db so we have to search evry row so it will be very very slow.
we cant use indexing also it will not helpful for content. -- check the indexing video in sql.

we will create the inverted index. 
1) tokenization- splitting text by words
2) word elimination- eliminte a , an the 
3) stemming--converting word to root word
4) spell correct.

content| doc_id   -- word | list of doc_id(inverted index)


**how will you shard the data**

by word
so we need to do the intersection also of doc id which we get from ach shard and then return the result.

by document_id
if an shard is down so also we can return the result.
whatever data we get from shards , combine it and return .
so we should do sharding by doc_id.

suppose delhi is the capial of india
and delhi is in shard 1 and capial is in shard 2 and shard1 not reachable so it will never return the result.

but in case of sharding by doc _id 
in 1 shard - delhi is capital of india and in shard 2 delhi cm is in jail so will get the result if shard1 is down also.

also if we are doing sharding by doc _id so also we need inverted index because in each shard it serach the word in inverted index only.

here we will use elastic search no sql db. 

if million query per second so we need to create replica of each shard.
