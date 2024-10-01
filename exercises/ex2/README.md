# Exercise 2 - Environment Setup - 2

In this exercise, we now set up the database tables and the staging configuration on the Snowflake side.

## Exercise 2.1 Create table in Snowflake

1. Login to you Snowflake account
2. Click on Data -> Databases and then select your database and then the schema where you want your new table. Click on the Table and then select the warehouse which you want to use as your runtime. 
3.	Click on the "Create" button on the top right corner, then select "Table" -> "Standard". You will get an SQL editor. You can use this below SQL statement as a reference.
```sql
create or replace TRANSIENT TABLE <Database>.<Schema>.<Table Name> (
	PRODUCT VARCHAR(50),
	CUSTOMER VARCHAR(50),
	LOCATION VARCHAR(50),
	CONSDEMAND NUMBER(38,0),
	KEYFIGUREDATE DATE,
	UNITS VARCHAR(4),
	COMMENTS VARCHAR(100)
);
```
4. Once you have decided on your SQl statment, then use tht "Create Table" button to run the SQL statment. This would then create your table in your database. 
<br>![](/exercises/ex2/images/02_02_0010.png)

5.  Go to the Home screen of SAP Cloud Integration. Click on "Monitor" and then "Integrations and API". Select the tile called "Security Material". 
6.  Select the Runtime as "All" on the top of the right pane. Then use the "Create" drop down menu and select "User Credentials".
7.  Create a credentials for storing the Snowflake basic authentication details. Remember this name.
 

## Exercise 2.2 Create external stage using AWS S3

You are free to choose any bulk loading mechanisim [defined by Snowflake](https://docs.snowflake.com/en/user-guide/data-load-overview). In this excercise, we use the [Amazon S3 as an external staging area](https://docs.snowflake.com/en/user-guide/data-load-s3). In this step we configure an external stage on Snowflake.

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
6.  Go to the Home screen of SAP Cloud Integration. Click on "Monitor" and then "Integrations and API". Select the tile called "Security Material". 
7.  Select the Runtime as "All" on the top of the right pane. Then use the "Create" drop down menu and select "Secure Parameter".
8.  Create two secure parameters, one for storing the AWS Access Key and the other for the AWS Secret key. Remember these parameter names.


## Summary

In this excercise, you have create a table and set up an external stage on Snowflake to import data. You can also consider a directory name and a file name for test purposes This could be the place inside Amason S3 bucket to stage your data from SAP IBP.

In the next step we will work with the iFlow to read data from SAP IBP and then store it on the above stage and then copy it to the table we created. Click here to continue - [Exercise 3 - Reading from SAP IBP and Writing to Snowflake ](../ex3/README.md)
