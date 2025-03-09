s3- system that allows you to store large files.

Functional requirement

1) files so huge so it cant store in single machine.
2) resume donwoad from whre it failed.

non functional

s3 should be highly available but eventually consistent system.

solution

Divide the file into chunks but what should be the size of chunk.
suppose 50 tb file is there and you divide the chunk into 100 kb so ow may chunks total
=50*10^12 bytes and 100 kb =10^5 bytes
so no of chunks =50 *10^7 chunks=500 million chunks.

so the size should be balanced. by default hdfs (hadoop distributed file system has size =128 MB)

**how uplaod works**

Suppose there is a client who wants to upload a large file. The client requests the app server and starts sending the stream of data. The app server on the other side has a client (HDFS client) running on it.
HDFS also has a NameNode server to store metadata and data nodes to keep the actual data.
The app server will call the name node server to get the default chunk size, NameNode server will respond to it ( say, the default chunk size is 128 MB).
Now, the app server knows that it needs to make chunks of 128 MB. As soon as the app server collects 128 MB of data (equal to the chunk size) from the data stream, it sends the data to a data node after storing metadata about the chunk. Metadata about the chunk is stored in the name node server. For example, for a given file F1, nth chunk - Cn is stored in 3rd data node - D3.
The client keeps on sending a stream of data, and again when the data received by the app server becomes equal to chunk size 128 MB (or the app server receives the end of the file), metadata about the chunk is stored in the name node server first and then chunk it send to the data node.

**Downloading a file**

Similar to upload, the client requests the app server to download a file.
Suppose the app server receives a request for downloading file F1. It will ask the name node server about the related information of the file, how many chunks are present, and from which data nodes to get those chunks.
The name node server returns the metadata, say for File 1, goto data node 2 for chunk 1, to data node 3 for chunk 2, and so on. The application server will go to the particular data nodes and will fetch the data.
As soon as the app server receives the first chunk, it sends the data to the client in a data stream. It is similar to what happened during the upload. Next, we receive the subsequent chunks and do the same.



