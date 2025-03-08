**Rate limiter controls the rate of the traffic sent from the client to the server**
 It helps prevent potential DDoS attacks.

**Functional Requirement**

if the request is as per the allowed limits then allow it otherwise reject it.

We have two types of rate limiter
1) client side- we cant rely on it because somebody can change the UI code or disable the java script 
2) server side

a) so we need to implement server side rate limiter
b) rate limiting by a particular key like user_id
c) time limit is also variable (like hr, mins, seconds)
d) limit also we need to define --how many request 
e) level of stictness-very small % of up  is okay but down is not okay.

**Non Functional**

consistency vs availability- so availability is required . here consistency means exact no of request we are serving which defined in rate limiter so that is not the case.
consistency vs low latency-- low latency is required here.

**BOE calculations**

suppose if we are creating watchtweets rate limiter so particular user can see 100 tweets per minute.

No of tweets you can see per day
so twitter daily users=500M
so daily user can see the tweets =200
no of tweets see per day=500M*200=500*10^6*200=10^11 =100 billion per day
no of tweets per second=100*10^9/10^5=100*10^4=1 million/sec

**List of API**

handleReq(function,key) like handleReq(watchTweet,user_id) 
if it is below 100 allow-200
above 100- error 429

**HLD**

user- api gateway- appserver and api gateway has rate limiter.

so GEODNS will be there which has ip of api gateways
like x.com---s1,s2,s3,s4

so user will get any ip of api gateway and it hit the api gateway through which api gateway check the rate limiting part and forward the request to respective app server.

**Deep dive**

so how api gateway will manage the rate limiting.

approach 1- will take hasmap and for particular user stores the timestamp of every request.

List<Req> l=hm.get(u1);

remove all the request which is older than 1 min/1 hr(based on condition)
if (l.size()<allowed limit)
 hm.add(time);
 allow;
else
reject;

con- size of map will be huge if duration is huge.

approach 2
consider a new window starting every min
and assume 5 req/day.

mainatain a count how many request we get within this min

if(count<allowed)
allow
count++;
else 
reject

con
not exact
suppose if request comes at 10:00:56,10:00:57,10:00:58,10:00:59,10:01:00,10:01:01,10:01:02

so you serve 7 request within 1 min.

apprach 3

so this apprach is similar like apprach 2 but it can cnsider the pre min request also not ignoring completely.so it is bit more exact in comaprioson to previos one

ex- u1 -1001 -40 request  
    u1- 1002- 12 till now
    now the time is 10:02:30 and limit is  50 req /min
    so 30 seconds of prev min matters so =30/60*100=50% matters
    so prev-min count=40 and 50% of 40 =20
    current count=12
    so total count=20+12=32
    if(total<limit)
         current count++;
         allow;
         else
         reject;
