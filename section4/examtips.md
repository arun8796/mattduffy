=== Lecture 23: Elastic Load Balancers

* multiple SSL certificates can terminate on a single ELB
* ELBs are NOT free, you are charged by the hour and by the Gb of data transferred
* ports available to use on ELB include: 25(smtp), 80(http), 443(https), and 1024-65535(custom)
* Service that ARE free include: Autoscaling, Beanstalk, and CloudFormation

==== HTTP Codes Exam tips
* 200 - the request has succeded
* 3xx - the request has been redirect to another URI
* 4xx - client error (page not found, etc)
* 5xx - internal server/application error

==== SDK Exam tips
* know the available sdks
 * iOS, android, Javascript (Browser)
 * Java, .Net, Node.js, PHP, Ruby, Python
* Default region is usually us-east-1, some have default and need to be set before used.


