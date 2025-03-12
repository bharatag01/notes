**Functional Requirements**

1) Send and receive messages
2) 1:1 chatting not group
3) only text messages not photos/videos.
4) message stored on backend not on device like whatsapp.
5) Get my recent conversation

**Non Functional Requirement**

1) Availability vs consistency- We are looking for consistency . Here consistency means mesage sent but not receives at receiver db and order of messages also.
2) low latency vs consistency- looking for consistency.

**Back of Envelope Calculation**

active users- 500 millon users= 50 cr
daily messages per user= 500*10 million= 5 Billion messages per day

RPS messages- 5 billion/10^5 = 5*10^9/10^5=5*10^4= 50 K messages per second.
peak message per second= 1 lakh message per second.

storage = mesage has user id, timestamp, content,receiver id= 100 B per message
1 day = 5 *10^9*100 B= 5*10^11= 1 TB(approx)
1 year=400 TB
5 year=2 PB(peta byte)
definetly we need sharding

Read heavy or write heavy
it is both. message read atleast 1 time and send also 1 time.

**API design**

1) sendMessage(conversation_id, user_id,content)
   If I click into a conversation, then I get the list of most recent messages. Let’s call that getMessages. 
3) getMessages(conversation_id,content,timestamp)
4) getRecentConversation(user_id)

**HLD**

**How server will send a message to reciever**

there are 3 ways

1) polling- receiver always poll to server is ther any message for me. so it is not real time.
2)long polling- server always create a new connection with reciever one any message comes. so multiple connection.
3) websocket-it will create the pipeline between sender and receiver. only connection created once.

**How to make sure messsage is delivered only once.**

suppose client doesnt get ack after x sec so it retires so how server identifies it is duplicate request.
By using imdempotency
every message has timestamp(last character typed) +user_id+device_id to identify duplciate message.

**How to shard data.**
user_id and conversation_id

one to one 

sent-2     1
get-1      1
getList-1  all

recent_conversation table shard by user_id to overcome all shard in case of getallconversation (user)

user_id  conversation_id
101      1011
101      1012

group chat-- conversation id +user_id

sent-500(inefficent)  1+max users of that group(because recent conversation also changed) --so 
will update the recet_conversation table here not all 500 shards. because recent conversation have max 1 tb data because it conatins
only user_id and conversation id of that user. 
get-1                 1
getList-1  

we can do sharding by user_id because it is 1:1 chat not group chat
but in case of group chat we can use the comination of conversation_id+user_id.
For systems like, Slack, MS Team, Telegram that can have large groups, userID based sharding will be ridiculously expensive.

**How to maintain consistency of messages.**

suppose messages is going to send between A->B .so A gets delivered but B is not able to see the mesage.
so first save in shard B and then A. suppose it doesnt save in shard A then take it from browser cache.

**which DB to use**

we can use cache to avoid multiple read from DB and save last 24 hrs messages in cache and user wil read from cache
and also while writing will write through cache and for DB will use cassadra DB because of heavy write.




