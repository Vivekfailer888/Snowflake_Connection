# Snowflake_Connection
# Connecting Snowflake with  AWS[ S3 Bucket, IAM ] as a Cloud Infrastructure[Storage]
# CREATE A SNOWFLAKE CONSOLE.
Step 1- Create a snowflake account and Login./n
Step 2- Go to Manage -> Compute -> Warehouses.
Step 3- Create a warehouse by clicking on [+Warehouse] and Give the specification that is needed.
# CREATE  AWS CLOUD STORAGE.
Step 4- Go to AWS website.
Step 5- Clicking on All Services then select on S3 on Storage for Cloud storage.
Step 6- In General configuration on S3 [Bucket name] and Select AWS region.
# CREATE  AWS IAM ROLE. 
Step 7- Clicking on All Services then select on IAM[Identity and Access Management] on Security, Identity,& Compliance for Cloud storage.
Step 8- Click Create Role and In Confi. Select trusted entity [AWS account] ,An AWS account[This account], Options [click on box of [require external ID] 
and fill any number in it].
Step 9- In Add permissions select on [AmzonS3FullAccess] and Give role name.
# Integration of snowflake with S3 and IAM 
Step 10-In Project-> Workspaces -> Add new and use your created warehouse.
step 11-  CREATE OR REPLACE STORAGE INTEGRATION PBI_Integration
          TYPE = EXTERNAL_STAGE
          STORAGE_PROVIDER = 'S3'
          ENABLED = TRUE
          STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::825765422200:role/powerbi.role'
          STORAGE_ALLOWED_LOCATIONS = ('s3://powerbi.project/')
          COMMENT = 'Optional Comment'


//description Integration Object
          desc integration PBI_Integration;

 //drop integration PBI_Integration
Step 12- Use your name on INTEGRATION as [BI_Integration] and then STORAGE_AWS_ROLE_ARN copy your ARN no. of role in IAM.
Step 13- STORAGE_ALLOWED_LOCATIONS copy your S3[Name].
Step 14- Run esc integration PBI_Integration; Command and Copy STORAGE_AWS_IAM_USER_ARN and STOARGE_AWS_EXTERNAL_ID
Step 15- Go to IAM and edit trust policy and Update AWS and ExternalId.
Step 16- Create database , Schema and table


CREATE database PowerBI;

create schema PBI_Data;

create table PBI_Dataset 
(
Year int,	Location string,	Area	int,
Rainfall	float, Temperature	float, Soil_type string,
Irrigation	string, yeilds	int,Humidity	float,
Crops	string,price	int,Season string

);
Step 17- Create a stage 
select * from PBI_Dataset;

//drop database test;

create stage PowerBI.PBI_Data.pbi_stage
url = 's3://powerbi.project'
storage_integration = PBI_Integration

//desc stage s1

//drop stage s1;

Step 18- Copy data from csv file to [PBI_Dataset] Table.
copy into PBI_Dataset 
from @pbi_stage
file_format = (type=csv field_delimiter=',' skip_header=1 )
on_error = 'continue'

list @pbi_stage
