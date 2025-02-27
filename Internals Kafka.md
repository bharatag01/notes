Persistent queue is called kafka. 
retention period - atleast x days event shouldnt be loose.

Topics - we need a way to segregate diff kind of events. aparticular type of event is called topic.
kafka broker- kafka internally works on mutiple machines. the machine which is running kafka called broker.'
partition- diving a topic between multiple brokers. so it can configure at the time of set up hw many partition required for particular topic.so it can consume load.
internally some algo divide the data between the partition like consistent hashing.

replica- it is exact copy of each partition to avoid the single point of failure. supoose broker is down so data should not be lost.

the messages in same partition should be in FIFO but in other partition it can be in any order. so if we want the messages should be consumed in fifo so put in same partition like for
particual userid all msessage should be in same partition.

consumer group - which consume the message.
offset - kafka internally maintains a map whidh define each yopic consumed how many messages. 
