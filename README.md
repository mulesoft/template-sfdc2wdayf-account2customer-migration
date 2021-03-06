
# Anypoint Template: Salesforce to Workday Account to Customer Migration	

<!-- Header (start) -->
Moves a large set of accounts from Salesforce to Workday Financials. You can trigger this manually, or programmatically with an HTTP call. Accounts are upserted so that the migration can be run multiple times without creating duplicates. This template uses batch to efficiently process many records at a time.

![701965e3-714c-44a3-b9d5-e84658b679d1-image.png](https://exchange2-file-upload-service-kprod.s3.us-east-1.amazonaws.com:443/701965e3-714c-44a3-b9d5-e84658b679d1-image.png)
<!-- Header (end) -->

# License Agreement
This template is subject to the conditions of the <a href="https://s3.amazonaws.com/templates-examples/AnypointTemplateLicense.pdf">MuleSoft License Agreement</a>. Review the terms of the license before downloading and using this template. You can use this template for free with the Mule Enterprise Edition, CloudHub, or as a trial in Anypoint Studio. 
# Use Case
<!-- Use Case (start) -->
As an admin I want to migrate Salesforce accounts to Workday customers.

This template provides help for the process of migrating accounts from Salesforce, where you can specify the filtering criteria and desired behavior when the corresponding customer already exists in Workday.

As implemented, this template leverages the Mule batch module. The batch job in this template:

1. Queries all existing accounts at Salesforce that match the filtering criteria to get the Salesforce IDs.
2. Queries Workday for existing customers based on the Salesforce IDs retrieved in step 1.
3. Inserts or updates customers in the Workday system based on the results of these queries.
4. Sends the migration results to the email addresses that you configure and outputs to the console as well.
<!-- Use Case (end) -->

# Considerations
<!-- Default Considerations (start) -->

<!-- Default Considerations (end) -->

<!-- Considerations (start) -->
There are certain prerequisites that must be considered to run this template. All of them deal with the preparations in both source and destination systems, that must be made for the template to run smoothly. Failing to do so can lead to unexpected behavior of the template.
<!-- Considerations (end) -->

## Salesforce Considerations

- Where can I check that the field configuration for my Salesforce instance is the right one? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US">Salesforce: Checking Field Accessibility for a Particular Field</a>.
- Can I modify the Field Access Settings? How? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US">Salesforce: Modifying Field Access Settings</a>.

### As a Data Source

If a user who configures the template for the source system does not have at least *read only* permissions for the fields that are fetched, then an *InvalidFieldFault* API fault displays.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault 
[ApiFault  exceptionCode='INVALID_FIELD'
exceptionMessage='Account.Phone, Account.Rating, Account.RecordTypeId, 
Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are 
attempting to use a custom field, be sure to append the '__c' 
after the custom field name. Reference your WSDL or the 
describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```

## Workday Considerations

### As a Data Destination

There are no considerations with using Workday as a data destination.

## Workday Financials Considerations

### As a Data Destination

**Important:** When creating new Workday customers, an existing customer category must be chosen. To do this, this template uses a lookup table with predefined mappings between Salesforce data (Industry) and Workday Customer Category ID. Workday customer categories can be user defined, therefore for the mapping, you must edit this manually before running this template.

To view all Workday Customer Categories, type "customer categories" into the search field in Workday and select the Customer Categories report. 

In the report you should see all the customer categories available. Select Integration IDs > View Integration IDs from available actions of each customer category and update the function *industryToCustomerCategoryMapping* in the template's DataWeave component 'Prepare put customer request' manually. The default customer category ID can be edited inside the same DataWeave component in the row where customer category ID is set. This category is used if the source account's Industry value does not match the defined mappings in the function.

# Run it!
Simple steps to run this template.
<!-- Run it (start) -->
Whichever way you choose to run this template, this is an example of the output you see after browse to the HTTP endpoint:

```
Batch Process initiated
ID: 6eea3cc6-7c96-11e3-9a65-55f9f3ae584e
Records to be processed: 3
Started execution on: Mon Jan 25 10:08:16 CET 2019
```
<!-- Run it (end) -->

## Running On Premises
In this section we help you run this template on your computer.
<!-- Running on premise (start) -->

<!-- Running on premise (end) -->

### Where to Download Anypoint Studio and the Mule Runtime
If you are new to Mule, download this software:

+ [Download Anypoint Studio](https://www.mulesoft.com/platform/studio)
+ [Download Mule runtime](https://www.mulesoft.com/lp/dl/mule-esb-enterprise)

**Note:** Anypoint Studio requires JDK 8.
<!-- Where to download (start) -->

<!-- Where to download (end) -->

### Importing a Template into Studio
In Studio, click the Exchange X icon in the upper left of the taskbar, log in with your Anypoint Platform credentials, search for the template, and click Open.
<!-- Importing into Studio (start) -->

<!-- Importing into Studio (end) -->

### Running on Studio
After you import your template into Anypoint Studio, follow these steps to run it:

1. Locate the properties file `mule.dev.properties`, in src/main/resources.
2. Complete all the properties required per the examples in the "Properties to Configure" section.
3. Right click the template project folder.
4. Hover your mouse over `Run as`.
5. Click `Mule Application (configure)`.
6. Inside the dialog, select Environment and set the variable `mule.env` to the value `dev`.
7. Click `Run`.
<!-- Running on Studio (start) -->

<!-- Running on Studio (end) -->

### Running on Mule Standalone
Update the properties in one of the property files, for example in mule.prod.properties, and run your app with a corresponding environment variable. In this example, use `mule.env=prod`. 
After this, you can trigger the use case by browsing to the local HTTP connector with the port you configured in your file. For example for port `9090` browse to `http://localhost:9090/migrateaccounts` and this outputs a summary report and sends it in the e-mail.

## Running on CloudHub
When creating your application in CloudHub, go to Runtime Manager > Manage Application > Properties to set the environment variables listed in "Properties to Configure" as well as setting the mule.env value.
<!-- Running on Cloudhub (start) -->
Once your app is all set up and started, if you choose as the domain name `sfdcaccountmigration` to trigger the use 
case, browse to `http://sfdcaccountmigration.cloudhub.io/migrateaccounts` and the report is sent to the emails you configure.
<!-- Running on Cloudhub (end) -->

### Deploying a Template in CloudHub
In Studio, right click your project name in Package Explorer and select Anypoint Platform > Deploy on CloudHub.
<!-- Deploying on Cloudhub (start) -->

<!-- Deploying on Cloudhub (end) -->

## Properties to Configure
To use this template, configure properties such as credentials, configurations, etc.) in the properties file or in CloudHub from Runtime Manager > Manage Application > Properties. The sections that follow list example values.
### Application Configuration
<!-- Application Configuration (start) -->
**Application Configuration**

- http.port `9090`
- page.size `200`

**Salesforce Connector Configuration**

- sfdc.username `bob.dylan@orga`
- sfdc.password `DylanPassword123`
- sfdc.securityToken `avsfwCUl7apQs56Xq2AKi3X`

**Workday Connector Configuration**

- wdayf.username `joan`
- wdayf.tenant `acme_pt1`
- wdayf.password `joanPass123`
- wdayf.host `your_impl-cc.workday.com`
 
- wdayf.country `USA`
- wdayf.state `USA-CA`
- wdayf.postalCode `90001`
- wdayf.city `San Francisco`
- wdayf.street `Main Street 123`
- wdayf.phone `123-4567`

**SMTP Services Configuration**

- smtp.host `smtp.gmail.com`
- smtp.port `587`
- smtp.user `gmail_user`
- smtp.password `gmail_password`

**Email Details**

- mail.from `batch.migrateaccounts.migration%40mulesoft.com`
- mail.to `your.email@example.com`
- mail.subject `Batch Job Finished Report`
<!-- Application Configuration (end) -->

# API Calls
<!-- API Calls (start) -->
Salesforce imposes limits on the number of API calls that can be made. However, we make API call to Salesforce only once during migration, so this is not something to worry about.
<!-- API Calls (end) -->

# Customize It!
This brief guide provides a high level understanding of how this template is built and how you can change it according to your needs. As Mule applications are based on XML files, this page describes the XML files used with this template. More files are available such as test classes and Mule application files, but to keep it simple, we focus on these XML files:

* config.xml
* businessLogic.xml
* endpoints.xml
* errorHandling.xml
<!-- Customize it (start) -->
<!-- Customize it (end) -->

## config.xml
<!-- Default Config XML (start) -->
This file provides the configuration for connectors and configuration properties. Only change this file to make core changes to the connector processing logic. Otherwise, all parameters that can be modified should instead be in a properties file, which is the recommended place to make changes.
<!-- Default Config XML (end) -->
<!-- Config XML (start) -->
<!-- Config XML (end) -->

## businessLogic.xml
<!-- Default Business Logic XML (start) -->
Functional aspect of the template is implemented on this XML, directed by one flow responsible of executing the logic.
For the purpose of this particular template the *mainFlow* just executes a batch job which handles all the migration logic.
This flow has Error handling that points to errorHandlingFlow defined in *errorHandling.xml* file.
<!-- Default Business Logic XML (end) -->
<!-- Business Logic XML (start) -->
<!-- Business Logic XML (end) -->

## endpoints.xml
<!-- Default Endpoints XML (start) -->
This is the file where you find the inbound side of your integration app. This template has only an HTTP Listener as the way to trigger the use case.

**HTTP Listener Connector** - Start Report Generation

- `${http.port}` is set as a property to be defined either in a property file or in CloudHub environment variables.
- The path configured by default is `migrateaccounts` and you are free to change for the one you prefer.
- The host name for all endpoints in your CloudHub configuration should be defined as `localhost`. CloudHub routes requests from your application domain URL to the endpoint.
<!-- Default Endpoints XML (end) -->
<!-- Endpoints XML (start) -->
<!-- Endpoints XML (end) -->

## errorHandling.xml
<!-- Default Error Handling XML (start) -->
This file handles how your integration reacts depending on the different exceptions. This file provides error handling that is referenced by the main flow in the business logic.
<!-- Default Error Handling XML (end) -->
<!-- Error Handling XML (start) -->
<!-- Error Handling XML (end) -->
<!-- Extras (start) -->
<!-- Extras (end) -->
