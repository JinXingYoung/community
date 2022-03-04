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

### Goals

- Abstract DAO layer for different databases
- Support MariaDB xxx
- Support MySQL xxx
- Keep backward compacitiy for PostgreSQL
- Migrate from PostgreSQL to MariaDB/MySQL

In order to support different DB to harbor we propose support mariadb and mysql in Harbor which can meet the needs of many users. 

Verify compatibility based on versions mysql 8.0 and mariadb 10.5.9.

### Overview


```
todo add diagram(组建X数据库总体图)
```

### Component Detail

**Core**
```
core Database 代码逻辑图
(initDatabase 分支走向，
某些业务sql语句不兼容的则判断database driver适配sql语句)
```

**Exporter**

```
exporter database 代码逻辑图
(initDatabase 分支走向，
某些业务sql语句不兼容的则判断database driver适配sql语句)
```

**Notary**
```
图(根据数据库类型使用不同配置文件启动notary)
```

### Database Compatibility

**MariaDB xxxx**
f1 Pass xxx
f2 Pass xxx
f3 Pass xxx

**MySQL xxxxx**
f1 Pass xxx
f2 Pass xxx
f3 Pass xxx

### Database Migration

Migrate from PostgreSQL to MariaDB/MySQL

Tool, xxxx

* Expand DB support for mysql and mariaDB, compatible with the PostgreSQL. User can chose the suitable DB to use.

- [Problems solved for compatibility](https://docs.qq.com/doc/DVVV2VHV4eHNNbmdi)

* Provide data migration documentation and tool to migrate from PostgreSQL to Mysql/mariaDB in same harbor version.

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
