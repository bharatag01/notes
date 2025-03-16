
API- List<Restaurant> list=findnearbyReastaurants(lat,lon)

brute force

for every restaurant calcualte the dstance bewtween user location to restaurant and put in min heap.
but it is v.v. slow .

optimization

select * from restaurants where lat<a+x and lat>a-x and long>B-y and long<B+y. 
If I get 100 restaurants return else increase x and y.

divide the world into areas such that each area has 100 restaurnat.

option 1- divide the world into equal parts and search. if it is major reaturants nearby then fine but no then it is very slow.
option 2- divide the world into variable size.

we consider the world as node and divide it further recursively into four nodes untill each node has <=x restaurnats.

class Node
{
topLeft(x,y)
bottoom right(x2,y2)
count of res=
children
}


find restaurnats(loca)
{
node =goToNode(location);
if(node .rest.size>=100)
return
else
List<Node> =findNearbyRectangles()
find x closet restaurnat and return
}

GoToNode(loc)
{
ptr=root
while(ptr has children)
go to child containing loc
}

node max size=1 Kb
and for 100 milion restaurant=100 Gb max



