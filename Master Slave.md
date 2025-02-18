When a website’s traffic surges and the only database available is one master, the database will become overburdened with reading and writing requests.To scale out your application, you need two data sources- one to handle the write operations and the other one to handle the read operations. This is master slave, generally we are writing to master and read from slaves but we can change this also based 
on condition

**Sharding**- when actual data cant fit in single machine.

**Replica**-Queries can't be handled by 1 machine

Master get the write queries and it is resposbility of master to send it to slaves.

**in Available system**
Master get the request and write it and send the reposne back to user successful. but it is possible mster dies while writng data to slaves.
so system will never be consistent and data will be lost.

**Eventual consistency**
Master get the request and write it and also write to X slaves and send the reposne back to user successful.
if master dies data will be in 1 slave atleast. Eg- Facebook news feed

**Consistent**
Master get the request and write it and also write to all slaves and send the response to user.
My writes will be very slow  and latency is very high.

Read request always serve by any random slaves.

If master died so we use leader election algorithm to decide the new master.
