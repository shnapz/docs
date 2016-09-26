# Topics

This folder contains requirements for topic-related operations available from KWC

## Status Codes

* **404 Not Found**
    * Error code 40401 – Topic not found.
    * Error code 40402 – Partition not found.
* **422 Unprocessable Entity** – The request payload is either improperly formatted or contains semantic errors
* **500 Internal Server Error** –
    * Error code 50001 – Zookeeper error.
    * Error code 50002 – Kafka error.
    * Error code 50003 – Retriable Kafka error. Although the operation failed, it’s possible that retrying the request will be successful.
    * Error code 50101 – Only SSL endpoints were found for the specified broker, but SSL is not supported for the invoked API yet.

## List topics

> GET /topics

Get a list of Kafka topics. 

##### Response JSON object:

* topics (array)
* name (string) - name of the topic
* partitions (int) - number of partitions
* replicationFactor (int) - number of replicas

##### Example request:

```javascript
GET /topics HTTP/1.1 
```

##### Example response:

```javascript
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "name": "test2",
        "partitions": 2,
        "replicationFactor": 1
    },
    {
        "name": "test3",
        "partitions": 3,
        "replicationFactor": 1
    },
    {
        "name": "test1",
        "partitions": 1,
        "replicationFactor": 1
    }
]
```

## Topic Details

> GET /topics/(topic_name: string)

Get metadata about a specific topic.

##### Parameters:

* topic_name (string) – name of the topic to get metadata about

##### Response JSON object:

* name (string) – Name of the topic
* configs (map) – Per-topic configuration overrides
* partitions (array) – List of partitions for this topic
* partitions[i].partition (int) – the ID of this partition
* partitions[i].leader (int) – the broker ID of the leader for this partition
* partitions[i].replicas (array) – list of replicas for this partition, including the leader
* partitions[i].replicas[j].broker (array) – broker ID of the replica
* partitions[i].replicas[j].leader (boolean) – true if this replica is the leader for the partition
* partitions[i].replicas[j].in_sync (boolean) – true if this replica is currently in sync with the leader

##### Status Codes:

*404 Not Found –
   * Error code 40401 – Topic not found

##### Example request:

```javascript
GET /topics/test HTTP/1.1
```

##### Example response:

```javascript
HTTP/1.1 200 OK
Content-Type: application/json

{
  "name": "test",
  "configs": {
     "cleanup.policy": "compact"
  },
  "partitions": [
    {
      "partition": 1,
      "leader": 1,
      "replicas": [
        {
          "broker": 1,
          "leader": true,
          "in_sync": true,
        },
        {
          "broker": 2,
          "leader": false,
          "in_sync": true,
        }
      ]
    },
    {
      "partition": 2,
      "leader": 2,
      "replicas": [
        {
          "broker": 1,
          "leader": false,
          "in_sync": true,
        },
        {
          "broker": 2,
          "leader": true,
          "in_sync": true,
        }
      ]
    }
  ]
}
```

## List Topic Partitions

> GET /topics/(string: topic_name)/partitions

Get a list of partitions for the topic.

##### Parameters:

* topic_name (string) – name of the topic

##### Response JSON object:

* partitions[i].partition (int) – the ID of this partition
* partitions[i].leader (int) – the broker ID of the leader for this partition
* partitions[i].replicas (array) – list of replicas for this partition, including the leader
* partitions[i].replicas[j].broker (array) – broker ID of the replica
* partitions[i].replicas[j].leader (boolean) – true if this replica is the leader for the partition
* partitions[i].replicas[j].in_sync (boolean) – true if this replica is currently in sync with the leader

##### Status Codes:

*404 Not Found –
   * Error code 40401 – Topic not found
   
##### Example request:

```javascript
GET /topics/test/partitions HTTP/1.1
```
##### Example response:

```javascript
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
      "partition": 1,
      "leader": 1,
      "replicas": [
        {
          "broker": 1,
          "leader": true,
          "in_sync": true,
        },
        {
          "broker": 2,
          "leader": false,
          "in_sync": true,
        }
      ]
    },
    {
      "partition": 2,
      "leader": 2,
      "replicas": [
        {
          "broker": 1,
          "leader": false,
          "in_sync": true,
        },
        {
          "broker": 2,
          "leader": true,
          "in_sync": true,
        }
      ]
    }
]
```

## Get Topic Partition Details

> GET /topics/(string: topic_name)/partitions/(int: partition_id)

Get metadata about a single partition in the topic.

##### Parameters:

* topic_name (string) – name of the topic
* partition_id (int) – ID of the partition to inspect

##### Response JSON object:

* partitions[i].leader (int) – the broker ID of the leader for this partition
* partitions[i].replicas (array) – list of replicas for this partition, including the leader
* partitions[i].replicas[j].broker (array) – broker ID of the replica
* partitions[i].replicas[j].leader (boolean) – true if this replica is the leader for the partition
* partitions[i].replicas[j].in_sync (boolean) – true if this replica is currently in sync with the leader

##### Status Codes:

*404 Not Found –
   * Error code 40401 – Topic not found
   
##### Example request:

```javascript
GET /topics/test/partitions/1 HTTP/1.1
```
##### Example response:

```javascript
HTTP/1.1 200 OK
Content-Type: application/json

 {
   "partition": 1,
   "leader": 1,
   "replicas": [
     {
       "broker": 1,
       "leader": true,
       "in_sync": true,
     },
     {
       "broker": 2,
       "leader": false,
       "in_sync": true,
     }
   ]
 }
```
