**Functional**

1) create an id when requested. so it is individual service which cretaes the id who ever service requested.
   it should be integer not string
   it should be 64 bit long. 
   increasing id.  if public id are consecutive so it should easy to predict the id sequence.
   it should no be random also like 1 2 41 then 30 not correct because when we write the data so we have to move 30 before 41 so writes will slow.
   because in db it stored in increasing order.

**Non functional**

it should be consistent but very high availble because evry service need id and if it is dow so nothing will work.
consistent menas it shoud always generate unique id.

Very low latency.

**Back of envelope calculation**

suppose if I am taking the example of twitter id generation for tweets.
dau= 500 million
% of user post daily=1 %= 5 million
so every day id generation=5 million
every second id generation=5 million/10^5=50 id generation/sec

suppose in peak time the load is 5x so 250 id generaton/sec.

**API**

long generateId();
it will be rpc calls because it interacts with internal services so for lower latency will use rpc.

**HLD**

user- LB- appservice--rpc calls--IdGenerationService

**Deep dive**

how to generate unique id with high availability.

approach 1

1) take the last id from db.
2) new_id=last_id+random np
3) save new id
4) return id;

cons- spof
      not scalable if suppose mutiple servces call db at same time so they get the same id which is incorrect and chances of unique d generation will be less

approach 2

each server has last_id which is generated
each server has no.

so suppose server 1 has last_id =0 . so id will generate 32^0+1,32^1+1,32^2+2 and so on.
for server 2 the id willl be 32^0+2,32^1+2,32^2+2 and so on.

cons
kind of sequential id.
id are not incremental because server 1 got the request 2 times which genertae id 1,33 but server 2 got request after that which genrate 2 , so id are not incremetal.


apprach 3

what if we return getTime() --epoch time --no of milliseconds passed from 1970 
so every server will return getTime() which is unique and incremental but what if two servers retrun the time at same time s it will not be unique
so for that wil use getTime()+serverId - suppose time is 1236789 and serve id is 1 so it will be 12367891

why not server id+getTime()-- server 2 got the request which generate 212345789 and after that server 1 will generate 112345789 so it will be not incremental.

so thats why always use getTime+server_id from each server.

**twitter snowflake alogrithm**

timestamp+datacenter_id(has multiple data center in same server)+server_id+counter(how many id generated till now by that server in that data center in that millsecond)

uuid -- which is universal unique identifier- which gives incremetal id from uuuid v7 and internally it is integer only.
uuid are 128 bit number..

For small-scale projects or systems with no strict ordering, UUIDs can work.
❌ For a Twitter-scale system, Snowflake-like IDs are much better.

Not Sequential	UUIDs are random, making it hard to sort tweets chronologically.
Indexing Overhead	Random distribution leads to poor database indexing performance.




