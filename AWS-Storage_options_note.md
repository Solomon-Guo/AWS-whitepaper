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

###Ideal Usage Patterns
- storage and distribution of static web content and media.
- each object has a unique HTTP URL
- serve as an ###origin store### for a CND, such as CloudFront.
- No storage provisioning is required, works well for fast growing website hosting data intensive
- as a data store for computation and large-scale analytics
- as a highly durable, scalable, and secure solution for backup and archival of critical data, and to provide disaster recovery solutions
- _S3's version_ capability is available to protect critical data from inadvertent deletion

###Performance
- acces from within EC2 in the same region is fast, designed so that server-side latencies are insignificant relative to Internet latencies.
- built to scale storage,requests, and users to support a virtually unlimited number
- to speed access to relevant data, pair SE with a database. S3 stores the actual infroamtion and the database serves as the repository for associated metadata. S3 as a DISK HOST

###Durability and Availability
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

###Cost Model
- pay only for what you use and no minimum fee
- pricing components:
    - storage(/GB/month)
    - data transfer in or out(/GB/Month)
    - requests(/K requests/Month)
    - free usage tier: up to 5G

###Interfaces
- standards-based REST
- SOAP web services APIs
- SDK ingerated AWS Command Line provide high-level, Linux-Like S3 file commands for common operations, such as ls, cp, mv, sync, etc.
- Web base console

###Anti-Patterns
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


##CloudFront
- CDN is a web service for content delivery
- your website's dynamic, static, and streaming content available from a global network of edge locations
- supports all files that can be served onver HTTP
- optimized to work with other AWS services

###Ideal Usage Patterns
- ideal for distribution fo frequently-accessed static content that benefites from edge deliverry
- popular website images, videos, media files or software download
- deliver dynamic web applications over HTTP
- stream audio and video files to web browsers and mobile devices

###Performance
- designed for _low-lateency and high-bandwidth_ delivery of content
- End users get both:
    - lower latency: the time it takes to load the first byte of the object
    - high sustained data transfer rates: to deliver popular objects to end user at scale

###Durability and Availability
- CDN is an edge cache, does not provide durable storage
- provides high availability by using a distributed global network of edge locations
- increased reliability and availability because there is no longer a central potin of failure
- copies of your files are now held in edge locations around the world

###Cost Model
- no long-term contracts or required minimum monthly commitments
- 2 pricing components:
    - regional data transfer out(per GB)
    - requests(per 10,000)
- often cheaper(as well as faster) to deliver content from S3 through CloudFront rather than directly from S3

###Scalability and Elasticity
- seamless scalability and elasticity
- the service automatically responds as demand increases or decreases without any intervention from you
- CloudFront also uses multiple layers of caching at each edge location and collapses simultaneous request for the same object before contacting your origin server

###Interfaces
- Console
- CloudFront API

###Anti-PAtterns
- Programmatic cache invalidation: while CloudFront supports cache invalidation, AWS recommends using object versioning rather than programmatic cache invalidation
- Infrequently requested data


##Simple Queue Service(SQS)
- reliable, highly scalable
- hosted message queuing service
- temporary storage
- delivery of short(up to 64KB) text-based data message
- a temporary dat repository for messages that are waiting for processing
- supports a virtually unlimited number of queues and supports unordered, at-least-once delivery of messages
使用 Amazon SQS,您可以分离应用程序的组件以便其独立运行,Amazon SQS 同时还可以简化组件间
的消息管理。分布式应用程序的任何组件均可将消息存储在拥有故障保护功能的队列中。消息可包含高达
256 KB 的任何格式文本。任何组件均可在之后利用 Amazon SQS API 以编程方式检索消息。对于大于
256 KB 的消息,可以通过使用 Amazon S3 来存储更大负载且适用于 Java 的 Amazon SQS 扩展客户端
库进行管理。
队列在产生并保存数据的组件以及接收数据进行处理的组件之间起到缓冲作用。这就意味着,如果产生消
息的组件工作速度快于接收处理消息的组件,或如果产生消息的组件或接受处理消息的组件仅间歇性地连
接到网络,队列可解决因此而产生的问题。
Amazon SQS 确保每条消息至少传送一次,并且支持与同一队列交互的多个读取器和写入器。单个队列
可由多个分布式应用程序组件同时使用,无需这些组件互相协作以共享队列。
Amazon SQS 设计为始终可用并传送消息。因此而放弃的一项功能是不保证消息的先进先出传送。对许
多分布式应用程序而言,每条消息均可独立存在,并且只要所有的消息得以传送,次序并不重要。如果您
的系统要求保留次序,您可以在每条消息中放置序列信息,以便队列返回消息时将其重新排序。


###Ideal Usage Patterns
- suited to multiple application components must communicate and coordinate their work in a loosely couple manner
- in producer-consumer scenarios where some componets may work faster or slower than others
- the number of interacting components changes with time or load

###Performance
- a distributed queuing system that is optimized for horizontal scalability
- not for single-threaded sending or receiving speeds
- a single client can send or receive SQS messages at rate of about 5 to 5-50 message per second
- can be achieved by requesting multiple messages( up to 10) in a single call

###Durability and Availability
- messages stored in SQS are highly durable but _temporary_.
- all messages are stored redundantly across multiple servers amd DC
- retention configurable on a per-queue basis, from one hour to 14 days

###Cost Model
- pay for what you use
- provides 100,000 requests per month at no charge
- beyond the free tier:
    - priced per 10,000 requests
    - amount of data transferred in and out (priced per GB per month)

###Scalability and Elasticity
- both highly elastic and massively scalabel
- unlimited number of computers to read and write unlimited number of messages at any time
- unlimited numbers of queues and messages per queue for any user

###Interfaces
- HTTP Query web services interface
- SDK and APIs provides both management and data inferface

###Anti-Patterns
- _MUST_ Binary or large messages. otherwish store to RDS and store a pointer to the data in SQS
- Long-term storage: SQS only stored 14 days
- High-speed message queuing or very short tasks, use DynamoDB or a message-queuing system hosted on EC2


##RDS
- provides capabilities of MySql, Oracle, or MsSQL as a managed, cloud-based service
- eliminates much pf the administrative overhead associated with launching, managing, and scaling your own relational database

###Ideal Usage Patterns
- Ideal for existing applications that rely on RDS database engines
- offers full compatibility and direct access to native database engins
- should work unmodified with RDS

###Performance
- a combination of configurable instances
- running on Amazon's proven, world-class infrastructure with fully-automated maintenance and backup operations
- Provisioned IOPS instance(1,000 - 30,000 IOPS, instance lifetime)

###Durability and Availability
- leverages EBS volumes as its data store
- two types of database backups that are __replicated across multiple Availability Zones__:
    - automated DB instance backups:
        - full daily backup of your data during the specified backup windows
        - also capture DB transaction logs
        - no additional charge, can be retained for up to 8 days
        - can be used to do a ponit-in-time restore
        - any opint from the start of the retention period to about the last 5 mins from current time
    - user-initiated database snapshots:
        - can be created at any time
        - kept until explicitly deleted
        - allow you to restore your database to a known state
- Multi-AZ deployment synchronously replicating your data between a primaru RDS instance and a standby instance in another ZA
    - automatically failover to standy(typically takes about 3mins)
    - is the built-in asynchronous replication provide by RDS read replicas, you can use either feature alone, or both in combination

###Cost Model
- size of the database instance
- the deployment type(Single-AZ/Multi-AZ)
- the AWS region
- pricing for RDS is based on several factors:
    - the DB instance hours(per hour)
    - the amount of provisioned database storage(per GB-month adn per million I/O Requests)
    - additional backup storage(per GB-month)
    - data transfer in/out (per GB per month)

###Scalability and Elasticity
RDS resouces can be scaled elastically:
    - database storage size
    - database storage IOPS rate
    - database instance compute capacity
    - the number of read replicas

- supports "pushbutton scaling" of both database storage and compute resources
    - to scale RDS database storage elastically ,can be added either immediately or during the next maintenace windows
    - to scaling computations resources from one DB instance tupe to another
- also enables ou to scale out beyond the capacity of a single database deployment fro read-heavy database workloads by createing one or more read replicas
- administrator may configure multiple RDS instance and leverage database partitioning or sharding to spread the workload over multiple DB instance

###Interfaces
- API
- Console
- no AWS data API for RDS, provide directly access

###Anti-Patterns
- Index and query-focused data: Many cloud-based solution don't require advanced features found in a relational database, such as joins and complex transaction. If more oriented toward indexing and querying data -> DynamoDB
- Numeriys BLOBs: RDS support binary large objects(BLOBs), if your application makes heavy use of them(_audio files, cideos, images,and so on_), you may find S3
- Other database platfroms: install on EC2 by yourself
- Complete control: install on EC2 by yourself


##DynamoDB
is a fast, fully-managed NoSQL database service that make it simple and cost-effective to store and retrieve any amount of data, and serve any level of request traffic.
- structured data in tables
- indexed by primary key
- allow low-latency read and write
- items ranging from 1byte up to 64KB
- supporting 3 data types: number, string, and binary
- in both scalar and multi-valued sets
- Tables do not have fixed a schema, so each data item can have a different number of attributes
- DynamoDB is integrated ith other services:
    - EMR - analytics
    - Redshift - data warehouse
    - Data Pipeline - data import/export
    - S3 - backup and archive

###Ideal Usage Patterns
flexible NoSQL database ,low read and write latencies, ability to scale storage and throughput up or down as needed without code changes or downtime

###Performance
- SSDs and limiting indexing on attributes provides high throughput and low latency (single-digit milliseconds typical for average server-side response times, and frastically reduces the cost of read and write operations
- perdicatable performance can be archieved by defining the provisioned throughput capacity required fro a given table.

###Durability and Availability
- built-in fault tolerance that automatically and synchronously replicates data across three AZ in a region

###Cost Model
3 pricing components:
    - provisioned throughput capacity (per hour)
    - idnexed data storage (per GB per month)
    - data transfer in or out (per GB per month)

###Scalability and Elasticity
- no limit to the aount of data you can store in an DynamoDB table
- the service automatically allocates more storage as you store more data using the DynamoDB wrute APIs
- Data is automatically partitioned and re-partitioned as needed
- use of SSDs provides perdictable low-lateency resonse time at any scale
- you can simply 'dial-up/down' the read and write capacity of a table as your needs change

###Interfaces
- low-level REST API
- higher-level SDK
- while standard SQL isn't available for DynamoDB, you may use the DynamoDB select opertation to create SQL-Like querise that retrieve a set of attributes based on criteria that you provide
- Console

###Anti-Patterns
- Perwritten application tied to a traditional relational database: just use AWS RDS
- Joins and or complex transaction: RDS or EC2 self-managed database
- BLOB data: go to S3
- Large data with low I/O rate: DynamoDB uses SSD drives and is optimized for workloads with a high I/O rate per GB stored. go S3


##ElastiCache
- easy to deploy, operate, and scale a distributed, in-memory cache in the cloud
- supports two polular open-source caching engines: Memcached and Redis
- seamlessly work with existing Memcached environments
- supports Redis master/slave replication can be used to achieve cross AZ redundancy

###Ideal Usage Patterns
- improves app performance by storing critical pieces of data in memory for low-latency access
- used as a database front end in _read-heavy_ application, improving performance and reducing the load by chaching the results of I/O-intensive queries
- used to manage web session data, cache dynamically-generated web pages, to cache results of computationally-intensive calculations
- for complex data structures, such as lists,sets, hashes,and sorted sets, the Redis engine is often used as an in-memory NoSQL database

###Performance
- cache layer is very dependent on the caching strategy and the hit rate at the application level, so it's difficult to provide general guidance

###Durability and Availability
- By definition, an in0memory cache stores transient data(somethings like)
- With Memcached engine, all ElastiCache nodes in a single cache cluster are provisioned in a single AZ
    - ElastiCache automatically monitors the health of your cache nodes and replaces them in the events
    - during the failure, performance will reduce but still available
    - to enhaced fault-tolerance, run redundant cache clusters in different AZ
    - _single cache cluster run on single AZ, multiple cache cluster should run on different AZ_
- With Redis engine, supports replication to up to 5 read replicas for scaling
    - place read replicas in other AZ
    - automatically reoaire or replace the primary node(if possible), using the same DNS name
    - if primary node down, you can failover to the read replicas with an API call

###Cost Model
Only single procing component: per cache node-hour consumed

###Scalability and Elasticity
- cache node types supporting from 213 MB up to 68 GB of memory per node
- add or delete cache nodes to cache cluster at any time
- Memcached cache engin has Auto Discovery to handle cache nodes

###Interfaces
- console
- the ElastiCache command line tools
- HTTP Query API
- various SDKs

###Anti-Patterns
- Persistent data: no need detail for this


##Redshift
- fast, fully-managed, petabyte-scale data warehouse service that makes it simple and cost-effective to _efficiently analyze all your data using your existing business intelligence tools_
- optimized for datasets that range from a few hundred gigabytes to a petabyte or more
- manages the work needed to set up, operate, and scale a data warehouse, from provisioning the infrastructure capacity to automating ongoing administrative tasks such as backups and patching

###Ideal Usage Patterns
Ideal for analyzing large datasets useing your existing business intelligence tools:
- Anazlyze global sales data for multiple products
- store historical stock trade data
- analyze ad impressions and clicks
- aggregate gaming data
- analyze social trends
- measure clinical quality, operation efficiency, and financial performance in the health care space

###Performance
- very high query performance on datasets ranging in size from hundreds of gigabytes or more
- uses columnar storage, data compression, and zone maps to reduce the amount of I/O needed to perform queries
- massively parallel processing(MPP) architecture that parallelizes and distributes SQL operations to take advantage of all available resources
- underlying hardware is designed for high performance data processing that uses local attached storage to maximize throughput.

###Durability and Availability
- 3 copies of your data -- all data written to a node in your cluster is automatically replicated to other nodes within the cluster
- all data is continuously backed up to S3
- snapshots are automated, incremental, and continuous:
    - for a user-defined period (1 - 35 days) at any time
    - can create one or more manual snapshots until explicityly deleted
- continuously monitors the health of the cluster and automatically re-replicates data from failed drives and replaces nodes as necessary

###Cost Model
3 pricing compownents:
- data warehouse node hours
    - Compute node hours are the total number of hours run across all compute nodes for the billing period
- backup storage
    - the storage associated with automated and manual snapshots for an Redshift data warehouse cluster
    - increasing the backup retention period or taking additional snapshots increases the backup storage consumed by the Redshift data warehouse cluster
    - there is no additional charge for backup storage up to 100% of your provisioned storage for an active data warehouse cluster
- data tansfer
    - no data transfer charge for data ransferred to or from Redshift outside of VPC
    - data transfer to or from Redshift in VPC accrues standard AWS data transfer charges

###Scalability and Elasticity
- "pushbutton scaling" pf compute nodes within a data warehouse cluster
- with console or API call easy to up or down cluster
- An cluster can be start as little as a single 2 TB XL node and scale all the way to a hundreds 16 TB 8XL nodes for 1.6 PB of compressed user data
- Redshift will place your existing cluster into read-only mode, provision a new cluster of your chosen size, and then copy data from your old cluster to your new one in parallel. querise can continue running against the old cluster while the new one is being provisioned

###Interface
- Redshift Query API provides a management interface to manage data warehouse clusters programmatically
- SDK
- AWS CLI
- API do not provide a data interface, uses industry standard ODBC and JDBC connections and PostgreSQL drivers
- Data can be loaded into Redshift from a range of data sources including S3, DynamoDB, and Data Pipeline.

###Anti-Patterns
- OLTP workloads: Redshifts is a _column-oriented database_ suited to data warehouse and analytics, where queries are typically performed over very large datasets. not for traditional row-based database system
- BLOB data: go S3


##Databases on EC2
EC2, together with EBS volumes, provides an ideal platform for you to operate your own self-managed relational database in the cloud. Many leading database solutions are available as prebuilt, ready-to-use EC2 AMIs

###Ideal Usage Patterns
- requires a specific traditional relational database not supported by RDS
- users who require a maximum level of administrative control and configurability

###Performance
depends on many factors:
- EC2 instance type
- number and configuration of EBS volumes
- database software and its configration
- application workload
- similar to same database installed on similiary configured on-permises equipment
- To increase database performance you can scale up memory and compute resources by choosing a larger EC2 instance size
- For _database storage_, it is usually best to use EBS Provisioned IOPS volumes
- To _scale up I/O performance_, you can increase the Provisioned IOPS, change the number of EBS volumes, or use software RAID 0 (disk striping) across multiple EBS volumes, which will aggregate total IOPS and bandwidth
- can also scale the total database system performance by scaling horizontally with database clustering, replication, and multiple read slaves
- in general, they same as physical server environment

###Durability and Availability
- provide persistent storage for structured data using EBS volumes as the data store
- enhanced by using EBS snapshots, or by using 3th-party database backup utilities to store backup at S3

###Scalability and Elasticity
can take advantage of the scalability and elasticity of the underlying AWS platform
