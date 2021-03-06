# Quickstart memsql-kafka-connector guide

This guide will show basic functionality of `memsql-kafka-connector`

## Requirements

* docker

## Set up environment

To be able to run the setup script, you must first export your MemSQL license key as follows:

```
export LICENSE_KEY=<memsql_license_key>
```

Then run the setup script:

```
./setup-script.sh
```

This script will start all the required components: 
* zookeeper 
* kafka 
* schema-registry 
* kafka-rest
* kafka-connect
* memsql

Then the kafka-connect job will be launched with this configuration:

```
{
    "name": "memsql-sink-connector",
    "config": {
        "connector.class":"com.memsql.kafka.MemSQLSinkConnector",
        "tasks.max":"1",
        "topics":"memsql_json_songs",
        "connection.ddlEndpoint" : "memsql-kafka:3306",
        "connection.database" : "test",
        "connection.user" : "root"
    }
 }
```

This job will read `memsql_json_songs` topic 
and then write all records to MemSQL `test.memsql_json_songs` table

## Ingest data

To ingest some data to `memsql_json_songs` topic you can execute `ingest-data.sh` script, 
which will add some sample data to kafka.

```
./ingest-data.sh
```

After that, you will see that the data has been added to the database.

```
docker exec -it memsql-kafka bash
memsql

use test;
show tables;
select * from memsql_json_songs;
```