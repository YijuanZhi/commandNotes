# AWS Udemy Learning Course Notes

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
4. S3 Intelligent Tiering
   - Desinged to optimize costs by automatically moving data to the most cost-effective access tier without performance impact or overhead.
5. S3 Glacier
   - Low-cost storage class for data archiving, but slow retrieval speed.
6. S3 Glacier Deep Archive
   - Lowest-cost storage class, retrieval time of 12 hours is acceptable.
