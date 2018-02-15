---
layout: single
title: "NoSQL Data modelling challenges"
excerpt: "NoSQL data modelling, demystifying AWS Dynamodb!! "
tags: [aws-dynamodb, data-modelling, bigdata, nosql]
image:
  feature: nosql-database.jpg
  credit: 
  creditlink: 
comments: true
---
* [Introduction](#introduction)
* [There are no tables and no relationship either](#no_tables)
* [Know thy data!!](#know_thy_data)
* [Data Access Patterns](#data_access_patterns)
* [You need a magic key!!](#magic_key)
* [You pay AWS for your mistakes!!](#pay_aws)
* [Finding the magic key for dynamodb](#magic_key_for_dynamo)
* [What is the size of Data?...It matters!](#size_of_data)
* [Why Do I need to know how AWS stores my data?](#how_aws_stores_data)
* [Do not put all eggs in one basket!!](#eggs_in_basket)
* [Hot Key!!](#hot_key)
* [Partition Key Magic (It is value not the name)](#partition_key_magic)
* [How do I know the Partition Key in advance?](#partition_key)
* [Eureka!!!](#eureka)

<a name="introduction"></a>
## Introduction
The developers like me who were taught RDBMS in their college as gospel of database systems, have been hardwired to think in terms of relations. I remember that in my RDBMS course the professor will insist on calling a table as relation. As soon as we are given a problem of data modelling instinctively we think in terms of relationship amongst different "tables". We are tuned to do the data modelling in terms of these relationships.
Unfortunately this way of thinking leads to a dead end in NoSQL data modelling. Let me take the example of a simple problem of modelling a hierarchical data. Let's say I want to model for a Key-Value NoSQL Database (e.g. dynamodb, cassandra).  Here is the hierarchy I would like to model.

<a name="no_tables"></a>

## There are no tables and no relationship either

Looking at this diagram my brain immediately starts thinking in terms of tables and their relationship. So I got a company table, which has one-to-many relationship with Business table, which has one-to-many relationship with Brand table. And then I would start creating three tables in my key-value datastore and model this relationship. And trust me I  hit a dead end in my NoSQL database. Everything I do after this just creates more problems. For example next I would like to get all brands from a given business? Or All businesses in a given company. Scan the business table? Scan Brands table?
![Alt](/assets/images/hierarchical-data.png "Hierarchical Data")

Over the years, here is what I have learnt in designing a NoSQL databases. 

Erase the two terms from your brain, "Relationship" and "Table".
Imprint two new terms in your brain "Collection" & "Items".
There is only one table err! one Collection. Remember there is nothing like "Relationship", and any relationship requires more than one entity. But if you think in terms of one and only one collection you can not think about relationship.

<a name="know_thy_data"></a>
## Know thy data!!

So next comes even more interesting part. When designing an RDBMS you don't care about what data your tables will contain. The only thing you care about is the column name and their types. You can put any data in those tables as long as it matches with the type of the column. And instinctively whenever people like me embark on modelling a database first thing we do is forget about what real data we shall have in our database. For instance in the example above I would not care if the user inserts a row in the Business table which contains completely absurd random data as long as it conforms to the schema of that table. You will reach hit the bottom of the sea, if you ignore what data (items) are going into your "Collection". This appears very counter intuitive to people like me who have developed this habit of not worrying about what user is going to put in his/her database. 

<a name="data_access_patterns"></a>
## Data Access Patterns 

So why is it important? To answer this let us try modelling above hierarchy. The first question to ask when you are going to model this hierarchy is what queries will be running against this collection. For instance let's say that user wants to:

1. Get All Businesses for a given company.
2. Get All Brands for a given Business.
3. Get all ancestors of a given brand. e.g. (Brand2 have ancestors Business 1 and Company. )

We can also think of the queries that may not be needed by our application. For example a query to find all the brands in a given company. Although this query may be required in future. But we can ignore this in our modelling and instead handle it at application level. In our application we query our database to get all businesses in the company and then all brands in all business of that company and then merge all the results. A simple typescript or Java map will be able to do that.

<a name="magic_key"></a>
## You need a magic key!!

So the first requirement in NoSQL modelling is to study data access patterns for your collection. Once you have a good handle on the data access patterns, the modelling your data as NoSQL database is pretty darn simple. What you need to is to find the right key to implement those queries. This will depend on the choice of NoSQL database. 

<a name="pay_aws"></a>
## You pay AWS for your mistakes!!
Let's do it for the dynamodb a key-value/document datastore. First thing first, you pay Amazon for a bad database design. And trust me Amazon will not object if you pay them for your own faults. Let's suppose you like me designed your NoSQL following RDBMS best practices, with three tables and primary secondary key relationship with them. And lets say you have millions of records in those tables. And you implement the above queries following the SQL kind of queries by scanning one table then second table then third table. You also create GSI's to support your queries. The end result is not just your application performance is slow but you are actually paying Amazon huge some for your mistakes.

<a name="magic_key_dynamo"></a>
## Finding the magic key for dynamodb
Dynamodb organises your data in terms of partitions. The challenge is to find the right partition key to access your data such that you don't pay Amazon for unnecessary access to your data and also achieve high throughput and best performance. In order to achieve this you need to look at the contents of your collection. You need to identify those values (not attributes/columns) but values which can provide the best access to items in your collection. With the help of these values you can construct a partition key and optionally a range key to implement these queries. Let's do this for our above example.

<a name="size_of_data"></a>
## What is the size of Data?...It matters!
Lets look at our data and make the following assumption.

1. It is a multi tenant system, there is a very large number (>100,000) of companies.  
2. Each company has several businesses. (Average >10 business per company).
3. Each Business has more numerous brands. (Average 100 brands per business).

This makes a large enough database with over of 100 million brands. Let's look at this query (The one which  initially was not needed):

Get All Brands for a given company.
Let us suppose that we are querying for company X, who has 100 business and 500 brands per business. This query is essentially going to fetch 50,000 items from the collection. Let's also suppose that each of that item in collection is 4K in size. That would mean that we are going to fetch around 195MB of data. This is very important to note for the following considerations.

<a name="how_aws_stores_data"></a>
## Why Do I need to know how AWS stores my data?
AWS reads the data in terms of 4KB chunks. When reading 50,000 items of 4K in size you are going to read 50,000 chunks of data. Each chunk is read via a read capacity unit with usually < than a second response time. This would mean that if you want to read all of these 50,000 items in one minute we shall need 50K/60 i.e. 834 read capacity unit. What if I wan to read all 50,000 items in one second? Then I need 50,000 read capacity units. And here comes partition into equation. There are only 3,000 read capacity units per partition so you will need 17 partitions where these 50K items should be distributed. 

<a name="eggs_in_basket"></a>
## Do not put all eggs in one basket!!
But the problem is not solved yet, you may have 17 partitions but unfortunately let us suppose all of your 50,000 items are stored in the same partition. So although you have allocated 50,000 RCUs but since your data from this query is residing only in one partition, you will still have to make 17 calls to the same partition, which will mean that it will take minimum 17 seconds to run this query.

<a name="hot_key"></a>
## Hot Key!!
Consider this if you have items which are more often accessed than the other items in collection. Let's say Company X is very popular and 60% of the time users are browsing the brands in this company. Now first we had single partition containing all the 50,000 brands for this company and now we are frequently accessing this partition almost 60% of the time. This would result in what is called "Hot Partition" and essentially result in paying much more money to AWS then you should be paying. 

<a name="data_distribution"></a>
## Data Distribution
So what we need to do. The first thing we want to do is distribute the data for our company across multiple partitions, so that our read capacity units fetch the data in parallel. Ideally those 50,000 items should be distribute to all 17 partitions with 3000 items each. If we have this distribution then we should be able to read all these items in one second as the data will be fetched in parallel from each of 17 partition allocated with 3000 RCUs.

<a name="partition_key_magic"></a>
## Partition Key Magic (It is value not the name)
This distribution of data across multiple partitions is controlled by the partition key value. Each partition key value is computed as a hash and partition is basically associated with this hash value. Let's say if you pick company name as your partition key then for company X all items will be in the same partition. On the other hand if you have partition key as unique (as any RDBMS database designer will love to do by assigning a sequence or uuid as a key), in that case though your data will be distributed uniformly across all partitions but it makes it difficult to query the database as you need the uuid/sequence key in order to fetch items from collection. Remembering a uuid or number sequence may not be very natural to human, and because of the random nature of these keys they do not allow for any association between two or more items in the collection. Let me explain it little more. Take our query again "Get all the brands for company X' , suppose that our partition key is uuid for each item in the collection, we can not fetch all these items until we know all the keys of 50,000 brands. We may try to create a GSI on the company name, but does that solve the problem? Perhaps no.

<a name="partition_key"></a>
## How do I know the Partition Key in advance?

So we come to a situation where we need some uniform distribution of 50,000 brands of company X across all 17 partitions, and we also need to know the partition key for each of them. If we have unique partition key like a uuid, then we can not know all the 50,000 key in advance. And if you do not have partition key with distinct values you hit the hot key problem. It appears to be irreconcilable. Does it?

Let's look at the hot key problem little more, we said that if we use the company name as the partition key, it makes it easier for us to run the query "Get all brands for company X", as we already know the name of the company i.e. X. But this creates the hot key issue as one of the company may be too popular. Can we make our partition key distinct enough? Remember we don't need the partition key to be unique for uniform distribution across 17 partitions. In fact the 50,000 items can be uniformly distributed across 17 partitions with 3000 items in each partition. So in reality we need 17 distinct partition keys for this case. 

<a name="eureka"></a>
## Eureka!!!
So what we do? It is easy we just need to find some magic suffix that can group all those items in a group of 3000 each. This could be some other property of the data, e.g. year, or a random number generated via modulo arithmetic to result in numbers from 1-17. And our partition key could be <Company Name>-<Magic Suffix>.

   