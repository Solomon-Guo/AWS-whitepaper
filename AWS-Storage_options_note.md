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
- serve as an ###origin store### for a CND, such as CloudFront.
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

###Ideal Usage Patterns###
- archiving offsite enterprise information,media assets, research and scientific data, digital perservation and magnetic tape replacement

###Performance###
- low-cost
- for data infrequently accessed and long lived
- jobs typically complete in 3 to 5 hours

###Durability and Availability###
- 99.999999999%(11 nines) per year period durability
- redundantly in multiple facilities
- on multiple devices within each facility
- data integrity checks is regualr, systematic, and automatically self-healing

###Cost Model###
- pay for what you use and no minimum fee
- storage(/GB/Month)
- data tranfer out(/GB/Month)
- requests(per thousand UPLOAD and RETRIEVAL requests per month)
- you can retrieve up to 5% of your average monthly storage(pro-rated daily) for free each month. if you choose to retrieve more than this amount of data in a month, you are charged an additional(per GB) retrueval fee. there is also a pro-rated cjarge(per GB) for items deleted prior to 90 days.

###Scalability and Elasticity###
- scales to meet your growing and often _unperdictable_ storage requirements.
- A single archive is limited to 4TB
- no limit to the total amount of data you can store in the service

###Infterfaces###
- Glacier APIs provide both management and data operations
- native, standards-based REST web service interface, as well as Java and .NET SDKs.
    - the AWS managemant console or Glacier API can be used to _create oraganize the archives in Glacier_. API can upload and retrieve archives, monitor the status of your jobs and also configure your vault to send you a notification via SNS when you jobs complete.
- can used as a storage class in S3 by using object lifecycle management to provide automatic, policy-driven atchiving from S3 to Glacier
- Using Glacier as a storage class in S3, you use the S3 APIs
- Using "native" Glacier, you use the Glacier APIs
- Objects archived to Glacier via S3 can only be listed and retrieved via the S3 APIs or the AWS management console ( they are not visible as archives in an Glacier vault)

###Anti-Patterns###
- Rapidly changing data - Data that must be updated very frequently might be better served by a storage solution with lower read/write latencies, such as Amazon EBS or a database.
- Real time access—Data stored in Amazon Glacier is not available in real time. Retrieval jobs typically require 3-5 hours to complete, so if you need immediate access to your data, Amazon S3 is a better choice.


## EBS
provides durable block-level storage for use with EC2
- off-instance
- network-attached storage(NAS)
- persistes independently from the running life of a single EC2 instance
- like a physical hard drive
- use EBS volume to boot an EC2 instance( EBS-root AMIs only)
- attach multiple EBS volumes to a single EC2 instance
- attache to only one EC2 at any point in time
- create point-in-time snapshots of volumes, which are persisted to S3
- snapshots can be used as the starting point for new EBS volume, and to protect data for long-term durability
- snapshot can be used to instantate as many volumes as you wish
- snapshot can be copied _across AWS regions_, for geographical expansion, data center migration and disaster recovery
- EBS volumes range from 1 GB to 1 TB, and are allocated in 1 GB increments

###Ideal Usage Patterns###
- for data that changes relatively frequently and requires long-term persistence
- particaularly well-suited for use as the primary storage for a database or file system, or access to raw block-level storage
- providesed IOPS volumes are particularly well-suited from use with database application that require a high and consistent rate of random disk reads and writes

###Performance###
- Standard volumes and Provisioned OPS volumes. They differ in performance characteristics and pricing model
- Standard volumes:
    - offer cost effective storage for moderate or bursty I/O requirements.
    - designed to deliver approximately 100 I/O operations per second(IOPS)
    - well-suited for use as boot volumes, burst capability at start-up times.
- Provisioned IOPS volumes:
    - feliver perdictable, high performance for I/O intensive workoads such as databases
    - specify an IOPS tate when ctrating a volume, and then EBS provisons that rate for the lfetime of the volume
    - up tp 2,000 IOPS per Provisioned IOPS volume
    - stripe multiple volumes together to feliver thousandsof IOPS per EC2
- EBS volumes are NAS device, network I/O performed intensived
- fully utilize the Provisioned IOPS on an EBS volumes:
    - selected EC2 instance types as EBS-optimized instances
    - deliver dedicateed throughput between EC2 and EBS
    - options between 500 Mbps and 1,000 Mbps depending on the instance type
    - deliver within 10% of the Provisioned IOPS performance 99.9% of the time

###Durability and Availability###
- EBS volumes data is replicated across _multiple servers in a single AZ_ to prevent the loss of data from the failure of any single component
- durability depend on both the size of volume and data that changed since last snapshot
    - 20GB or less of modified data since most recent snapshot can expect _an annual failure rate(AFR) between 0.1% and 0.5%_
    - more that n20GB of unmodified data since last snapshot _expect higher failure rates that are roughly proportional to the increase in modified data_
- frequently create snapshots of EBS volumes to maximize both durability and availability
- available cross AZs in a region
- EBS snapshots can also be copied from one region to another, and easily be shared with other user accounts
- easy-to-use disk clone or disk image mechanism for backup, sharing, and disaster recovery

###Cost Model###
3 components:
- provisioned storage
- I/O request
- snapshot storage

EBS standard volumes:
    - per GB-month
    - per million I/O requests
EBS Provisioned IOPS volumes:
    - per GB-month
    - per Provisioned IOPS-month
for both:
    - snapshots are charged per GB-month
    - EBS snapshot copy is charged for the data transfered between regions
    - the standard EBS snapshots chatges in the destination region
    - *for EBS volumes, you are charged for _provisoned(allocated)_ storage, whether or not actually use it*
    - *for EBS snapshots, you are charged only for _actually used(consumed)_*
    - snapshots are incremental and compressed, so generally, snapshot is much less than volumes consumed
    - _no charge_ for transferring information among the various AWS storage offerings

###Scalability and Elasticity###
- using console or APIs, EBS volumes can seasily and rapidly be provisioned and released to scale in and out with your totoal storage demands
- EBS volumes resized:
    - create and attach a new EBS volume and using it together with existing ones
    - expand the size of a single EBS volume by using a snapshot:
        1. detach the original EBS volume
        2. create a snapshot and store into S3
        3. create a new EBS volume from the snapshot, but specify a larger size than the original volume
        4. attach the new, lager volume to instance in place of the original
        5. an OS-level utility must also be used to expand the file system
        6. Delete the original EBS volume

###Interfaces###
- API in both SOAP and REST formats
- create, delete, describe, attach, and detach volumes for instances
- create, delete, and describe snapshots from EBS to S3
- copy snapshots from one region to anoter
- Management console also
- no AWS data API for EBS
- instrad, EBS presents a block-device infterface to EC2 instance, like local disk drive

###Anti-Patterns###
EBS using to persisted beyond the life of a single EC2 instance
not for
- Temporary storage: such as scratch disks, buffers, queues, and caches, consider using local instance store volumes, SQS or ElastiCache(Memcached or Redis)
- highly-durable storage: use S3 or Glacier
- Static data or web content: use S3


##EC2 instance store volumes
- also called ephemeral(short life) drivers, provide temporary block-level storage for many instance tupes.
- perconfigured and pre-attached block of disk storage
- varies by EC2 instance type
- large instances tend to provide both more and larger instance store volumes
- some instance types only EBS storage only(t1)
- in addition, the storage-optimized EC2 instance family provides special-purpose instance storage targeted to specific use cases
    - HI1 provide very fast solid-state drive(SSD)-backed instance for over 120,000 random read IOPS, and are optimized for very high random I/O performance and low cost per IOPS
    - HS1 for very high storage density, low storage cost, and high storage density, low storage cost, and high sequential I/O performance.

###Ideal Usage Patterns###
- local instace store volumes are ideal for temporary storage of inforomation that is continually changing
    - buffers
    - caches
    - scratch data
    - other temporary content
    - data replicated across a fleet of instances, such as a load-balanced pool of web server
    - boot device
- cannot be detached or attached to another instance
- High I/O instances provide instance store volumes backed by SSD, and are ideally suited for many high performance database workloads

###Performance###
- non-SSD-based instance store works like physical hard disk
- To increase aggregate IOPS, or to improve sequential disk throughput, multiple volumes can be grouped together using RAID 0 ( disk striping) software.
- High I/O Instances 10K to 100K low-latency, random 4KB random IOPS
- High Storage Instances delivering 2.6GB/sec of sequential read and write performance when using a block size of 2MB

###Durability and Availability###
- EC2 lical instance store volumes are _not intended to be used as durable disk storage._
- persists only during the lilfe of the associate EC2 instance
- data on instance store volumes is _persistent across orderly instance reboot_
- stopped, re-started, terminates, or fails, all data on the instance store volumes is _LOST_.

###Cost Model
- any local instance store volumes, if the instance tupe provides them
- no additional charge for data storage on local instance store volumes
- data transferred to and from EC2 instance store volumes from other AZ or outside of an EV2 region may incur data transfer charge(In or out AZ/Region may charge)

###Scalability and Elasticity
- number and storage capacity of EC2 local instance store volumes are fixed and defined by the instance type
- you can't increase or decrease the number of instance store volumes on a single EC2 instance
- use S3 or EBS

###Interfaces
- no separate management API for EC2 instance store volume
- using the block device mapping feature of the EC2 API and Console
- cannot create or destroy, but can control whether or not they are exposed to the instance, and what device name is used
- no separate management API for instance store volumes
- like EBS volume, like local hard disk

###Anti-Patterns
- persistent storage
- relational database storage
- shared storage: dedicated to a single EC2 instance, and cannot be shared with other systems or users
- Snapshots

##AWS Import/Export
accelerates moving large amounts of data into and out of AWS using portable storage devices for transport.
- using A,azon's high-speed internal network
- bypassing the Internet
- supoorts importing and exporting for EBS snapshots, S3 buckets, and Glacier vaults

###Ideal Usage Patterns###
- In general, if loading your data over the Internet would take a week or more, you should consider using AWS import/export
- initial data upload to AWS
- content distribution
- regular data interchange ti/from your customers or business associates
- transfer to S3 or Glacier for off-site backup and archival storage
- quick retrieval of large backups from S3 or Glacier for disaster revocery

###Performance###
- each AWS Import/Export station is capable of loading data at over 100MB oer second
- unoist cases the rate of the data load will be bounded by a combination of the read or wirte speed of your protable storage device and, for S3 data loads the average obkect(file) size

###durability and Availabiliry###
99.999999999%(11 nines) durability. becuase data is import to EBS snapshots, S3, Galcier

###Cost model###
- you pay only for what you use
- pricing components
    - a oer-devices fee
    - a data load time charge(per data-loading-hour)
    - possible return shipping charges
    - For the destination storage(EBS Snapshot, S3, and Glacier)

###Scalbility and Elasricity###
- limited only by the capacity of the devices that you send to AWS
    - S3: 5TB per object
    - Glacier: 4TB per single archive
    - the aggreate total amount of data the can be imported is virtually unlimited
    - Only limited for per object/file, total capacity is not limited

###Interfaces###
- submit an AWS Import/Export job for each storage device shipped
- each job request requires a manifest file, a YAML-formatted text file that contains a set of key-value paire that supply the required information
    - your device ID
    - secret access key
    - return address
- job can be created using a command line tool, SDK, or a mative REST API
- the job request is tied to the storage device through a signature file
    - in the root directory(S3)
    - by a barcode taped to the device(EBS and Galcier)

###Anti-Patterns###
- not suit for sunply data that is more easily trasferred over the Internet


##AWS Storage Gateway##
connects an on-premises software appliance with cloud-based storage to provide seamless and secure intrgaration between an oranization's on-permises IT environment and AWS's storage infrastructure.
- securely store dat a tothe AWS cloud for scalable and cost-effective storage
- supports industry-standard storage protocoles
- provides low-lateency performance by maintaining frequently access
- serve as a cloud-hosted solution, together with EC2, that mirrors your entire production environment
- create either gateway-cached or gateway-stored volumes that can be mounted as iSCSI devices by your on-permises applications
- _Gateway-cached_ volumes allow you to utilize S3 for your primary data, while reretaining some portion of it locally in a cache for frequently accessed data.
    - minimize the need to scale your on-premises storage infrastructure, orivudubg your applications with low-lateency access to access data
    - sotage volumes up tp 32TB and mount them as iSCSI from on-permises application servers
    - Data written to these volumes is stored in S3, with only a cache of recently written and recently read data stored locally on-permises storage hard ware.
- _Gateway-stored_ stored your primary data locally, while asynchronously backing up that data to AWS
    - providing durable, off-site backups
    - volumes up to 1TB and mount as iSCSI devices
    - Date written to gateway-stored volumes is stored on-permises storage hardware, and asynchronously backed up tp S3 in the from of EBS snapshots
- Take care the differ between them, one is cached, for utilize on-permises storage. Anthoer is stored, using for backup

###Ideal Usage Patterns
- corporate file sharing
- enabling existing on-permsises backup store on S3
- disaster recovery
- data mirroring to cloud-based compute resources

###Performance
- soeed and confiuration of your underlying local disks
- the network bandwidth between your iSCSI initiator and gateway VM
- the amount of local storage allocated to the gateway VM
- the bandwidth between the gateway VM and S3
- for gateway cached volumes,it's important that you provide enough local cache storage to store your recently accessed data
- (heavy depend) your internet bandwidth to speed up the upload of your on-premises application data to AWS
- _Only uploads data htat has changed_

###Durability and Availability
- on-permises data upload ti S3, so depend on S3, check S3 setion for detail

###Cost model
4 pricing components:
- gateway usage(per gateway per month)
- snapshot storage usage(per GB per month)
- volume storage usage(per GB per month)
- data transfer out(oer GB per month)

###Scalability and Elasticity
- in both gateway-cached and gateway-stored volume configurations, AWS Storage Gateway stores data in S3(scalability and elasticity automatically)
- 所以Storage gateway严重依赖S3

###Interfaces
- Console can be use to download Storage Gateway VM image
- select between a gateway-cached or gateway-stored configuration
- Allocate local storage to your installed on permises gateway from direct attached storage(DAS),NAS, or SAN
- Activate your on-permises by associating your gateway's IP address with your AWS account and select an AWS region for your gateway to store uploaded data
- Use the Console to create AWS storage Gateway volumes and attach these volumes as iSCSI devices to your on-permises application servers

###Anti-Patterns
- Database storage: EC2 and EBS volumes are a natural choice for database storage and workloads
