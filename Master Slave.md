Sharding- when actual data cant fit in single machine.
Replica-Queries can't be handled by 1 machine

Master get the write queries and it is resposbility of master to send it to slaves.

in Available system
Master get the request and write it and send the reposne back to user successful. but it is possible mster dies while writng data to slaves.
so system will never be consistent and data will be lost.

Eventual consistency
Master get the request and write it and also write to X slaves and send the reposne back to user successful.
if master dies data will be in 1 slave atleast. Eg- Facebook news feed

Consistent
Master get the request and write it and also write to all slaves and send the response to user.
My writes will be very slow  and latency is very high.

Read request always serve by any random slaves.

