# Data Sources Options and Requirements

Redash supports several types of data sources, and if you set it up using the provided images, it should already have the needed dependencies to use them all. Starting from version 0.7 and newer, you can manage data sources from the UI by browsing to `/data_sources` on your instance.

If one of the listed data source types isn’t available when trying to create a new data source, make sure that:

1. You installed required dependencies.
2. If you’ve set custom value for the `REDASH_ENABLED_QUERY_RUNNERS` setting, it’s included in the list.

## PostgreSQL / Redshift / Greenplum

* Options:
  * Database name (mandatory)
  * User
  * Password
  * Host
  * Port

* Additional requirements:
  * None


## MySQL

* Options:
  * Database name (mandatory)
  * User
  * Password
  * Host
  * Port

* Additional requirements:
  * `MySQL-python` python package


## Google BigQuery

* Options:

  * Project ID (mandatory)

  * JSON key file, generated when creating a service account (see [instructions](https://developers.google.com/identity/protocols/OAuth2ServiceAccount#creatinganaccount)).

  * UDF Source URIs (see [more details](https://cloud.google.com/bigquery/user-defined-functions#api) )
    * References to JavaScript source files stored in Google Cloud Storage (comma separated)


* Additional requirements:

  * `google-api-python-client`, `oauth2client` and `pyopenssl` python packages (on Ubuntu it might require installing `libffi-dev` and `libssl-dev` as well).


## Graphite

* Options:
  * URL (mandatory)
  * User
  * Password
  * Verify SSL certificate


## MongoDB

* Options:
  * Connection String (mandatory)
  * Database name
  * Replica set name

* Additional requirements:
  * `pymongo` python package.


For information on how to write MongoDB queries, see [_documentation_](user-guide/queries/querying_mongodb.md).

## ElasticSearch

For information on how to write ElasticSearch queries, see [_documentation_](user-guide/queries/querying_elasticsearch.md).

## InfluxDB

* Options:

  > * Url
  >   * String of format `influxdb://username:password@localhost:8086/databasename`

* Additional requirements:

  * `influxdb` python package.


## Presto

* Options:

  > * Host (mandatory)
  >   * Address to a Presto coordinator.
  >
  > * Port
  >   * Port to a Presto coordinator. 8080 is the default port.
  >
  > * Schema
  >   * Default schema name of Presto. You can read other schemas by qualified name like FROM myschema.table1.
  >
  > * Catalog
  >   * Catalog (connector) name of Presto such as hive-cdh4, hive-hadoop1, etc.
  >
  > * Username
  >   * User name to connect to a Presto.

* Additional requirements:

  * `pyhive` python package.


## Hive

...

## Impala

...

## URL

A URL based data source which requests URLs that return the [_results JSON format_](how-rd-works/data-source-results-format.md)

Very useful in situations where you want to expose the data without connecting directly to the database.

The query itself inside Redash will simply contain the URL to be executed (i.e. http://myserver/path/myquery)

* Options:
  * Url - set this if you want to limit queries to certain base path.


## Google Spreadsheets

* Options:
  * JSON key file, generated when creating a service account (see [instructions](https://developers.google.com/identity/protocols/OAuth2ServiceAccount#creatinganaccount)).

* Additional requirements:
  * `gspread` and `oauth2client` python packages.


Notes:

1. To be able to load the spreadsheet in Redash - share your it with your ServiceAccount’s email (it can be found in the credentials json file, for example 43242343247-fjdfakljr3r2@developer.gserviceaccount.com).
2. The query format is “DOC_UUID|SHEET_NUM” (for example “kjsdfhkjh4rsEFSDFEWR232jkddsfh|0”)
3. Alternatively, one can create a new Google BigQuery table using the Google Spreadsheet in question as a source, and then use Redash’s BigQuery connector to query the spreadsheet indirectly. This way, the SQL used to query the spreadsheet (via BigQuery table) is far more flexible than the direct query of the type (“kjsdfhkjh4rsEFSDFEWR232jkddsfh|0”) mentioned above. ([BigQuery integrates with Google Drive](https://cloud.google.com/blog/big-data/2016/05/bigquery-integrates-with-google-drive)).

## Python

Execute other queries, manipulate and compute with Python code

This is a special query runner, that will execute provided Python code as the query. Useful for various scenarios such as merging data from different data sources, doing data transformation/manipulation that isn’t trivial with SQL, merging with remote data or using data analysis libraries such as Pandas (see [example query](https://gist.github.com/arikfr/be7c2888520c44cf4f0f)).

While the Python query runner uses a sandbox (RestrictedPython), it’s not 100% secure and the security depends on the modules you allow to import. We recommend enabling the Python query runner only in a trusted environment (meaning: behind VPN and with users you trust).

* Options:
  * Allowed Modules in a comma separated list (optional). NOTE: You MUST make sure these modules are installed on the machine running the Celery workers.


Notes:

* For security, the python query runner is disabled by default. To enable, add `redash.query_runner.python` to the `REDASH_ADDITIONAL_QUERY_RUNNERS` environmental variable. If you used the bootstrap script, or one of the provided images, add to `/opt/redash/.env` file the line: `export REDASH_ADDITIONAL_QUERY_RUNNERS=redash.query_runner.python`.

## Vertica

* Options:
  * Database (mandatory)
  * User
  * Password
  * Host
  * Port

* Additional requirements:
  * `vertica-python` python package


## Oracle

* Options

  > * DSN Service name
  > * User
  > * Password
  > * Host
  > * Port

* Additional requirements

  * `cx_Oracle` python package. This requires the installation of the Oracle [instant client](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html).


## Treasure Data

* Options

  > * Type (TreasureData)
  > * API Key
  > * Database Name
  > * Type (Presto/Hive[default])

* Additional requirements
  * Must have account on [https://console.treasuredata.com](https://console.treasuredata.com/)


Documentation: [https://docs.treasuredata.com/articles/redash](https://docs.treasuredata.com/articles/redash)

## Microsoft SQL Server

* Options:

  * Database (mandatory)
  * User #TODO: DB users only? What about domain users?
  * Password
  * Server
  * Port

* Notes:

  * Data type support is currently quite limited.

  * Complex and new types are converted to strings in `Redash`
    * Coerce into simpler types if needed using `CAST()`

  * Known conversion issues for:
    * DATE
    * TIME
    * DATETIMEOFFSET


* Additional requirements:

  * `freetds-dev` C library
  * `pymssql` python package, requires FreeTDS to be installed first


## JIRA (JQL)

* Options:

  > * URL (your JIRA instance url)
  > * Username
  > * Password


For information on how to write JIRA/JQL queries, click [here](user-guide/queries/querying_jira_jql.md).