# Database-Migration-Using-AWS-DMS

GOAL: Migrating current database to a cloud native database with Amazon Aurora. 

The current architecture includes:

- An platform / application that uses a MariaDB database.

AWS Service Used: I have used AWS Database Migration Service to migrate the current database to Amazon Aurora.

OBJECTIVES:

1. Create an Amazon Aurora database.
2. Migrate existing database to Aurora using AWS DMS.
3. Update DNS records to reflect the migration.

NOTES:

AWS Database Migration Service:

1. AWS Database Migration Service helps you migrate databases to AWS quickly and securely.
2. AWS Database Migration Service supports homogeneous migrations such as Oracle to Oracle.
3. Heterogeneous migrations between different database platforms, such as Oracle or Microsoft SQL Server to Amazon Aurora.

AWS Aurora:

1. Amazon Aurora is up to five times faster than standard MySQL databases and three times faster than standard PostgreSQL databases.
2. Amazon Aurora is fully managed by Amazon Relational Database Service (RDS).
3. Automates time-consuming administration tasks like hardware provisioning, database setup, patching, and backups.

STEP 1: Create Aurora target database

TASK 1.1 Create an Aurora database using Amazon Relational Database Service.

<img width="1124" height="218" alt="{53D47EED-C599-481F-9F2E-324C7498D536}" src="https://github.com/user-attachments/assets/0e90ddd6-4574-44f9-8958-bdb2d35fe41d" />

Choose Create database

<img width="1840" height="263" alt="{2DDDE8E1-E9D3-4A9B-93C9-E2E9DB0D66C2}" src="https://github.com/user-attachments/assets/6b53bf4b-b115-4d1f-851c-62c6b734f1fe" />

Engine Options

<img width="1830" height="608" alt="{6D3010AE-EAE6-40B8-9867-345D1DCD1A6B}" src="https://github.com/user-attachments/assets/507f0aaf-f0a3-41d8-a921-9b925125537f" />

Version - Choose Latest Version

<img width="1828" height="213" alt="{90AD0892-D1CD-42C7-8394-2FCE7B63BD11}" src="https://github.com/user-attachments/assets/9815563f-e5d8-4ba2-a523-4337343f09d6" />

Templates - Choose your preference

<img width="1834" height="210" alt="{15E8E138-C9C9-4A6D-AA6B-A64D07B7F224}" src="https://github.com/user-attachments/assets/dbc79544-7cc4-4bec-b4db-0f2e9de1f850" />

Then Fill in the credential settings

Cluster storage configuration

<img width="1811" height="313" alt="{B0D2ABBD-292F-4FA7-8CC9-5D3884EA4DAC}" src="https://github.com/user-attachments/assets/ff3577f6-68eb-403b-a781-1c954fae6e4c" />

Instance Configuration

<img width="1249" height="417" alt="{8FA5DBE7-DF18-4FC3-BA9B-15D2A1B02C6F}" src="https://github.com/user-attachments/assets/4a1e9b0a-e1a4-4592-a0b6-3a9d8a0f038b" />

Availability & Durability

<img width="923" height="220" alt="{C6117DDB-E020-448A-9487-0E99BA241583}" src="https://github.com/user-attachments/assets/3a484834-ccb7-4957-88d4-305e76a46a96" />

Connectivity

In the Connectivity section:
For Virtual private cloud (VPC), select Target.

Public access:
<img width="1799" height="188" alt="{43820606-B0FD-4301-A42F-CF6A7E8A4B6A}" src="https://github.com/user-attachments/assets/c5f44d7b-d0ce-4a62-8d90-a3c0ba9bd686" />

<img width="1816" height="389" alt="{842B8C4C-2570-4B67-880C-244A990E9EC0}" src="https://github.com/user-attachments/assets/b7dafa4c-4933-4ae0-9ecf-5f58427e35d0" />

Monitoring:

<img width="1827" height="471" alt="{B411F5C2-52AE-4B2E-8F9E-CE90A1FE94E8}" src="https://github.com/user-attachments/assets/d855687f-3c0d-4f84-af14-75eb715404c5" />

Additional Configuration:

<img width="1842" height="242" alt="{A5050494-0B61-4475-91EA-BD0EDAC5534D}" src="https://github.com/user-attachments/assets/e1793fc1-9735-4a57-88b6-f54345d922be" />

Encryption:

<img width="1195" height="291" alt="{AAEE7909-EE88-45C0-890E-B420717DE7F1}" src="https://github.com/user-attachments/assets/1eee8b85-b9ce-465e-aa75-ca7d8cbf5e5a" />

Then click Create Database

<img width="313" height="55" alt="{7F173192-61F1-42F0-8A53-AC34B1C6D5EB}" src="https://github.com/user-attachments/assets/551f1425-16c7-4a78-b994-7b1560242e4a" />

Make sure both Cluster and Instance are available

Click on the instance and choose the Connectivity & security tab and Copy the Endpoint value to a text editor for use in a future step.

TASK 1.2: Create database user

You will need EC2

<img width="498" height="172" alt="{59D05B35-06FF-43CE-8BD6-2CDDC765E2D9}" src="https://github.com/user-attachments/assets/24749a77-1cab-4c83-8979-6988428da0f1" />

Choose Instances and select your -DB and then click connect

Use Session Manager tab and click connect after use the below code

mysql -u admin -h ENDPOINT -p

after entering your pwd

create user using the below sql

CREATE USER 'land'@'%' IDENTIFIED BY 'water';
GRANT ALL ON earth.* TO 'gzaeuyg'@'%';
FLUSH PRIVILEGES;
QUIT



















