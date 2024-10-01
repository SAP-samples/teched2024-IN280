# Exercise 3 - Reading from SAP IBP and Writing to Snowflake

Once the pre-requisites are completed, we can now start to read data from SAP IBP. At a high level, the end to end flow would look like this,
<br>![](/exercises/ex3/images/03_01_0010.png)

## Exercise 3.1 Overview
The package you just created contains 2 iflows. One is for exporting data from SAP IBP into Snowflake. For this purposes, we have the "<i>Read from IBP and write Snowflake</i>" iFlow. This is a custom iFlow. It receives the parameters from an external trigger or via the configurable screen. It uses the standard reusable iflows from SAP to extract data. It communicates with SAP IBP using Websocket RFC calls using the destination we have defined in BTP Cockpit. Reading data from IBP is done in an asynchronous way. First a batch is create, then the fetch query is executed. When the data is prepared by the backend, then the tech status is changed. On this change, data is then fetched, mapped and then written to the staging location. It would then store the data as a JSON file inside the Amazon S3 bucket which we configured with secure parameters in SAP Cloud Integration. It would also use the Snowflake credentails to execute the copy statements from the external staging location into the database table in Snowflake.  

## Exercise 3.2 Configure the parameters for the Custom iFlow.
For the flow to work we need the following configuration, which can be sent as a JSON payload or configured via the User Interface in the iFlow.

| Parameter        | Example value  | Description |
| :---             | :---           | :---          |
| AWSBucketName    | hcp-XXXXXX     | AWS Bucket ID    |
| AWSAccessKeyName | AccessKey      | Secure parameter which stores the AWS access key  |
| AWSDirectory     | data/sap/in    | AWS directory path in your bucket    |
| AWSFile          | test.json      | AWS file name    |
| AWSFileType      | application/json | File type      |
| SFAddress        | org-account.sfcomputing.com    | URL of your Snowflake instance    |
| SFDatabase       | test_db        | Name of your Snowflake database    |
| SFSchema         | public         | Name of your Snowflake Schema    |
| SFWarehouse      | mywh           | Name of your Snowflake warehouse    |
| SFTable          | test_table     | Name of your Snowflake Table    |
| SFCredentials    | SnowFBasic     | Name of your Snowflake secure parameter in CI    |
| SFStageName      | test_stage     | Name of your external stage in Snowflake    |
| IBPQuerySelect   | PRDID, CUSTID  | Attribute names to be selected from IBP's planning area    |
| IBPQueryFilterString      | UOMTOID eq 'EA'    | Filter string for the RFC query    |
| IBPFields        | PRDID, CUSTID  | Attribute fields to be mapped from IBP    |
| IBPQueryOrderBy  | PRDID, CUSTID  | Orderby criteria    |
| IBPDestination   | TECD24_IBP     | Name of your IBP destination in BTP  |
| IBPQueryTypeOfData   | KeyFigures | KeyFigures to be selected in BTP  |
| IBPBatchKey      | UserXXKey   | Name of batch key - can be any string  |
| IBPPackageSizeInRows   | 100000   | Max rows to be selected in one batch  |
| IBPQueryTimeAggregationLevel  | 3 | Time profile aggregation level |
| IBPQueryOffset1  | 0              | Offset, typically 0  |


## Exercise 3.3 Mapping.
Once data is read from SAP IBP, it has to be mapped to the database table structure of Snowflake. In the FETCH and WRITE local sub process, there is a step "MAP IBP to SF". It contains a groovy script to map the source and the target. One can change this to adapt the payload from SAP IBP to the database structure in Snowflake. Amazon S3 bucket is completly agnostic to this payload.
```groovy
 try{
        JSONArray results_array = new JSONArray();  
        def count = 1;
        JSONArray valueJSONArray = input.get("IBPReadKeyFigures").get("item");
        if(valueJSONArray.length() > 0) {
            
            def ibp_data = valueJSONArray.collect{[
                "PRODUCT" : it.PRDID,
                "CUSTOMER" : it.CUSTID,
                "LOCATION" : it.LOCID,
                "UNITS" : it.UOMID,
                "COMMENTS" : "From IBP - Count",
                "CONSDEMAND" : it.ACTUALSQTY as BigDecimal,
                "KEYFIGUREDATE" : it.TSTFR
                ]
            }
            
            def builder = new JsonBuilder();
            def result_array_data = []
            
            Integer counterValue = message.getProperty('OverallLogCounter') as Integer ?:0;
            counterValue = counterValue + 1;
            
            def root = builder ibp_data
        	
        	def prettyBody = builder.toPrettyString();
    	    long len = prettyBody.getBytes().length; 
        	String payloadSize = len.toString(); 
        	
        	// CLEAR and SET HTTP HEaders 
        	message.setHeader("Content-Length", payloadSize);         	
        	message.setBody(prettyBody);
        	
        }
```

In the above snippet you can see that PRDID; CUTID, LOCID, UOMID, ACTUALSQTY and TSTFR from IBP are mapped to PRODUCT, CUSTOMER, LOCATION, UNITS, CONSDEMAND, KEYFIGUREDATE column names of the Snowflake database table.

## Exercise 3.4 Staging.
In this excercise, staging the data is done via a local process. Here the Request-Reply-receiver combination is used. The Amazon AWS adapter for S3 message protocol is used. Bucket name is reused from the configuration parameter. The Secure parameter alias name for AWS Access key and the Secret key are reused here for the adapter configuration. In the Procesing tab, the Directory and File name are used as File Access parameters. 
<br>![](/exercises/ex3/images/03_04_0010.png)


## Exercise 3.5 Copying.
To execute the copy statement, we ned the request-reply-receiver combination. We consider the Snowflake adapter as the adapter type. The configuration of this adapter is as follows:

### Connection

| Parameter        | Example value  | Description |
| :---             | :---           | :---          |
| Authentication   | Database Account     | Type of authentication    |
| Credentials Name Alias | SFCredits      | Secure parameter which stores the Snowflake user credentials  |
| Address     | jdbc:snowflake://${header.SFAddress}   | Snowflake address in this format   |
| Database         | SFDatabase     | Name of your Snowflake database    | 
| Schema           | SFSchema       | Name of your Snowflake Schema    |
| Warehouse        | SFWarehouse    | Name of your Snowflake warehouse    | 

### Processing

| Parameter        | Example value  | Description |
| :---             | :---           | :---          | 
| Operation        | Bulk Upsert    | Operation to be executed on the Snowflake instance    |
| Table            | SFTable        | Name of your Snowflake Table    |
| Staging Location | Amazon S3      | Name of your Staging location   |
| Access Type      | External Stage | Type of staging to be used      |
| Stage            | Stagename/AWSDirectory | Name of your external stage and the AWS directory which stores the JSON file  |
| File             | AWSFile        | If it is left empty, all the files in the directory would be copied    |
| Optional Parameters | match_by_column_name='CASE_INSENSITIVE' file_format =  (TYPE = 'JSON'  STRIP_OUTER_ARRAY = TRUE) | Optional parameters to process JSON file    |

## Summary

In this excercise, When you run this iFlow, you can notice that data is read from SAP IBP and then written to a JSON file in the Staging location. You can use any suitable tool to read this file on the S3 bucket. At the end, a copy statement is executed by hte adapter to move the file contents from the staging location into the database.

In the next step we will work with the iFlow to read data from Snowflake database table and then store it in the planning area in SAP IBP. Click here to continue - [Exercise 4 - Reading from Snowflake and writing to SAP IBP ](../ex4/README.md)
