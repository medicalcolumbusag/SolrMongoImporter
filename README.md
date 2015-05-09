# Solr Mongo Importer
Welcome to the Solr Mongo Importer project. This project provides MongoDb support for the Solr Data Import Handler.

## Features
* Retrive data from a MongoDb collection
* Authenticate using MongoDb authentication
* Map Mongo fields to Solr fields (for accessing nested fields "." (dot) string eg.: *Params.Size*)
* Date conversion of field value to required format

## Classes

* MongoDataSource - Provides a MongoDb datasource
    * database (**required**) - The name of the data base you want to connect to
    * host     (*optional* - default: localhost)
    * port     (*optional* - default: 27017)
    * username (*optional*)
    * password (*optional*)
* MongoEntityProcessor - Use with the MongoDataSource to query a MongoDb collection
    * collection (**required**)
    * query (**required**)
* MongoMapperTransformer - Map MongoDb fields to your Solr schema
    * mongoField (**required**)
    * dateFormat (*optional*)

## Installation
1. Firstly you will need a copy of the Solr Mongo Importer jar.

    Getting Solr Mongo Importer
    1. [Download the JAR from github](https://github.com/phadadi/SolrMongoImporter/releases/download/v1.1.0/solr-mongo-importer-1.1.0.jar)
    2. Build your own using the ant build script you will need the JDK installed as well as Ant
2. You will also need the [MongoDB Java driver 3.x JAR](http://mvnrepository.com/artifact/org.mongodb/mongo-java-driver)

3. Place both of these jar's in your Solr libaries folder (I put mine in 'lib' folder with the other jar's)
4. Add lib directives to your solrconfig.xml

```xml
<lib dir="./lib/" regex="solr-mongo-importer.*\.jar"/>
<lib dir="./lib/" regex="mongo-java-driver.*\.jar"/>
```

##Usage
Here is a sample data-config.xml showing the use of all components
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<dataConfig>
     <dataSource name="MongoSource" type="MongoDataSource" database="Inventory"/>
     <document name="Products">
         <entity name="Product"
                 processor="MongoEntityProcessor"
                 query="{'Active':1}"
                 collection="ProductData"
                 datasource="MongoSource"
                 transformer="MongoMapperTransformer">
             <field column="title" name="title" mongoField="Title"/>
             <field column="description" name="description" mongoField="LongDescription"/>
             <field column="brand" name="brand" mongoField="Brand"/>
             <field column="size" name="size" mongoField="Params.Size"/>
             <field column="created" name="created" mongoField="Created" dateFormat="yyyy-MM-dd HH:mm:ss"/>
         </entity>
     </document>
 </dataConfig>
```
