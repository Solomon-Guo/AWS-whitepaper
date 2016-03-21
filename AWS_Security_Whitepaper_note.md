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
