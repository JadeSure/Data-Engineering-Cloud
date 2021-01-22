# Data-Engineering-Cloud

## What's different between two models: Inmon and Kimball？ 
Inmon model needs a long term to build the system from different data source (OLTP) to create data warehouse, which can be used by different department(data mart) to use the model.
drawback: 
Kimball model based on application to look for needed data from data source. more dynamic.
## OLTP and OLAP？
OLTP: Online Transaction Processing, for RDBMS: increase, modify, delete, check(search)
OLAP: On-Line Analytical Processing, for Data warehouse
## data mart?
prepared the data for relative applications, which will be used directly.
## What does Data Lake mean？
Do not care data categories when it loaded data, which is used in machine learning. 
## IaaS，PaaS，SaaS ？ 
IaaS: Infrastructure as a Service PaaS: Platform as a Service SaaS: Software as a Service 
## The feature of DBMS: what is ACID ？ meaning？ 
ACID(Atomicity, Consistency, Isolation, Isolation)  
Atomicity (Abort/commit) only have this two status, True or False. eg.  all the instructions within a transaction will successfully execute, or none of them will execute;  
Consistency: The database must be consistent before and after the transaction;  
Isolation: Multiple transactions occur independently without interference;  
Durability: The changes of a successful transaction occurs even if the system failure occurs. For instance, if Bob’s account contains $120, this information should not disappear upon hardware or software failure.
