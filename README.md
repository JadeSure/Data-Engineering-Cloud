# Data-Engineering-Cloud

## Day1 What's different between two models: Inmon and Kimball？ 
Inmon model needs a long term to build the system from different data source (OLTP) to create data warehouse, which can be used by different department(data mart) to use the model.
drawback: 
Kimball model based on application to look for needed data from data source. more dynamic.
## OLTP and OLAP？
OLTP: Online Transaction Processing, for RDBMS: increase, modify, delete, check(search)
OLAP: On-Line Analytical Processing, for Data warehouse
## Data mart?
prepared the data for relative applications, which will be used directly.
## What does Data Lake mean？
Do not care data categories when it loaded data, which is used in machine learning. 
## IaaS，PaaS，SaaS ？ 
IaaS: Infrastructure as a Service PaaS: Platform as a Service SaaS: Software as a Service 
## The feature of DBMS: what is ACID？ meaning？ 
ACID(Atomicity, Consistency, Isolation, Isolation)  
Atomicity (Abort/commit) only have this two status, True or False. eg.  all the instructions within a transaction will successfully execute, or none of them will execute;  
Consistency: The database must be consistent before and after the transaction;  
Isolation: Multiple transactions occur independently without interference;  
Durability: The changes of a successful transaction occurs even if the system failure occurs. For instance, if Bob’s account contains $120, this information should not disappear upon hardware or software failure.

## The relationship of group, user, policy and role?
One group has many users, which has the permission to access other services.  
policy is formated as json type that writes the limitation for applications. (The mininum unit) It gives permissions to what a user/Group/Role is able to do.  
Role is for AWS service to service.

## What does programmatic access? what should provide?
Enables an access key ID and secret access key for the AWS API, CLI, SDK, and other development tools. this can be used in scripts and thus to automate processes and actions. Can get autogenerated password or custom password.

## what does console access? the function of MFA?
Console access: Controlling user access to the AWS Management Console (AWS console login);


MFA: Multi-factor authentication, it use to increase the security of your AWS environments. Signing in to MFA-protected accounts requires a user name, password, and an authentication code from an MFA device.

## The type of permission policy? what does that present? what does least privillage?
Identity Based Permission: Permission policy has three kinds of types:  
Predefined aws policy(management aws policy), can only be read. It cannot be modified.  
Customerised policy(by yourself requirements to define a policy).  
Inline policy(only use once, cannot be checked by other identity), 
Least privillage: As a best practice, define permissions for only specific
resources in specific accounts. Alternatively, you can grant least privilege
using condition keys.  
### Advanced permissions
Resource based permission, eg. bucket permission with other services;  
Session based permission, eg. visit a website with token based on session time;  

## Day2 Different between DATABASE and DBMS?
DBMS: Database Management System. DBMS eg. mysql, mssql --> database： increase, delete, modify, query.

## Can I install database in S3?
No. S3 bucket is not a file system. Everything in the S3 is seen as an object.  
S3 cannot be a database, but it can store data.

## What does key mean in S3? Value mean in S3?
Key is an object name. s3 bucket is the unique domain name + file name.  
value is an object. Every file in the S3 is an object.

## Where can I implement S3 in Inmon and Kimball.
Where Inmon and Kimball is a DBMS. structured data

In the OLTP data Sources part. Because S3 can be access easily without file size limitaion, which can be used to store the initial(middle) data from any source. Then, we can set an alert or lambda to reminder or automatic operation data processing in the next part. Besides, it can store NoSQL data, such as video, audio, pictures...  
another example, lambda read data from s3 --> save it into aws database
Can store data rather than build database system in the S3.

## Implementation codes in EC2
### connect to the ec2 server
`ssh -i my-first-key-pair.pem ec2-user@ec2-3-25-179-100.ap-southeast-2.compute.amazonaws.com`

### check ec2 describe (AWS command line)
`aws ec2 describe-instances --region ap-southeast-2`

### check aws configure
`aws configure
		- access key ID
		- access key
		- region name
		- output formate`

### check available s3 bucket
`aws s3 ls`

### copy files from s3 to EC2
`aws s3 cp --recursive [name of bucket eg. s3://] [local address eg. /home/ec2-user]`

### terminate your aws instance
`aws ec2 terminate-instances --instance-ids [your id]`

## The process of building a server host in the EC2?
```python
sudo yum update
sudo su
yum update -y
yum install httpd -y 
cd /var/www/html/
vi index.html (edit in vim)
service httpd start
```

## How many kinds of Load Balancer?
• Application Load Balancer (http and https traffic): according the
traffic load to leading different server(In the application layer, Layer 7).  
• Network Load Balancer (TCP/UDP traffic): leading the traffic to the
server which have higher capacity in the transfer layer (Layer 4),
use for extreme performance.  
• Classic Load Balancer: can use http/https and TCP traffic by X-Forwarded and sticky seesions.

## What does X-Forwarded and sticky sessions?
Due to internal servers they communicate with their private IP address(internal network),
X-Forwarded helps the server when they need to identify the original
request source (public IP address).  
Known the original request source can help us to do some basic data
analysis, such as geographic location or orgnasation’s name.  
Sticky sessions: each request come from the same source can be hosted
into the same server. Disadvantage: can cause load unbalanced because of keeping to connect to this server.

## What is the risk of using aws configure to save credentials?
For security reason, can not store credentials in ec2 instance, that will
cause huge loss. Using roles in this way has several benefits. Because role
credentials are temporary and rotated automatically, you don't have to
manage credentials, and you don't have to worry about long-term
security risks. In addition, if you use a single role for multiple instances,
you can make a change to that one role and the change is propagated
automatically to all the instances.

## If it contains aws configure and EC2 instance role to access S3, which one has a higher priority?
IAM role. When an IAM role is attached to the instance, the AWS CLI
automatically and securely retrieves the credentials from the instance
metadata.  
Using roles to grant permissions to applications that run on EC2
instances requires a bit of extra configuration. An application running on
an EC2 instance is abstracted from AWS by the virtualized operating
system. Because of this extra separation, an additional step is needed to
assign an AWS role and its associated permissions to an EC2 instance and
make them available to its applications. This extra step is the creation of
an instance profile that is attached to the instance. The instance profile
contains the role and can provide the role's temporary credentials to an
application that runs on the instance. Those temporary credentials can
then be used in the application's API calls to access resources and to
limit access to only those resources that the role specifies. Note that
only one role can be assigned to an EC2 instance at a time, and all
applications on the instance share the same role and permissions.
[reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html#roles-usingrole-ec2instance-permissions)

## Day3 What are the characteristics of lambda？
AWS Lambda is a serverless compute service that runs your code in response to events and automatically manages the underlying compute resources for you. You can use AWS Lambda to extend other AWS services with custom logic, or create your own back-end services that operate at AWS scale, performance, and security.  
The code you run on AWS Lambda is called a “Lambda function.” After you create your Lambda function it is always ready to run as soon as it is triggered, similar to a formula in a spreadsheet. Each function includes your code as well as some associated configuration information, including the function name and resource requirements. Lambda functions are “stateless,” with no affinity to the underlying infrastructure, so that Lambda can rapidly launch as many copies of the function as needed to scale to the rate of incoming events.[reference](https://aws.amazon.com/lambda/features/#:~:text=AWS%20Lambda%20is%20a%20serverless,scale%2C%20performance%2C%20and%20security.) 

## What is the function of Database transaction log？
When you do a recovery/data failed, AWS will first choose the most recent daily back up, and then apply transaction logs relevant to that to backup database. This allows you to do a point in time recovery down to a second,  within  the  retention period.

## Database encryption is used in which direction?
transit: using SSL(Secure Socket Layer) (data is transited)
rest: using AES-256 encryption (data is stored)

## The comparision with Multi-AZ and Read Replicas?
Multi-AZ:  synchronized  database  in  another Availability Zone. For database failure **recovery** only. (eg. if the master does not crash, backup will never be used. otherwise, it will transfer automatically.)  
Read Replicas: asynchronous for improving performance. Allow you to have a *read-only* copy of your production database. Use for read-heavy database workloads *(scaling)*.

## Day4 Please list the local development process by steps, use github to control code and repository. Begin from git clone
```python
git clone git@github.com:JadeSure/Data-Engineering-Cloud.git
git branch dwh // create a new branch, named dwh
git checkout dwh // change to the new branch, dwh
git status // check is any changed in this branch
git add test.py // add changed file to index
git commit -m 'message'//commit changes in Index
git checkout master // switch to the master branch first
git merge dwh -m 'message' // merge dwhbranch back to master branch
git status // check is any changed in this branch
git add test.py // add changed file to index
git commit -m 'message'//commit changes in Index
git push --set-upstream origin master // push the change into remote repositoryin github webpage create a pull request
git branch -d dwh // delete the feature branch which is already merged into the remote main branch
git pull origin master// updated the changes in remote main branch into local main branch
```
1. clone from github to local  
2. create a new branch and modify everything in this branch  
3. pull the newest github version in master  
5. add and comments change in local branch  
6. go back to main, merge branch and main(master). If conflict, modify codes; (or push into branch, ask an admin to agree your merging process)   
7. push into github master.

## What is the different between Batch Processing and Real Time Processing?
Batch Processing: 
• **blocks of data** that have already been stored over a period of time.  
• to process **large volumes** of data to get **more detailed insights** than it is to get fast analytics results.  
• provides the modeling-ready data for **machine learning**, or writes the data to a data store that is optimized for **analytics and visualization**.  

Real Time Processing:  
• deals with streams of data that are captured in  real-time and processed with **minimal latency** to generate real-time (or  near-real-time) reports or automated responses.   
• typically arrives in an **unstructured or semi-structured format**, such as JSON, and has the same processing requirements as batch processing, but with shorter turnaround times to support real-time consumption.

## What does Data Extraction/Ingestion? Direction?
• Pull/push the data: push by client to our server or pull from client by sftp, ftp, API...  
• Landing server mechanism: a middle layer between data input source and ETL process to reduce input complexity.(less firewall, NCAL, security group; 1 to M)  
• Basic data information check: jump footer and header lines; EOT(end of transferring) to show that this file has been finished transferring.  
• Key points, such as file format, file layout, file encoding, file delimiter and so forth.

## Day5 Data Warehouse: a historical version of transactional database
**Fundamental Concepts**  
**Basic Dimension Table Techniques**  
**Basic Fact Table Techniques**  
**Slowly Changing Dimension Techniques(SCD)**
## Fundamental Concepts
1. Gather requirements and data realities: understand the needs of business and the realities of the underlying source data, and data realities are uncovered by meeting with source system experts and doing high-level data profiling to assess data feasibilities.  
2. Collaborative dimensional modeling workshops: blueprint  
3. Four step dimensional design process
• Select the business process  
• Declare the grain  
• Identify the dimensions
• Indentify the facts
5. Business process: operational activities performed by your organization.(a specific design target, grain...)  
6. Grain: levels at which facts at measured (keep the lowest possible grain, the grain must be declared before choosing dimensions or facts every candidate dimension or fact must be consistent with the grain) eg. city, suburb, town...  
7. Dimensions for descriptive context: contain the entry points and descriptive labels that enable the DW/BI system to be leveraged for business analysis. provides (who, what, where, when, why and how)  
8. Facts for measurements: based on a business process event for numeric. eg. the quantity of a product sold by windows function, aggregation.  
10. Star schemas and OLAP cubes: consist of fact tables linked to associated dimension table via primary/foreign ky relationships.  
11. Graceful extensions to dimensional models: dimensions can be added to an existing fact table by creating new foreign key columns, presuming they do not alter the fact table's grain; Attributes can be added to an existing dimension table.  

## Basic Dimension Table Techniques
• Dimension table structure  
• Dimension surrogate keys  
• Natural, durable and supernatural keys  
• Drilling down  
• Degenerate dimensions  
• Denormalized flattened dimensions  
• Multiple hierarchies in dimensions  
• Flags and indicators as dimension attributes  
• Null attributes in dimensions  
• Calendar date dimension  
• Role playing dimensions  
• Junk dimensions  
• Snowflaked dimensions  
• Outrigger dimensions
#### Dimension table structure
Every dimension table has a single primary key column, which is embedded as a foreign key in any associated fact table.  
Dimension tables are usually wide. With the Star Schema.

#### Dimension surrogate keys
The dimension surrogate keys are simple integers, assigned in sequence, starting with value 1, every time a new key is needed.  
The date dimensions exempt from the surrogate key rule; this highly predictable and stable dimension can use a more meaningful primary key.  

#### Natural, durable and supernatural keys
natural key(business key): created by os system based on business rules;  
durable key(supernatural key): won't change (eg. old customer come back) should be independent of the original business process and should be simple integers assigned in sequence beginning with 1.   

#### Drilling down
adding a row header to an existing query; the new row header is a dimension attribute appended to the GROUP BY expression in an SQL query;

#### Degenerate dimensions
do not need atomic (3NF is enough).  
The degenerated dimension is placed in the fact table with the explicit acknowledgment that there is no associated dimension table.(two simple do not need to extend)

#### Denormalled flattened dimensions
merge more dimensions to 1(they have surrogate keys with each other)

#### Multiple hierarchies in dimensions
calendar date dimension: day, month, year... or region, state, district, city...  

#### Flags and indicators as dimension attributes
Cryptic abbreviations as true/false, yes/no flags to 1/0.

#### Null attributes in dimesnion
do not leval 'Null' in here, which may make a misunderstanding; leave unknown or not applicable.

#### Calendar date dimension
do not need surrogate key, date can be a primary key ==? to metadata

#### Role playing dimensions
a single physical dimension can be referenced multiple times in a fact table, with each reference linking to a logically distinct role for the dimension.

#### Junk dimensions
for too many 'join' to different similar tables, which can be used to combine together to avoid meaningless joins. eg. 3, 3, 3 tables and then combine it to 3*3*3 to 27 rows one table.

#### Snowflaked dimensions
multiple hierarchy with including each other (not recommend)

#### Outrigger dimensions
with dimensions and subdimension (one layer extension)....

## Basic Fact Table Techniques
• Fact table structure  
• Additive, semi-additive, and non-additive facts  
• Nulls in fact tables  
• Conformed facts  
• Transaction fact tables  
• Periodic snapshot fact tables  
• Factless fact tables  
• Aggregated fact tables or cubes
• Consolidated fact tables

#### Fact table structure  
Contains many dimension key(forign key for the dimension table) and also include fact information.
#### Additive, semi-additive, and non-additive facts 
additive: can be added (sum up)    
semi-additive: not sure numerical value can be sumed. eg. shop1 and shop2(1 updated but another one not) for the current semi-additive.  
non-additive: eg. non-additive

#### Nulls in fact tables  
no null; replace it with 0, N/A...
#### Conformed facts  
they can share the same dimensions...
#### Transaction fact tables  
only additive; injection and append.
#### Periodic snapshot fact tables 
Sales in a period for the fact tables

#### Accumulating snapshot fact tables
Consuming periodically/customer life cycle

#### Factless fact tables 
no transaction (rarely use)
#### Aggregated fact tables or cubes
make a summary based on a certain fact
#### Consolidated fact tables
combine relative fact tables together to get a consolidated fact table.

## Slowly Changing Dimension Techniques
1. Type1: Sometimes: modify directly in the original table (overwrite);  
2. Type2: Frequently: add a transaction and modify effective date;  
3. Type3: Rarely: type1 + type2 (less use)
4. Type4: Mini dimension: 

## Advanced Fact table
#### Fact table with surrogate key (rarely use)
fact table do not have primary key(all is foreign key) ps: multi foreign key can become a primary key.

#### Numeric values as attriutes or facts
put numeric value to a range(band)

#### Header/line fact tables
for retail, eg. order ID with multi transaction ID (children)  

#### Profit and loss fact tables using allocations
profit = revenue - cost

#### Multiple currency facts
tracking multiple currencies with a daily currency 
