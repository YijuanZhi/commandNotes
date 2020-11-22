# AWS S3

## IAM: Identity Access Management

### Features
1. Centralized control of your AWS account
2. Shared access to your aws account
3. Granular permissions
4. Identity Federation(like Facebook, LinkedIn)
5. Multifactor authentication
6. Provide temporary access for users/devices and services where necessary
7. Allows you to set up your own password rotation policy
8. Integrates with many different services

### Key Terminology For IAM
1. ***Users***: End users such as people, employees of an organization etc.
2. ***Groups***: A collection of users. Each user in the group will inherit the permissions of the group.
3. ***Policies***: Policies are made up of documents, called Policy documents. The doc is in JSON and give permission s as to what a User/group/Role is able to do.
4. ***Roles***: You create roles and then assign them to AWS resources.

## S3 (Simple Storage Service)

### Features
1. Tiered Storage Available
2. Lifecycle Management
3. Versioning
4. Encryption
5. MFA(Multi-Factory Access) Delete
6. Secure data using Access Control Lists and Bucket Policies

### Key Fundamentals of S3
1. Key: name of the object
2. Value: sequences of data
3. Version ID
4. Metadata
5. Subresources

### Properties
1. S3 is Object-based(file oriented)
2. Files can be from 0 bytes to 5TB
3. Unlimited Storage
4. Files are stored in ***Buckets***
5. S3 is a ***universal*** namespace, names must be unique globally
6. Successful uploads will generate a HTTP 200 status code
7. Read after write consistency for PUTs of new objects
8. Evetual Consistency for overwrite PUTs and DELETEs

### Different S3 Storage Classes
1. S3 Standard 
   - 4*9 availability
   - 11*9 durability
   - Stored redundantly across multiple devices, designed to sustain the loss of 2 facilities concurrently
2. S3 IA(Infrequently Accessed)
   - For data accessed less frequently, but requires rapid access when needed. Changed with retrieval fee.
3. S3 One Zone IA
   - S3 IA but doesn't require the multiple availability zone
   - May not be availiable when needed, 99.5%
4. S3 Intelligent Tiering
   - Desinged to optimize costs by automatically moving data to the most cost-effective access tier without performance impact or overhead.
5. S3 Glacier
   - Low-cost storage class for data archiving, but slow retrieval speed.
6. S3 Glacier Deep Archive
   - Lowest-cost storage class, retrieval time of 12 hours is acceptable.

### Restricting Bucket Access
1.  BUcket Policies - Applies across the whole bucket.
2.  Object policies - Applies to individual files
3.  IAM Policies to Users & Groups - Applies to Users & Groups.

### S3 Encryption 
*Achieved by SSL/TLS*

Encryption at rest(server side) is achieved by:
- S3 managed keys - SSE - S3
- AWS key management service, managed keys - SSE - KMS
- Server side encryption with customer provided keys -SSE-C
Client Side encryption: encrypted by client and uploaded to S3.

# Versioning
1. Stores all versions of an object(including all writes and even if you delete an object, which will just create a delete marker on top of that file).
2. Once Version is enabled, it cannot be disabled, only suspended, which will not record future version update but keep the previous version updates.
3. MDA delete capability.

### S3 Object Lock
1. Governance mode: Users can't overwrite or delete an object servion or alter its lock settings unless they have special persimissions.
2. Compliance mode: a protected object versioncan't be overwrittern or deleted by any user, including the root user in your AWS account.
3. S3 Gacier Vault lock: specify conteols such as WORM in a vault lock policy and lock the policy from future edits. Can be applied or removed anytime.

### S3 Performance
1. `mybucketname/foler1/subfoler1/myfile.jpb` the prefix is `/folder1/subfolder1`
2. Can achieve a high number of request per second per prefix: 3,500 put/copu/post/delete and 5,500 get/head.
3. Can get better performance by spreading read across different prefixes.
4. If using SSE-KMS, there is KMS limits
5. Use multipar tuploads to increase performance when uploading files to S3, recommanded for any files over 100mb, must be used for any file over 5gb. 
6. Use S3 byte-range fetches to increase performance when downloading files to S3.

### S3 Select
Allows use a simple SQL expression to return on ly the data from the store instead of retriving the entire object, which can imporves the performance of underlying application.

### Cloud Front
1. Edge location: the location where content will be cached. This is separate to an AWS region/AZ.
2. Origin: the source of files that CDN will distribute. Can be S3 Bucket, EC2 Instance, an Elastic Load Balancer or Route 53.
3. Distribution: name given the CDN which consists of a collection of Edge locations.
   - Web Distribution: used for websites
   - RTMP: dor adobe media streaming.
4.  Objects are cached for the lofe of the TTL(Time to Live)
5.  Objects can be invalidated for limited access to other objects.

### S3 Storage Gateway
1. File Gateway: for flat files, stored directly on S3.
2. Volume Gateway:
   - Stored Volumes: Entire Dateset is stored on site and is asynchronously backed up to S3.
   - Cached Volumes: Entire dateset is tored on S3 and the most frequently accessed data is cached on site.
3. Gateway Virtual tape Library

### Athena vs Macie
1. Athena:
   - I tis an interactive query service
   - Allows you to query data located in S3 using standard SQL for analysis purposes.(log, report....)
   - Serverless
2. Macie:
   - Uses AI to analyze data in S3 and helps identify PII(Personal Identifiable Information)
   - Can be used to analyse CloudTrail logs for syspicious API acticity
   - Dashboards, reports and alerting
   - Prevent ID theft


# AWS EC2(Elastic Compute Cloud)

