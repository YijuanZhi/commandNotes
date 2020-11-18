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