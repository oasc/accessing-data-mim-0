# Mechanisms

## M1 REST API with JSON Description

#### 1. Overview

This Mechanism defines how public sector data systems can expose data in an interoperable, machine-readable way using **REST APIs described in JSON via OpenAPI 3.1**. It fulfils all capabilities and requirements of MIM1 (Accessing Data) and is grounded in widely adopted open standards from IETF, OGC, and the OpenAPI Initiative.

The Mechanism is intentionally technology-neutral at the payload level: it mandates _how_ data is exposed and described, not _what_ the data model is (that is the domain of MIM2). It is suitable for a wide range of smart city data — sensor readings, mobility data, environmental monitoring, permit registries, events, and more.

***

#### 2. Core Standards Referenced

| Standard                                                       | Role in this Mechanism                                                                                                      |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **OpenAPI Specification 3.1** (OAI)                            | Formal machine-readable description of all API endpoints, parameters, request/response schemas, content types, and examples |
| **RFC 9110** – HTTP Semantics (IETF)                           | Foundation for all HTTP interactions: methods, status codes, content negotiation via `Accept` / `Content-Type` headers      |
| **RFC 9111** – HTTP Caching (IETF)                             | Cache-Control, ETag, Last-Modified headers for efficient data retrieval                                                     |
| **RFC 7807 / RFC 9457** – Problem Details for HTTP APIs (IETF) | Structured, machine-readable error responses                                                                                |
| **draft-ietf-httpapi-ratelimit-headers** (IETF HTTPAPI WG)     | `RateLimit`, `RateLimit-Policy` headers to communicate quota status to consumers                                            |
| **JSON Schema (2020-12)**                                      | Schema definitions for all request and response payloads, embedded in or referenced from the OpenAPI description            |
| **OGC SensorThings API Part 1** (OGC/ISO 15075-1)              | Recommended profile for IoT/sensor data streams with built-in subscription support via MQTT                                 |
| **OGC API – Features Part 1** (OGC/ISO 19168-1)                | Recommended profile for geospatial feature data with standardised `bbox`, `datetime`, and property filters                  |
| **AsyncAPI 3.0**                                               | Optional complementary description of subscription/notification channels (Webhooks, MQTT, SSE)                              |
| **RFC 8288** – Web Linking (IETF)                              | `Link` headers and hypermedia relations for pagination, self-describing resources                                           |

***

#### 3. Mechanism Description

**3.1 Data Retrieval over HTTP(S)**

All data resources are exposed as **HTTPS endpoints** following RESTful resource-oriented design. Each resource collection is addressable via a stable URI. Clients retrieve data using standard HTTP `GET` requests.

Supported response formats are negotiated using the HTTP `Accept` header (per RFC 9110), with the server indicating the chosen format via `Content-Type`. The Mechanism requires support for at least one machine-readable format; **JSON (`application/json`)** is the baseline, and implementations are encouraged to also support **GeoJSON (`application/geo+json`)**, **JSON-LD (`application/ld+json`)**, and optionally CSV or Parquet for bulk downloads.

Format negotiation can also be achieved via the `?f=` query parameter as a fallback for clients unable to control HTTP headers (following OGC API – Common practice).

**3.2 Formal API Description in JSON**

Every API instance conforming to this Mechanism MUST publish an **OpenAPI 3.1 description document**, accessible at a well-known path (e.g. `/api` or `/openapi.json`). This document:

* Lists all endpoints with their HTTP methods, path parameters, query parameters, and response schemas
* Declares all supported content types per endpoint using the OpenAPI `content` map
* Embeds or references **JSON Schema 2020-12** definitions for all payload structures
* Includes at least one worked example per endpoint (`examples` or `x-` extension fields)
* Documents authentication requirements using OpenAPI `securitySchemes` (see Section 3.5)

The OpenAPI document itself is the machine-readable contract (R1.3) and the payload schema pointer (R1.4).

**3.3 Structured and Queryable Interface**

All collection endpoints follow a uniform URL pattern:

```
GET /collections/{collectionId}/items
```

This pattern is borrowed from **OGC API – Features**, which has been adopted as ISO 19168-1, and ensures structural consistency across implementations from different vendors and cities.

Filtering is performed through standard query parameters, all documented in the OpenAPI description:

| Query Parameter                          | Purpose                                                     | Standard Basis                   |
| ---------------------------------------- | ----------------------------------------------------------- | -------------------------------- |
| `datetime`                               | Filter by time range or instant (`2024-01-01T00:00:00Z/..`) | OGC API – Features               |
| `bbox`                                   | Geospatial bounding box filter                              | OGC API – Features               |
| `properties` (or domain-specific params) | Attribute-level filtering                                   | OGC API – Features Part 3 / CQL2 |
| `limit` / `offset`                       | Pagination                                                  | OGC API – Common                 |
| `f`                                      | Format override                                             | OGC API – Common                 |

Responses include **RFC 8288 `Link` headers** with `rel=next`, `rel=prev`, `rel=self`, and `rel=alternate` to support pagination and format discovery without out-of-band knowledge.

For time-series or sensor data, the **OGC SensorThings API** resource model (Things → Datastreams → Observations) is the recommended profile, with identical query parameter conventions.

**3.4 Subscriptions and Change Notifications**

For use cases requiring near-real-time updates, the Mechanism supports three subscription patterns of increasing complexity:

**Polling (baseline fallback):** Clients repeatedly `GET` a collection endpoint. The API supports `If-None-Match` / `If-Modified-Since` headers (RFC 9111) to enable conditional requests, returning `304 Not Modified` when data has not changed, minimising bandwidth. An `X-Next-Update` header (or equivalent OpenAPI extension) communicates the next expected data refresh time.

**Webhooks:** Clients register a callback URL via a `POST /subscriptions` endpoint described in the OpenAPI document. When data changes, the server issues an HTTP `POST` to the registered URL with a change payload. The subscription resource and callback schema are described using **AsyncAPI 3.0** or as an OpenAPI extension.

**MQTT / SSE (for streaming/IoT):** When using the OGC SensorThings API profile, MQTT publish/subscribe is available as Part 1 already defines the binding. Server-Sent Events (`text/event-stream`) can be offered as a lightweight browser-friendly alternative.

All subscription endpoints and their payload schemas are formally described in the API description document, fulfilling R1.3 for the notification channel.

**3.5 Access Control**

When access restrictions apply, the Mechanism defers to **MIM3** (actor identification) and **MIM6** (authorisation and data asset-level policies). Concretely:

* Actor identification is achieved via **OAuth 2.0 / OpenID Connect** bearer tokens (`Authorization: Bearer …`), declared as an `openIdConnect` or `oauth2` security scheme in the OpenAPI description.
* Authorisation policies (role-based or attribute-based) are enforced at the API gateway or resource server level.
* The OpenAPI description documents which endpoints require which scopes, making the access model machine-readable and auditable.

**3.6 Operational Headers and Best Practices**

Implementations following this Mechanism SHOULD include the following HTTP response headers on all collection endpoints:

| Header                          | Purpose                                                        |
| ------------------------------- | -------------------------------------------------------------- |
| `Cache-Control`                 | Freshness and cacheability (RFC 9111)                          |
| `ETag` / `Last-Modified`        | Conditional request support (RFC 9111)                         |
| `RateLimit`, `RateLimit-Policy` | Quota transparency (IETF draft-ietf-httpapi-ratelimit-headers) |
| `X-Next-Update`                 | Next expected data refresh timestamp                           |
| `Link`                          | Pagination and alternate format links (RFC 8288)               |

Error responses MUST use the **RFC 9457 Problem Details** format (`application/problem+json`), providing `type`, `title`, `status`, `detail`, and optionally `instance` fields. This ensures errors are machine-parseable by generic API clients.

A `GET /health` or `GET /status` endpoint SHOULD be provided, returning a simple structured JSON document indicating service availability, API version, and data freshness.

**3.7 Why This Mechanism Is Interoperable**

This Mechanism achieves interoperability through several complementary layers:

**Syntactic interoperability** is achieved by mandating JSON as the baseline wire format and OpenAPI 3.1 as the formal description language. Any client capable of parsing an OpenAPI document can auto-discover endpoints, parameters, and schemas without prior bilateral agreement.

**Structural interoperability** is achieved through the uniform OGC API – Features / SensorThings resource and query model. A client written for one smart city data portal can query another without reconfiguration, because the endpoint pattern, filtering parameters, and pagination conventions are standardised.

**Semantic interoperability** is supported by the MIM2 data model reference mechanism: the OpenAPI description points to the schema definitions used, which can be JSON-LD contexts or domain ontologies. This Mechanism does not prescribe the model but ensures it is referenceable.

**Protocol interoperability** is guaranteed by grounding everything in IETF HTTP standards (RFC 9110, 9111), which are universally implemented across all languages, platforms, and API gateways.

**Discovery interoperability** is enabled by the well-known OpenAPI document path and the `Link` headers on every response. Aggregators and data catalogues (e.g. DCAT-AP conformant portals) can crawl and index APIs without custom connectors.

***

#### 4. Requirements Mapping Table

| Req. ID                 | Requirement Text                                                                                                                          | How This Mechanism Implements It                                                                                                                                                                                                                                                                     |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **R1.1**                | Systems SHALL allow retrieval of data in at least one machine-readable format. Formats can be specified through HTTP Content-Negotiation. | JSON (`application/json`) is the mandatory baseline format. Additional formats (GeoJSON, JSON-LD, CSV) are optionally supported. Format selection uses the HTTP `Accept` header per RFC 9110, or the `?f=` query parameter as a fallback.                                                            |
| **R1.2**                | Data SHALL be retrievable via at least one standard web-based mechanism.                                                                  | Data is retrieved via HTTPS `GET` requests on standard URI-addressed collection endpoints following RESTful conventions and the OGC API – Features URL pattern (ISO 19168-1).                                                                                                                        |
| **R1.3**                | Access mechanism(s) SHALL be formally described.                                                                                          | A machine-readable **OpenAPI 3.1** description document is published at a well-known URL (`/api` or `/openapi.json`). It formally describes all endpoints, methods, parameters, schemas, content types, security, and examples. Subscription channels are additionally described using AsyncAPI 3.0. |
| **R1.4**                | Data Models used by payloads should be specified (see MIM2).                                                                              | The OpenAPI description references JSON Schema 2020-12 definitions for all request and response payloads. These schemas can in turn reference external MIM2-conformant data models via `$ref` or JSON-LD `@context`.                                                                                 |
| **R2.1**                | Systems SHALL provide data through a structured and consistent interface.                                                                 | All collection resources follow the uniform OGC API – Features pattern (`/collections/{id}/items`). Pagination, filtering, and response envelope structures are consistent across all endpoints and documented in the shared OpenAPI description.                                                    |
| **R2.2**                | Systems SHALL support basic querying and/or filtering (e.g. by time, location, attributes).                                               | Filtering is supported via standardised query parameters: `datetime` for temporal filtering, `bbox` for geospatial filtering, and property-level filters (OGC API – Features Part 3 / CQL2). All parameters are documented in the OpenAPI description.                                               |
| **R3.1**                | Systems for use cases that require notifications SHALL provide at least one mechanism to receive updates on data changes.                 | At minimum, polling with conditional HTTP requests (`If-None-Match`, `If-Modified-Since`, `304 Not Modified`) is supported. Implementations with real-time requirements provide Webhooks (POST callbacks) and/or MQTT (via OGC SensorThings API profile).                                            |
| **R3.2**                | Systems MAY support multiple subscription mechanisms depending on use case.                                                               | The Mechanism defines three tiers: (1) conditional polling as a universal baseline, (2) Webhooks for event-driven consumers, and (3) MQTT or Server-Sent Events for streaming/IoT use cases. All are described in the API description document.                                                      |
| **R4 (MIM3/MIM6 ref.)** | Access restrictions SHALL follow MIM3 actor identification and MIM6 authorisation requirements.                                           | OAuth 2.0 / OpenID Connect bearer tokens are used for actor identification. Authorisation scopes are declared in the OpenAPI `securitySchemes` section, making the access policy machine-readable. Data-asset-level policies are enforced at the API gateway level in conformance with MIM6.         |

