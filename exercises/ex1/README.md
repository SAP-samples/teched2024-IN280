# Exercise 1 - Environment Setup - 1

In this exercise, we will prepare the SAP IBP instance, the BTP destination as well as the SAP Cloud Integration instance.

## Exercise 1.1 Activate Communication Arrangement for communication scenario - SAP_COM_0931 in SAP IBP

1. Create a communication Arrangement for SAP_COM_0931
2. Create a Communication Scenario
3. Create or assign a Communication user for this scenario

After completing these steps you will have created...

1. A Communication Setup in SAP IBP
<br>![](/exercises/ex1/images/01_01_0010.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello World! | ). 
```



## Exercise 1.2 Activate RFC Destination in SAP BTP Cockpit

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex1/images/01_02_0010.png)


## Exercise 1.3 Copy and Deploy SAP Cloud Integration Standard package "SAP IBP - Reusable Integration Flows" (OPTIONAL)

Do this step only once if your own Cloud Integration tenant does not have this package deployed. If you are using any of SAP TechEd 2024 Cloud Integration tenants this step is already done - DO NOT DO THIS step.  You should do this only on your own SAP Cloud Integration tenant, if not done.

1. Go to the Home page of your SAP Integration Suite. Click on the "Discover" button and then "Integrations" on the left navigation pane.
2. In the search Bar on the right, search for ["SAP IBP Reusable"]
<br>![](/exercises/ex1/images/01_01_0010.png)
3. If you see the "SAP IBP - Reusable Integration Flows", the press the "Copy" button on the top right corner. This would import the package in your SAP Cloud Integration tenant. 

## Exercise 1.4 Copy and Deploy SAP Cloud Integration Session IN280 package

<b>IMPORTANT</b>:- Create a package for your user like this - Session IN280 User <your user id> e.g. <b>Session IN280 User 13</b>

1. Go to the Home page of your SAP Integration Suite. Click on the "Design" button and then "Integrations and APIs" on the left navigation pane.
2. Click on the "Create" Button on the top right to create a new Package with the name - "Session IN280 User <your user id>"
<br>![](/exercises/ex1/images/01_02_0010.png).
3. Give it a short description, version and your name as vendor and press eht "Save" button on the top right corner. Click on the "Design" button and then "Integrations and APIs" on the left navigation pane. You should now see your package in the list of packages.
4. Now, open the package Session In280 <User ID> and then select the Artifacts tab. You would see the two iFlows for this excercise. Use the Actions button to copy the iFlows into your own package (Select your package using the Select button) which you created in the previous step. Here is an example of how the iFlows were copied into the package [Session IN280 User 13](/exercises/ex1/images/01_03_0010.png). Repeat this step for both the iFlows.
<br>![](/exercises/ex1/images/01_03_0010.png)
After completing these steps you will have created...

1.  A Communication Setup in SAP IBP 

2.	Active RFC destination on your BTP Account
3.  Deployed the reusable iflows from SAP IBP
4.  Copied the Sample contents which was provided for Session IN280.
 
## Summary

You've now setup the environment on the SAP Side. You can now set up the database tables and the staging configuration on the Snowflake side.

Continue to - [Exercise 2 - Environment Setup - 2](../ex2/README.md)

