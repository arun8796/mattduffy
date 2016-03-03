=== AWS Database Essentials

==== ElastiCache
* two types: Redis and Memchached


==== AWS RDS (relationsal dbs)
* Oracle, MS SQL, MySQL, Postresql, Aurora


==== AWS NoSQL options
* DynamoDB
 * DynamoDB does NOT allow embedded data structures in a nosql document.  Booooo!

==== OLTP vs OLAP
* Online Trnx Processing usually joins records from multiple tables to return 1 (or a small number of rows) at a time
* Online Analytical Processing usually works in large batch modes to reduce big data sets down to summary totals

==== AWS Redshift
* data warehousing service

RDS -> OLTP
DynamoDB -> nosql
Redshift -> OLAP
ElastiCache -> in memory, scalable caching


==== DynamoDB
* fully managed service
* provisioned on SSD for sigle digit millisecond latency
* spread across 3 geographically distinct data centers (not exactly the same as availability zones)
* eventually consisten reads are the default configuration
* Strongly consistent Reads have a gurantee of replication across data centers before returns result.
* DynamoDB Write Capacity Unit: 1 write op per second as a baseline price
		1,000,000 writes per day = 1mm / 24hrs / 60mins / 60secs = 11.2 writes per second
		need to provision 12 DynamoDB Write Capacity Units per day

===== Queries
* finds items in table using only primary key attribute values
* you must provide a hash key attribute name and distinct value to search for
 * optional range key to return multiple items returned
 * ProjectionExpress parameter to only return certain attributes in the result
 * by default, queries are eventually consistent
 * can specify strong consistency

===== Scans
* scans the entire table, then filters out values for desired result
* very slow and expensive operations as table sizes grow; can use up your provisioned throughput

===== Provisioned Throughput
* you must pre-provision the amount of db throughput you think you will need, both read and write
* every read is rounded up to the next largest 4kb block size, starting at 4kb (3kb of data read => is actually 4kb read throughput, 6kb => 8kb)
* eventual consistent reads default to 2 reads per second
* strongly consistet reads are 1 read per second

* all write operations are 1kb of provisioned throughput
* all write operations consist of 1 write per second of provisioned throughput.

question: you have an application that requires 10 items of 1kb eventually consistent reads. What 
should you set read throughput to?
 * calculate read units are needed per item: 1kb / 4kb = 0.25, round to nearest whole number is 1 read unit
 * 1 read item x 10 items = 10 read units
 * using eventual consistency, 10 read units / 2 = 5 units of read throughput

question: you have an application that requires to read 10 items of 6kb per second using eventual consistency. 
What should the read throughput be set to?
 * 6kb => rounds up to 8kb / 4kb = 2 read units per item
 * 2 read units per item * 10 items = 20 read units per second
 * using eventual consistency, 20 read units per second / 2 = 10 read units total per second.

question: you have an application that requires to read 5 items of 10kb per second.  Using eventual consistency,
what should the read throughput be set to?
 * 10kb => rounds up to 12kb / 4 = 3 read units per item
 * 3 read units per item * 5 = 15 read units per second
 * using eventual consistency, 15 read units per second / 2 = 7.5 => rounds up to 8 read units total per second

question: you have an application that requires to read 5 items of 10kb per second. Using strong consistency,
what shoudl the read throughput be set to?
 * 10k => 12kb / 4kb = 3 read units per item
 * 3 read units * 5 items = 15 read units per second
 * 15 read units / 1 = 15 total read units per second

question: you have an application that requires to write 5 items of 10kb per second.  What should the write throughput be set to?
 * each write consists of 1kb of data, you need to write 5 items per second of 10kb per each write
 * 5 items * 10kb = 50 write units per second

question: you have an application that requires to write 5 items of 10.5kb per second.  What should the write throughput be set to?
 * 5 items * 11kb = 55 write units per second


===== DynamoDB API error codes
* if you exceed your provisioned throughput; 400 HTTP Status Code: ProvisionedThroughputExceededException
 * you exceeded your maximum allowed throughput for a table or for one or more global secondary indexes

===== Web Identity Providers for DynamoDB
* authenticate with some service like Facebook, google, etc, using API call to AssumeRoleWithIdentity
 * authenticate with provider
 * recieve a token from ID provider
 * your code calls AWS AssumeRoleWithIdentity API with provider token and specifies the ARN (amazon resource name) of the AIM Role
 * minimum access time is 15mins, default access time is 1hour, can specify readOnly, writeOnly, readWrite access

===== Conditional Writes
* due to eventual consistency and multi datacenter distribution, dirty writes are avoided by clients getting current value and sending write op, DynamoDB checks if value has changed since last read, if not, update, if so, do not update.
* conditional writes are idempotent

===== Atomic Counters
* can use the UpdateItem operation to increment / decrement a value without interfereing with other write operations.
* UpdateItem called on atomic counter updates are NOT idempotent.  They will always update the current value, no matter how many times called.

===== Batch Operations
* BatchGetItem request can retrieve up to 1MB of data which can contain as many as 100 items.
* a single BatchGetItem request can retrieve itesm from multiple tables


DynamoDB section quiz:
You have a motion sensor which writes 600 items of data every minute. Each item consists of 5kb. Your application uses eventually consistent reads. What should you set the read throughput to?
 * 5kb => 8kb / 4kb = 2 read units per item
 * 2 read units per item * [(600/60)=10] = 20 read units per second
 * 20 read units per sec / 2 eventual consistency = 10 read units per sec total

 You have a motion sensor which writes 600 items of data every minute. Each item consists of 5kb. What should you set the write throughput to?
 * 10 items per second * 5kb = 50 write units per sec

You have an application that needs to read 25 items of 13kb in size per second. Your application uses eventually consistent reads. What should you set the read throughput to?
 * 13kb => 16kb / 4kb = 4 read units
 * 4 read units * 25 items = 100 read units per second
 * 100 read units per sec / 2 eventual consistency = 50 read units provisioned throughput needed

You have an application that needs to read 25 items of 13kb in size per second. Your application uses strongly consistent reads. What should you set the read throughput to?
 * 13kb = 16kb / 4kb = 4 read units
 * 4 read units * 25 items = 100 read units per second
 * 100 read units per sec / 1 strongly consistent = 100 provisioned read unit throughput


