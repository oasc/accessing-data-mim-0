<!--
# SPDX-License-Identifier: CC0-1.0
# SPDX-FileCopyrightText: Authors
-->

# Capabilities and requirements

## Terminology

> The key words MAY, MUST, MUST NOT, and SHOULD in this document are to be interpreted as described in [BCP 14](https://www.rfc-editor.org/info/bcp14), [[RFC2119](https://www.rfc-editor.org/rfc/rfc2119)], [[RFC8174](https://www.rfc-editor.org/rfc/rfc8174)] when, and only when, they appear in all capitals, as shown here. 

## C.1 Modalities for Accessing Data

Public sector data systems should support a variety of standardized methods for accessing data, including downloads, subscriptions, streams, and real-time feeds, to ensure flexibility and usability for different types of users and use cases.

### Minimal interoperability:

Data **MUST** be retrievable via at least one standard web-based mechanism  
Systems **MUST** allow retrieval of data in at least one machine-readable format  

```
### Additional best practice to consider: 

Data **SHOULD** be available in common downloadable formats (see MIM3)
Systems **SHOULD** support bulk data access or export  
Systems **SHOULD** provide at least one subscription mechanism  
Systems **SHOULD** provide real-time data streaming when appropriate  
Time series datasets **SHOULD** allow querying over specified time ranges  
Systems **MAY** support access via message queue systems  
```

## C.2 API Interface and Behavior

APIs provided for accessing data should be well-structured, documented, and adhere to best practices for interface design and behavior, enabling consistent, predictable, and interoperable integration across systems.

### Minimal interoperability:

APIs **MUST** be documented using a machine-readable specification  
APIs **MUST** support standard content negotiation  
APIs **MUST** include metadata for last modified timestamp, available data formats, pagination, and rate limits  
APIs **MUST** return appropriate HTTP status codes and error messages  
APIs **MUST** follow RESTful URL conventions with predictable endpoint structures

```
### Additional best practice to consider: 

APIs **SHOULD** support retrieval of current data  
APIs **SHOULD** support retrieval of historical data when applicable  
APIs **SHOULD** support geospatial querying when applicable (see MIM7)  
APIs **SHOULD** support subscription to changes when applicable  
APIs **SHOULD** expose next expected update timestamp  
APIs **SHOULD** support explicit versioning of endpoints  
APIs **SHOULD** provide example payloads or test queries  
APIs **SHOULD** support standard HTTP caching headers  
APIs **SHOULD** communicate rate limit status via standard HTTP headers  
APIs **SHOULD** return structured error bodies  
APIs **MAY** support partial responses or query projections  
APIs **MAY** expose a standard health/status endpoint  
APIs **MAY** support pagination for datasets exceeding 1000 records
```

## C.3 Direct File Download Capability

Systems should provide direct file download access as a simple, universal method for data retrieval that works without API complexity and enables bulk data access.

### Minimal interoperability:

In situations where API access is not possible:

Data **MUST** be available for direct download in at least one structured format (CSV, JSON, or XML)  
Download URLs **MUST** be stable and predictable  

```
### Additional best practice to consider: 

Downloaded files **SHOULD** include metadata headers or companion files  
Systems **SHOULD** provide both full and incremental data exports
```

## C.4 Publish & Subscribe Mechanisms

### C.4.1 Message Queue Integration Capability

Support asynchronous data delivery through standardized messaging protocols for reliable, high-throughput integration between systems.

### Minimal interoperability:

Systems **MUST** implement at least one standard messaging protocol (AMQP, Apache Kafka, or MQTT) 
Systems **MUST** support structured message formats (see MIM1) 
Systems **MUST** provide message acknowledgment mechanisms  
Systems **MUST** ensure at-least-once delivery guarantees  
Systems **MUST** implement durable message storage for critical data  

```
### Additional best practice to consider:  

Systems **SHOULD** support topic-based message routing  
Systems **SHOULD** provide message ordering guarantees within partitions/topics  
Systems **SHOULD** implement configurable retention policies  
Systems **SHOULD** support batch message processing  
Systems **SHOULD** provide monitoring and metrics for queue health  
Systems **MAY** support exactly-once delivery semantics  
Systems **MAY** implement message filtering or routing rules  
Systems **MAY** support multiple consumer groups  
```

### C.4.2 Real-Time Data Streaming Capability

Enable continuous data delivery through standardized streaming protocols for applications requiring up-to-date information.

### Minimal interoperability:

Systems **MUST** support at least one streaming protocol (WebSockets, Server-Sent Events, or HTTP/2 streaming)  
Systems **MUST** send structured data payloads (JSON format minimum)  
Systems **MUST** include timestamps and sequence identifiers in stream messages  
Systems **MUST** provide connection heartbeat or keep-alive mechanisms  
Systems **MUST** handle client disconnections gracefully with reconnection support  

```
### Additional best practice to consider: 

Systems **SHOULD** support client-side filtering or subscription parameters  
Systems **SHOULD** implement backpressure handling for slow consumers  
Systems **SHOULD** provide stream replay capability for recent data  
Systems **SHOULD** support multiple concurrent streaming connections  
Systems **MAY** support compression for high-volume streams  
Systems **MAY** implement stream checkpointing for reliability
```

## C.5 Meta data 

### Minimal interoperability:

Systems **MUST** provide consistent and clear metadata accompanying datasets  
Metadata **MUST** be available in a machine-readable format  

## C.6 Reliability and quality when accessing data

### Minimal interoperability:

API acccess MUST include a Service License Agreement (see MIM3)

