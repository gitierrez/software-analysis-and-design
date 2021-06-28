# REST API Design

Designing consistent RESTful web service interfaces.

# 1. Architectural Style of the Web

The scalability of the web's scalability is governed by a set of key constraints, called the *Web's architectural style*:

1. Client-server
2. Uniform interface
3. Layered system
4. Cache
5. Stateless

## 1.1. Client-Server

The web is a client-server based system, in which clients and servers have distinct parts to play and may be implemented and deployed independently.

## 1.2. Uniform Interface

The interactions between clients and servers depend on the uniformity of their interfaces. This interface defines four constraints:

1. **Identification of resources**: each distinct web resource may be addressed by a unique identifier, such as URI.
2. **Manipulation of resources through representations**: clients manipulate representations of resources (HTML, JSON), not the resource itself.
3. **Self-descriptive messages**: *metadata* to convey additional details regarding the resource state, the representation format and size, and the message itself. An HTTP message provides *headers* to organize the various types of metadata into uniform fields.
4. **Hypermedia as the engine of application state**: a resource's state representation includes links to related resources.

## 1.3. Layered System

The layered system constraints enable network-based intermediaries such as *proxies* and *gateways* to be transparently deployed between a client and a server.

## 1.4. Cache

Cache constraints instruct a web server to declare the *cacheability* of each response's data. This can help to reduce client-perceived latency, increase the overall availability and reliability of an application, and control a web server's load. It reduces the overall cost of the web.

A cache may exist anywhere along the network path between the client and the server. They can be in an organization's web server network, within specialized content delivery networks, or inside a client iself.

## 1.5. Stateless

A web server is not required to memorize the state of its client applications. As a result, each client must include all of the contextual information that it considers relevant in each interaction with the web server.

Web servers ask clients to manage the complexity of communicating their application state so that the web server can service a much larger number of clients.

---

The rest of this document contains the rules for designing RESTful web services.

# 2. Identifier Design with URIs

## 2.1. URI Format

The rules presented in this section pertain to the *format* of a URI. RFC 3986 defines the generic URI syntax as:

`URI` = `scheme`://`authority`/`path`[?`query`][#`fragment`]

### 2.1.1. / must be used to indicate a hierarchical structure.

`http://api.canvas.restapi.org/shapes/polygons/quadrilaterals/squares`

### 2.1.2. Trailing / must should not be included in URIs

As the last character within a URI's path, a forward slash (/) adds no semantic value and may cause confusion.

Instead of `http://api.canvas.restapi.org/shapes/` use `http://api.canvas.restapi.org/shapes`

### 2.1.3. Hyphens should be used to improve the readability of URIs

`http://api.example.restapi.org/blogs/mark-masse/entries/this-is-my-first-post`

### 2.1.4. Underscores should not be used in URIs

Use hyphens instead of underscores.

### 2.1.5. Lowercase letters should be preferred in URI paths

URIs are case-sensitive except for the `scheme` and `host` components. Avoid uppercase when possible.

Instead of `HTTP://API.EXAMPLE.RESTAPI.ORG/My-folder/My-Doc` use `http://api.example.restapi.org/my-folder/my-doc`

### 2.1.6. File extensions should not be included in URIs

The period character is commonly used to separate the file name and extension portions of a URI. A REST API should not include *artificial* file extensions in URIs to indicate the format of a message's entity body. Instead, they should rely on the media type communicated through the `Content-Type` header.

Avoid URIs like `http://api.college.restapi.org/students/transcripts/2005/fall.json`

## 2.2. URI Authority Design

This section covers the naming conventions that should be used for the authority portion of a REST API.

### 2.2.1. Consistent subdomain names should be used for your APIs.

The top-level domain and first subdomain names should identify its service owner, e.g. `foursquare.com`. Additionally, it should be preprended with `api`, e.g. `https://api.foursquare.com`

### 2.2.2. Consistent subdomain names should be used for your client developer portal.

Many REST APIs have an associated website, known as a *developer portal*, to help onboard new clients with documentation, forums, and self-service provisioning of secure API access keys. If an API provides a developer portal, by convention it should have a subdomain labeled `developer`. For example:

`https://developer.foursquare.com/docs`

## 2.3. Resource Modeling

The URI path conveys a REST API's resource model, with each forward slash separated path segment corresponding to a unique resource within the model's hierarchy. For example, this URI design:

`https://api.soccer.restapi.org/leagues/seattle`

indicates that each of these URIs should also identify an addressable resource:

- `https://api.soccer.restapi.org/leagues`
- `https://api.soccer.restapi.org`

Resource modeling is an exercise that establishes your API's key concepts. This process is similar to the data modeling for a relational database schema, or the classical modeling of an object-oriented system.

### 2.3.1. Resource Archetypes

When modeling API resources, we can start with some basic resource archetypes, analogous to design patterns.

#### 2.3.1.1. Document

A document resource is a concept akin to an object instance or database record. A document's state representation typically includes both *fields* with values and *links* to other related resources.

The *document* is the conceptual *base archetype* of the other resource archetypes. In other words, the three other resource archetypes can be viewed as specializations of the document archetype.

Example: `http://api.soccer.restapi.org/leagues/seattle`

#### 2.3.1.2. Collection

A collection resource is a server-managed *directory* of resources. Clients may propose new resources to be added to a collection. A collection resource chooses what it wawnts to contain and also decides the URIs of each contained resource.

Example: `http://api.soccer.restapi.org/leagues`

#### 2.3.1.3. Store

A store is a client-managed resource repository. A store resource lets an API client put resources in, get them back out, and decide when to delete them.

Stores do not create new resources. Instead, each stored resource has a URI that was chosen by a client when it was initially put into the store.

For example, a user of a client program can use the REST API to insert a document resource named *alonso* in its store of favorites:

`PUT /users/1234/favorites/alonso`

#### 2.3.1.4. Controller

Controller resources are like executable functions, with parameters and return values; inputs and outputs.

Like a traditional web application's use of HTML forms, a REST API relies on controller resources to perform application-specific actions that cannot be logically mapped to one of the CRUD standard methods.

Controller names typically appear as the last segment in a URI path, with no *child* resources to follow them in the hierarchy.

For example, a controller resource can allow a client to resend an alert to a user:

`POST /alerts/1234/resend`

## 2.4. URI Path Design

Each URI path segment, separated by /, represents a design opportunity. Assigning meaningful values to each path segment helps to clearly communicate the hierarchical structure of a REST API's resource model design.

### 2.4.1. A singular noun should be used for document names

A URI representing a document resource should be named with a singular noun or noun phrase path segment.

### 2.4.2. A plural noun should be used for collection names

A URI identifying a collection should be named with a plural noun, or noun phrase, path segment.

### 2.4.3. A plural noun should be used for store names

A URI identifying a store should be named with a plural noun, or noun phrase, as its path segment.

### 2.4.4. A verb or verb phrase should be used for controller names

Like a function, a URI identifying a controller resource should be named to indicate its action.

### 2.4.5. Variable path segments may be substituted with identity-based values

The URI Template syntax allows designers to clearly name both static and variable segments.

`http://api.soccer.org/leagues/{leagueId}/teams/{teamId}/players/{playerId}`

### 2.4.6. CRUD function names should not be used in URIs

URIs should be used to uniquely identify resources, not HTTP methods.

Instead of `GET /deleteUser?id=1234` use `DELETE /users/1234`

## 2.5. URI Query Design

The query component of a URI contains a set of parameters to be interpreted as a variation or derivative of the resource that is hierarchically identified by the path component. It can provide clients with additional interaction capabilities such as ad hoc searching and filtering.

`http://api.college.restapi.org/students/morgan/send-sms?text=hello`

### 2.5.1. The query component of a URI may be used to filter collections or stores.

A URI's query component is a natural fit for supplying search criteria to a collection or store.

`GET /users?role=admin`

### 2.5.2. The query component of a URI should be used to paginate collection or store results

A REST API client should use the query component to paginate collection and store results with the `pageSize` and `pageStartIndex` parameters.

When the complexity of the client's pagination or filtering requirements exceeds the simple formatting capabilities of the query part, consider designing a special controller resource that partners with a collection or store. For example, the following controller may accept more complex inputs via a request's *entity body* instead of the URI's query part:

`POST /users/search`

# 3. Interaction Design with HTTP

REST APIs embrace all aspects of the HTTP/1.1, including its request methods, response codes and message headers.

## 3.1. Request Methods

Each HTTP method has specific, well-defined semantics within the context of a REST API's resource model.

- `GET` retrieves a representation of a resource's state.
- `HEAD` retrieves the metadata associated with the resource's state.
- `PUT` adds a new resource to a store or updates a resource
- `DELETE` removes a resource from its parent.
- `POST` creates a new resource within a collection or executes controllers.

### 3.1.1. GET and POST must not be used to tunnel other request methods

*Tunneling* refers to any abuse of HTTP that masks or misrepresents a message's intent and undermines the protocol's transparency.

Always make proper use of the HTTP methods.

### 3.1.2. GET must be used to retrieve a representation of a resource

A client's `GET` request message may contain headers but no body.

The architecture of the web relies heavily on the nature of the `GET` method. Clients count on being able to repeat `GET` requests without causing side effects. Caches depend on the ability to serve cached representations without contacting the origin server.

### 3.1.3. HEAD should be used to retrieve response headers

A client's `HEAD` request may contain headers but no body.

Clients use `HEAD` to retrieve the headers without a body. Clients can use this method to check whether a resource exists or to read its metadata.

### 3.1.4. PUT must be used to both insert and update a stored resource

The `PUT` request message must include a representation of a resource that the client wants to store. However, the body of the request may not be *exactly* the same as a client would receive from a subsequent `GET` request.

For example, a REST API's store resource may allow clients to include only the mutable portions of the resource state in the request message's representation.

### 3.1.5. POST must be used to execute controllers

Clients use the `POST` method to *invoke* the function-oriented controller resources. A `POST` request message may include both headers and a body as inputs to a controller resource's function.

The `POST` request method is *unsafe* and *non-idempotent*, which means that its outcome is unpredictable and not guaranteed to be repeatable without potentially undesirable side effects.

### 3.1.6. DELETE must be used to remove a resource from its parent

A client uses `DELETE` to request that a resource be completely removed from its parent, which is often a collection or store. Once a `DELETE` request has been processed for a given resource, the resource can no longer be found by clients. Therefore, any future attempt to retrieve the resource's state representation, using either `GET` or `HEAD`, must result in a `404` status returned by the API.

For soft-deletes, the API should employ a special controller resource and direct its clients to use `POST` instead of `DELETE` to interact.

### 3.1.7. OPTIONS should be used to retrieve metadata that describes a resource's available interactions

Clients may use the `OPTIONS` request method to retrieve resource metadata that includes an `Allow` header value like `Allow: GET, PUT, DELETE`

In response to an `OPTIONS` request, a REST API may include a body that includes further details about each interaction option.

## 3.2. Response Status Codes

|Category|Description|
|--------|-----------|
|1xx: Informational|Transfer protocol-level information|
|2xx: Success|Request was accepted successfully|
|3xx: Redirection|Client must take some additional action in order to complete their request|
|4xx: Client Error|Errors caused by client|
|5xx: Server Error|Errors caused by server|

### 3.2.1. 200 (OK) should be used to indicate nonspecific success

Indicates the REST API successfully carried out whatever action the client requested. Should include a response body.

### 3.2.2. 201 (Created) must be used to indicate successful resource creation

A REST API responds with the `201` status code whenever a collection creates, or a store adds, a new resource at the client's request.

### 3.2.3. 202 (Accepted) must be used to indicate successful start of an asynchronous action

A `202` response indicates that the client's request will be handled asynchronously. Typically used for actions that take a long while to process.

Controller resources may send `202` responses, but other resource types should not.

### 3.2.4. 204 (No Content) should be used when the response body is intentionally empty

The `204` status code is usually sent out in response to a `PUT`, `POST`, or `DELETE` request, when the REST API declines to send back any status message or representation in the response message's body.

An API may also send `204` with a `GET` request, to indicate that the requested resource exists, but has no state representation to include in the body.

### 3.2.5. 301 (Moved Permanently) should be used to relocate resources

`301` indicates that the REST API's model has been significantly redesigned and a new *permanent* URI has been assigned to the client's requested resource. The REST API should specify the new URI in the response's `Location` header.

### 3.2.6. 302 (Found) should not be used

The intended semantics of `302` have been misunderstood by programmers and incorrectly implemented in programs. The confusion centers on whether it is appropriate for a client to always automatically issue a follow-up `GET` request to the URI in response's `Location` header, **regardless of the original request's method**.

The intent of `302` is that this automatic redirect behavior only applies if the client's original request used either the `GET` or `HEAD` method. Instead of using `302`, the status codes `303` or `307` should be used instead.

### 3.2.7. 303 (See Other) should be used to refer the client to a different URI

`303` indicates that a controller resource has finished its work, but instead of sending a potentially unwanted response body, it sends the client the URI of a response resource. This can be the URI of a temporary status message, or the URI to some already existing, more permanent, resource.

Generally speaking, the `303` status code allows a REST API to send a reference to a resource without forcing the client to download its state. Instead, the client may send a `GET` request to the value of the `Location` header.

### 3.2.8. 304 (Not Modified) should be used to preserve bandwidth

Similarly to `304`, the response body must be empty. 

`204` is used when there is nothing to send in the body, whereas `304` is used when there *is* state information associated with a resource but the client already has the most recent version of the representation.

### 3.2.9. 307 (Temporary Redirect) should be used to tell clients to resubmit the request to another URI

A `307` indicates that the REST API is not going to process the client's request. Instead, the client should resubmit the request to the URI specified by the response message's `Location` header.

### 3.2.10. 400 (Bad Request) may be used to indicate nonspecific failure

`400` is the generic client-side error status, used when no other `4xx` error code is appropriate.

### 3.2.11. 401 (Unauthorized) must be used when there is a problem with the client's credentials

`401` indicates that the client tried to operate on a protected resource without providing the proper authorization.

### 3.2.12. 403 (Forbidden) should be used to forbid access regardless of authorization state

`403` indicates that the client's request is formed correctly, but the REST API refuses to honor it. A `403` is *not* a case of insufficient client credentials (that would be a `401`).

REST APIs use `403` to enforce application-level permissions. For example, a client may be authorized to interact with some, but not all of a REST API's resources. If the client attempts a resource interaction that is outside of its permitted scope, the REST API should repsond with `403`.

### 3.2.13. 404 (Not Found) must be used when a client's URI cannot be mapped to a resource

`404` indicates that the REST API can't map the client's URI to a resource.

### 3.2.14. 405 (Method Not Allowed) must be used when the HTTP method is not supported

`405` indicates that the client tried to use an HTTP method that the resource does not allow.

The response must include the `Allow` header, which lists the HTTP methods that the resource supports.

### 3.2.15. 406 (Not Acceptable) must be used when the requested media type cannot be served

`406` indicates that the API is not able to generate any of the client's preferred media types, as indicated by the `Accept` request header. For example, a client request for data formatted as `application/xml` will receive a `406` if the API is only willing to format data as `application/json`.

### 3.2.16. 409 (Conflict) should be used to indicate a violation of resource state

`409` tells the client that they tried to put the REST API's resources into an impossible or inconsistent state. For example, when a client tries to delete a non-empty store resource.

### 3.2.17. 412 (Precondition Failed) should be used to support conditional operations

`412` indicates that the client specified one or more preconditions in its request headers, effectively telling the REST API to carry out its request only if certain conditions were met. A `412` response indicates that those conditions were not met, so instead of carrying out the request, the API sends this status code.

### 3.2.18. 415 (Unsupported Media Type) must be used when the media type of a request's payload cannot be processed.

`415` indicates that the API is not able to process the client's supplied media type, as indicated by the `Content-Type` request header. For example, a client request including data formatted as `application/xml` will receive a `415` if the API is only willing to process data formatted as `application/json`.

### 3.2.19. 500 (Internal Server Error) should be used to indicate API malfunction

`500` is the generic REST API error response. Usually happens when exceptions are raised within the API code.

# 4. Metadata Design

## 4.1. HTTP Headers

Various forms of metadata can be conveyed through the *entity headers* contained within HTTP's request and response messages. These headers can provide information about a requested resource, indicate something about the representation carried by the message or serve as directives to control intermediary caches.

### 4.1.1. Content-Type must be used

The `Content-Type` header names the *type* of data found within a request or response message's body. This header is a specially formatted text string known as a *media type*.

Clients and servers rely on this header's value to tell them how to process the sequence of bytes in a message's body.

### 4.1.2. Content-Length should be used

The `Content-Length` header gives the size of the entity-body in bytes.

With this, a client can know whether it has read the correct number of bytes from the connection. Also, a client can make a `HEAD` request to find out how large the entity-body is, without downloading it.

### 4.1.3. Last-Modified should be used in responses.

The `Last-Modified` header applies to response messages only. The value of this header is a timestamp that indicates the last time that something happened to alter the representational state of the resource.

Clients and cache intermediaries may rely on this header to determine the freshness of their local copies of a resource's state representation. This header should always be supplied in response to `GET` requests.

### 4.1.4. ETag should be used in responses

The `ETag` (entity tag) header identifies a specific "version" of the representational state contained in the response's entity. It may be any string value, as long as it changes along with the resource's representation. This header should always be sent in response to `GET` requests.

Clients may choose to save an `ETag` header's value for use in future `GET` requests, as the value of the conditional `If-None-Match` request header. If the REST API concludes that the entity tag hasn't changed, then it can save time and bandwidth by not sending the representation again.

### 4.1.5. Stores must support conditional PUT requests

A store resource uses the `PUT` method for both insert and update, which makes it hard for a REST API to know the true *intent* if a client's `PUT` request. Through headers, HTTP provides the necessary support to help an API resolve any potential ambiguity. A REST API must rely on the client to include the `If-Unmodified-Since` and/or `If-Match` request headers to express their intent.

The `If-Unmodified-Since` header asks the API to proceed with the operation if, and only if, the resource's state representation hasn't changed since the time indicated by the header's supplied timestamp.

The `If-Match` header's value is an `ETag`. The `If-Match` header makes the request conditional, based upon an exact match of the header's supplied entity tag value and the representational state's current entity tag value, as stored or computed by the REST API.

For example:

Client 1 sends a `PUT` request in order to store some new data that it identifies with a URI path of `/objects/123`. This is a new URI that the REST API has never seen before. Therefore, the REST API interprets the request as an *insert* and creates a new resource and returns a `201` (Created).

Later, client 2 sends a `PUT` request to store some data in the same `/objects/123` URI. Now the REST API is able to map this URI to an existing resoource, which makes it unclear about the client's intent. Without any additional information, the API is forced to return a `409` (Conflict) response.

If client 2 decides to update the stored data, it may retry its request to include the `If-Match` header. However, if the supplied header does not match the current entity tag value, the REST API must return `412` (Precondition Failed). If the supplied condition does match, the REST API must update the stored resource's state, and return a `200` (OK) or `204` (No Content) response. If the response does include an updated representation of the resource's state, the API must include values for the `Last-Modified` and `ETag` headers that reflect the update.

### 4.1.6. Location must be used to specify the URI of a newly created resource

The `Location` response header's value is a URI that identifies a resource that may be of interest to the client. In response to the successful creation of a resource within a collection or store, a REST API must include the `Location` header to designate the URI of the newly created resource.

In a `202` response, this header may be used to direct clients to the operational status of an asynchronous controller resource.

### 4.1.7. Cache-Control, Expires and Date response headers should be used to encourage caching

When serving a representation, include a `Cache-Control` header with a max-age value (in seconds) equal to the freshness lifetime. For example:

`Cache-Control: max-age=60, must-revalidate`

To support legacy HTTP 1.0 caches, a REST API should include an `Expires` header with the expiration date-time. For example:

`Date: Tue, 15 Nov 1994 08:12:31 GMT`
`Expires: Thu, 01 Dec 1994 16:00:00 GMT`

### 4.1.8. Cache-Control, Expires, and Pragma response headers may be used to discourage caching

If a REST API's response must not be cached, add `Cache-Control` headers with the value `no-cache` and `no-store`. In this case, also add the `Pragma: no-cache` and `Expires: 0` header values to interoperate with legacy HTTP 1.0 caches.

### 4.1.9. Caching should be encouraged

The `no-cache` directive will prevent any cache from serving cached responses. REST APIs should not do this unless absolutely necessary. Using a small value of `max-age` as opposed to adding `no-cache` directive helps clients fetch cached copies for at least a short while without significantly impacting freshness.

### 4.1.10. Expiration caching headers should be used with 200 (OK) responses

Set expiration caching headers in responses to successful `GET` and `HEAD` requests. Although `POST` is cacheable, most caches treat this method as non-cacheable. You need not set expiration headers on other methods.

### 4.1.11. Expiration caching headers may optionally be used with 3xx and 4xx responses

Consider adding caching headers to `3xx` and `4xx` responses. Known as *negative caching*, this helps reduce the amount of redirecting and error-triggering load on a REST API.

### 4.1.12. Custom HTTP headers must not be used to change the behavior of HTTP methods.

You can optionally use custom headers for informational purposes only. Implement clients and servers such that they do not fail when they do not find expected custom headers.

If the information you are conveying through a custom header is important for the interpretation of the request or response, include that information in the body of the request or response or the URI used for the request.

## 4.2. Media Type Design

To identify the form of the data contained within a request or response message body, the `Content-Type` header's value references a media type.

Media types have the following syntax:

`type/subtype [;parameter]`

The `type` may be one of: `application`, `audio`, `image`, `message`, `model`, `multipart`, `text`, or `video`. Most REST APIs work with the `application` type. In a hierarchical fashion, the `subtype` is subordinate to its `type`.

`Parameters` are in the form of `attribute=value` pairs, that are separated by a leading semi-colon character.

Example:

`Content-type: text/html; charset=ISO-8859-4`

# 5. Client Concerns

## 5.1. Versioning

A REST API is composed of an assembly of interlinked resources: its resource model. The version of each resource is conveyed through its representational form and state.

### 5.1.1. New URIs should be used to introduce new concepts

A resource's representational form and state may change over time, but the identifier must consistently address the same thought, which no other URI can identify.

Furthermore, every character in a resource's URI contributes to its identity. Therefore, the version of a REST API, or any of its resources, typically should not be signified in a URI. For example, including a version indicator like `v2` in a URI conveys that the *concept* itself has multiple versions, which is usually not the intent.

A URI identifies a resource, independent of the version of its representational form and state. A REST API should only introduce a new URI if it intends to expose a new concept.

### 5.1.2. Schemas should be used to manage representational form versions

The version of the form of a REST API's resource representation is managed through versioned schema documents. Clients use media type negotiation to bind to the representational forms that best suit their needs.

Adding fields and links to new schema versions is a great way to introduce new features without impacting backward compatibility.

### 5.1.3. ETags should be used to manage representational state versions

The `ETag` values associated with each individual resource are a REST API's most fine-grained versioning system.

## 5.2. Security

Many REST APIs expose resources that are associated with a specific client and/or user, containing private information or operations intended to be executed by a restricted audience.

### 5.2.1. OAuth may be used to protect resources

OAuth (Open Authorization) is an open standard that provides secure authorization using a consistent approach for all clients.

The OAuth protocol's flow is summarized below:

1. A client obtains the *artifacts* needed to interact with a REST API's protected resources.
2. Using the *artifacts*, the client requests an interaction with a REST API's protected resource.
3. The REST API (or an intermediary acting on its behalf) validates the client request's OAuth-based authorization information.
4. If the validation check succeeds, the REST API allows the client's interaction with the protected resource to proceed.

Architecturally, the OAuth protocol helps a REST API address security concerns in a manner that is complementary to the resource-centric and stateless nature of its interactions with clients.

### 5.2.2. Reverse proxies may be used to protect resources

An API reverse proxy is a network-based intermediary that may be used to secure a REST API's resources.

A reverse proxy is a server that typically sits behind the firewall in a private network and directs clients requests to the appropriate backend server. It is used for load balancing, caching and improving security.

## 5.3. Response Representation Composition

### 5.3.1. The query component of a URI should be used to support partial responses

There may be times when a REST API offers a resource state model that includes more data than the client wishes to receive. In order to save on bandwidth, and possibly accelerate the interaction, a REST API's client can use the query component to trim response data with the `fields` parameter.

The `fields` query parameter allows clients to request only the resource state information that it deems relevant for its particular use case. The REST API must parse the request's query parameter's *inclusion list* and return a partial response. Same thing can be done with an *exclusion list*.

## 5.4. CORS

Cross-Origin Resource Sharing (CORS) is the W3C's proposed approach to standardize cross-origin requests from the browser. CORS is an alternative to JSONP that supports all request methods.

REST APIs may use the CORS-proposed `Access-Control-Allow-Origin` HTTP header to list the set of origins that are permitted cross-origin access to its resources. Most modern browsers support CORS by sending special HTTP request headers such as `Origin` and `Access-Control-Request-Method`.

The `Origin` header value identifies the requesting JavaScript client's schema/host/port source location.

The `Access-Control-Request-Method` header value is sent in the CORS preflight request to indicate which HTTP method will be used in the client's actual request.




