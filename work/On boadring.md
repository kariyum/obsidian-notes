
### Schema Management in development
Schema Config CSV

[ref](https://cognira.atlassian.net/wiki/spaces/APP/pages/2541977603/Schema+Management+in+development#Schema-Config-CSV)
Table names are soft-coded and are replayed at runtime based on some schema configuration file (csv).

#### Schema types
A schema type is used to group similar tables in terms of business/functional purpose using keyspaces. For example, `configurations` schema types holds all the configuration-related tables.

We can regroup even further for example, `congira_configurations_keyspace` that will hold all Cognira specific configuration tables.

#### Schema type versioning
Currently just a placeholder. We work with one version which is latest for all schema types.

### Microservices and table columns
What tables each microservices is interacting with at runtime.

#### Table version
Specifies what each microservice will be using in terms of table version.
```scala
val table_name: String = s"${schema_config.keyspace(MetaData.configurations)}.${schema_config.tableName("tenant_configuration")}"
```
- `MetaData.configurations` corresponds to the **configurations** schema type.
- `schema_config.keyspace(schema_type)`   returns the keyspace for that schema_type. The mapping is based on the Kubernetes configmap named *cluster*. ==Do we assume that the schema type always has one unique keyspace? If so why not have the schema type used as the keyspace ? Then how can we have cognira_configurations and for example client_configurations keyspace==
- `schema_config.tableName(RAW_TABLE_NAME)` : returns the full tableName which is `RAW_TABLE_NAME + TABLE_VERSION` the table_version is taken from the schema config file.
- `schema_config` is a Class that reads the Schema Config CSV into a map.

### Cognira Development Process
[ref](https://cognira.atlassian.net/wiki/spaces/APP/pages/1316782081/Cognira+Development+Process)

#### ..Schema Management
[Schema manager repo](https://bitbucket.org/cognira/schema-manager/src/master/)

Any changes in the schema should be updated with a script : 
- Simple CQL commands
- PySpark ETL jobs:
	- Required to migrate schema non-empty tables
	- Occasional
**Schema Migration** can run any number of scripts in a single run in sequence, following a pre-defined alphabetical order. 
```
a-create-table.cql
b-insert-data.cql
```
