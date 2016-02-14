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


##Glacier

- an extremely low-cost storage service, highly secure, durable, and flexible for _data backup and archival_.
- $0.01 per gigabyte per month
- customer don't need to worry about :
    - capacity planning
    - hardware provisioning
    - data replication
    - hardware failure detection and repair
    - time-consuming hardware migrations
- stire data ub Gkacuer as arcguvers
- organize your archivers in vaults, access control useing AWS Identity and Access Management servier(IAM)
- designed for use with other AWS services
- seamlessly move data between S3 and Glacier using _data lifecycle policies_
- can also use _AWS Import/Export_ to accelerate moving large amounts of data into Glacier using portable storage devices for transport

__Ideal Usage Patterns__
- archiving offsite enterprise information,media assets, research and scientific data, digital perservation and magnetic tape replacement

__Performance__
- low-cost
- for data infrequently accessed and long lived
- jobs typically complete in 3 to 5 hours

__Durability and Availability__
- 99.999999999%(11 nines) per year period durability
- redundantly in multiple facilities
- on multiple devices within each facility
- data integrity checks is regualr, systematic, and automatically self-healing

__Cost Model__
- pay for what you use and no minimum fee
- storage(/GB/Month)
- data tranfer out(/GB/Month)
- requests(per thousand UPLOAD and RETRIEVAL requests per month)
- you can retrieve up to 5% of your average monthly storage(pro-rated daily) for free each month. if you choose to retrieve more than this amount of data in a month, you are charged an additional(per GB) retrueval fee. there is also a pro-rated cjarge(per GB) for items deleted prior to 90 days.

__Scalability and Elasticity__
- scales to meet your growing and often _unperdictable_ storage requirements.
- A single archive is limited to 4TB
- no limit to the total amount of data you can store in the service

__Infterfaces__
- Glacier APIs provide both management and data operations
- native, standards-based REST web service interface, as well as Java and .NET SDKs.
    - the AWS managemant console or Glacier API can be used to _create oraganize the archives in Glacier_. API can upload and retrieve archives, monitor the status of your jobs and also configure your vault to send you a notification via SNS when you jobs complete.
- can used as a storage class in S3 by using object lifecycle management to provide automatic, policy-driven atchiving from S3 to Glacier
- Using Glacier as a storage class in S3, you use the S3 APIs
- Using "native" Glacier, you use the Glacier APIs
- Objects archived to Glacier via S3 can only be listed and retrieved via the S3 APIs or the AWS management console ( they are not visible as archives in an Glacier vault)

__Anti-Patterns__
- Rapidly changing data - Data that must be updated very frequently might be better served by a storage solution with lower read/write latencies, such as Amazon EBS or a database.
- Real time access—Data stored in Amazon Glacier is not available in real time. Retrieval jobs typically require 3-5 hours to complete, so if you need immediate access to your data, Amazon S3 is a better choice.


##AWS Import/Export
accelerates moving large amounts of data into and out of AWS using portable storage devices for transport.
- using A,azon's high-speed internal network
- bypassing the Internet
- supoorts importing and exporting for EBS snapshots, S3 buckets, and Glacier vaults

__Ideal Usage Patterns__
- In general, if loading your data over the Internet would take a week or more, you should consider using AWS import/export
- initial data upload to AWS
- content distribution
- regular data interchange ti/from your customers or business associates
- transfer to S3 or Glacier for off-site backup and archival storage
- quick retrieval of large backups from S3 or Glacier for disaster revocery

__Performance__
- each AWS Import/Export station is capable of loading data at over 100MB oer second
- unoist cases the rate of the data load will be bounded by a combination of the read or wirte speed of your protable storage device and, for S3 data loads the average obkect(file) size

__durability and Availabiliry__
99.999999999%(11 nines) durability. becuase data is import to EBS snapshots, S3, Galcier

__Cost model__
- you pay only for what you use
- pricing components
    - a oer-devices fee
    - a data load time charge(per data-loading-hour)
    - possible return shipping charges
    - For the destination storage(EBS Snapshot, S3, and Glacier)

__Scalbility and Elasricity__
- limited only by the capacity of the devices that you send to AWS
    - S3: 5TB per object
    - Glacier: 4TB per single archive
    - the aggreate total amount of data the can be imported is virtually unlimited
    - Only limited for per object/file, total capacity is not limited

__Interfaces__
- submit an AWS Import/Export job for each storage device shipped
- each job request requires a manifest file, a YAML-formatted text file that contains a set of key-value paire that supply the required information
    - your device ID
    - secret access key
    - return address
- job can be created using a command line tool, SDK, or a mative REST API
- the job request is tied to the storage device through a signature file
    - in the root directory(S3)
    - by a barcode taped to the device(EBS and Galcier)

__Anti-Patterns__
- not suit for sunply data that is more easily trasferred over the Internet
