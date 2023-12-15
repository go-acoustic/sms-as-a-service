# SMS as a service

### SMS as a Service Graph API Documentation 

The new Acoustic Campaign APIs introduced since 2023 are built with GraphQL, a flexible query language that allows you to return as much or as little data as you need. GraphQL is an application layer that parses the query you send and returns (or changes) data accordingly. Acoustic will continue to introduce more GraphQL APIs over time. 

Unlike REST APIs, where multiple endpoints return different data, GraphQL always exposes one endpoint and allows you to determine the structure of the returned data. Our API uses the following endpoint: https://app.goacoustic.com/api/graph 

This document will walk through the GraphQL operations and object types to help you understand the essential components of a request. 

## Using the Graph API 
 
<img width="462" alt="image" src="https://github.com/go-acoustic/sms-as-a-service/assets/30212233/c7086046-99cb-445b-b3f3-cfca41048e70">

## Importing into Postman 

1. Open and select "index.graphql". 

2. Copy the file contents of index.graphql. 

3. In your local Postman, create a new API and expand the menu to see the Import or Create options. 

4. In the new API, select Create and set the Definition Type of "GraphQL" and Definition format of "GraphQL SDL" and click Create Definition. 

5. In the created API definition, paste the contents copied from index.graphql  in step 3 and click Save. 

6. Download "sms-as-a-service.postman_collection.json" collection.

## Acquiring an API Key 

To generate an API Key, log in to MyAcoustic and click Manage on your subscription. Open the API keys tab and click Generate API Key. 

 <img width="360" alt="image" src="https://github.com/go-acoustic/sms-as-a-service/assets/30212233/22795563-3e83-426b-aa33-4800d7e11462">


## Authorization 

Use your API Key and set it as a header like so: 
```
"x-api-key": "{api_key}" 

"x-acoustic-region": "{aws_region}”
```

Where the region should be: us-east-1, ap-south-1, ap-southeast-1, ap-southeast-2, ca-central-1, eu-central-1 

For Ticketmaster, this should be ca-central-1. 

Both the x-api-key and x-acoustic-region are required on each API call. 

## Queries 

Queries perform the READ operation and do not change or alter any data. The result of each query will be formatted in the same way as the query itself. For example: 

 

*Query to find all codes with all available properties*
```
query codeAll { 

      CodeAll (channel: SMS) { 

        id 

        name 

        type 

        supportsInboundMessaging 

        description 

        creationDate 

        modifiedDate 

        keywords { 

            id 

            name 

        } 

    } 

} 
 ```

*Query to find code by ID*
*returns code name and ID, name of keywords assigned to the code*
```
query codeById { 
    CodeById(id: "109af94c-43eb-4e39-aceb-b7e90dbd6139") { 
        id 

        name 

        type 

        supportsInboundMessaging 

        description 

        creationDate 

        modifiedDate 

        keywords { 

            id 

            name 

        } 

    } 
}
```
 

*Query to find code by the name*
*returns code ID and ID, name of keywords assigned to the code*
```
query codeByName { 
    CodeByName(name: "code name") { 
        id 
        keywords { 
            id 
            name 
        } 
    } 
} 
```

## Mutations 

 

Mutations are special queries that perform the CUD (Create, Update, Delete) operations to modify your data. Mutations return an instance of the object you just modified so you can query the data you changed.  

 
## Multiple queries or mutations in one request 

 

You can also send multiple queries in one request, and they will be executed one after the other. The following query returns the ID and name for Codes 109af94c-43eb-4e39-aceb-b7e90dbd6139 and 222af11c-34eb-e349-ebac-e90db613d9b7. 

*Query to find code by two IDs*
*returns code name and ID, name of keywords assigned to the code*
```
query codeById { 
    code1: CodeById(id: "109af94c-43eb-4e39-aceb-b7e90dbd6139") { 
        name 
        keywords { 
            id 
            name 
        } 
    } 
    code2: CodeById(id: "967dec9e-84b2-4783-a8e7-76cb909e8222") { 
        name 
        keywords { 
            id 
            name 
        } 
    } 
} 
``` 
## Object Types 

 

Object types are a collection of fields used to describe the set of possible data you can query using the API. They can also have arguments on the fields to pass parameters when querying data. 

## Fields 

Fields specify properties or attributes of objects to help define what information to retrieve from a query. Every object in the schema contains fields that can be queried by name to retrieve specific properties of the object. 

### Notes on fields - Campaigns 

- sendingHours: The startHour and endHour String fields represent an {hour}:{minute} format, where the value of hour is between 00 and 23 and the value of minute is between 00 and 59. Example values are 00:15, 23:59 

- date-time string: at UTC, such as 2019-12-03T09:54:33Z, compliant with the date-time format. 

- status: When updating the status of a subscription group, valid values are: PAUSE, RESUME, COMPLETE 

- statuses: When querying objects by status, valid values are: RUNNING, COMPLETED, and PAUSED.  

- types: When querying objects by type, valid values are: SMS_ONE_WAY_OPT_IN, SMS_TWO_WAY_OPT_IN, SMS_DOUBLE_OPT_IN, SMS_GLOBAL_OPT_OUT 

### Notes on fields – SMS messages and mailings 

- name/tags: Those properties accept only letters, numbers, and the following characters: - _ 

- mediaLink: A publicly accessible URL to media file, which would be attached to a MMS message. Supported file formats are: jpg, jpeg, png, gif, tiff, bmp, ics, ical, ifb, icalendar, vcf, csv, pdf, rtf, mp4, mov, webm, mpeg 

- shortenLinks: Determine if URLs present in SMS text body would be shortened. Shortened version of URL would look like ex. https://acs1.tc/XXXXXXXXXX 

- characterMapping: Determine if non-GSM standard characters would be replaced to ASCII, according to predefined mapping 

- dataSetId: Identifier of a Silverpop Database. Only databases enabled for SMS usage are accepted. 

- subscriptionGroupIds: Identifiers of SMS subscription groups. Only RUNNING subscription groups with type SMS_ONE_WAY_OPT_IN or SMS_TWO_WAY_OPT_IN are accepted. 

- status: When querying SMS messages by status, valid values are: DRAFT, SCHEDULED, SENDING, SENT, CANCELLING, FAILED, OTHER 

- scheduledTimezone: Specifies timezone in which scheduledDate was provided. Supported format according to IANA Time Zone Database. 

- TestSendInput.identityPhoneNumber: Allows to personalize message in test send as any recipient in provided dataSet 

- MessageContentFilter.name: Allows to search SMS messages by name. Accepts wildcard character: * 

 

*Query to find all codes with all available properties*

```
query codeAll { 

    CodeAll (channel: SMS) { 

        id 

        name 

        type 

        supportsInboundMessaging 

        description 

        creationDate 

        modifiedDate 

        keywords { 

            id 

            name 

        } 

    } 

} 
```

## Arguments 

You can pass arguments in a query to specify what data to return (i.e., filter the search results) and narrow down the results to only the specific ones you’re after. For example: 

*Query to find code by ID* 
*returns code name and ID, name of keywords assigned to the code*

```
query codeById { 

    CodeById(id: "109af94c-43eb-4e39-aceb-b7e90dbd6139") { 

        name 

        keywords { 

            id 

            name 

        } 

    } 
}
```

## Variables 

You can use variables to pass dynamic values to your arguments. They are written outside of the query string itself in the variables section and passed to the arguments. 

When we start working with variables, we need to do three things: 

1. Replace the static value in the query with $variableName 

2. Declare $variableName as one of the variables accepted by the query 

3. Pass variableName: value in the separate, transport-specific (usually JSON) variables dictionary 

*Query to find code by ID*
*returns code name and ID, name of keywords assigned to the code*

```
query codeById ($id: ID!) { 

    CodeById(id: $id) { 

        name 

        keywords { 

            id 

            name 

        } 

    } 
```
 

*Variables*
```
{ 
"id": "109af94c-43eb-4e39-aceb-b7e90dbd6139" 
} 
 ```

 ## More resources 

Here we have covered a few simple examples.  

There is an excellent primer on GraphQL here https://graphql.org/learn/queries/ to go further. 

This document is enough to get started and covers some simple examples. The complete GraphAPI that should be attached to this document contains all of the Queries and Mutations that are available in the API, and further examples. 
