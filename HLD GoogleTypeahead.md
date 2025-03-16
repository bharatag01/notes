**Functional**

a) 5 suggestions.
b) Show sugeestion after every character type
c) Ranking- based on freq of search and it should give more weightage to recent one.

**Non Functional**

We want available system and with low latency , we need to compete with human typing speed.

**Back of Enveleope calc**

Daily average user (DAU)- 1 Billion
every user search per day-10
total search query- 10 BIllion.
we search when we are typing char every time so avg char type before search- 5 for eg- when we are typing sun so at s also 5 suggestion and after u also so on.
how many search suggestion per day- 10 Billion *5 =50 Billion=50 *10^9
so Request per second =50*10^9/10^5 (seconds in approx)= 50*10^4 RPS =5 lakh RPS
at peak time the request will be double =1 million RPS

**size of data** 

10 Billion queries per day- but 10% is only unique so 1 Billion unique search every day
Average length of query=10 Character
1 day =10 Bilion character
1 year=4000*10^9 character=4*10^12 character every year
1 Tb= 10^12 Kb
so in 1 year= 4 TB
5 years =20 TB
which is huge so we need to do sharding. typically we store 3-4 Tb in 1 machine so we definetly need sharding.

**read or write heavy**

read- 50 B read per day.
write - every search query 10 B /day
so ratio is 5:1
it means it is both read and write heavy. if ratio is 100:1 so we can say it is towards one thing only atleast if the system is 10 :1.

No single databse can handle both read and writes.

**List of API**

a) getSuggestion(prefix_term, limit = 5)
b) updateFrequency(search_term)-it is internal call from search service to google typeahead service.
The Google service which provides search results to a user’s query makes an internal call to Google’s Typeahead service 
to update the frequency of the search term.

**How will I store the data so i can handle search suggestion**

We can use trie apprach but for this large distributed based system we cant use trie.
We can maintain two HashMaps or Key-Value store as follows:
Frequency HashMap stores the frequency of all search terms as a key-value store.
Top5Suggestions HashMap stores the top five suggestions corresponding to all possible prefixes of search terms.

but we cant store both hashmap in 1 db so we have to store in cache machines like key value.
we need to do sharding also so we can use sharding key as first 3 char.

getSuggestion()
{
return suggestionMap(term);
}

updaecount(term)
{
UpdatedMap.get(term))+1
}


**how to handle read heavy and write heavy both**

we cant control the read query but for write query we can use sampling technique which works on probability.
suppose we randomly rejecting 90 % of request and only writing 10 % so also it gives the same result. This is sampling
Twitter also find the trends according to sampling only and check in 10% request only.
**so only caching is required here but for persistent the information we can use no sql db like cassandra and save while creating the backup of 1 hr each.
here DB is not required because consistency is not issue and if everything will delete from cache so within 1hr all suggestions willl be same because of randominization**

**How to hadndle recency**

we can use time decay factor.
suppose salman is serach till 1 lakh times and sachin is 25000 times
so we can use time decay factor and divide the freq by time decay factor, suppose it is 10
so salmand count is 10k and sachin count is 2500
on next day salman search 5k and sachin 10k so total is salmand count is 15k but sachin count is 12500 and next day again divide by time decay factor so salman count is 1500 and sachin is 1250.
on next day salman search 5k and sachin 10k so total is salmand count is 6500 but sachin count is 11250. 

Architectural diagram 

user---> LB--->App Server---->cache---asynchronsly aved in db--DB


**Notes - in deep dive will discuss  hashmap ,sharding , samplling, time decay**






