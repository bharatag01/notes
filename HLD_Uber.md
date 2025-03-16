Functional

1) Drivers need to regularly notify the service about their current location and 
their availability to pick passengers. 
2) Passengers get to see all the nearby available drivers. 
3) Customer can request a ride; nearby drivers are notified that a customer is 
ready to be picked up. 
4) Once a driver and a customer accept a ride, they can constantly see each 
other’s current location until the trip finishes. 
5) Upon reaching the destination, the driver marks the journey complete to 
become available for the next ride.

Non Functional

Highly avaialable-Real-time operations (like booking, ride tracking) must be always available.
Low latency 

Back of envelope calculation

cab booking

DAU=100 million
% of person book cab=5%
so daily cab booking request=5 million
so per second=5 million/0.1 =50 cabs/second
in peak time it is 5x=250cabs/second

but Each ride generates hundreds of reads so it is read heavy.

driver also send the updates in every 10 sec
suppose driver=10 million
how many request per second=10 million/10=1 milion request /sec

it is both read and write heavy.

API

GET v1/api/searchNearbyCab(user_id,lat,lon)
POST v1/api/UpateDriverLocation(lat,lon)

deep dive

here driver location kepp changing. 
so suppose if we consider the world as a node and recusively divide the world into 4 nodes til each quadrant has <=10 cabs.
it is called quad tree.

suppose if we divide the data based on city so we are taking city as sharding _key so bangalore shard has bangalore diver like this.
and we create the tree structure at midnight as per the peak time of yesterday and once cab driver send teh update

1) read the prev location from cache
2) go to that location delete it
3) update the new location as leaf node
4) finally update the driver location in cache.

so we solved the problem
but here we are dealing with tree only so instead of that we can use geohash.

it is similar like apprach but geohash dvide the world into equal parts and each location have some prefix.
so azu=india and if azumz=UP.

we can use no sql DB like redis which supports geohash and if any driver send the location 
suppose UpateDriverLocation(driver_id,lat,lon)
so it calculates teh geohash of new location and update the new location in cache.

getNearbyDrivers(lat,long)
calcualte geohash of location and search List cab=cache.get(geoHash);--egg01
if(size>=10)
return;
else
getNearby rectangle(search in egg02,egg03)like that.

Find all locations within a 5 kilometer radius of a given location, and return the distance to each location:
GEOSEARCH bikes:rentable FROMLONLAT -122.2612767 37.7936847 BYRADIUS 5 km WITHDIST

user--apiGateway--search cabs---LB---appserver----DB


driverapp---update location of driver---redis







