## 1.0 MongoDB Essentials

### Terminology Translation

|RDBMS|MongoDB|
| -------- | --------   |
| Database | Database   |
| Table    | Collection |
| Row      | Document   |
| Index    | Index      |
| JOIN     | Embedded document, document references or $lookup to combine data from different collections |

```
Mongo DB supports a hundred levels of depth for your documents​
```

See More: [Terminology and Concepts](https://www.mongodb.com/compare/mongodb-mysql)

See More: [SQL to MongoDB Mapping Chart](https://docs.mongodb.com/manual/reference/sql-comparison/)

### [Data Types](https://www.tutorialspoint.com/mongodb/mongodb_datatype.htm)
  - **String**− This is the most commonly used datatype to store the data. String in MongoDB must be UTF-8 valid.
  - **Integer**− This type is used to store a numerical value. Integer can be 32 bit or 64 bit depending upon your server.
  - **Boolean**− This type is used to store a boolean (true/ false) value.
  - **Double**− This type is used to store floating point values.
  - **Min/ Max keys** − This type is used to compare a value against the lowest and highest BSON elements.
  - **Arrays**− This type is used to store arrays or list or multiple values into one key.
  - **Timestamp**− ctimestamp. This can be handy for recording when a document has been modified or added.
  - **Object**− This datatype is used for embedded documents.
  - **Null**− This type is used to store a Null value.
  - **Symbol**− This datatype is used identically to a string; however, it's generally reserved for languages that use a 
    specific symbol type.
  - **Date** − This datatype is used to store the current date or time in UNIX time format. You can specify your own date 
    time by creating object of Date and passing day, month, year into it.
  - **Object ID** − This datatype is used to store the document’s ID.
  - **Binary data** − This datatype is used to store binary data.
  - **Code** − This datatype is used to store JavaScript code into the document.
  - **Regular expression** − This datatype is used to store regular expression.

### Query Language

![Query Language](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/query_lang.jpg)

See More: [Query Language](https://www.mongodb.com/compare/mongodb-mysql)

See More: [SQL to MongoDB Mapping Chart](https://docs.mongodb.com/manual/reference/sql-comparison/)

### Why MongoDB is effective

**RDBMS Data**

- Separate tables for different data
- Tied together by primary key
- Complex representation

**SQL Query for data from RDBMS**

- Data is discrete in several tables​
- Complex to select​

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/rdb_table.jpg)

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nosql_docu_arrow.jpg)

**Select data from MongoDB**

- Data is in only an object​
- Easy to handle​

![NoSQL_Document](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nosql_docu.jpg)

See More: [Query Language](https://www.lynda.com/Moodle-tutorials/Why-Mongo/573253/611681-4.html?autoplay=true)

### Aggregation

- Similar to GROUP BY function in SQL​

![Aggregation](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/aggregation.jpg)

See More: [SQL to Aggregation Mapping Chart](https://docs.mongodb.com/manual/reference/sql-aggregation-comparison/index.html)

### Performance

**Index**

- 64 indexes
- Single field
- Compound
- Unique

```
Mongo does support up to 64 indexes per collection​
```

**Sharding**

- Partition data onto different machines
- Provides scalability via software
- Autosharding supported by Mongo
- Challenging to set up

Using sharding means that you can scale your application across several smaller systems rather than investing in a larger, more expensive solution.​

While sharding is aimed at scalability, it's arguably even more important to ensure the uptime of your system, and for this, Mongo provides replication.​

**Replication**

- Reliability
- Maximizes uptime
- Replica set
- Automation failover

With MongoDB, you set up a replica set for your database with a primary server and some number of secondary servers, which keep copies of the primary servers data. If your primary server fails, one of the secondary servers will be promoted to the primary slot, and the database will continue to act as usual. When the original primary comes back online, it will be added back to the system as a new secondary server in the system. 


### Debugging​​

```
db.number.explain("executionStates")​
```

![Debug](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/debug.jpg)

## Stored Procedures

- ### Create:  

```
function addNumbers( x , y ) {
    return x + y;
}
```

- ### Save: 

```
> db.system.js.save({_id:"addNumbers", value:function(x, y){ return x + y; }});
```

- ### Utilize: 

```
> db.system.js.find()
{ "_id" : "addNumbers", "value" : function cf__3__f_(x, y) {
    return x + y;
} }
```

- ### Result: 

```
> db.eval('addNumbers(17, 25)');
42
```

See More: [Getting Started With Stored Procedures in MongoDB](http://pointbeing.net/weblog/2010/08/getting-started-with-stored-procedures-in-mongodb.html)

## 1.1 Infrastructure

MongoDB can be run in the cloud through MongoDB Atlas, or on local server.
If MongoDB is in the cloud it has no hardware requirements.

### Hardware

MongoDB does not support 32-bit x86 platforms.

MongoDB 3.4 supports x86_64/amd64, ARMv8-A. The Enterprise version also supports s390x, and POWER8(little endian). 

Use RAID10 and SSD drives for optimal performance.
SAN and Virtualization:
Ensure that each mongod has provisioned IOPS for its dbPath, or has its own physical drive or LUN.
Avoid dynamic memory features, such as memory ballooning, when running in virtual environments.
Avoid placing all replica set members on the same SAN, as the SAN can be a single point of failure.

Hardware use depends on storage engine used by MongoDB. Storage Engines manage how data is stored and memory and on disk.

### WiredTiger
Default storage engine for all versions is currently WiredTiger. It is multithreaded and benefits from use of XFS file system.

Starting in 3.4, the **WiredTiger internal cache, by default, will use the larger of either:**

**50% of (RAM - 1 GB)**, or **256 MB**.

For example, on a system with a total of 4GB of RAM the WiredTiger cache will use 1.5GB of RAM (0.5 * (4 GB - 1 GB) = 1.5 GB). 

A system with a total of 1.25 GB of RAM will allocate 256 MB to the WiredTiger cache because that is more than half of the total RAM minus one gigabyte (0.5 * (1.25 GB - 1 GB) = 128 MB < 256 MB).
from: https://docs.mongodb.com/manual/core/wiredtiger/#memory-use

### MMAPv1
Supported until MongoDB 4.0 is MMAPv1 which was default for versions 3.0 and older. It is concurrent and minimum requirements if using MMAPv1 is access two real cores or one physical CPU for mongod or mongos. XFS or EXT4 file system is preferable.

### In-Memory
MongoDB Enterprise has In-Memory Storage Engine. It is concurrent, not persistent, and maintains little disk data. It uses 50% of physical RAM - 1 GB but this can be reconfigured. Can be used in instances that are part replica sets or sharded clusters. 

For more info https://docs.mongodb.com/manual/administration/production-notes/, https://docs.mongodb.com/manual/core/inmemory/

Some case studies of other companies planning hardware requirements are found here https://www.mongodb.com/blog/post/capacity-planning-and-hardware-provisioning-mongodb-ten-minutes



Possibly useful example for calculating size of database where contents are added and stored for a length of time.

formula for calculating size
N = i * (t + 10) + p

p: Number of assets that are permanently in the system

t: Max number of days an asset is kept in the system after ingestion

i: Number of assets ingested per day (24h)

N: Total number of assets in the system at any given time.

Disk space (Megabytes) = N * 0,01

this example is from https://learn.fotoware.com/02_FotoWeb_8.0/05_Configuring_sites/Setting_the_MongoDB_instance_that_FotoWeb_uses/MongoDB_disk_and_memory_requirements



## Operating Systems
Supports Windows 7.
Recommended operating systems for production use with MongoDB:
* Amazon Linux
* Debian 8
* RHEL / CentOS 6.2+
* SLES 12
* Ubuntu LTS 16.04
* Windows Server 2012 R2

Linux kernel version 2.6.36 or later and XFS or EXT4 filesystem.

MongoDB 3.4 Enterprise Advanced is compatible with IBM POWER SYSTEM. For more information https://developer.ibm.com/linuxonpower/2017/01/17/mongodb-3-4-now-available-for-new-platforms-including-power/

MongoDB may run more efficiently on Linux: https://stackoverflow.com/questions/24178128/why-mongodb-performance-better-on-linux-than-on-windows

## 1.2 GUI Application


List of **Free** GUI applications available:

- [Navicat for MongoDB](https://www.navicat.com/en/products/navicat-for-mongodb)
- [NoSQLBooster](https://nosqlbooster.com/downloads)
- [**MongoDB Compass**](https://www.mongodb.com/products/compass)
- [Studio 3T](https://studio3t.com)
- [NoSQL Manager](https://www.mongodbmanager.com/download)
- [Robo 3T](https://robomongo.org/)

## 1.3 Install and Cloud Service

A. To run on Local Machine follow these steps:
[![MongoDB](https://webassets.mongodb.com/_com_assets/cms/mongodb-logo-rgb-j6w271g1xn.jpg)](https://www.mongodb.com/)


- Download latest Community/Enterprise MongoDB version from [MongoDB Download Page](https://www.mongodb.com/download-center?jmp=nav#community).
    > You can also follow steps explained in [this](https://docs.mongodb.com/manual/installation/) page for installation depending on your System.

- Run the Installer(.msi) for unattended installation.
    > Note: Remember to uncheck "Install MongoDB Compass" (there is a bug in the Installer which will halt the installation if we select installing Compass)

- After Installation is successfully completed copy the path and add it to Environment Variable ` "<C:\Program Files\MongoDB\Server\4.0\bin >"`

- Connect using the following command: `mongo host:port/database -u <dbuser> -p <dbpassword>`
  


B. To run MongoDB Cluster on Cloud, [SIGNUP](https://mlab.com/signup/) for [![mLab](https://mlab.com/company/brand/img/downloads/mLab-logo-onlight.png)](https://mlab.com/) which will give you free 500MB of Sandbox for development (You can choose between Azure, AWS and Google Cloud).

After you finish sginning up and creating you very first MongodDB database.
You can now connect using the MongodDB Compass (Robo3T or any other MongoDB GUIs) using the following details.

  - Hostname: ds237832.mlab.com (looks something like this)
  - Port: 37832 (will be different in your case)
  - Database: Example_Database
  - Username: **dbuser**
  - Password: **dbpassword**

> **Note:**  REMEMBER TO CREATE USER BEFORE CONNECTING TO MongoDB SERVER.

## 1.4 License

MongoDB is available with AGPL v3.0 license. That license is open source and requires the source code of any changes/work done using the software to be released. The AGPL license can be found here http://www.gnu.org/licenses/agpl-3.0.html/

The drivers are available under Apache License v2.0 .

Commercial license is available with MongoDB Enterprise Advanced, which offers additional monitoring, authentication and administration features.

More info here: https://www.mongodb.com/community/licensing

## Cost Categories
To compare the economics of deploying MongoDB and
Oracle, we consider the total cost of ownership (TCO) of
example applications using these databases. TCO captures
both the upfront and ongoing costs associated with building and running a database. It includes personnel
costs (e.g., developer salaries) in addition to the cost of
hardware, software and support. Table 1 describes the cost
categories we consider in this analysis.


![Cost Details](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/new6.png)

Follow the below table for Cost Analysis.<br>

![Cost Analysis](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/new7.png)

![Cost Analysis](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/new8.png)



# Apache Ni-Fi

Open source data flow configuring tool, downloaded from [link](https://nifi.apache.org/download.html)
Distributed under [Apache License](https://www.apache.org/licenses/LICENSE-2.0). 




#  ZappySys LLC(formerly used)

## Annual Subscription For Standard and Professional Edition **(Yearly)**

| Product       | Price         |
| ------------- |:-------------:|
| [Annual Subscription – SSIS PowerPack Standard Edition Includes 40+ SSIS components/tasks](https://zappysys.com/purchase/)     | $999/year |
| [Annual Subscription – SSIS PowerPack Professional Edition Includes 70+ SSIS components/tasks](https://zappysys.com/purchase/)     | $1299/year      |

## Perpetual License **(One-Time)**

| Product       | Price         |
| ------------- |:-------------:|
| [SSIS PowerPack Standard Edition (1 year support & upgrades included) Includes 40+ SSIS components/tasks](https://zappysys.com/purchase/)     | $1899 |
| [SSIS PowerPack Professional Edition (1 year support & upgrades included) Includes 70+ SSIS components/tasks](https://zappysys.com/purchase/)     | $2399      |

> **Note:** There is no option to buy Perpetual Licence for individual components.

### Components used in our Project and their Pricing.
<hr>

1. [SSIS MongoDB Integration Pack](https://zappysys.com/products/ssis-powerpack/ssis-mongodb-integration-pack/)
    - [SSIS MongoDB Source (Query/Read/Extract)](https://zappysys.com/products/ssis-powerpack/ssis-mongodb-source/)
    - [SSIS MongoDB Destination (Bulk Insert, Delete)](https://zappysys.com/products/ssis-powerpack/ssis-mongodb-destination/)
    - [SSIS MongoDB ExecuteSQL Task](https://zappysys.com/products/ssis-powerpack/ssis-mongodb-executesql-task/)
  
  
2. [SSIS JSON Integration Pack](https://zappysys.com/products/ssis-powerpack/ssis-json-integration-pack/)
    - [SSIS JSON Generator Transform](https://zappysys.com/products/ssis-powerpack/ssis-json-generator-transform/)


| Product       | Price         |
| ------------- |:-------------:|
| [SSIS MongoDB Integration Pack](https://zappysys.com/products/ssis-powerpack/ssis-mongodb-integration-pack/)     | $699/Year  |
| [SSIS JSON Integration Pack](https://zappysys.com/products/ssis-powerpack/ssis-json-integration-pack/)     | $699/year     |

ZappySys has a Blog section with many potentially useful articles for how to use its components at  https://zappysys.com/blog/?s=Mongodb . 

## 1.5 How MongoDB stores data on-disk?

## How SQL Server stores file ?

- A record, also known as a row, is the       smallest storage structure in a SQL         Server data file.
  
- Data records are stored in a fixedvar       format

    ![FixedVar](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/how1.PNG)

 - SQL Server organizes and stores            records in smaller units of data, known    as pages. 
  
 - A page is always exactly 8 KB (8192        bytes) in size and contains two major      sections, the header and the body. The     header has a fixed size of 96 bytes and    has the same contents and format,          regardless of the page type. It contains   information such as how much space is      free in the body, how many records are     stored in the body, the object to which    the page belongs and, in an index, the     pages that precede and succeed it.

    ![Page](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/how2.PNG)

## How MongoDB Stores Data on-disk?

- MongoDB is a document-oriented database. 
  
- Instead of storing your data in tables      made out of individual rows, like a         relational database does, it stores your    data in collections made out of             individual documents.
  
- In MongoDB, a document is a big JSON blob   with no particular format or schema.
  
- **WiredTiger Storage Engine**.
  Default for all versions is currently WiredTiger. It is multithreaded and benefits from use of XFS file system.
WiredTiger uses document-level concurrency control for write operations. As a result, multiple clients can modify different documents of a collection at the same time.

- A given mongo database is broken up into    a series of BSON files on disk, with        increasing size up to 2GB. BSON is its      own format, built specifically              for MongoDB. MongoDB stores the data on     the disk as BSON in your data path          directory, which is usually /data/db.

    ![Data File Allocation](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/how3.PNG)

 ![To Sum Up](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/how4.PNG)

![BSON Encoding](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/how5.PNG)


- We can find BSON file specification and information about WiredTiger storage Engine in the links below:
	- http://bsonspec.org/
	
    - https://docs.mongodb.com/v3.0/core/wiredtiger/

## 1.6 Operation

### Stopping MongoDB server.

To stop mongod from mongo shell use these commands

`use admin`

`db.shutdownServer()`

### Restarting MongoDB server.

Go to Services list of your PC. Click on MongoDB Server service and then click "Start".
![RestartMongo picture](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/RestartMongo.JPG)

Additional info at [This Link](https://docs.mongodb.com/manual/tutorial/manage-mongodb-processes/)

### Repairing an improperly shut down DB

When a Mongo database is stopped incorrectly it may lead to data corruption and prevent MongoDB from restarting. To fix it do a back up and use the command

`mongod --repair`

This command will rebuild indexes, erase all corrupted data and create empty files for missing data.
For more info see [Link](https://docs.mongodb.com/manual/tutorial/recover-data-following-unexpected-shutdown/)


## 2.0 Schema Design

Schema Design depends on the design of the application using it. 

Schema design comes in two versions: Embedded and Referencing.

In Embedded design the collection stores multiple documents nested within other documents.
If data from several SQL tables is usually accessed together, it can be nested in one collection, which will improve speed. 

In Referencing design the collection stores reference ids of documents from other collections.If needed, the reference ids are used to access corresponding documents from other collections.

Schemas can also be designed in tree format.

### The below Process shows how to collapse 2 RDBMS Tables to 1 BSON Documents.

### Step1: Drawing Diagram

### What are the rules to model a document db?

- natural and intuitive structures
- 1:1 or 1:many relationship is candidates for embedding (HAS-A relationship, Composition relationship)
- many-to-many relationship is candidates for referencing

More consider: 

Even if there is 1:1 or 1:many relationship, if: 

- A parent document is frequently read, but an embedded document is rarely accessed,
- One part of a document is frequently updated and constantly growing in size, while the remainder of the document is relatively static,

then, consider using referencing, and if: 

- The document size exceeds MongoDB’s current 16MB document limit
- Referenced from many different sources
- To model large, for example, hierarchical data sets

then, consider using referencing.

[RDBMS Modeling]

![Sch_Dia](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/task_erd.jpg)

[Document DB Modeling]

![Sch_Dia](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/task_erd2.jpg)

### Step2: Filling out schema mapping chart that we will use to map some of the tables we are transferring

![Sch_Dia](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/sch_map.jpg)



### Step3: create a schema in which we embed an array of sub-documents within primary document

```
import_product_M collection: 

{
	pid: 1,
	gtin: “000123456789”,
	...
	,
	import_product_version_M: 
	[
		{ 
                        pid: 1,
                        gtin: “000123456789”,
			version_id: 1,
			version: 1973,
			productName_en: "Lenobo T570",
			productName_fr: "Lenobo T570",
			...
			,
			import_product_version_allergen_info:
			[
				allergen_name_value: "Apple allergen",
				allergen_name_value: "Banana allergen", ...

			]
		},
		{ 
			version_id: 2,
			version: 1979,
			productName_en: "SAMSUNG Monitor ABC001",
			productName_fr: "SAMSUNG Monitor ABC001",
			...
			,
			import_product_version_allergen_info:
			[
				allergen_name_value: "Apple allergen",
				allergen_name_value: "Banana allergen", ...			
			]
		},
		...
	]
}
```

### What are the advantages of BSON Document model?

- Provides more natural and intuitive way to represent data
- Performance and Scalability

	- less cares about relationships among entities. Using relationship keys spend more resources costly 
	- distributing the database across multiple nodes (a process called sharding), to achieve massive horizontal scalability on commodity hardware
	- no longer needs to worry about the performance penalty of executing cross-node JOINs (should they even be possible in the existing RDBMS) to collect data from different tables.

- can be accessed with a single call to the database
- The MongoDB document is physically stored as a single object, requiring only a single read from memory or disk (On the other hand, RDBMS JOINs require multiple reads from multiple physical locations)

See More: [RDBMS to MongoDB Migration Guide](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/doc/RDBMS_to_MongoDB_Migration_Guide.pdf)

## 2.1 ETL (Extract, Transform, Load) Tool

### Commercial VS. Own solution

![Query Language](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/etl_commercial.jpg)

### Data Migration using Node.js

![Query Language](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/etl_own1.jpg)

![Query Language](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/etl_own2.jpg)

### Inside node.js: ​

1. Check the depth from Schema Mapping Chart Metadata to figure out which parent or child collection is​
2. Select parent from RDBMS // using mssql-npm JS​
3. Select child from RDBMS    // using mssql-npm JS  ​
4. Insert data using nested loop into MongoDB // using mongoose JS​

### pseudo code:

```js
while (parent != null) {​

    1) create parent object in JSON format​
    while (child != null) {​
           2) create child object in JSON format​
           3) insert parent-child // or update parent-child object into MongoDB​
    }​
}​
```

### More consider: ​

- How to migrate the incremental data​
- How to prove the integrity of migration data
- Why not add Backup & Recovery, Archiving & Purging functions later

<hr />

Refer to the following document to follow the Best Practices while using ETL tools for MongoDB.

[ETL Best Practices](https://www.mongodb.com/partners/partner-program/technology/certification/etl-best-practices)


SSIS and Node.js are being explored as ETL tools

## 2.2.0 Data Migration

There are several ways we can Migrate data from source RDBMS to MongoDB:
 - Export all the data in JSON file format and Import them in already created Collection on MongoDB.
 - Use SQL server tool SSIS and ZappySys tools to Migrate data. 

SSIS and ZappySys have been show to work for migrating data from both SQL Server and MySQL to MongoDB.
More information can be found in [ETL Tool section](https://github.com/Gs1TestTeam/MongoDB_Task/wiki/2-2.-ETL-Tool)

## Data Migration using SSIS

We can use SSIS to migrate data from RDBMS to MongoDB.

Steps to follow for Migration:

> Prerequisite: Must have [Visual Studio 2017](https://visualstudio.microsoft.com/) installed.

 - Download and Install [SQL Server 2014 ](https://www.microsoft.com/en-ca/download/details.aspx?id=42299)

 - Download and Install [CData SSIS Component](https://www.cdata.com/drivers/mongodb/download/ssis/)

 - Download and Install [SSDT](https://docs.microsoft.com/en-us/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)

 - Alternatively, we can also download & install [ZappySys's SSIS PowerPack](https://zappysys.com/products/ssis-powerpack/)
 and download individual MongoDB Components.

## Creating an SSIS Project

 - Open Visual Studio.
 - Click on File > New > Project
 - Select Business Intelligence > Integration Services from the left Pane.<br>

 ![SSIS Package](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/ssis_package.png)

 - Drag and drop a Data Flow Task from the menu to create a Data Flow in which the migration process will be defined.
![data flow task](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/addDataFlowtask.jpg)

## Connecting to MongoDB as a destination

In the Data Flow task, drag and drop a ZS MongoDB Destination component from the SSIS Toolbox.

- Editing ZS(Zappy's Sys) MongoDB Destination.
  - Add a "New Connection" for MongoDB.<br>
  ![MongoDB Connection](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/4.png)
  - Edit the Connction property Accordingly.<br>
  ![Edit MongoDb Connection](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/5.png)
  - Edit Connection properties. Select newly created MongoDb Connection from drop down.<br>
  ![Add MongoDB](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/6.png)
  - Edit Component properties and add the Mongo Table in which we want to Import the data.<br>
  ![Add Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/7.png)
  - Lastly, match/map the Source and Destination Columns.<br>
  ![Map Columns](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/8.png)

## 2.2.1 Migrating from MS-SQl to MongoDB using SSIS.

### Data Migration using SSIS

We can use SSIS to migrate data from SQL Server to MongoDB.

Steps to follow for Migration:

> Prerequisite: Must have [Visual Studio 2017](https://visualstudio.microsoft.com/) installed.

 - Download and Install [SQL Server 2014 ](https://www.microsoft.com/en-ca/download/details.aspx?id=42299)

 - Download and Install [CData SSIS Component](https://www.cdata.com/drivers/mongodb/download/ssis/)

 - Download and Install [SSDT](https://docs.microsoft.com/en-us/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)

 - Alternatively, we can also download & install [ZappySys's SSIS PowerPack](https://zappysys.com/products/ssis-powerpack/)
 and download individual MongoDB Components.

### How to create a SSIS Package for Migration

 - Open Visual Studio.
 - Click on File > New > Project
 - Select Business Intelligence > Integration Services from the left Pane.<br>

 ![SSIS Package](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/ssis_package.png)

 - Create a Data Flow. Add "OLE DB Source" from the left pane (Drag + Drop).<br>

 ![Data Flow](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/dataflow1.png)

 -  Add "ZS MongoDB Destination" from the left pane (Drag + Drop).<br>

 ![Data Flow Destination](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/dataflow2.png)

 - Edit OLE DB Source properties and add source database/table to fetch data.
   - Add a "New" Server.
   - If it is a local server, hosted locally on your workstation use Windows Authentication else use SQL Server Authentication for different Accounts for remote connections.

   ![Add Server](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/1.png)

 ![Add Data Base](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/2.png)

- After we have completed adding the Server, Selecting the database and table from which the data has to be migrated, we can now feed the output to MongoDB destination as follows.



![Feeding Output to Mongo](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/3.png)


- Editing ZS(Zappy's Sys) MongoDB Destination.
  - Add a "New Connection" for MongoDB.<br>
  ![MongoDB Connection](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/4.png)
  - Edit the Connction property Accordingly.<br>
  ![Edit MongoDb Connection](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/5.png)
  - Edit Connection properties. Select newly created MongoDb Connection from drop down.<br>
  ![Add MongoDB](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/6.png)
  - Edit Component properties and add the Mongo Table in which we want to Import the data.<br>
  ![Add Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/7.png)
  - Lastly, match/map the Source and Destination Columns.<br>
  ![Map Columns](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/8.png)


## How to migrate Parent-Child data from RDB to MongoDB using ZappySys compents.
 - Contains folowing components:
   - Add Multiple RDBMS Source.
   - Add & Merge Join component for performing JOINS on the RDBMS tables.
   - Add a ZS JSON Generator Transform component for creating a JSON Document from the output of the Merge.
   - Finally, Add a MongoDB Component and Feed the Data to MongoDB Collection.

Layout of the Package.<br>
![SSIS Package](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/ssis_package_new.png)

### Step-by-Step Procedure to follow

1. Create an Integration Services package using Visual Studio Follow **How to create a SSIS Package for Migration**.

2. Do the following under the "Data Flow" tab in the Designer Pane:

    1. Add Parent and Child table Data Source - **OLE DB Source**.<br>
    ![Parent/Child Data Source](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/new1.png)

    2. Right Click to view "Advance Editor" and edit the "Input/output Properties" and make the **IsSorted** field **True** for both Parent and Child.<br>
    ![Edit Property 1](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/new2.png)
    ![Edit Property 2](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/new3.png)

    3. Add a **Merger Join** Component. You can use anytype of joins to get the results.<br>
    ![Merge Join](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/new4.png)

    4. Add a **JSON Generator Transform** component to convert the data into JSON format to later feed it to **MongoDB Destination**.
     
         ![JSON Generator Transform](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/json1.png) 

         - Create Parent and Child elements. Remember to change **Output Mode** According to your need maerked with Number 1 on the screen shot.

        ![ADD JSON](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/json2.png) 

    5. Finally, Add MongoDB Destination.
    
    
## 2.2.2 Migrating from MySQL to MongoDB using SSIS.

Migrating from MySQL to MongoDB via SSIS will be described below. In this case we will be migrating two nested sample tables, one will have some information about people uniquely identified by field "ID", and another table about the hobbies of these people, connected by field "resident" as foreign key to the first table.

1. To be able to make a connection to MySQL to fetch data from SSIS, and MySQL ODBC driver needs to be set up if it is not already. Below is an image of a set up MySQL ODBC driver in System DSN tab of ODBC Data Source Administrator window.
![MYSQL ODBC](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/SetupODBCDriver.JPG)
To set up MySQL ODBC driver follow this tutorial https://kb.iu.edu/d/amsw

2. Create a new Integration Services Project in SSDT. Instructions can be found [Here](https://github.com/Gs1TestTeam/MongoDB_Task/wiki/2.2.0-Data-Migration#creating-an-ssis-project)

3. Click on the Data Flow Task. To connect to MySQL database drag and drop an ADO NET Source from SSIS toolbox.

![add an ado net source](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step1ADOnetsource.JPG)
Right click on ADO NET source and click "Edit". 
In the ADO.Net Source Editor select "New...", and in the "Configure ADO.Net Connection Manager" select "New...", to add a new connection.

In the "Connection Manager", select the provider as ".Net Providers\MySQL Data Provider" and fill out the form with server name, database name, port number, user id and password for the mySQL server. 
![mysql connection](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step2mySqlConnect.jpg)

Click "test connection" to test if it works, then click "ok" in the "Configure ADO.Net Connection Manager".

Back in the "ADO.Net Source Editor" you will now see the connection you just created listed as the ADO.NET connection manager. Select "data access mode" as "SQL command", and write the SQL command to extract ordered data using an ORDER BY clause.

![getting mysql data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step3getdatafromMySql.jpg)

4.The data being migrated into MongoDB in this example will be put into a parent-child document structure. Create a second connection for the embedded child table (1 connection per table). 

![multiple tables](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step4MultipleTableSources.jpg)

5. For both ADO NET Sources, right click each ADO NET Source and select "Show Advanced Editor...". In advanced editor window select "Input and Output Properties" tab, click on "ADO NET Source Output" and set IsSorted to True in the "common properties".

![set IsSorted key](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step5setIsSortedKey.jpg)

Expand "ADO NET Source Output" and "Output Columns". Click on the column sorted by in SQL statement (in this case "ID") and set SortKeyPosition to indicate the order it was sorted in, in this case "1". 

![sortkey position parent](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step6sortkeypositionsetParent.jpg)

Do the same in the ADO Source for child table, but since child table was sorted by field "resident"(the foreign key), click on the field "resident" and set SortKeyPosition to "1".
In cases where the data was sorted in descending order this field would be set as a negative value, and if data was sorted by multiple columns, SortKeyPosition for each column should be set to indicate sorting order, such as 1 for the first, -2 for second descending, 3 for last ascending. Unsorted columns should be set to 0.

6. Drag and drop a JSON Transform component to the Data Flow Task. Connect the two ADO NET Sources to the JSON Transform component by clicking and dragging the blue arrow to the JSON component. 

7. Double click the JSON Transform component to open the "JSON Generator Transform" Window. The data from ADO Sources are listed under "Datasets". Click on a dataset name to open "Add/Edit Dataset" window. Here you can change the dataset name (in this case it will be renamed as "Parent", and the nested dataset will be called "Child"), and check the "Root dataset" check box to set it as top level element. You can also see where the data for this dataset will be coming from in the "Select input for this dataset" drop down.
![json transform datasets](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step7JsonGeneratorDatasetsA.jpg)
You can also see the name of the dataset input from each ADO NET Source by clicking on the arrow connecting the ADO NET component to the JSON Transform generator and seeing its "DestinationName" in "Common Properties".
![dataset input name](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step7JsonGeneratorDatasetsB.jpg)

8. Click on "Mappings" then click on "insert new element under selected node" icon to open the "Add/Edit Attribute" window. Select "Add Multiple (Bound)" to add multiple records, and check off the boxes of the columns you want to add as parent document.

![json mapping top level](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step8jsonmapping.jpg)

9. To add a nested table, first create an element to store it in. Click on "Mappings" then click on "insert multi row element under selected node", and in the "Add/Edit Element(Array of rows)" window select "Document Array" as "Element type", write the element's name in "Element Name" box, select "Child" dataset as "Dataset for this element". 
Next create a join. Select the columns from each dataset that the join should be on. **Then click on "+" to add them**. To finish click "OK".

![json mapping add nested element](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step9jsonMapping.jpg)

10. To add nested columns click on the newly created array element then click on the "insert new element under selected node" icon. In the "Add/Edit Attribute" windows and select "Add Multiple(Bound)" and check off the boxes of the columns that should be in the nested document. click "OK".

![json mapping fill nested element](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step10jsonMapping.jpg)

The completed nested JSON mapping should look like this:

![json mapping](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step11mappingResult.JPG)

11. Add a ZS MongoDB Destination component, and connect the output of the JSON Transform to it. Refer to the [Data Migration section](https://github.com/Gs1TestTeam/MongoDB_Task/wiki/2.2.0-Data-Migration#connecting-to-mongodb-as-a-destination) for configuring a MongoDB connection. The final product should look like this:

![final chart](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/step12MySQLtoMongoDBviaSSIS.JPG)

12. Click "Start" to run the migration.

Migration form MySQL to MongoDB using Apache NiFi Dataflow.


This page explores the option of migrating data from MySQL to MongoDB using Apache NiFi.

## Data Migration from Mysql to MongoDB using Apache-Nifi:

### Installing Apache-Nifi:
  - Download [Apache-Nifi.](https://nifi.apache.org/download.html) Only download the binaries.

  - Extract it anywhere, it's path independent.

  - After Extraction is complete navigate to **[Extract Location]\nifi-1.7.1-bin\nifi-1.7.1\conf** and open the Properties file **"nifi.properties"**. We would need to edit some attributes in this file.

  - Before editing make sure you have JAVA Home Path defined as your Environment Variable, if not already do it.
  ![JAVA_HOME_PATH](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi1.png)

  - Open **"nifi.properties"**:
    - Edit: **nifi.ui.banner.text** = [ Put whatever name you like ]
    - Edit: **nifi.web.http.port** = 8080 (or whatever port is free).
    - Edit: **nifi.sensitive.props.key** = (Set up a pass key of Your Choice).
    - After Editing all above three properties Save & Close.


  - Navigate to **[Extract Location]\nifi-1.7.1-bin\nifi-1.7.1\bin** and right click on **run-nifi** and **"Run as Administrator"**.

  > NOTE: It takes time for Nifi to start so be advised not to try and open moments after executing it. Wait for few minutes and then Start.

  - You can see the status of Nifi by executing **status-nifi**.


  - To start Nifi:
    - Open any web-browers and type **localhost:[Port Number you set in the *nifi.web.http.port*]**
    For example: http://localhost:8080/nifi/

### Creating First Simple one table-to-collection Migration.

 - Before we start creating out Migration Template we need to Configure our Database connections for both MySQL and MongoDB:

      - Right Click on the Workspace and click on **Configure**.
      ![CONFIGURE](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi2.png)

      - On the **NiFi Flow Configuration** page under **Control Services** click on the **+**.

      - Select **DBCPConnectionPool** for MySQL and later after Configuring it Select **MongoDBControllerService** for MongoDB.
      ![MYSQL-CONNECTION](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi3.png)

      ![MongoDB-CONNECTION](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi7.png)

      - Click on the Settings Button on the right:
      ![SETTING](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi4.png)

      - Follow the Configuration settings for both MySQL and MongoDB from the images below:
      ![MYSQL](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi5.png)
      ![MONGODB](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi6.png)

      - After you are done configuring the Database Controller Services it will automatically validate for connection and Enable it.
      ![ENABLE](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi8.png)

 - Drag and Drop **Processor** onto the Wokrspace and Add an **ExecuteSQL** processor.
 ![ExecuteSQL](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi9.png)

 - Configure ExecuteSQL, Select **MySQLConnection** as the value for Database Connection Pooling Service and the Add a SQL Query for the Data pull:
 ![ExecuteSQL](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi10.png)

 - The Output of the **ExecuteSQL** component will be a Avro Flat File which needs to be converted into **JSON** before moving it to **MongoDB Collection**.

 - Add a **ConvertAvroToJSON** component for the conversion process.
 ![CONVERT](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi11.png)

- Now we have all the Data pulled from MySQL Table in a JSON Format. We need to put the Record in MongoDB. To Achieve that we need Couple of Reader and Writer components.

- To add an **AvroSchemaRegistry** which would have the Schema of the JSON records - Right Click and Configure. Under Controller Service add a  **AvroSchemaRegistry** service. Add a Schema and give it a name of your Choice.
To convert the JSON into Avro Schema use this tool: [JSON-to-Avro](http://avro4s-ui.landoop.com/)
![AvroSchemaRegistry](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi12.png)


- Add a **JsonTreeReader** and configure. Add the name of the schema you created in the above step.
![JsonTreeReader](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi13.png)

- The final list of **Controller Services** will look like this.
![Controller](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi14.png)

- Finally add a **PutMongoRecord** componenet which is used to bulk insert into a MongoDB Collection.
![PutMongoRecord](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi15.png)

- Configure the **PutMongoRecord** component. Set the Property **Record Reader** to the **JsonTreeReader** which it will use to parse the incoming JSON File.
![PutMongoRecord2](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi16.png)

- Final Migration Template will look something like this.
![Migration](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi17.png)

- To run the Migration click on the Play button:
![Play](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi18.png)

- After the Migration is **Successful** all the component will turn Green.
![Success](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/nifi19.png)

## 2.2.4 Migration Test

### Migration Case By Data Type

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/migration_case_by_data_type.jpg)

References: 

[1) Migrating Data By Type](https://www.lynda.com/SQL-Server-tutorials/Extracting-modified-data/156150/167737-4.html?autoplay=true)​

[2) How to create / run Daily Batch Job](https://www.lynda.com/SQL-Server-tutorials/Using-SQL-Server-Agent-execute-ETL-package/156150/167743-4.html?autoplay=true)​

[3) Data Quality Service for Data Integrity](https://www.lynda.com/SQL-Server-tutorials/Using-Data-Quality-Services-find-duplicate-data/156150/167748-4.html?autoplay=true)​

[4) How to utilize Data Warehouse](https://www.lynda.com/SQL-Server-tutorials/Introduction-data-analysis-SQL-Server-Analysis-Services-SSAS/156150/167753-4.html?autoplay=true) 

### Test Case By Data Type

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_case_by_datatype.jpg)

### Implementation ETL (Extract, Transform, and Load) Tool and Data Integrity Tool for Test

For this test, we implemented SSIS (SQL Server Integration Service) package, and to monitor data integrity, we implemented Data migration monitoring App using Angular for front-end and node.js for back-end.

See More: 

[Implementing SSIS (SQL Server Integration Service)](https://github.com/Gs1TestTeam/MongoDB_Task/wiki/2.2.1-Migrating-from-MS-SQL)

[Migration Monitoring App with Angular for front-end](https://github.com/Gs1TestTeam/GS1_Angular_Prototype_Auto_Deploy)

[Migration Monitoring App with Node.js for back-end](https://github.com/Gs1TestTeam/GS1_Angular_Prototype_Backend)

### TEST Result

<hr/>

### For First Test, 

```
Test Case ID: GS_MIG_002

Test Scenario: Migrate Dynamic Data

Test Case: 02. Migrate SQL server's live data 
(import_product, import_product_version...) 
to MongoDB (without Temp DB)
```

<b>Pre-Condition:</b> 

1. Start SQL Server
 - Confirm the service status (SQL Server and SQL Server Browser must be started)

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_precondition1.jpg)

2. Set up tables on SQL Server 
- Create tables: import_product, import_product_version
- Make 100 source data to migrate

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_precondition2.jpg)

3. Set up a MongoDB​
 - Create collection: import_product_ms 

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_precondition3.jpg)

4. Write SSIS package

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_precondition4.jpg)

5. Implement Migration data Monitoring App

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_precondition5.jpg)

<b>Test Step:</b> 

1. Open Migration data Monitoring App

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_precondition5.jpg)

2. Open SSIS package with Visual Studio

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step1.jpg)

3. Execute ETL tool to migrate data (with SSIS)

- Confirm the complete of SSIS in normal

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step3.jpg)

4. Confirm the data on target database (MongoDB)

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step4.jpg)

<b>Expected Result:</b>

- Target DB's document count is the same to the source DB's row count.​
- There is no different data between source and target DB.

<b>Actual Result:</b> 

- Using Data Monitoring App, confirm that the data count is the same and there is no data difference

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_result2.jpg)

- As a result of the test, Actual Result is the same to Expected Result.

<b>Status(Pass/Fail): Pass</b>  

<hr/>

### For second Test, 

```
Test Case ID: GS_MIG_002

Test Scenario: Migrate Dynamic Data

Test Case: 03. Migrate SQL server's live data 
(import_product, import_product_version...) 
to MongoDB (without Temp DB), generating unmatched data
```

<b>Pre-Condition:</b> 

This is the same to GS_MIG_002_02 Test Case.

<b>Test Step:</b> 

1. Open Migration data Monitoring App

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_precondition5.jpg)

2. Open SSIS package with Visual Studio

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step1.jpg)

3. Set up the break-point 

4. Execute ETL tool to migrate data (with SSIS)

- Confirm the complete of SSIS in normal

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step3.jpg)

5. Confirm that execution is suspended in break-point

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step5.jpg)

6. Confirm the data on target database (MongoDB)

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step4.jpg)

7. Change MongoDB data to test data difference check: pid, gtin, gln, productName_en, productName_fr 

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step6.jpg)

8. Resume SSIS package

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step7.jpg)

9. Confirm tests collection has document on MongoDB.

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step9.jpg)

<b>Expected Result:</b>

- There is different data between source and target DB.​

<b>Actual Result:</b> 

- Using Data Monitoring App, confirm that there is different data.

![RDB_Table](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/test_step8.jpg)

- As a result of the test, Actual Result is the same to Expected Result.

<b>Status(Pass/Fail): Pass</b>  

## 2.2.5.  Consuming Migrated Data

As we can see in 2.2.3 "[Migration Case By Data Type](https://github.com/Gs1TestTeam/MongoDB_Task/wiki/2.2.3-Migration-Test)" section, after migrating we can utilize the migrated data using the existing SQL based BI tools such as Tableau, Microsoft Strategy, and Qlik. In this section, we will talk about how to consume the migrated data with the SQL based BI tools.

The MongoDB Connector for Business Intelligence (BI) allows users to create queries with SQL and visualize, graph, and report on their MongoDB Enterprise data using existing relational business intelligence tools such as Tableau, MicroStrategy, and Qlik. ([MongoDB.com](https://docs.mongodb.com/bi-connector/current/ ))

To test BI Connector, we need four components: 

- MongoDB database: Data storage.​
- BI Connector: Provides a relational schema and translates SQL queries between your BI tool and MongoDB.​
- ODBC data source name (DSN): Holds authentication and connection configuration data.​
- BI Tool: Data visualization and analysis.​

The below figure shows a system diagram to use data from MongoDB with the Business Intelligence Tools.

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/consum_data1.jpg)

**Quick Test**

**1. Set up MongoDB, ODBC, BI connector**

- We can follow the wonderful guideline in MongoDB official website for "[Quick Start Guide for Windows](https://docs.mongodb.com/bi-connector/current/local-quickstart/)" 

**2. Start Mongod and Mongosqld**

 - Start Mongod

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/consum_data8.jpg)

- Start Mongosqld

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/consum_data9.jpg)

**3. Connect to MySQL and MongoDB on Tableau**

- Connect to RDBMS (MySQL)

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/consum_data2.jpg)

- Connect to MongoDB using ODBC driver

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/consum_data3.jpg)

- Confirm the connection in normal

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/consum_data6.jpg)

**4. Select data using SQL in Tableau**

- Join testview table in MySQL and testview_mongo, testview_mongo_nested_mongodata

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/consum_data7.jpg)

<hr />

### How to use Microsoft Power BI with MongoDB

1. Set up the test environment

- Same to 1, 2 of Tableau BI.

2. Set up the Power BI

- Refer to the guideline in MongoDB official website for "[Connect from Microsoft Power BI Desktop](https://docs.mongodb.com/bi-connector/master/connect/powerbi/)" 

3. Connect and Display data on MongoDB using Power BI
​
![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/powerbi_01.jpg)

### See More: 

**1. Integration RDBMS, MongoDB on Hadoop**

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/hadoop1.jpg)

- Apache Hadoop: an open-source software collection to use massive amounts of data using MapReduce programming model
- MapReduce: a programming model to implement for processing and generating big data sets with a parallel, distributed algorithm on a cluster
- Apache Hive: provider a SQL-like interface to query data stored in various databases and file systems
- Apache Spark: an open-source framework to distribute data on cluster-computing framework
- [MongoDB Hadoop Integration](https://www.youtube.com/watch?v=Y_JkdnkC4fw)
- [MongoDB Connector for Spark](https://docs.mongodb.com/spark-connector/current/)

**2. MongoDB Charts**

- MongoDB Charts: a tool to visualize data from MongoDB data​
- Docker: a digital container to contain software packages and operating system

​![Consuming Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/mongo_chart1.jpg)

- [MongoDB Charts beta installation walkthrough](https://www.youtube.com/watch?v=9JmZbuSTwes)
- [MongoDB Charts](https://docs.mongodb.com/charts/current/)



## 3.0 Indexing


### Function

Indexes improve query execution by storing a portion of the collection in a small B-tree collection that can be searched quickly, and saves query execution time by avoiding collection scan where all records in the collection are scanned. 

### How to use

A default _id index field is automatically generated by MongoDB when a new document is added and uniquely identifies all documents in a collection, and cannot be removed.
An _id can be used as shard key in sharded clusters, or can be replaced by ObjectId.

To create an index use command which will only work if the index does not already exist:

`db.<collectionname>.createIndex(< index key and type >, <options> )`

The process of building indices heavily impacts the database and prevents read and write operations.
To avoid this impact index construction can be run in the background using Background Construction. 

To remove an index use the following command:

`db.<collectionname>.dropIndex(< index key and type >)`

or

`db.<collectionname>.dropIndexes()`

To see indexes associated with collection:

`db.<collectionname>.getIndexes()`

### Single Field Index: Embedded field index vs embedded document index

Indexes can be created in embedded fields as well as embedded documents.
An index on an embedded field can be created using a dot notation on a top level field and will support queries that search embedded document fields.

`db.<abstract_collection>.createIndex( { "location.city":1 } )`

An index on an embedded document indexes the entire document and returns results that match exactly, including all the fields and the order.

examples: https://docs.mongodb.com/manual/core/index-single/

### Compound Index

![compound index](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/compoundindex.JPG)

One index structure holds references to multiple fields (max 32) and supports queries that match on multiple fields.

`db.<collectionname>.createIndex( { <field1>: <type>, <field2>: <type2, ...  } )`

Index will sort fields based on the order they are listed in.
Index prefix is the first field listed in the create statement. Index will support queries that do not search all the fields it sorts by, but prefix(the very first field) must be included in query and fields be in the same order.

### Multikey Index

Created automatically if one of the fields being indexed is an array. Does not support multiple array fields in one index.

### Index Intersection

Index intersection is the use of multiple indexes to support queries. Index intersection can support more queries than a compound index can because it is not limited by prefixes.

## 3.1 Historical Data Management

**1. What is the purpose of historical data?​**

- Analyzing audit fields for security reason​
- Tracking data changes 

**2. Schema Design for Historical Data​**

When we are talking about Schema Design for Historical Data​, there is no specific rules. However, there are some considerable parts like below: 

- All collections include a sequential number field​
- All collections include an audit trail​
- Historical collections store data on different physical space (collection)​
- Historical collections include full document of active collection

**2-1. what is audit trail: a security-relevant chronological record (Wikipedia)​**

Example: ​

CREATE_PROGRAM_ID​: application id that creates data​

CREATE_OBJECT_TIME​: created time​

UPDATE_PROGRAM_ID​: application id that updates data​

UPDATE_OBJECT_TIME​: updated time​

ARCHIVE_PROGRAM_ID​: application id that archive data​

ARCHIVE_OBJECT_TIME​: archived time​

ARCHIVE_STATUS_CODE​: archive yes/no (*: be subject to archiving)​

**2-2. Example of Historical Collection**

[​Active Collection]​

```js
{​
     _ID: "1", // 1) increment version number​
     GTIN: "1000000001",​
     NAME: "RED",​
     ...​
        CREATE_PROGRAM_ID​: "CreateGtinPgm", // 2) Audit fields​
        CREATE_OBJECT_TIME​: "2018-01-01 21:00.36",​
        UPDATE_PROGRAM_ID​: "UpdateGtinPgm",​
        UPDATE_OBJECT_TIME: ​"2018-01-05 21:00.36",​
        ARCHIVE_PROGRAM_ID​: "PurgeDataBatPgm",​
        ARCHIVE_OBJECT_TIME​: "2018-04-05 21:00.36",​
        ARCHIVE_STATUS_CODE​: "*"​
}​
```

[​Historical Collection]​

```js
{ // 3) different physical space​
     _ID: 1,​
     _ID_ORIGIN: 1, // 4)  full document of active collection​
     GTIN_ORIGIN: "1000000001",​
     NAME_ORIGIN: "RED",​
     ...​
        CREATE_PROGRAM_ID_ORIGIN​: "CreateGtinPgm",​
        CREATE_OBJECT_TIME​_ORIGIN: "2018-01-01 21:00.36",​
        UPDATE_PROGRAM_ID​_ORIGIN: "UpdateGtinPgm",​
        UPDATE_OBJECT_TIME_ORIGIN: ​"2018-01-05 21:00.36",​
        ARCHIVE_PROGRAM_ID​_ORIGIN: "PurgeDataBatPgm",​
        ARCHIVE_OBJECT_TIME​_ORIGIN: "2018-04-05 21:00.36",​
        ARCHIVE_STATUS_CODE​_ORIGIN: "*",​
        CREATE_PROGRAM_ID : "CreateHisPgm",​
        CREATE_OBJECT_TIME : "2018-01-01 21:00.36",​
        UPDATE_PROGRAM_ID : "UpdateHisPgm",​
        UPDATE_OBJECT_TIME: "2018-01-05 21:00.36",​
        ARCHIVE_PROGRAM_ID : "PurgeDataBatPgm",​
        ARCHIVE_OBJECT_TIME : "2018-04-05 21:00.36",​
        ARCHIVE_STATUS_CODE : "*"​
}​
```

**3. Managing Historical Data: implement functionalities in application level**

- When the application inserts new rows in progress table, then it inserts the rows to history table at once.

![Historical Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/data_history1.jpg)

- When the application update the existing rows in progress table, then it inserts the rows to history table at once.

![Historical Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/data_history2.jpg)

- Using the gathering data in historical table, we can track data changes history and analyze audit fields for security reason​​.

![Historical Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/data_history3.jpg)

**4. Archiving and Purging Data: implement Batch Jobs and run them in scheduler​**

- Archiving: moving data that is no longer actively used to a separate storage such as .tar or .zip file (techtarget.com)​
- Purging: freeing up space in the database (dba.stackexchange.com)​
- Active data and Historical data are different in archiving and purging period.  ​
- The period of Active data is shorter because of the query speed ( about 3 months)​
- The period of Historical data is longer because of analysis purpose ( about 3 years)​

**Procedure for Archiving and Purging: ​**

- First batch job marks asterisk (*) to the subject data for purging​
- Second batch job archives the marked data​
- Third batch job purges the archived data

1) For example, if GTIN 10000001 is subject to purging, then First Batch job marks the '*'s

![Historical Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/data_history4.jpg)

2) Second Batch job makes archiving file and move to other storage in proper time​
  - umount /dev/vg0/mdb-snap01 dd if=/dev/vg0/mdb-snap01 | gzip > mdb-snap01.gz​

3) Third Batch job purges the marked data in proper time

![Historical Data](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/data_history5.jpg)

**5. MongoDB solution**

Automatic Archiving and Purging​
- Mongo DB has not supported native Job scheduling. We should use other scheduling tools such as Linux Cron,  Windows Scheduler, or other scheduling tools.

**Batch Job Program List**

|Seq|Program Name|Description​|
|---|------------|-----------|
|1|​ExtractDataJob​|Find purge data and mark '*'​|
|2|​ArchiveDataJob​|Create an archiving file with the marked data​|
|3|​PurgeDataJob​|Delete the marked data​|

**Batch Job Program Detail**

- ExtractDataJob: Find purge data and mark '*'

pseudo code: 
```js
1) Create 'ExtractDataForPurge' store procedure​

function ExtractDataForPurge () {​
    1) Extract the purge data // refer to the purge rule​
    2) Mark the '*' in the extracted data​
}​
2) Periodically call 'ExtractDataForPurge' procedure in ExtractDataJob on scheduler​
     // refer to the business rule​
```

- ArchiveDataJob: Create an archiving file with the marked data

```js
1) Create 'ArchiveDataJob' store procedure​

function ArchiveDataForPurge () {​
    1) Create a 'Snapshot' for the marked data ​
        // cursor.snapshot()​
    2) Create an 'Archving file'​
       // umount /dev/vg0/mdb-snap01 dd if=/dev/vg0/mdb-snap01 | gzip > mdb-snap01.gz​
    3) Move an 'Archving file' to the specific location of specific server​
    4) Update ARCHIVE_PROGRAM_ID, ARCHIVE_OBJECT_TIME​
}​

2) Periodically, call 'ArchiveDataForPurge' procedure in ArchivingDataJob  on scheduler​
     // refer to the business rule​
​```

- PurgeDataJob: Delete the marked data

```js
1) Create 'PurgeDataForPurge' store procedure​

function PurgeDataJob () {​
    1) Delete the marked data​
        // db.delete({ARCHIVE_OBJECT_TIME: not null})​
}​

2) Periodically, call 'PurgeDataForPurge' procedure in PurgeDataJob  on scheduler​
     // refer to the business rule​
```

**Restoring: store data from archiving file to MongoDB​**

- To restore data, issue the following sequence of commands:​

lvcreate --size 1G --name mdb-new vg0 gzip -d -c mdb-snap01.gz | dd of=/dev/vg0/mdb-new mount /dev/vg0/mdb-new /srv/mongodb​

### As more considerable topics:

**What is difference between archiving and backup? ​**

Archival vs Backup​

An archive is a collection of historical records that are kept for long-term retention and used for future reference. Typically, archives contain data that is not actively used. Basically, a backup is a copy of a set of data, while an archive holds original data that has been removed from its original location.​

https://www.solarwindsmsp.com/content/backup-archive​

**What is difference between purging and deleting?** ​

Purging​

Database administrators purge data from tables when the table rows exceed several hundreds or even millions of records that are no longer needed. However, purging lets you archive the records even though you remove them from tables. Administrators move records from a large table to an archive table. Purging records from a large table speeds up queries.​

Deleting​

Deleting information removes it permanently from a table, and this task doesn't keep a copy of the records. Deleting is done by the administrator or you can delete records from your users' application. Deleting a record, for example, is used when the data entry employee makes a mistake when creating a record and needs to delete the bad record from the database.​

https://itstillworks.com/difference-between-purging-deleting-data-entry-12082.html​

**What is difference between recovery and restore?​**

Difference between restoring and recovering. ... Recovering files typically refers to saving one or more files, while a restore usually refers replacing a complete system or hard drive from a full system backup. Restore means restoring a database backup from a backup medium. Recovery" means recovering redolog information.​

https://pc.net/helpcenter/answers/difference_between_recover_and_restore ​

## 3.2 Supply

BI Connector tool can be used to display back data from MongoDB



## 4.0 Backup and Restore Methods

### Backup Methods: **mongodump** and **mongorestore**

* The **mongodump** command can be used to create a BSON data dump of MongoDB contents. The **mongorestore** command is used to restore BSON data dump to MongoDB. 
* these commands should be run outside of shell and from mongo installation's \bin folder. 
* These commands can be used to back up contents of entire instance, a database, a collection or query results.
* **Mongodump** only gets documents not indices, indexes are recreated later by **mongorestore**. 
* Needs mongod to be running. Not advisable to use  db.fsyncLock()/ db.fsyncUnlock()
* Good for small deployment backups or when other tools are not available. Can slow down performance if memory does not fit all data (for large data), because it reads entire database through memory. For large data it is recommended to use replica sets and run mongodump on a secondary server to preserve performance.
* Can be used with gzip files and has archive option

`mongodump --host <server:port> --db <dbname> --collection <collectionname> --out <filepath>`

`mongorestore --host <server:port> --db <dbname> --collection <collectionname> --drop <filepath>`

Options:

**--host <server:port>**
connect to serverName:portNumber (localhost if omitted)

**--db <dbname>**
select database to backup/restore to

**--collection <collectionname>**
select collection to dump/restore to (needs --db present)

**--query="{  status: 'A' }"**
add query to filter output

**--drop**
mongorestore by default inserts all records. If a record with same id exists, it is not overwritten. Mongorestore with --drop will delete the records and recreate the data in BSON dump.

### Replica sets
Replica sets have oplogs. This allows running **mongodump** with **--oplog** and **mongorestore** with **--oplogReplay** options that allow point in time backups.

### Backup Methods: Cloud Manager

* Cloud Manager: for cloud based MongoDB deployments that are not on the MongoDB cloud
* A Backup Agent runs within infrastructure and  automatically backs up replica sets and sharded clusters by creating snapshots of data at user-specified intervals, 
* Allows recovery from snapshot or from checkpoint between snapshots, and at selected points in time
* Snapshots are full backups, stored by MongoDB organization and and storage costs money. Size of snapshot = size of all indexes + documents for all databases

### Backup Methods: Ops Manager

* Ops Manager: for non-cloud MongoDB deployments.
* Same software as Cloud Manager but can be installed on own infrastructure, available in Enterprise Advanced subscriptions.

More info from [MongoDB's website](https://docs.mongodb.com/manual/core/backups/)


## 4.1 High Availability (Replication)

## 4.2 Scalability (Sharding)

### Revision History

|Date      |Comment      |Writer|Version|
|----------|-------------|------|-------|
|2018-08-30|Trial Version|      |0.1    |

## RoadMap

![Query Language](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/images/roadmap.jpg)

See More: [RDBMS to MongoDB Migration Guide](https://github.com/Gs1TestTeam/MongoDB_Task/blob/master/doc/RDBMS_to_MongoDB_Migration_Guide.pdf)

## Planning (Week1)

 - Define the requirements (output: business and technical requirements)
 - Research MongoDB essential features (output: MongoDB essential features)
 - Sign up for Cloud Servers and Install GUIs for RDB, MongoDB (output: account and user guideline)
 - Researhing licence, server required resource (output: licence and server required resource)

## Schema Design (Week2)

 - Create Schema Desgin Mapping Chart (From RDB to MongoDB) (output: Schema Desgin Mapping Chart.xls)
 - Write MongoDB schema using JSON format (output: JSON format for GS1's collections)
 - Create a tool to create the sample data to RDB (output: Data generate application)
 - Reseaching ETL tools to extract, transform, and load data from RDB to MongoDB (output: ETL tools and test result)

## App Integration (Supply) (Week3)

 - Create Application Changes Impact Analysis chart (to identify the scope for changing GS1's application) (output: Application Changes Impact Analysis chart.xls)
 - Write guidelines to change application for developers (output: developer guideline)
 - Write the scenarios to deploy GS1's application (output: application deploy scenarios)

## Data Migration (Week4)

 - Create collections to MongoDB (in Could service or local or team server) (output: collections on MongoDB)
 - Migrate data from RDB to MongoDB (measuring time, resource: hard disk, memory, CPU etc) (output: migration result report)
 - Deploy application (testing functionalities) 

## Operating (Week5, 6)

 - Research how to repond to the structure changes on MongoDB (output: how to repond to the structure changes)
 - Research and test Replication and Sharding (ouput: Introducign Replication and Sharding)
 - Write the strategies to introduce High Availability and Scalability for GS1 Canada's systems (output: Introducing High Availability and Scalability for GS1 Canada's systems)

## Documenting (Week7) 

 - Organize and write documentations with the outputs of each step (output: GS1 Canada Data Migration Report with MongoDB.doc)










