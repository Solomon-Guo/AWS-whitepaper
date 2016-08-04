###IT Assets Become Programmable Resources
In a non-cloud environment you would have to provision capacity based on aguess of a theoretical maximum peak. This can result in periods where expensiveresources are idle or occasions of insufficient capacity. With cloud computing,you can access as much or as little as you need, and dynamically scale to meetactual demand, while only paying for what you use.

Global, Available, and Unlimited Capacity Using the global infrastructure of AWS, you can deploy your application to the AWS Region2 that best meets your requirements (e.g., proximity to your end users, compliance, or data residency constraints, cost, etc.).


##Scalability
###Scaling Vertically
Scaling vertically takes place through an increase in the specifications of an individual resource (e.g., upgrading a server with a larger hard drive or a faster CPU). This way of scaling can eventually hit a limit and it is not always a cost efficient or highly available approach. However, it is very easy to implement and can be sufficient for many use cases especially in the short term.

###Scaling Horizontally
Scaling horizontally takes place through an increase in the number of resources (e.g., adding more hard drives to a storage array or adding more servers to support an application). This is a great way to build Internet-scale applications that leverage the elasticity of cloud computing. Not all architectures are designed to distribute their workload to multiple resources, so let’s examine some of the possible scenarios.

####Stateless Applications 
When users or services interact with an application they will often perform a series of interactions that form a session. A stateless application is an application that needs no knowledge of previous interactions and stores no session information. Such an example could be an application that, given the same input, provides the same response to any end user. A stateless application can scale horizontally since any request can be serviced by any of the available compute resources (e.g., EC2 instances, AWS Lambda functions). With no session data to be shared, you can simply add more compute resources as needed. When that capacity is no longer required, any individual resource can be safely terminated (after running tasks have been drained). Those resources do not need to be aware of the presence of their peers – all that is required is a way to distribute the workload to them.

How to distribute load to multiple nodes
Push model:
_A popular way _to distribute a workload is through the use of a load balancing solution like the Elastic Load Balancing (ELB) service. Elastic Load Balancing routes incoming application requests across multiple EC2 instances. An alternative approach would be to implement a DNS round robin (e.g., with Amazon Route 53). In this case, DNS responses return an IP address from a list of valid hosts in a round robin fashion. While easy to implement, this approach does not always work well with the elasticity of cloud computing. This is because even if you can set low time to live (TTL) values for your DNS records, caching DNS resolvers are outside the control of Amazon Route 53 and might not always respect your settings.
Pull model: 
_Asynchronous_ event-driven workloads do not require a load balancing solution because you can implement a pull model instead. In a pull model, tasks that need to be performed or data that need to be processed could be stored as messages in a queue using Amazon Simple Queue Service (Amazon SQS) or as a streaming data solution like Amazon Kinesis. Multiple compute nodes can then pull and consume those messages, processing them in a distributed fashion.

Stateless Components
In practice, most applications need to maintain some kind of state information.For example, web applications can use HTTP cookies to store information about a session at the client’s browser (e.g., items in the shopping cart). The browser passes that information back to the server at each subsequent request so that the application does not need to store it. However, there are two drawbacks with this approach. First, the content of the HTTP cookies can be tampered with at the client side, so you should always treat them as untrusted data that needs to be validated. Second, HTTP cookies are transmitted with every request, which means that you should keep their size to a minimum (to avoid unnecessary latency).
Consider only storing a unique session identifier in a HTTP cookie and storing more detailed user session information server-side. Most programming platforms provide a native session management mechanism that works this way, however this is often stored on the local file system by default. This would result in a stateful architecture. A common solution to this problem is to store user session information in a database. 
Amazon DynamoDB is a great choice due to its scalability, high availability, and durability characteristics. For many platforms there are open source drop-in replacement libraries that allow you to store native sessions in Amazon DynamoDB.
S3 or EFS (Elastic File System) placing large files in a shared storage layer
Amazon Simple Workflow Service (Amazon SWF) can be utilized to centrally store execution history and make these workloads stateless.

Stateful Components
Like databases, application designed to running on a single server, clients need long time connection. For those case, You might still be able to scale those components horizontally by distributing load to multiple nodes with “session affinity.

How to implement session affinity
For HTTP/S traffic, session affinity can be achieved through the “sticky sessions” feature of ELB.
If you control the code that runs on the client, you need contral client connect back to same server by using DNS or Server discover API, in short, do it by yourself if ELB is not work for you.

Distributed Processing
Amazon EMR （Hadoop), Offline batch jobs can be horizontally scaled
Amazon Kinesis data streaming

##Disposable Resources Instead of Fixed Servers
Traditional infrastructure environment, need fixed resources due to:
- upfront cost
- lead time of introducing new hardware
- configure software/hardware
- testing

with AWS, you can dynamically provisioned

##Instantiating Compute Resources
To achieve an automated and repeatable process

###Bootstrapping
While launch EC2 or RDS, you can start with a defualt configuration, execute automated bootstrapping

AWS OpsWorks natively supports Chef recipes or Bash/PowerShell scripts. In addition, through custom scripts and the AWS APIs, or through the use of AWS CloudFormation support for AWS Lambda-backed custom resources8, it is possible to write provisioning logic that acts on almost any AWS resource.

Golden Images
a snapshot of a particular state of that resource
You can customize an Amazon EC2 instance and then save its configuration by creating an Amazon Machine Image (AMI) and launch more instances from the AMI 

Hybrid
AWS Elastic Beanstalk follows the hybrid model. It provides preconfigured run time environments (each initiated from its own AMI10) but allows you to run bootstrap actions (through configuration files called .ebextensions11) and configure environmental variables to parameterize the environment differences

###Infrastructure as Code
AWS CloudFormation to launch resources

##Automation
AWS Elastic Beanstalk is the fastest and simplest way to get an application up and running on AWS. Developers can simply upload their application code and the service automatically handles all the details, such as resource provisioning, load balancing, auto scaling, and monitoring.

Amazon EC2 Auto recovery: You can create an Amazon CloudWatch alarm that monitors an Amazon EC2 instance and automatically recovers it if it becomes impaired.

Auto Scaling: With Auto Scaling, you can maintain application availability and scale your Amazon EC2 capacity up or down automatically according to conditions you define.

Amazon CloudWatch Alarms: You can create a CloudWatch alarm that sends an Amazon Simple Notification Service (Amazon SNS) message when a particular metric goes beyond a specified threshold for a specified number of periods. 

Amazon CloudWatch Events: The CloudWatch service delivers a near real-time stream of system events that describe changes in AWS resources. 

AWS OpsWorks Lifecycle events17: AWS OpsWorks supports continuous configuration through lifecycle events that automatically update your instances’ configuration to adapt to environment changes. These events can be used to trigger Chef recipes on each instance to perform specific configuration tasks. For example, when a new instance is successfully added to a Database server layer, the configure event could trigger a Chef recipe that updates the Application server layer configuration to point to the new database instance.

AWS Lambda Scheduled events18: These events allow you to create a Lambda function and direct AWS Lambda to execute it on a regular schedule.

##Loose Coupling
IT systems should be designed in a way that reduces interdependencies—a change or a failure in one component should not cascade to other components

Well-Defined Interfaces
allow the various components to interact with each other only through specific, technologyagnostic interfaces (e.g., RESTful APIs).

Service Discovery
services are meant to be loosely coupled, they should be able to be consumed without prior knowledge of their network topology details. Apart from hiding complexity, this also allows infrastructure details to change at any time.
For an Amazon EC2 hosted service a simple way to achieve service discovery is through the Elastic Load Balancing service. Because each load balancer gets its own hostname you now have the ability to consume a service through a stable endpoint.

Asynchronous Integration
The two components do not integrate through direct point-topoint interaction but usually through an intermediate durablestorage layer. tight coupling is one by one interact, loose coupling is a queue between two components.
an Amazon SQS queue 
a streaming data platform like Amazon Kinesis

- A front end application inserts jobs in a queue system like Amazon SQS. A back-end system retrieves those jobs and processes them at its own pace.
- An API generates events and pushes them into Amazon Kinesis streams. A back-end application processes these events in batches to create aggregated time-series data stored in a database.
- Multiple heterogeneous systems use Amazon SWF to communicate the flow of work between them without directly interacting with each other.
- AWS Lambda functions can consume events from a variety of AWS sources (e.g., Amazon DynamoDB update streams, Amazon S3 event notifications, etc.). In this case, you don’t even need to worry about implementing a queuing or other asynchronous integration method because the service handles this for you.

Graceful Failure
Graceful failure in practice
A request that fails can be retried with an exponential backoff and Jitter strategy19 or it could be stored in a queue for later processing. For front-end interfaces, it might be possible to provide alternative or cached content instead of failing completely when, for example, your database server becomes unavailable. The Amazon Route 53 DNS failover feature also gives you the ability to monitor your website and automatically route your visitors to a backup site if your primary site becomes unavailable. You can host your backup site as a static website on Amazon S3 or as a separate dynamic environment

##Services, Not Servers
Managed Services
On AWS, there is a set of services that provide building blocks that developers can consume to power their applications. These managed services include databases, machine learning, analytics, queuing, search, email, notifications, and more. In short, you have any things in AWS as in the traditional

Serverless Architectures
These architectures can reduce costs because you are not paying for underutilized servers, nor are you provisioning redundant infrastructure to implement high availability
mobile apps ---> Amazon Cognito, so that you don’t have to manage a back end solution to handle user authentication, network state, storage, and sync. Amazon Cognito generates unique identifiers for your users. Those can be referenced in your access policies to enable or restrict access to other AWS resources on a per-user basis. Amazon Cognito provides temporary AWS credentials to your users, allowing the mobile application running on the device to interact directly with AWS Identity and Access Management (IAM)- protected AWS services. For example, using IAM you could restrict access to a folder within an Amazon S3 bucket to a particular end user.

##Databases
###Relational Databases
Scalability
scale vertically -- upgrade instance or add more faster storage
Amazon RDS for Aurora -- is a database engine designed to deliver much higher throughput compared to standard MySQL running on the same hardware
Scale horizontally (read-heavy) -- creating one or more read replicas

High Availability
creates a synchronously replicated standby instance in a different Availability Zone (AZ).

###NoSQL Databases
Scalability
Amazon DynamoDB in particular manages table partitioning for you automatically, adding new partitions as your table grows in size or as read- and write-provisioned capacity changes.

High Availability
synchronously replicates data across three facilities in an AWS region

In short, DynamoDB is depends AWS services not form you own design

Data Warehouse
A data warehouse is a specialized type of relational database, optimized for analysis and reporting of large amounts of data. It can be used to combine transactional data from disparate sources (e.g., user behavior in a web application, data from your finance and billing system, CRM, etc.) making them available for analysis and decision-making.
>>>> Amazon Redshift <<<<

Scalability
- massively parallel processing (MPP): Amazon Redshift MPP architecture enables you to increase performance by increasing the number of nodes in your data warehouse cluster.
- columnar data storage
- targeted data compression encoding schemes

High Availability
- multi-node clusters: in which data written to a node is automatically replicated to other nodes within the cluster.
- S3 store backup: Data is also continuously backed up to Amazon S3
- auto replact failed node: monitors the health of the cluster and automatically re-replicates data from failed drives and replaces nodes

Search
Amazon CloudSearch and Amazon Elasticsearch Service (Amazon ES)
Amazon CloudSearch is a managed service that requires little configuration and will scale automatically. On the other hand, Amazon ES offers an open source API and gives you more control over the configuration details. Amazon ES has also evolved to become a lot more than just a search solution. It is often used as an analytics engine for use cases such as log analytics, real-time application monitoring, and click stream analytics.

Scalability
use data partitioning and replication to scale horizontally. 

High Availability
data redundantly across Availability Zones

##Removing Single Points of Failure
Introducing Redundancy
- failover: recovered on a secondary resources with some time. during the resources remains unavailable
- active redundancy: requests are distributed to multiple redundant compute resources, and when one of them fails, the rest can simply absorb a larger share of the workload. 

Detect Failure
You should aim to build as much automation as possible in both detecting and reacting to failure.
- ELB and Amazon Route53 to configure health checks and mask failure by routing traffic to healthy endpoints.
- Auto Scaling can be configured to automatically replace unhealthy nodes.
- AWS OpsWorks and AWS Elastic Beanstalk --> EC2 auto recover

Durable Data Storage
Replication can take place in a few different modes

Durability: No replacement for backups
Regardless of the durability of your solution, this is no replacement for backups. Synchronous replication will redundantly store all updates to your data—even those that are results of software bugs or human error. However, particularly for objects stored on Amazon S3, you can use versioning29 to preserve, retrieve, and restore any of their versions. With versioning, you can recover from both unintended user actions and application failures.

Data durability in practice
It is important to understand where each technology you are using fits in these data storage models. Their behavior during various failover or backup/restore scenarios should align to your recovery point objective (RPO) and your recovery time objective (RTO). You need to ascertain how much data you would expect to lose and how quickly you would be able to resume operations. For example, the Redis engine for Amazon ElastiCache supports replication with automatic failover, but the Redis engine’s replication is asynchronous. During a failover, it is highly likely that some recent transactions would be lost. However, Amazon RDS, with its Multi AZ feature, is designed to provide synchronous replication to keep data on the standby node up-to-date with the primary

Automated Multi-Data Center Resilience

A shorter interruption in a data center is a more likely scenario. For shortdisruptions, because the duration of the failure isn’t predicted to be long, thechoice to perform a failover is a difficult one and generally will be avoided. OnAWS it is possible to adopt a simpler, more efficient protection from this type offailure. Each AWS region contains multiple distinct locations called AvailabilityZones. Each Availability Zone is engineered to be isolated from failures in otherAvailability Zones. An Availability Zone is a data center, and in some cases, anAvailability Zone consists of multiple data centers. Availability Zones within aregion provide inexpensive, low-latency network connectivity to other zones inthe same region. This allows you to replicate your data across data centers in asynchronous manner so that failover can be automated and be transparent foryour users

In fact, many of the higher level services on AWS are inherently designed
according to the Multi-AZ principle
- Amazon RDS provides high availability and automatic failover support for DB instances using Multi-AZ deployments, 
- Amazon S3 and Amazon DynamoDB your data is redundantly stored across multiple facilities

Fault Isolation and Traditional Horizontal Scaling
Shuffle Sharding
Similar to the technique traditionally used with data storage systems, instead of spreading traffic from all customers across every node, you can group the instances into shards. For example, if you have eight instances for your service, you might create four shards of two instances each (two instances for some redundancy within each shard) and distribute each customer to a specific shard. 

##Optimize for Cost
###Right Sizing
AWS offers a broad range of resource types and configurations to suit a plethora of use cases. For example, services like Amazon EC2, Amazon RDS, Amazon Redshift, and Amazon Elasticsearch Service (Amazon ES) give you a lot of choice of instance types. 

you can reduce cost by selecting the right storage solution for your needs. For example, Amazon S3 offers a variety of storage classes, including Standard, Reduced Redundancy, and Standard-Infrequent Access. Other services, such as Amazon EC2, Amazon RDS, and Amazon ES support different Amazon Elastic Block Store (Amazon EBS) volume types (magnetic, general purpose SSD, provisioned IOPS SSD) that you should evaluate.


###Elasticity
Auto Scaling to dispatch resources

Where possible, replace Amazon EC2 workloads with AWS managed services that either don’t require you to take any capacity decisions (e.g., ELB, Amazon CloudFront, Amazon SQS, Amazon Kinesis Firehose, AWS Lambda, Amazon SES, Amazon CloudSearch) or enable you to easily modify capacity as and when need (e.g., Amazon DynamoDB, Amazon RDS, Amazon Elasticsearch Service)

###take Advantage of the Variety of Purchasing Options
Reserved Capacity
Reserved capacity options exist for other services as well (e.g., Amazon Redshift, Amazon RDS, Amazon DynamoDB, and Amazon CloudFront).

Spot Instances
allow you to bid on spare Amazon EC2 computing capacity. Since Spot Instances are often available at a discount compared to On-Demand pricing, you can significantly reduce the cost of running your applications.

Spot Instances are ideal for workloads that have flexible start and end times.Your Spot Instance is launched when your bid exceeds the current Spot marketprice, and will continue run until you choose to terminate it, or until the Spotmarket price exceeds your bid. If the Spot market price increases above your bidprice, your instance will be terminated automatically and you will not be chargedfor the partial hour that your instance has run.As a result, Spot Instances are great for workloads that have tolerance tointerruption. However, you can also use Spot Instances when you require more predictable availability:
Bidding strategy: You are charged the Spot market price (not your bid price) for as long as the Spot Instance runs. Your bidding strategy could be to bid much higher than that with the expectation that even if the market price occasionally spikes you would still be saving a lot of cost in the long term. 
Mix with On-Demand: Consider mixing Reserved, On-Demand, and Spot Instances to combine a predictable minimum capacity with “opportunistic”access to additional compute resources depending on the spot market price. Thisis a great way to improve throughput or application performance.
Spot Blocks for Defined-Duration Workloads: You can also bid for fixed duration Spot Instances. These have different hourly pricing but allow you to specify a duration requirement. If your bid is accepted your instance will continue to run until you choose to terminate it, or until the specified duration has ended; your instance will not be terminated due to changes in the Spot price (but of course, you should still design for fault tolerance because a Spot Instance can still fail like any other EC2 instance).

##Caching
Application Data Caching
Amazon ElastiCache is a web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud. It supports two open-source in-memory aching engines: Memcached and Redis.

Edge Caching
CDN --> Amazon CloudFront

##Security
###Utilize AWS Features for Defense in Depth
- VPC
- AWS WAF, a web application firewall, can help protect your web applications from SQL injection and other vulnerabilities in your application code.
- IAM 
- data encryption

###Offload Security Responsibility to AWS
AWS is responsible for the security of the underlying cloud infrastructure and you are responsible for securing the workloads you deploy in AWS

###Reduce Privileged Access
This is particularly important 
in an elastic compute environment where servers are temporary. You can use  Amazon CloudWatch Logs to collect this information 
IAM roles to grant permissions to applications running on Amazon EC2 instances through the use of short-term credentials. 

###Security as Code
you can create an AWS CloudFormation script that captures your security policy and reliably deploys it.

Additionally, for greater control and security, AWS CloudFormation templates can be imported as “products" into AWS Service Catalog40. This enables centralized management of resources to support consistent governance, security, and compliance requirements, while enabling users to quickly deploy only the approved IT services they need. You apply IAM permissions to control who can view and modify your products, and you define constraints to restrict the ways that specific AWS resources can be deployed for a product

###Real-Time Auditing
it is possible to implement continuous monitoring and automation of controls to minimize exposure to security risks. Services like AWS Config, Amazon Inspector, and AWS Trusted Advisor continually monitor for compliance or vulnerabilities giving you a clear overview of which IT resources are in compliance, and which are not. 
