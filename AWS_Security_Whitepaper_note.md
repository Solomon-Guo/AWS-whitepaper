##Shared Security Responsibility Model
- AWS is responsible for securing the underlying infrastructure that supports the cloud
- you're responsible for anything you put on the cloud or connect to the cloud
- reduce your operaional burden in many ways, and in some cases may even improve your default security posture without additional action on your part

The amount of security configuration work you have to do varies depending on which services you select and how sensitive your data is.

###AWS securitu Responsibilities
- comprised of the hardware, software, networking, and facilities the run AWS serveices
- AWS is responsible for the security configuration of its products that are considered managed services
    - handle basic security tasks like guest operating system (OS) and database patching, firewall configuration, and disaster revocery
    - all you have to do is configure logical access controls for the resources and protect your account credentials

###Customer Security Responsibilitieswo
- AWS products tat fall into the well-understood category of Infrastructure as a Service (IaaS) -- such as EC2, VPC, and S3 -- are completely under your control and require you to perform all of the necessary security configuration and management tasks.
- AWS manageed services like RDS or Redshift provide all of the resouces you need in order to perform a specfic task--but without the configuration work that  can come with them. With managed services, you don't have to worry about launching and maintining instances, patching the guest OS or database, or replicating database -- AWS handles that for you
- you should protect your AWS account credentials and setup individual user accounts with Amazon Identity and Access Management (IAM)

##AWS Glocal Infrastructure Security
AWS operates the global cloud infrastrucure that you use to provision a variety of basic computing resources such as processing and storage.
includes: facility, network, hardware, and operational sofrware

###AWS Compliance Program
The IT infrastructure that AWS provides to its customers is designed and managed in alignment with security best practices and a variety of IT security standards.

###Physical and Environmental Security
- many years of experience in run a good data center
- housed in nondescript facilities
- Pysical access is strictly controlled both at the perimeter and at building ingress points
- only provides data center access and information to employees and contractors who have a legitimate business need for such privileges.

###Fire Detection and Suppression
installed with standard

###Power
Fully redundant and maintainable witout impact to operations, 24 hours a day, and seven days a week
UPS and generators

###Climate and Temperature
- maintain a constant operating temperature for servers and other hardware

###Management
monitors electrical, mechanical, and life support systems and equipment so that any issues are immediately identified

###Storage DEvice Decommissioning
AWS procedures include a decommissioning process that is designed to prevent customer data from being exposed to unauthorized individuals

##Business Continuity Management
Data Center Business Continuity Management at AWS is under the direction of the Amazon Infrastructure Group

###Availability
In case of failure, automated processes move customer data traffic awawy from the affected area. Core applications are deployed in an N+1 configration, there is sufficient capacity to enable traffic to be load-balanced to the remaining sites
data place within multiple geopraphic regions as well as across multiple availability zone within each region
physically separated:
- typical meteropolitan region
- lower risk flood plains
power
- UPS
- fed via different grids

###Incident Response
- employs industry-standard diagnostic prcedures to drive resolution during business-impacting events
- Staff operator provide 24*7*365

###Company-Wide Executive Review
recently reviewed the AWS services resiliency plans

###Communication
implemented varous methods of internal communication at ga global level to help emplyees understand their individual roles and responsibilities and to communicate significant events in a timely manner

also implemented vaarious methods of external communication to support its customer base and the community

##Network Security
implemented a world-class network infrastructure that is carefully monitored and managed

###Secure Network Architecture
Devices are in place to monitor and control communications at the external boundary of the network and at key internal boundaries within the network
ACLs. or traffic flow policies, are established on each managed interface, which manage and enforce the flow of traffic

###Secure Access Points
AWS has strategically placed a limited number of access points to the cloud to allow for a more comprehensive monitoring of inbound and outbound communications and network traffic. These customer access points are called API endpoints, and they allow secure HTTP access, which allows you to establish a secure communication session with your storage or compute instances whitin AWS.

###Transmission Protection
- HTTPS
- additional layers of network security: VPC provides a private subnet within the AWS cloud

###Amazon Corporate Segregation
- AWS is segregation from the Amazon Corporate network
- AWS developers and administrators can access AWS cloud components in order to maintain them
- connect to AWS network through a bastion host with logging all activity

###Fault-Tolerant Design
- Data center are blt in clusters in various global regions; all of them are online and serving customer; no data center is cold .
- Core applications are deployed in an N+1 configuration
- flexibility to place instances and store data within multiple georaphic regions as well as across multiple availability zones within each region;
- each availability zone is designed as an independent failure zone( physical separated)
- you should architect AWS usage to take advantage of multiple regions and availability zones
- all communications between regions is across public internet infrastructure; therefor, appropriate encryption methods should be used to protect sensitive data
- AWS GovCloud(US) is an isolated AWS Region designed to allow US government agencies and customers move workloads into the cloud by helping them meet certain regulatory and compliance requirements

###Network monitoring and protection
- automated monitoring systems to provide a high level of service performance and availability
- designed to detect unusual or unauthorized activities and conditions at ingress and egress comminication points
    - server and network usage
    - port scanning activities
    - applications usage
    - unauthorized intrusion attempts
- ability to set custom performance metrics thrsholds for unusual activity
- monitor key operational metrics. Alarms are configured to automatically notify operations and management personnel when early warning thresholds are crossed on key operational metrics.On-call scheduale and pager
- documentation is maintained to aid and inform operations personnel in handling incidents or issues
- conferencing system is used which supports communication and logging capabilities, collaboration
- AWS security monitoring tools help identify several types of service (DoS) attacks, in cluding distributed, flooding, and software/logic attacks.
    - DDoS attack: AWS API endpoints are hosted on large, internet-scale, world-class infrastructure that benefits from the same engineering expertise that has built Amazon into the world's largetsonline retailer.
    - Man in the Middle (MITM) Attacks. All of the AWS APIs are available via SSL-protected endpoints wihich and log them to the instance's console.Use SSL for all of your interactions with AWS
    - IP Spoofing, EC2 instances acannot send spoofed network traffice by AWS-controlled, host-based fire wall infratructure
    - Port Scanning, Unauthorized port scans by EC2 customers are a violation of the AWS acceptable Use Policy
    - Packet sniffing by other tenants, not possible for a virtual instance, you can place your interface into promiscuous mode then the hypervisor will not deliver any traffice to them that is not addressed to themz

##AWS Access
- segregated from Amazon Corporate network
- requires a separate set of credentials for logical access
- requires SSH public-key authentucation through a bastion host
- access through the AWS access management system with approvals

###Account Review and Audit
- reviewed every 90 days
- explicit re-approval is required or access to the resource is automatically revoked
- Access is also automatically revoke when resign
- When changes in an employee's job function occur, need explicitly approval or revoked

###Background checks
criminal background checks

###Credemtials Policy
Passwords must be complex and are forced to be changed every 90 days

##Secure Design Principles
follows secure software development best practices

##Change Management
###Software
- change are thoroughly reviewed, tested, approved and well-comunicated
- push into production in a phased deployment starting with lowest impact areas
- AWS performs self-audits of changes to key services to monitor quality, maintain high standards, and facilitate continuous improvement of thr chage management process

###Infrastucture
- The infrastruture team maintains and operates a UNIX/Linux configuration management framework to address hardware scalability, availability, auditing, and security management
- monitoring results obtain or update their configuration and software

##AWS Account Security Features
- Credentials for acces control
- HTTPS endpotins
- IAM user account
- Activity logging
- Trusted Advisor security checks

###AWS Credentials
- Password: root account or IAM user login to console
- Multi-Factor Authentication(MFA): root or IAM login
- Access Keys: Digitally signed request to AWS API
- Key Pairs : EC2 instance and CloudFront signed URLs
- X.509 Certificates: Digitally signed SOAP requests to API and SSL server certificates

- you can download a report for your account at Security Credentials page
- rotate your access ketys and certificates on a regular basis
- supports multiple concurrent access keys and certificates

####Password
just password for account

####AWS MFA
- need to enabl
- need to provid a six-digit single-user code in addition to your standard user name and password
- you get this single-use code from an authentication device that you keep in your physical possession
- support hardware tokens and virtual MFA devices
- support API and IAM and resources that sipport ACLs

####Access Keys
- for sign API requests
- A request must reach AWS within 15 minutes of the time stamp in the request
- Signature Version 4 using HMAC-SHA256 protocal
- save in a safe place and not embed them in your code

####Key pairs
- use public/private key pair rather than a password
- for CloudFront , key pairs to create signed URLs for private content

####X.509 Certificates
- when you create a request, you create a digital signature with your private key and then include that signature in the request along with your certificate

###Individual User Accounts
-IAM fir creating and managing individual users within your AWS Account

###Secure HTTPS Access Points
- SSL
- ECDHE protocol

###Security Logs
- AWS cloudTrail provides a log of all requests for AWS resources within your account
- Log can storage to S3
- CloudWatch Logs feature to collect and monitor

###AWS Trusted Advisor Security Checks
- Inspects your AWS enviroment and makes recommendations whrn opportunities may exist to save money, improve system performance , or close security gaps

##AWS Service-Specific Security

###Compute Services
EC2 Security
- mutiple levels of sercurity
    - OS
    - firewall
    - signed API
- The Hypervisor
    - utilizes a highly customized version of the Xen hypervisor
    - virtualization of the physical resources leads to a clear sepatation between guest and hypervisor, resulting in additional security separation between the two
- instance isolation
    - different instances running on the same physical machine are isolated from each other via the Xen hypervisor
    - AWS firewall resides thithin the hypervisor layer, between the physical network interface and the instance's virtualinterface
    - physical RAM is sepatated using similar mechanisms
    - no access to raw disk devices
    - memory will scrubbed （set to zero) before return to free memory pool to assign to customer
    - encryoted file system on top if the virtualized disk device
- Host operating system
    - administrator access will audit
- Guest operating system
    - completely control by you, AWS don't have access rights
    - recommend security rules
    - EC2 key pair access
    - you also control the updating and patching of your guest OS, including security updates
    - AWS AMI update regularly with the latest patch, you need to launch new instance to adopt
- Firewall
    - is a function of which ports you open, and for what duration and purpose
- API access
    - sign by Amazon Secret Access Key
- permission
    - IAM

EBS sercurity
- restricted to the AWS account that created the volume
- redundantly stored in multiple physical locations with no additional charge, in the same availability zone
- recommend storage snapshot to S3
- you should conduct a specualized wipe procedure priot to deleting the volume for compliance with your established requirement
- Encrypt with AES-256 between EC2 instances and EBS storage. only available on EC2's more powerful instance types

Auto Scaling Security
- signed request to API
- IAM role

###Networking Services
ELB
- Take over the encryption and decryption work from the EC2 and manages it centrally on the load balance
- offers clients a single point of caontact, and  can also serve as the first line of defense against attacks on your network
- sercurity groups associate with ELB
- supprots end-to-end traffic encryption using TLS

VPC
- vpc with a single ppublic subnet only
- vpc with public and private subets
- vpc with public and private subets and hardward VPN access
- vpc with private subet onlu and hardware VPN access
- you must create VPC security groups specifically for your VPC, any EC2 security gourps you have create will not work inside your VPC
- each subnet in an VPC is associated with a routing table, and all network traffic leaving the subnet is processed by the routing table to determine the destination

Route53
*

ClouFront
- you can enable the service'r private content feature to control over who is able to download content from cloudFront

Direct connect Security


##Storage Services
###S3
Data Access
- IAM: User-Level Control
- ACL: AWS Account-Level Control;for resources access
- Bucket Policies: both AWS Account-level control and User-Level Control

Data Transfer: SSL encrypted

Data Storage:
- encrypte while upload with Client
- not place sensitive information in S3 metadata
- AWS S3 Server Side Encryption(SSE)
    - AES-256
    - every protected object is encrypted with a unique encryption key, rotated
    - key different in hosts

Data Durability and Reliability
- Versiong, you can use it to preserve, retrieve, and restore every verion of every obkect stored in S3 bucket
- MFA

Access Logs

Corss-Origin Resource Sharing(CORS)
- Modern browsers use the Same origin policy to block corss-site scripting attack
- With enable CORS policy, asset storaged in S3 bucket can be safely refrenced by external requests

###Glacier
- automatically enrypts the data using AES-256 and stores it durably in an immutable form
- only your account can access yourdata
- use IAM to control users

###Storage Gateway
- transfer over SSL to S3
- only uploads data that has changed
- each gateway(virtual machine) will automatically updates amd patcjes
- iSCSI protocol supports authentication between targets and initators via CHAP(Challenge-Handshake Authentication Protocol).protect against man-in-the-middle and playback attacks
- connection establish by AWS account in the console

###Import/Export
- requires that you securely identify and authenticate your storage device
- encrypted hardware

##Database Services
###DynamoDB
- durability and availability
- automatical backup
- API permission for IAM
- SSL-encrypted endpoints

###RDS
Access Control
- commone db account
- Sercurity Group
- IAM

Network Isolation
- VPC
- subnet for multiple AZ

Encryption
- ssl
- Transparent Dta Encryption(TDE)

Automated backups and Snapshot
- backup database nad transaction logs, up to last 5minutes or 35 days
- during the backup windows, storage I/O may be suspended, typically lasts a few minutes
- snapshots are user-initiated backup of your DB instance

DB Instance replicateion
- Multi-AZ deployment provisions and maintains a synchronous standby replica of your DB instance in a different AZ

Automatic Software Patching
- maintain windows
- Multi-AZ -> perform maintenance on standby -> promote standby to primry -> perform on old primary which becomes the new standby

Event Notification
- published via AWS SNS such as email and SMS

###Redshift
- scale up or down rapidly, significantly improve query speed
- columnar storage, data compression, zone maps to reduce the amount of IO needed to perform queries

Cluster Access
- Security groups to conrtol network access to your data warehouse cluster
- VPC to isolate your data warehouse cluster in your own virtual network and connect it to your existing IT infrastructure by VPN
- AWS account that creates the cluster has full access to the cluster, IAM account to assign different permission for access
- you grant database user permissions on a per-cluster basis instead of on a per-table basis
- a user can see data only in the table rows that were generated by his own activities; rows generated by other users are not visible to him.

Data Backups
- distributes your data across all compute nodes in a cluster
- all data written to a node in your cluster is continuously backed up to S3 using snapshots (period, 1 - 35 days)
- or you can take you own snapshot at any time
- automatically re-replicates data from failed drivers and replaces nodes as necessary

Data Encryption
- Data encryption keys encrypt data blocks in the cluster. Each data block is assigned a randomly-generated AES-256 key. These keys are encrypted by using the database key for the cluster.
- The database key encrypts data encryption keys in the cluster. The database key is a randomly-generated AES-256 key. It is stored on disk in a separate network from the Amazon Redshift cluster and encrypted by a masterkey. Amazon Redshift passes the database key across a secure channel and keeps it in memory in the cluster.
- The cluster key encrypts the database key for the Amazon Redshift cluster. You can use either AWS or a hardware security module (HSM) to store the cluster key. HSMs provide direct control of key generation and management, and make key management separate and distinct from the application and the database.
- The master key encrypts the cluster key if it is stored in AWS. The master key encrypts the cluster-key-encrypted database key if the cluster key is stored in an HSM.
- enabling encryption in your cluster will impact performance, even though it is hardware accelerated.

Database Audit Logging
- logs all SQL operations

Automatic Software Patching

SSL connections

###ElastiCache
Amazon ElastiCache is a web service that makes it easy to set up, manage, and scale distributed in-memory cache environments in the cloud.
- ElastiCache allows you to control access to your Cache Clusters using Cache Security Groups
- ElastiCache for Redis provides backup and restore functionality, where you can create a snapshot of your entire Redis cluster as it exists at a specific point in time.

##Application Services
###CloudSearch
CloudSearch is a managed service in the cloud that makes it easy to set up, manage, and scale a search solution for your website.
- Access to your search domain's endpoints is restricted by IP address so that only authorized hosts can submit documentsand send search requests.
- All Amazon CloudSearch configuration requests must be authenticated using standard AWS authentication.

###SQS
- SQS access is granted based on an AWS Account or a user created with AWS IAM.
- SQS is accessible via SSL-encrypted endpoints. The encrypted endpoints are accessible from both the Internet and from within Amazon EC2
- Data stored within Amazon SQS is not encrypted by AWS; however, the user can encrypt data before it is uploaded to Amazon SQS,

###SNS
set up, operate, and send notifications from the cloud. It provides developers with a highly scalable, flexible, and cost-effective capability to publish messages from an application and immediately deliver them to subscribers or other applications.
- ubscribe clients to these topics, publish messages, and have these messages delivered over clients’ protocol of choice (i.e., HTTP/HTTPS, email, etc.).
- SNS can be leveraged to build highly reliable, event-driven workflows and messaging applications without the need for complex middleware and application management.
- SNS access is granted based on an AWS Account or a user created with AWS IAM.
- HTTPS

###SWF (Simple Workflow Service)
makes it easy to build applications that coordinate work across distributed components
- access is granted based on an AWS Account or a user created with AWS IAM
- you cannot grant users associated with other AWS Accounts access to your SWF workflows

###SES (Simple Email Service)
an outbount-lonly email-sending service builty on Amazon;s reliable and scalable infrastructure
- required users to verifu their email address or domain in order to confirm  that they own it and to prevent others from using it. to verify a domain, SES required the sender to publish a DNS record that SES supplies as proof of control over the domain

maximize deliverability and dependability for all of our senders:
- content-filtering technologies to help detect and block message containing viruses or malware befor they can be sent;
- maintains complaint feedback loops with major ISPs
- uses a variety of techniques to measure the quality of each user's sending
- suooirtsd authentication mechanisms such as Sender Policy Framework(SPF) and DomainKeys IdentifiedMail (DKIM). When you authenticate an email, you provide evidence to ISPs that you own the domain. SES maikes it easy for you

- use credentialsto verify permission to interact with SES
- integrates with IAM so that you can specify which SES API actuibs a user can perform
- SMTP requeired TLS emcrypt: STARTTLS and TLS Wrapper
- HTTP pertected by TLS through HTTPS endpoint

###Elastic Transcoder Service
- IAM
- SSL-encrypted endpoint
- durability is provided by S3

###AppStream
- IAM
- STX Protocol to manage the streaming of your application from AWS to local devices
