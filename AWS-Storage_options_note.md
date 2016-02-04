#On-permise

Each of these traditional storage options differs in performace, durability, and cost, as well as in their interfaces. Architects consider all these factors when identifying the right storage solution for the task at hand, Notably, most IT infrastructures and application architectures employ multiple storage technologies in concert, each of which has been selected to satisfy the needs of a particular subclass of data storage technologies in concert, or for the storage of data at a particular point in its lifecycle, These combinations from a hierarchy of data storage tiers.

#AWS cloud storage options:

- Amazon S3: scalable storage in the cloud
- Glacier: Low-cost archive storage in the cloud
- EBS: Persistent block storage volumes for EC2 virtual machines
- EC2 instance storage: temporary block storage volumes for EC2
- _AWS Import/Export_: Large volume data transfer
- Storage Gateway: integrates on-premises IT environments with cloud storage
- CloudFront Global content delivery network(CDN)
- SQS: message queue service
- RDS: Managed relational database server for MySQL, Oracle, and MSSQL
- _DynamoDB_: Fast, perdictable, highly-scalable _NoSQL_ data store
- ElastiCache: In-memory caching service
- _Redshift_: Fast, powerful, full-managed, _petabyte-scale data warehouse service_
- Databases on EC2L Sekf-managed database on an EC2


##S3

- Highly-scalable, reliable, and low-latency data storage infrastructure at very low costs
- an Object from _1byte to 5 terabytes_ of data each
- number of the objects you can sotre in _an S3 buckets is virtually unlimite_
- supporting encryption at rest and provide multiple mechnisms to provide fine-grained control of access to resources
- supporting separate clients or application threads
- provides data lifecycle management capabilities, allow users to define rules to _automatically archive S3 data to Glacier, or to delete data at end of life_

_Ideal Usage Patterns_
- storage and distribution of static web content and media.
- each object has a unique HTTP URL
- serve as an __origin store__ for a CND, such as CloudFront.
- No storage provisioning is required, works well for fast growing website hosting data intensive
- as a data store for computation and large-scale analytics
- as a highly durable, scalable, and secure solution for backup and archival of critical data, and to provide disaster recovery solutions
- _S3's version_ capability is available to protect critical data from inadvertent deletion

_Peerformance_
- acces from within EC2 in the same region is fast, designed so that server-side latencies are insignificant relative to Internet latencies.
- built to scale storage,requests, and users to support a virtually unlimited number
- to speed access to relevant data, pair SE with a database. S3 stores the actual infroamtion and the database serves as the repository for associated metadata. S3 as a DISK HOST

_Durability and Availability_
- automaticlly and synchronously data acrosss both _multiple devices and multiple facilities within one region_
- Error correction is built-in, and no single points of failurei(in two facilities).
- 99.999999999%(11 nines) durability per object and 99.99% availability over a one-year period.
- versioning can protected from application failures and unintended deletions
- versioning can enable with Multi-Factor Authenticantion Delecte: valid AWS account credentials + six-digit code(OTP from physical token device)
- Reduced Redundancy Storage(RRS):
    - lower level of durability at lower cost
    - still stored on multiple devices in multiple location
    - 99.99% durability oer object over a given year
    - still 400 times more durability than a typical disk drive
    - for noncritical data that can be reproduced easily if needed, media or image

_Cost Model_
- pay only for what you use and no minimum fee
- pricing components:
    - storage(/GB/month)
    - data transfer in or out(/GB/Month)
    - requests(/K requests/Month)
    - free usage tier: up to 5G

_Interfaces_
- standards-based REST
- SOAP web services APIs
- SDK ingerated AWS Command Line provide high-level, Linux-Like S3 file commands for common operations, such as ls, cp, mv, sync, etc.
- Web base console

_Anti-Patterns_
- File system — Amazon S3 uses a flat namespace and isn’t meant to serve as a standalone, POSIX-compliant file system.
- Structured data with query — Amazon S3 doesn’t offer query capabilities: to retrieve a specific object you need to already know the bucket name and key.
- Rapidly changing data — Data that must be updated very frequently might be better served by a storage solution  with lower read / write latencies, such as Amazon EBS volumes, Amazon RDS or other relational databases, or Amazon DynamoDB.
- Backup and archival storage — Data that requires long-term encrypted archival storage with infrequent read access may be stored more cost-effectively in _Amazon Glacier_.
- Dynamic hosting—While Amazon S3 is ideal for websites with only static content, dynamic websites that depend on database interaction or use server-side scriptingshould be hosted on Amazon EC2.

