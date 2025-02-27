suppose data fits in single machine but still it has master and slaves also to avoid the single point of failure.
So how app server knows which is master if master changed. they can write to slave which is wrong.

**idea 1**

we can use directory server which has ip add of master if master changed so also it update the master add . so to write in db app server will chek with dir server who is master and after 
that they can write.

cons- add network hop dir server . every time for write need to check with dir server.
dir server is single point of failure.

**idea 2**

use zookeper. whch is single machine has set of files which is called nodes.
zookeper nodes are two types-
1) ephemeral nodes
2) persisnt nodes

ephermal nodes - owner can write the file.no one can access the file. owner has to send heatbeat to zookeepr that i am alive. 

so how it works.

every db instance (all replicas) try to become master and whoever can write it can become master and otehr are slaves . and master has to send heartbeat if zookeper doesnt get hearbeat 
in evry x sexonds so it deletes the file and owner and again all the slaves wll try to become master.

when master change so zookper inform app servers that master is changed. so all app server and slaves are in watchlist of zookeeper. 

we will have master slave architecture in zookper as well to avoid teh single point of failure.
