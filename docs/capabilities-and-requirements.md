# Capabilities and Requirements

## Terminology

> The key words MAY, MUST, MUST NOT, and SHOULD in this document are to be interpreted as described in [BCP 14](https://www.rfc-editor.org/info/bcp14), \[[RFC2119](https://www.rfc-editor.org/rfc/rfc2119)], \[[RFC8174](https://www.rfc-editor.org/rfc/rfc8174)] when, and only when, they appear in all capitals, as shown here.

{% hint style="warning" %}
This terminology is to be updated to ISO-style naming conventions:

* "shall" indicates a requirement
* "should" indicates a recommendation
* "may" is used to indicate that something is permitted
* "can" is used to indicate that something is possible, for example, that an organization or individual is able to do something
{% endhint %}

## C.1 Machine-readable data is retrievable through the web

Public sector data systems should support a variety of standardized methods for accessing data, including downloads, subscriptions, streams, and real-time feeds, to ensure flexibility and usability for different types of users, types of data and use cases.

### Minimal interoperability:

* R1. Systems MUST allow retrieval of data in at least one machine-readable format. Formats can be specified through HTTP Content-Negotiation
* R2. Data MUST be retrievable via at least one standard web-based mechanism
* R3. Access mechanism(s) MUST be formally described
* R4. Data Models used by payloads should be specified (see MIM2)

{% hint style="info" %}
_Examples_

* REST APIs over HTTP(S) endpoints described with OpenAPI
* Payloads are machine readable  (CSV, JSON, XML, Parquet, Protobuf …)
{% endhint %}

## C.2 Access is structured and queryable

Data access should be based on a uniform structure and support querying and filtering, enabling efficient reuse and integration.

### Minimal interoperability:

* Systems MUST provide data through a structured and consistent interface
* Systems MUST support basic querying and/or filtering (e.g. by time, location, attributes)

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
```

{% hint style="info" %}
_Examples_

* RESTful APIs with query parameters (e.g. NGSI-LD)
* Metadata-enabled filtering
{% endhint %}

## C.3 Changes in data can be subscribed to

Use cases that require notifications or signals (such as real-time data) should allow consumers to subscribe to changes in data, enabling timely and efficient data exchange without continuous polling.

### Minimal interoperability:

* Systems for use cases that require notifications SHALL provide at least one mechanism to receive updates on data changes
* Systems MAY support multiple mechanisms depending on use case

{% hint style="info" %}
Examples

* _Subscription/notification APIs_
* _Message queues (e.g. Kafka, RabbitMQ)_
* _Webhooks_
* _Polling (as a basic fallback)_
{% endhint %}

Link to licenses MIM3
