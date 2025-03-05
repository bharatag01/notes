**Rate limiter controls the rate of the traffic sent from the client to the server**

**Functional Requirement**

if the request is as per the allowed limits then allow it otherwise reject it.

We have two types of rate limiter
1) client side- we cant rely on it because somebody can change the UI code or disable the java script 
2) server side

a) so we need to implement server side rate limiter
b) rate limiting by a particular key like user_id
c) time limit s also variable (like hr, mins, seconds)
d) limit also we need to define --how many request 
e) level of stictness-very small % of up  is okay but down is not okay.

**Non Functional**

consistency vs availability- so availability is required . here consistency means exact no of request we are serving which defined in rate limiter so taht is not the case.
consistency vs low latency-- low latency is required here.

**BOE calculations**

No of tweets you can see per day
so twitter daily users=500M
so daily user can see the tweets =200
no of tweets see per day=500M*200=500*10^6*200=10^11 =100 billion per day
no of tweets per second=100*10^9/10^5=100*10^4=1 million/sec

**List of API**

handleReq(function,key) like handleReq(watchTweet,user_id)

**HLD**

user- api gateway- appserver and api gateway has rate limiter.

so GEODNS will be there which has ip of api gateways
like x.com---s1,s2,s3,s4

so user will get any ip of api gateway and it hit the api gateway through which api gateway check the rate limiting part and forward the request to respective app server.

**Deep dive**

so how api gateway will manage the rate limiting.



   
