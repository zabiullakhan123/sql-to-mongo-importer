## Introduction ##

This project moved to http://code.google.com/p/sql-to-nosql-importer/

sql-to-mongo-importer project reads from  sql databases, converts and then inserts them into mongodb.For this purpose it uses one properties file **(import.properties)** where mongodb related settings are listed and one xml file with sql database related settings and de-normalized schema and fields.For more info
http://wiki.apache.org/solr/DataImportHandler#Configuration_in_data-config.xml

But the configuration file **sql-to-mongo-importer** uses varies slightly from solr's.


## PROJECT STRUCTURE: ##
  1. **conf/** 	- all configuration files.
  1. **lib/**		- all libraries (.jar files) and  JDBC Driver for your SQL Database
  1. **src/**		- Source code of this project


## HOW TO RUN : ##

  1. change sql-db related settings in db-data-config.xml
  1. change mongodb related settings in import.properties
  1. run sql-to-mongo-importer by issuing command ./sql-to-mongo-importer

## CONFIGURATIONS : ##

> ### import.properties ###

  1. **url** - mongodb server host
  1. **autoCommitSize** - After how many no of documents it should commit to mongodb (If you get any java heap space error please reduce this value)
  1. **useAuth** - Whether authentication needs to be done for the mongodb database.
  1. **user**	- Username for authentication (It is required only if useAuth is true)
  1. **password** - Password for authentication (It is required only if useAuth is true)
  1. **db**	- mongodb database name for import. (If the given database is not there, it will be created).
  1. **sql-data-config-file** - SQL database settings and other schema related definitions, fields (default is db-data-config.xml).

> ### db-data-config.xml ###

> Most of the things are same as Solr's dataimport configuration file.For more info about Solr's configuration elements.
> > http://wiki.apache.org/solr/DataImportHandler

The root entity's name will be used as collection name. If collection is not there, then it will be created.Field name which is given in the value 	of root entity's pk attribute will be used as primary key for the collection.Let's list  the differences between our configuration file and Solr's dataimport configuration file.

  1. Solr uses two configuration files (schema.xml,db-data-config.xml).sql-to-mongo-importer uses only **one configuration file**.(db-data-config.xml).
  1. Field definitions have one more attribute "type".It is a mandatory one. Possible values are **(STRING, INTEGER, DOUBLE, LONG, DATE, BOOLEAN)**.
  1. Entity definitions have one more optional attribut "**multiValued**".Possible values are (true, false).
  1. In Solr fields are multivalued.But in sql-to-mongo-importer **entities are multivalued**.I don't see any usecase for having fields as multivalued (?).
  1. a) If one multivalued entity has only one field it will be converted to array list.
> > `"tags" : [ "mongodb", "couchdb", "hbase", "cassandra", "riak"]`
> > > b) If one multivalued entity has more than one field it will be converted to array list of objects.

> > `"tags" : [ { "name" : "mongodb" , "type" : "document"}, { "name" : "couchdb" , "type" : "document"},  { "name" : "hbase" , "type" : "bigtable"}, { "name" : "cassandra" , "type" : "bigtable"}, { "name" : "riak" , "type" : "key-value"}]`


## TODO : ##

  1. There is a memory leak which is causing java heap space error. Some help to fix this one is appreciated.
  1. Support more NoSQL databases (next couchDB?).