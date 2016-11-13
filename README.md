#SparqlMap - Client


SparqlMap - A SPARQL to SQL rewriter based on [R2RML](http://www.w3.org/TR/r2rml/) specification.

It can be used in allows both extracting RDF from an relational database and rewrite SPARQL queries into SQL.

The SparqlMap Client provides command line and web access to the SparqlMap core.

## Download

Get the latest release on the [release page](https://github.com/tomatophantastico/sparqlmap/releases).
Please note, that no MySQL driver is included. You will need to get it from the [MySQL page](https://dev.mysql.com/downloads/connector/j/)  and copy it into the ./lib folder.

## Overview over the mapping process

![SparqlMap overview](https://raw.github.com/tomatophantastico/sparqlmap/doc/doc/sparqlMap.png)



##Convert Relational Database into RDF

Most of the time, dump creation will take place on the command line.
In the binary distributions use the sparqlmap command. 
Calling sparqlmap without or with a wrong combination of options will present all options available.

Let's have a look at some samples:

### RDF Dump

```shell
./bin/sparqlmap -dburi "jdbc:mysql://192.168.59.103:3306/sparqlmaptest?padCharsWithSpace=true&sessionVariables=sql_mode='ANSI_QUOTES'" -dbuser sparqlmap -dbpass sparqlmap -r2rmlfile src/test/resources/hsql-bsbm/mapping.ttl -dump   
```
This sample creates a dump from a mysql database. Take note of the following:
* The database can be configured by either using the -dburi/-dbusername/-dbpass options or by providing a file with the database connection information, using the -dbfile option.
* When connecting to MySQL, ANSI_QUOTES and padCharsWithSpace have to be set, as in the above jdbc url.

### Mapping generation

Creating a R2RML representation of a default mapping is as easy as this:

```
./bin/sparqlmap -dbfile ./src/test/resources/hsql-bsbm/db.properties  -generateMapping
```
Here the database connection is provided by a file, e.g.:

[db.properties](https://raw.githubusercontent.com/tomatophantastico/sparqlmap/develop/src/test/resources/hsql-bsbm/db.properties)


## Rewrite SPARQL queries into SQL

For rewriting SPARQL queries into SQL SparqlMap can expose a SPARQL endpoint by an embedded tomcat.
The enpoint is started by 
```shell
./bin/sparqlmap -web
```
This will expose an SPARQL endpoint with a little snorql interface.

## R2RML conformance

SparqlMap conforms with the R2RML specification and was tested with PostgreSQL, MySQL and HSQL.


## Adding additional database drivers

Simply copy them into the lib folder.

## Building and installation

SparqlMap requires the following dependencies, which are not present in the main repos.

1. JSqlParser
```
git clone https://github.com/tomatophantastico/sparqlmap-jsqlparser.git 
cd sparqlmap-jsqlparser 
mvn install
```

2. Metamodel with a patch
```
git clone https://github.com/tomatophantastico/metamodel.git
cd metamodel
mvn install
```

3. SparqlMap core
```
git clone https://github.com/tomatophantastico/sparqlmap-core.git
cd sparqlmap-core
./gradlew publishToMavenLocal
```



## Using with MongoDB

ds.location  the host and port of the mongodb server, e.g.: localhost:11111
ds.identifier the database name
ds.username  username
ds.password  password


## Using with a JDBC database
First, make sure that a JDBC4 driver is in the classpath. SparqlMap already contains drivers for the most important FOSS RDBMs, for closed source RDBMs, they have to be added manually.


ds.location  the full jdbc url, e.g. jdbc:mysql://localhost:3306/test
ds.identifier ** not used **
ds.username  username of the RDBMS
ds.password  password of the RDBMS
ds.maxPoolSize max number of connections to the RDBMs, defaults to 10
