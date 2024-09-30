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



After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex2/images/02_01_0010.png)

2.	Insert this line of code.
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



## Exercise 2.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc = 0.
    response->set_status( i_code = 200
                     i_reason = 'Everything is fine').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex2/images/02_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)
