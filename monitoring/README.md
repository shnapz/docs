# Monitoring

Kafka Monitoring API is aimed to provide full set of Kafka components operational data exposed through JMX interface.

The Monitoring service interrogates all Kafka related MBeans meta information and stores it in internal state.

The object describing this meta information SHALL contain the following fields (but not limited to):
  - domain, e.g. kafka.server
  - type, e.g. ReplicaManager
  - [Optional] name, e.g. UnderReplicatedPartitions
  - [Optional] client-id
  - [Optional] node-id
  - [Optional] topic
  - attributes, e.g. Count, Value

Monitoring API URL Construction format:

  /api/monitoring
  /api/monitoring/{domain}
  /api/monitoring/{domain}/{type}
  /api/monitoring/{domain}/{type}?name={name}
  /api/monitoring/{domain}/{type}?name={name}&attributes={attributes}
  /api/monitoring/{domain}/{type}?client-id={client-id}
  /api/monitoring/{domain}/{type}?client-id={client-id}&attributes={attributes}
  /api/monitoring/{domain}/{type}?client-id={client-id}&topic={topic}
  /api/monitoring/{domain}/{type}?client-id={client-id}&topic={topic}&attributes={attributes}
  /api/monitoring/{domain}/{type}?client-id={client-id}&node-id={node-id}
  /api/monitoring/{domain}/{type}?client-id={client-id}&node-id={node-id}&attributes={attributes}

Supported Media Types:
  application/json

Responses:
  200 (OK) - returned by each endpoint on successful match of requested metric(s). May return single object or array of metric objects. In case of array each object contains attached meta info.
  
  400 (Bad Request) - returned in case API request match found but, for example, requested attributes list doesn't match to the one Metric provides.
  
  401 (Unauthorized) - returned in case request Authorization header is not provided
  
  403 (Forbidden) - returned in case request Authorization header is provided but doesn't have permissions for monitoring operations
  
  404 (Not Found) - returned if API request match not found
  
  

