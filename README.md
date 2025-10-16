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

STEP 2: Configure Database Migration Services (DMS)

To be able to launch a DMS Replication instance, it is necessary to specify what subnet group in the VPC the Replication instance was created. A subnet is a range of IP addresses in your VPC in a given Availability Zone. These subnets can be distributed among the Availability Zones for the AWS Region where your VPC is located. DMS Replication instance requires at least two Availability Zones. A replication instance performs the actual data migration between source and target endpoints. Your instance needs enough storage and processing power to perform the tasks that migrate data from your source database to your target database. How large this replication instance should be depends on the amount of data to migrate and the tasks your instance needs to do.

Task 2.1: Configure WordPress (MySQL) replication subnet group

<img width="805" height="171" alt="{325DE952-802C-489F-9811-B8D5C9AE0709}" src="https://github.com/user-attachments/assets/39c586ff-e85d-4e25-9cdc-36f06e28e3ec" />

In navigation group choose Subnet groups

Click Create Subnet group

Subnet group configuration:

<img width="997" height="466" alt="{42F6C0CA-FC74-4C08-AA61-BB97EA722633}" src="https://github.com/user-attachments/assets/40fd6841-495c-4099-b807-cc56c0ce7a4c" />

Add subnets

<img width="1003" height="170" alt="{221ADBDA-E6F2-4080-88D0-F4EC048D09AF}" src="https://github.com/user-attachments/assets/ab9ad7a8-3ccc-40a1-a3dd-85613337b08e" />

Task 2.2 Create replication instance

Create a replication instance that has sufficient storage and processing power to perform the tasks you assign and migrate data from your source database to the target database. The required size of this instance varies depending on the amount of data you need to migrate and the tasks that you need the instance to perform.

Choose Replication instances

<img width="336" height="199" alt="{B176ED79-9ED6-41CE-A777-8CD2B2C3E15A}" src="https://github.com/user-attachments/assets/86309a5f-b1e4-42c6-89e2-3e5cc621078d" />

then click create replication instance

<img width="825" height="514" alt="{88B42EA0-3EB9-4938-B587-70AE65A977CF}" src="https://github.com/user-attachments/assets/2d118682-2b52-4c51-9872-a3fe029296ea" />

connectivity and security

<img width="1225" height="237" alt="{DC8B1B80-04F1-4F24-A3DA-D73DEA0704CB}" src="https://github.com/user-attachments/assets/47e088e4-3653-4cb1-862b-e3511fce24a4" />

<img width="166" height="117" alt="{CCAB4FD3-EB97-4E24-A42A-96D630685BFF}" src="https://github.com/user-attachments/assets/0da41036-f46e-45fe-97b1-6152b5b1b7ae" />

<img width="1193" height="241" alt="{B71CC0E4-AF81-419B-A29E-A6250A33B5B6}" src="https://github.com/user-attachments/assets/ce73d23e-30df-40d5-9207-5931277b6734" />

Advanced Settings:

<img width="1229" height="295" alt="{7458DA62-BCAE-4713-AC4C-AE22B2457BD2}" src="https://github.com/user-attachments/assets/e6d0a9e9-c2be-4217-9aa7-54baa2529f6b" />

Click create replication instance

Task 2.3: Create source endpoint

Use your EC2 IPV4 and click on Endpoints under Migrate data

<img width="288" height="185" alt="{A450B994-2E5B-42FA-8810-ECE28B658779}" src="https://github.com/user-attachments/assets/76d341a9-ddba-480b-bf7b-a8df223d817f" />

click create Create endpoint

Endpoint type:

<img width="1000" height="288" alt="{8A1779F8-E632-4F86-A12F-4A4AE1BAEA46}" src="https://github.com/user-attachments/assets/55816aeb-f593-4f04-ad75-6c371b4a2fe1" />

Endpoint configuration:

<img width="996" height="644" alt="{E0534BA5-D4FB-493E-8AAE-81572963A4E6}" src="https://github.com/user-attachments/assets/a2c597c3-5bfd-446b-8ca2-c0cf31236d81" />

<img width="1005" height="475" alt="{A5660DDB-2FD2-4068-903A-269811E8D905}" src="https://github.com/user-attachments/assets/aea1652b-16d4-4073-861f-ae33162e099c" />

Then Run Test

<img width="1006" height="768" alt="{7AD99C11-FD42-4535-98D4-BB72FF61AEEE}" src="https://github.com/user-attachments/assets/96885f0a-85a6-4c6a-98bc-c5af795376ae" />

click Create endpoint

Task 2.4: Create target endpoint

Again click on Create endpoint

<img width="998" height="336" alt="{F382E3BE-862F-403C-945A-44699B20DAFD}" src="https://github.com/user-attachments/assets/67532a99-6cd0-481a-8f17-7f81039fe87e" />

Endpoint configuration:

<img width="1003" height="655" alt="{0471F0A3-1D8A-4E83-B797-7B7E53C7BC7E}" src="https://github.com/user-attachments/assets/d2c0eecb-f3b7-43fa-931a-917b3b14d2f6" />

select replication instance

<img width="989" height="321" alt="{6E14479A-D005-42BD-B1DF-FA02E4BD5986}" src="https://github.com/user-attachments/assets/7ebab7fe-f8bb-4e4c-9ecc-85f526cf9d92" />

then click on Run test and click Create endpoint

Task 2.5: Create migration task

Create a migration task. An AWS Database Migration Service (AWS DMS) task is where all the work happens. You specify what tables (or views) and schemas to use for your migration and any special processing, such as logging requirements, control table data, and error handling.

Choose Database Migration Task

<img width="328" height="196" alt="{4388415A-659E-4F3F-B8C7-5824B253ADA7}" src="https://github.com/user-attachments/assets/7e8c1fac-9727-4ce8-b95b-520a6aa3ac56" />

Then click Database Migration Task

Task Configuration:

<img width="1061" height="730" alt="{9CEA915C-8766-4239-ACF8-CB37A74675CE}" src="https://github.com/user-attachments/assets/8604a8a3-2fa6-4a96-8684-a78880e4c602" />

Task Settings:

<img width="1066" height="759" alt="{F1C4E375-C30A-41B1-A960-973AC4996E6F}" src="https://github.com/user-attachments/assets/46b93472-b72c-4f9a-876e-18ab58dd0008" />

Table Mappîngs:

<img width="1065" height="767" alt="{6D7584C0-77F2-4870-92DF-CD795E4D5B68}" src="https://github.com/user-attachments/assets/6de79f6f-473f-4e20-8ead-ffc74d8eec72" />

click Add new selection rule

Premigration assesment:

<img width="1070" height="436" alt="{E4509244-CE2A-4B72-ABBC-3A6E913C16EA}" src="https://github.com/user-attachments/assets/d6e6f6e1-22b0-4e70-acc9-ecf186bf5006" />

Then click on Create database migration task

Task 2.6: Update DNS records

Now that all databases have been migrated to Aurora, it’s time to update the DNS information so the application server can connect to the related Aurora database server. Both apps are using a DNS entry as a connection hostname. In a real-world application migration, once you have completed all of your testing and are ready to fully transition your databases to Aurora, you should perform the shutdown of the source servers and update the DNS records properly to reflect the new database servers running in Aurora.

Next in EC2, on seesion manager tab click connect and check the current database DNS record running:

bash: nslookup your-db

************************
**** EXAMPLE OUTPUT ****
************************

Server:         123.456.7.891
Address:        321.654.8.456#78

Name:   your-db.onpremsim.env
Address: xxx.yyy.1.879

the below cmd creates a variable to store the endpoint of the target database

bash: ADDR="ENDPOINT"

Run the following cmd to update the DNS configuration of wordpress-db to point to the target database:

HOST="your-db.onpremsim.env"
sudo touch /tmp/nsupdate.txt
sudo chmod 666 /tmp/nsupdate.txt
echo "server dns.onpremsim.env" > /tmp/nsupdate.txt
echo "update delete $HOST A" >> /tmp/nsupdate.txt
echo "update delete $HOST PTR" >> /tmp/nsupdate.txt
echo "update add $HOST 86400 CNAME $ADDR." >> /tmp/nsupdate.txt
echo "send" >> /tmp/nsupdate.txt
sudo nsupdate /tmp/nsupdate.txt

Verify the DNS name resolution again and check if it was updated to a CNAME pointing to your Aurora database (compare the output with the previous DNS lookup):

bash: nslookup wordpress-db

************************
**** EXAMPLE OUTPUT ****
************************

Server:         123.456.7.891
Address:        321.654.8.456#78

your-db.onpremsim.env      canonical name = mid-your-instance-1.fezrezrez.us-dd-2.rds.amazonaws.com.
Name:   mid-yourdb-instance-1.qsdqssqfq.us-zeze-2.rds.amazonaws.com
Address: 00.0.0.44

Run the following command to shutdown the source server:

sudo shutdown -h now

Conclusion:

We have successfully done the following:

1. Created an Amazon Aurora database
2. Migrated the existing database to Aurora with AWS DMS
3. Updated the DNS records to reflect the migration

REFERENCES:
https://docs.aws.amazon.com/dms/

































