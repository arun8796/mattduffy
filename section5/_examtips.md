=== Exam tips: S3

* S3 versioning cannot be disabled for a bucket once it is enabled
 * it can however be suspended
* S3 is 99.99% reliable and 99.999999999% durable
 * Reduced Reliability Storage option can be used for cheaper, less durable storage at 99.99% reliability and 99.99% durability
* S3 Lifecycle management can be used in conjunction with versioning
* S3 Encryption: upload/download via encrypted S3 endpoints
 * encrypt data at rest
 * manage your own keys via AWS KMS
 * Amazon S3 manage keys for you
 * or provide your own keys (AWS never has access to your keys)
 * Advanced Encryption Standard (AES 256)
* Minimum file size: 1 Byte, maximum file size: 5 TB
* No limit to number of objects stored in S3
* Maximum file size for a PUT operation is 5 GB


use X.509 certificates to secure access to bucket objects

All buckets are private by default

Integrates with IAM via roles

Allow multipart uploads (start stop restart uploads)

Spread across multipe availablity zones via eventual consistency.

S3 bucket URL endpoint format:
	https://s3-us-west-1.amazonaws.com/thefappingguru/testtickle/P7130050.JPG
	s3-us-west-1.	{ S3 region where bucket was created }
	amazonaws.com/	{ aws domain }
	thefappingguru	{ S3 unique bucket name for region }
	testtickle		{ folder inside bucket }
	P7130050.JPG	{ S3 object }

S3 website URL endpoint format:
	https://thefappingguru.s3-website-us-west-1.amazonaws.com
	thefappingguru. { folder name inside unique per region bucket name }
	s3-website-     { the unique S3 Bucket hosting a static website }
	us-west-1       { region where the bucket was orginally created }
	amazonaws.com   { aws domain }


CORS policy and permissions
* create a CORS policy file in the S3 bucket that contains the assets that will be loading into the main website (not putting the CORS policy into the main website S3 bucket - common mistake).

==== S3 Versioning
Once versioning is enabled on a bucket, it cannot be disabled, only temporarily suspended.  Versions of S3 objects each incur storage charges.  Only way to get rid of versioning is to delete all the versions in a bucket, delete the bucket, recreate the bucket with the same name.

==== Lifecyle Management & Glacier
* can be enabled on buckets with or without S3 Versioning enabled

==== CloudFront

