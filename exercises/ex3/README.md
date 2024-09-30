# Exercise 3 - Reading from SAP IBP and Writing to Snowflake

Once the pre-requisites are completed, we can now start to read data from SAP IBP. At a high level, the end to end flow would look like this,
<br>![](/exercises/ex3/images/03_01_0010.png)

## Exercise 3.1 Overview
The package you just created contains 2 iflows. One is for exporting data from SAP IBP into Snowflake. For this purposes, we have the "<i>Read from IBP and write Snowflake</i>" iFlow. This is a custom iFlow. It receives the parameters from an external trigger or via the configurable screen. It uses the standard reusable iflows from SAP to extract data. It communicates with SAP IBP using Websocket RFC calls using the destination we have defined in BTP Cockpit.

It would then store the data as a JSON file inside the Amazon S3 bucket which we configured with secure parameters in SAP Cloud Integration. It would also use the Snowflake credentails to execute the copy statements from the external staging location into the database table in Snowflake.  

## Exercise 3.2 Configure the parameters for the Custom iFlow.

| Parameter        | Example value  | Description |
| :---             |     :---:      | :---          |
| AWSBucketName    | hcp-XXXXXX     | AWS Bucket ID    |
| AWSAccessKeyName | AccessKey      | Secure parameter which stores the AWS access key  |
| AWSDirectory     | data/sap/in    | AWS directory path in your bucket    |
| AWSFile          | test.json      | AWS file name    |
| AWSFileType      | application/json | File type      |
| SFAddress        | org-account.snowflakecomputing.com    | URL of your Snwoflake instance    |
| SFDatabase       | test_db        | Name of your Snwoflake database    |
| SFSchema         | public         | Name of your Snwoflake Schema    |
| SFWarehouse      | mywh           | Name of your Snwoflake warehouse    |
| SFTable          | test_table     | Name of your Snwoflake Table    |
| SFCredentials    | SnowFBasic     | Name of your Snwoflake secure parameter in CI    |
| SFStageName      | test_stage     | Name of your external stage in Snowflake    |
| IBPQuerySelect   | PRDID, CUSTID  | Attribute names to be selected from IBP's planning area    |
| IBPQueryFilterString      | UOMTOID eq 'EA'    | Filter string for the RFC query    |
| IBPFields        | PRDID, CUSTID  | Attribute fields to be mapped from IBP    |
| IBPQueryOrderBy  | PRDID, CUSTID  | Orderby criteria    |
| IBPDestination   | TECHED24_IBP   | Name of your IBP destination in BTP  |
| IBPQueryTypeOfData   | KeyFigures | KeyFigures to be selected in BTP  |
| IBPBatchKey      | TECHED24_IBP   | Name of batch key - can be any string  |
| IBPPackageSizeInRows   | 100000   | Max rows to be selected in one batch  |
| IBPQueryTimeAggregationLevel  | 3 | Time profile aggregation level |
| IBPQueryOffset1  | 0              | Offset, typically 0  |


## Exercise 3.3 Mapping.
 
 
## Exercise 3.4 Staging.
## Exercise 3.5 copying.

## Exercise 2.2 Create external stage using AWS S3

You are free to choose any bulk loading mechanisim defined by Snowflake. In this excercise, we use the Amazon S3 as an external staging area. In this step we configure an external stage on Snowflake.

1.	In your Snowflake account, Click on Data -> Databases and then select your database and then the schema where you want your new stage.  
2.	Click on the "Create" button on the top right corner, then select "Stage" -> "External Stage -> Amazon S3". The Screen would look like below,

<br>![](/exercises/ex2/images/02_02_0020.png)

3.	Give the stage a name, enter your Amazon S3 URL.
4.  Select the Authentication switch to ON.
5.  Enter your AWS Key and then AWS Secret. Press "Create" button to create the external stage.
<br>![](/exercises/ex2/images/02_02_0030.png)

You can use this below SQL statement as a reference.
```sql
CREATE STAGE <Stage name> 
	URL = 's3://AWS KEY : AWS Secrect @s3-eu-central-1.amazonaws.com/AWS Bucket ID' 
	CREDENTIALS = ( AWS_KEY_ID = 'AWS KEY' AWS_SECRET_KEY = '*****' ) 
	DIRECTORY = ( ENABLE = true )
```


## Summary

In this excercise, you have create a table and set up an external stage on Snowflake to import data. You can also consider a directory name and a file name for test purposes This could be the place inside Amason S3 bucket to stage your data from SAP IBP.

In the next step we will work with the iFlow to read data from SAP IBP and then store it on the above stage and then copy it to the table we created. Click here to continue - [Exercise 3 - Reading from SAP IBP and Writing to Snowflake ](../ex3/README.md)
