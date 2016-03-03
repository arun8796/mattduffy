==== Simple Queue Service (SQS)
* messages can contain up to 256kb of data in any format
* SQS does NOT guarantee FIFO queues; if your system requires that message order be preserved, you should include sequencing information within your messages.
* SQS always PULLS messages, messages queues never PUSH out
* SQS process of events for an exmple work flow in web app:
 * SQS client asynchronously pulls the task message from the queue
 * retrieves the named file
 * processes the conversion
 * writes the image back to S3
 * writes a "task complete" message to another queue
 * deletes the original task
 * checks for more messages in the worker queue

Does NOT offer FIFO guarantees
Visibilty window of 12 hours
Provides at least once message delivery
256 KB message size (billed at 64kb chunks) 4 billable chunks per 256kb sized message
First 1mm messages per month are free, thereafter, $0.50 per each additional 1mm message
a single request can have from 1 to 10 messages , up to a maximum of 256kb payload
Each 64kb chunk is billed as 1 request, so 1 256kb message will be billed as 4x64kb message requests
Default delete by time is 4 days for unprocessed or unclaimed queue messages

Exam Tips
SQS messages can be delivered many time and in any order (NOT FIFO or idempotent)
visibilty timeout defaults to 30 second, can be	changed with ChangeMessageVisibility api call to up to 12 hours
Use SQS long polling to reduce the number of requests made to the queues when they are empty.  Long polling does not return until a new message is available or the 20 second timeout has been reached.

