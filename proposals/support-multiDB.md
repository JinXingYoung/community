# Proposal: `Support multi-DB in harbor`

Author: `Yvonne / @JinxingYoung, Yiyang / @YiyangHuang, Minglu / @Mingluhan`

Discussion:

- [DB adaptable issue](https://github.com/goharbor/harbor/issues/6534)

## Abstract

Expand DB support for multi-DB, initially mysql and mariadb.

## Background

There are many issues that discuss the need to support multiple databases. Currently, only PostgreSQL is supported, limiting the use of many users.

Since Harbor 2.0 use trivy instead of clair，there‘s no requirement to use PostgreSQL. A lot of people want to use mysql or mariadb. it's good to support mysql/mariaDB in external_database mode initially.

## Proposal

In order to support different DB to harbor we propose support mariadb and mysql in Harbor which can meet the needs of many users. 

### Goals

- Abstract DAO layer for different databases
- Support MariaDB 10.5.9
- Support MySQL 8.0
- Keep backward compatibility for PostgreSQL
- Migrate from PostgreSQL to MariaDB/MySQL

### Overview

![overview.png](images/multidb/overview.png)

### Component Detail

**Core/Exporter/Notary** 

![logic.png](images/multidb/logic.png)

### Database Compatibility

**MariaDB 10.5.9**

sql file | Compatibility test | comment
------------|------------|------------
 | 0001_initial_schema.up.sql | Pass | xxx
 | 0002_1.7.0_schema.up.sql | Pass | xxx
 | 0003_add_replication_op_uuid.up.sql | Pass | xxx 
 | 0004_1.8.0_schema.up.sql | Pass | xxx 
 | 0005_1.8.2_schema.up.sql | Pass | xxx 
 | 0010_1.9.0_schema.up.sql | Pass | xxx 
 | 0011_1.9.1_schema.up.sql | Pass | xxx 
 | 0012_1.9.4_schema.up.sql | Pass | xxx 
 | 0015_1.10.0_schema.up.sql | Pass | xxx 
 | 0030_2.0.0_schema.up.sql | Pass | xxx 
 | 0031_2.0.3_schema.up.sql | Pass | xxx 
 | 0040_2.1.0_schema.up.sql | Pass | xxx 
 | 0041_2.1.4_schema.up.sql | Pass | xxx 
 | 0050_2.2.0_schema.up.sql | Pass | xxx 
 | 0051_2.2.1_schema.up.sql | Pass | xxx 
 | 0052_2.2.2_schema.up.sql | Pass | xxx 
 | 0053_2.2.3_schema.up.sql | Pass | xxx 
 | 0060_2.3.0_schema.up.sql | Pass | xxx 
 | 0061_2.3.4_schema.up.sql | Pass | xxx 
 | 0070_2.4.0_schema.up.sql | Pass | xxx 
 | 0071_2.4.2_schema.up.sql | Pass | xxx 
 | 0080_2.5.0_schema.up.sql | Pass | xxx 

**MySQL 8.0**

sql file | Compatibility test | comment
------------|------------|------------
 | 0001_initial_schema.up.sql | Pass | xxx
 | 0002_1.7.0_schema.up.sql | Pass | xxx
 | 0003_add_replication_op_uuid.up.sql | Pass | xxx 
 | 0004_1.8.0_schema.up.sql | Pass | xxx 
 | 0005_1.8.2_schema.up.sql | Pass | xxx 
 | 0010_1.9.0_schema.up.sql | Pass | xxx 
 | 0011_1.9.1_schema.up.sql | Pass | xxx 
 | 0012_1.9.4_schema.up.sql | Pass | xxx 
 | 0015_1.10.0_schema.up.sql | Pass | xxx 
 | 0030_2.0.0_schema.up.sql | Pass | xxx 
 | 0031_2.0.3_schema.up.sql | Pass | xxx 
 | 0040_2.1.0_schema.up.sql | Pass | xxx 
 | 0041_2.1.4_schema.up.sql | Pass | xxx 
 | 0050_2.2.0_schema.up.sql | Pass | xxx 
 | 0051_2.2.1_schema.up.sql | Pass | xxx 
 | 0052_2.2.2_schema.up.sql | Pass | xxx 
 | 0053_2.2.3_schema.up.sql | Pass | xxx 
 | 0060_2.3.0_schema.up.sql | Pass | xxx 
 | 0061_2.3.4_schema.up.sql | Pass | xxx 
 | 0070_2.4.0_schema.up.sql | Pass | xxx 
 | 0071_2.4.2_schema.up.sql | Pass | xxx 
 | 0080_2.5.0_schema.up.sql | Pass | xxx 


### Database Migration

Migrate from PostgreSQL to MariaDB/MySQL

Tool, xxxx

```
add sth about migration tool
```

### How To Use

* Users can configure database type to use mysql/mariadb in external_database mode, postgresql is used by default.

set harbor_db_type config under external_database and db type under notary db config:
```
# Uncomment external_database if using external database.
# external_database:
#   harbor:
#     # database type, default is postgresql, options include postgresql, mariadb and mysql
#     type: harbor_db_type
#     host: harbor_db_host
#     port: harbor_db_port
#     ...
#   notary_signer:
#     type: notary_signer_db_type
#     host: notary_signer_db_host
#     port: notary_signer_db_port
#     ...
#   notary_server:
#     type: notary_server_db_type
#     host: notary_server_db_host
#     port: notary_server_db_port
#     ...
```

## Non-Goals

* Support other databases, such as MongoDB.
* Implement Mysql/Mariadb operator for internal database case.

## Rationale

* Why not expand other DBs

  * mysql or mariaDB is more popular.
  * In addition, versions before 1.6 are using mysql, some user may want to stick to the old ways.
