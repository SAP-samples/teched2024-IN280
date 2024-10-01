# Exercise 4 - Reading from Snowflake and writing it to SAP IBP

Once the pre-requisites are completed, we can now start to read data from SAP IBP. At a high level, the end to end flow would look like this,
<br>![](/exercises/ex4/images/04_01_0010.png)

## Exercise 4.1 Overview
The package you created and imported the sample contents have 2 iflows. One is for writing data from Snowflake into SAP IBP. For this purposes, we have the "<i>Write to IBP with data from Snowflake</i>" iFlow. This is a custom iFlow. It receives the parameters from an external trigger or via the configurable screen. It uses the standard reusable iflows from SAP to import data in batches. It communicates with SAP IBP using Websocket RFC calls using the destination we have defined in BTP Cockpit. Writing data to SAP IBP is done in an asynchronous way. First a batch is create, then data is written using the batch ID in a continuous loop to the staging location in SAP IBP planning area tables. Once all data packets are written, a post processing job is trigered which would validate the written data. On this change, data is then copied from staging tables into the actual tables of the planning area. 

## Exercise 4.2 Configure the parameters for the Custom iFlow.
For the flow to work we need the following configuration, which can be sent as a JSON payload or configured via the User Interface in the iFlow.

| Parameter        | Example value  | Description |
| :---             | :---           | :---          | 
| SFAddress        | org-account.sfcomputing.com    | URL of your Snowflake instance |
| SFDatabase       | test_db        | Name of your Snowflake database   |
| SFSchema         | public         | Name of your Snowflake Schema     |
| SFWarehouse      | mywh           | Name of your Snowflake warehouse  |
| SFTable          | test_table     | Name of your Snowflake Table      |
| SFCredentials    | SnowFBasic     | Name of your Snowflake secure parameter in CI |
| IBPFields        | PRDID, CUSTID  | Attribute fields to be mapped from IBP |
| IBPPlanningArea  | TECH24         | Name of your IBP Planning area         |
| IBPDestination   | TECH24_IBP     | Name of your IBP destination in BTP    |
| IBPQueryTypeOfData   | KeyFigures | KeyFigures to be selected in BTP       | 
| IBPPackageSizeInRows   | 100000   | Max rows to be selected in one batch   |
| IBPQueryTimeAggregationLevel  | 3 | Time profile aggregation level         | 


## Exercise 4.3 Selecting data.
To execute the select statement on Snowflake, we ned the request-reply-receiver combination. We consider the Snowflake adapter as the adapter type. The configuration of this adapter is as follows:

### Connection
The following details would establish the connection betweeen SAP Cloud Integration and Snowflake database directly.

| Parameter        | Example value  | Description |
| :---             | :---           | :---          |
| Authentication   | Database Account     | Type of authentication    |
| Credentials Name Alias | SFCredits      | Secure parameter which stores the Snowflake user credentials  |
| Address     | jdbc:snowflake://${header.SFAddress}   | Snowflake address in this format   |
| Database         | SFDatabase     | Name of your Snowflake database    | 
| Schema           | SFSchema       | Name of your Snowflake Schema    |
| Warehouse        | SFWarehouse    | Name of your Snowflake warehouse    | 

### Processing
In the processing tab, it is possible to define what you like to do on that established connection. For showing the flexibility of options, we have considered to execute a select statement. For this select the Operation as "Execute" from the drop down menu.

| Parameter        | Example value  | Description |
| :---             | :---           | :---          | 
| Operation        | Execute        | Operation to be executed on the Snowflake connection    |
| SQL Statement    | Select * ...   | Valid SQL statement - do test this on your Snowflake instance before using it here | 

## Exercise 4.4 Mapping.
Once data is read from Snowflake, it may be be mapped to the key figure definition in SAP IBP. In the FETCH and WRITE local sub process, there is a local process "MAP and POST to IBP". It contains a XSLT script to map the Snowflake payload to the SAP IBP multimap structure. One can change this to adapt the payload from Snowflake to the RFC payload structure for SAP IBP. Here is the sample content which can be used as a reference.
```xslt
 <multimap:Messages>
           <multimap:Message1> 
               <IBPWriteKeyFigures>                   
                   <xsl:attribute name="FieldList"><xsl:value-of select="$IBPFields"></xsl:value-of></xsl:attribute> 
           		   <xsl:attribute name="BatchKey"><xsl:value-of select="$IBPBatchKey"/></xsl:attribute>
           		   <xsl:attribute name="FileName">SnowflakeDataLoad</xsl:attribute>                    
                   <xsl:variable name="InputPayload" select="parse-xml($SFPayload)"/>
                   <!-- This variable InputPayLoad could contain the result set from Snowflake select statement-->
                    <xsl:for-each select="$InputPayload/Result/Rows/*">
                       <item>
                           <PRDID><xsl:value-of select="./PRODUCT"></xsl:value-of></PRDID>
                           <LOCID><xsl:value-of select="./LOCATION"></xsl:value-of></LOCID>
                           <CUSTID><xsl:value-of select="./CUSTOMER"></xsl:value-of></CUSTID>
                           <STATISTICALFORECASTQTY><xsl:value-of select="./CONSDEMAND"></xsl:value-of></STATISTICALFORECASTQTY> 
                           <!-- You can modify this to you own format or adapt it -->  
                           <KEYFIGUREDATE><xsl:value-of select="./KEYFIGUREDATE"></xsl:value-of></KEYFIGUREDATE>
                       </item>
                   </xsl:for-each> 
               </IBPWriteKeyFigures>
            </multimap:Message1>
    	</multimap:Messages>
```

In the above snippet you can see that values from PRODUCT, CUSTOMER, LOCATION, CONSDEMAND and KEYFIGUREDATE column names of the Snowflake database table are mapped to the key figure attributes of STATISTICALFORECASTQTY in SAP IBP.
 
## Summary

In this excercise, as you run this iFlow, you can notice that data is read from Snowflake and then written to a Staging table in SAP IBP. After post processing, the data is then copied into the live tables of the planning area. We tried to simplify the integration process, educate the participant doing this integration with basics so that this task can be productively improved.   
