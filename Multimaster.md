Consider a system of multi-master machines i.e. every machine in the system is a master. There is no slave. In the Multi-Master system, there is no need for reserved machines. Every single machine is a master.

**Read, Write Operations**
In Multi-Master, you can have tunable consistency. You can configure two variables: R and W.
R represents the minimum number of read operations required before a read request is successful.
W represents the minimum number of write operations required before a write request is successful.
Let X be the replication level and x=3

if R+W>X- consistent
W=3 , r=1 if any system is down while writing so it will be unavailable but consistent.

R+W<=X - Available
r=1 , w=1 read from 1 and write to other , system is available but not conistent.

Eg- Cassandra nosql DB uses multi master and facebook messenger uses cassandra db.
For distributed applications, Multi-Master is generally a better choice than Master-Slave because it provides:
✔ High availability (no single point of failure)
✔ Write scalability (multiple nodes can handle writes)
✔ Low latency (writes can happen in multiple regions)
