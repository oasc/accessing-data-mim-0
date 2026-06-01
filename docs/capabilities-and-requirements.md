# Capabilities and Requirements

## Terminology

> The key words MAY, MUST, MUST NOT, and SHOULD in this document are to be interpreted as described when, and only when, they appear in all capitals, as follows:
>
> * "SHALL" indicates a requirement
> * "SHOULD" indicates a recommendation
> * "MAY" is used to indicate that something is permitted
> * "CAN" is used to indicate that something is possible, for example, that an organization or individual is able to do something

## C1 Machine-readable data is retrievable through the web

Public sector data systems should support a variety of standardized methods for accessing data, including downloads, subscriptions, streams, and real-time feeds, to ensure flexibility and usability for different types of users, types of data and use cases.

### Requirements:

* **R1.1** Systems SHALL allow retrieval of data in at least one machine-readable format. Formats can be specified through HTTP Content-Negotiation
* **R1.2** Data SHALL be retrievable via at least one standard web-based mechanism
* **R1.3** Access mechanism(s) SHALL be formally described
* **R1.4** Data Models used by payloads should be specified (see MIM2)

{% hint style="info" %}
_Examples_

* REST APIs over HTTP(S) endpoints described with OpenAPI
* Payloads are machine readable  (CSV, JSON, XML, Parquet, Protobuf …)
{% endhint %}

## C2 Access is structured and queryable

Data access should be based on a uniform structure and support querying and filtering, enabling efficient reuse and integration.

### Requirements:

* **R2.1** Systems SHALL provide data through a structured and consistent interface
* **R2.2** Systems SHALL support basic querying and/or filtering (e.g. by time, location, attributes)

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

## C3 Changes in data can be subscribed to

Use cases that require notifications or signals (such as real-time data) should allow consumers to subscribe to changes in data, enabling timely and efficient data exchange without continuous polling.

### Requirements:

* **R3.1** Systems for use cases that require notifications SHALL provide at least one mechanism to receive updates on data changes
* **R3.2** Systems MAY support multiple subscription mechanisms depending on use case

{% hint style="info" %}
Examples

* _Subscription/notification APIs_
* _Message queues (e.g. Kafka, RabbitMQ)_
* _Webhooks_
* _Polling (as a basic fallback)_
{% endhint %}

## C4 Access to information can be restricted&#x20;

If such restrictions are in place, they SHALL follow the requirements defined in MIM3 and MIM6, specifically:

* actor identification (MIM3.C5.R0),&#x20;
* authorization and actor roles (MIM6.C1.R4) &#x20;
* data asset-level policies (MIM6.C1.R4)&#x20;

<br>
