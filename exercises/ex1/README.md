# Exercise 1 - Environment Setup - 1

In this exercise, we will prepare the SAP IBP instance, the BTP destination as well as the SAP Cloud Integration instance.

## Exercise 1.1 Activate Communication Arrangement for communication scenario - SAP_COM_0931 in SAP IBP

1. Create a Communication user
2. Create a Communication System using the created communication user(1) for inbound communication only
3. Create a Communication Arrangement for the Communication Scenario <b>SAP_COM_0931</b> from the created communication system(2)
   
<br>![](/exercises/ex1/images/01_01_0020.png)


## Exercise 1.2 Activate RFC Destination in SAP BTP Cockpit

1. Open the SAP BTP Cockpit on your sub account.
2. Click on Connectivity -> Destinations and then use the "Create Destination" link on the right hand side table bar to create a new destination configuration.
3. Give it a name, Type is RFC, give it a description and then use the Proxy type as Internet if you are directly accessing it.
4. Choose the authorization type as "Configured_user".
5. Enter the communication user name you created in the previous step in the input field for "Alias user" and password in the respective text box as input.
6. Configure these additional properties: 

```java
jco.client.serialization_format - columnBased
jco.client.tls_trust_all        - 1
jco.client.wshost               - name of your IBP host which you get while configuring the communication arrangement.
jco.client.wsport               - 443 

```
Once you have done it, you can test the connection. The Destination configuration would then look like the following screen,
<br>![](/exercises/ex1/images/01_01_0030.png)

## Exercise 1.3 Copy and Deploy SAP Cloud Integration Standard package "SAP IBP - Reusable Integration Flows" (OPTIONAL)

If you are using any of SAP TechEd 2024 SAP Cloud Integration tenants this step is already done - <b>SKIP TO THE NEXT EXERCISE</b>.  
Follow the below steps in your own SAP Cloud Integration tenant only if it does not have this standard package deployed.

1. Go to the Home page of your SAP Integration Suite. Click on the "Discover" button and then "Integrations" on the left navigation pane.
2. In the search Bar on the right, search for "<b>SAP IBP - Reusable Integration Flows</b>"
   
<br>![](/exercises/ex1/images/01_01_0010.png)

3. If you see the "<b>SAP IBP - Reusable Integration Flows</b>", select it and then press the "<b>Copy</b>" button on the top right corner. This would import the package in your SAP Cloud Integration tenant.
4. Click on Design -> Integrations & APIs, search for the copied "<b>SAP IBP - Reusable Integration Flows</b>" standard package, select and deploy all its artifacts.

## Exercise 1.4 Copy and Deploy SAP Cloud Integration Session IN280 package

<b>IMPORTANT</b>:- Create a package for your user like this - Session IN280 User XX. For example:- <b>Session IN280 User 13</b>

1. Go to the Home page of your SAP Integration Suite. Click on the "Design" button and then "Integrations and APIs" on the left navigation pane.
2. Click on the "Create" Button on the top right to create a new Package with the name - "Session IN280 User XX"
<br>![](/exercises/ex1/images/01_02_0010.png).

3. Give it a short description, version and your name as vendor and press the "Save" button on the top right corner. Click on the "Design" button and then "Integrations and APIs" on the left navigation pane. You should now see your package in the list of packages.
4. Download the package [Session IN280](/artifacts/Session-IN280.zip), unzip only this file on your local computer. You must see two ZIP files, do not extract them.
5. Go back to the SAP Cloud Integration, open your package you just created in Step 3 and then select the Artifacts tab. Click Edit and then click on the "Add" drop down button and then select "Integration Flow". Select the Upload check box and browse to select the "Read from IBP and write Snowflake.zip" file from the extracted folder in Step 4. Rename the flow as "UXX Read from IBP and write Snowflake" where XX is your user ID (for example:- U13 Read from IBP and write Snowflake). Repeat this to upload the other iFlow too. The Add Integration Flow pop up window would look like this: 
<br>[Session IN280 User 13](/exercises/ex1/images/01_04_0010.png). 
Repeat this step for both the iFlows. 

After completing these steps you will have created...

1.  A Communication Setup in SAP IBP 
2.	Active RFC destination on your BTP Account
3.  Deployed the reusable iflows from SAP IBP
4.  Created you own package and copied the Sample contents which was provided for Session IN280.
 
## Summary

You've now setup the environment on the SAP Side. You can now set up the database tables and the staging configuration on the Snowflake side.

Continue to - [Exercise 2 - Environment Setup - 2](../ex2/README.md)

