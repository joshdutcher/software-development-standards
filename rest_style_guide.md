# Company Development Standards - REST API Style Guide

REST APIs over HTTP are the most common channel of communication between applications

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).

## Use JSON for Data Input and Output

All REST APIs **MUST** emit data encoded as JSON. APIs that accept input bodies **MUST** accept the data encoded as JSON. JSON
 is a lightweight yet expressible format that just about every language has support for reading or writing.

- APIs **MUST** use the proper [Content-Type headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) (usually `Content-Type: application/vnd.api+json`).
- If the API must handle some other format due to some external requirement, it **MUST** be in addition to supporting JSON for equivalent actions. Which format is default is up to the developer.
- Properties of JSON emitted by APIs **SHOULD** be expressed in camelCase. This is important for any publicly-consumed API, less-so for internal-only APIs.

> REFERENCES
> - http://jsonapi.org/format/

## Use Proper HTTP Method for requests

### Background

One part of any HTTP request is the _method_ or _verb_, which instructs the web-server or API how to handle the request. For example, when making a request for a standard web page, the browser is making an HTTP `GET` request under the hood. Each of these verbs has a generally agreed-upon semantic meaning in the REST API Context:

- `GET` The GET method requests a representation of the specified resource. Requests using GET should only retrieve data.
- `POST` The POST method is used to submit an entity to the specified resource, often causing a change in state or side effects on the server
- `PUT` The PUT method replaces all current representations of the target resource with the request payload.
- `PATCH` Like PUT, but intended to refer to partial resource updates.
- `DELETE` The DELETE method deletes the specified resource.

In practice, `PUT` and `PATCH` are commonly used interchangeably. When in doubt, refer to the chosen application framework or language standard for choosing the appropriate method.

In addition to the normal RESTful HTTP methods, there are two more that may be of occasional interest.

- `HEAD` The HEAD method asks for a response identical to that of a GET request, but without the response body.
- `OPTIONS` The OPTIONS method is used to describe the communication options for the target resource.

There are several more available methods, but they are very seldom used. More details can be found at [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

### Usage Standards

This guide does not aim to specify a strict usage for each HTTP method, only the following high level guidelines:

- `GET` and `HEAD` requests **MUST** never make a change to an existing resource
- `POST`, `PUT`, and `PATCH` requests **SHOULD** return something useful to the client. Returning a 200 with an empty body is not that useful. Some examples of useful returns:
  - A 201 (Created) status code
  - A representation of the affected resource
  - A Location header that points to the URL of the new resource

> REFERENCES
> - https://tools.ietf.org/html/rfc7231

## Use Standard HTTP Status Codes for responses

### Background

HTTP defines a bunch of meaningful status codes that can be returned alongside a response. These can be leveraged to help the API consumers route their responses accordingly. A short list follows:

- `200 OK` Response to a successful GET, PUT, PATCH or DELETE. Can also be used for a POST that doesn't result in a creation.
- `201 Created` Response to a POST that results in a creation. Should be combined with a Location header pointing to the location of the new resource
- `204 No Content` Response to a successful request that won't be returning a body (like a DELETE request)
- `304 Not Modified` Used when HTTP caching headers are in play
- `400 Bad Request` The request is malformed, such as if the body does not parse
- `401 Unauthorized` When no or invalid authentication details are provided. Also useful to trigger an auth popup if the API is used from a browser
- `403 Forbidden` When authentication succeeded but authenticated user doesn't have access to the resource
- `404 Not Found` When a non-existent resource is requested
- `405 Method Not Allowed` When an HTTP method is being requested that isn't allowed for the authenticated user
- `410 Gone` Indicates that the resource at this end point is no longer available. Useful as a blanket response for old API versions
- `415 Unsupported Media Type` If incorrect content type was provided as part of the request
- `422 Unprocessable Entity` Used for validation errors
- `429 Too Many Requests` When a request is rejected due to rate limiting

There are several standard 500 codes, but most of those will be handled by the web server. See [here](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) for a more complete list of HTTP Status Codes.

### Usage Standards

- APIs **MUST** use `2xx` codes only for success responses and **SHOULD** use a _meaningful_ `2xx` code for a successful request
- APIs **MUST** use `4xx` codes only for client errors and **SHOULD** use a _meaningful_ `4xx` code for a client errors, where a client error is an error produced by an improper action specified by the client (bad data, improper parameters, etc)
- APIs **MUST** use `5xx` codes only for server errors and **SHOULD** return `500` or a more meaningful `5xx` code for server errors, where a server error is an error produced by a problem with the application itself outside of the client's control (application bug, backend connectivity issues, etc)
- APIs **MAY** attach special meaning to an arbitrary status code under the following conditions
  - Custom status codes **MUST NOT** override some other accepted standard status code's meaning
  - Custom status codes **MUST** adhere to the common status code scheme:
    - `2xx` for Successful requests
    - `4xx` for Client errors
    - `5xx` for Server errors
  - All Custom status codes **MUST** be clearly documented.

> REFERENCES
> - https://tools.ietf.org/html/rfc2616#section-10

## Resource-based URLs

It's **RECOMMENDED** for APIs that provide an interface for working with data from an underlying store to implement **Resource-Based URLs** as outlined below:

- For all routes that deal with a particular resource, the route's endpoint (or terminating part) is be the noun that represents the affected resource
  - For example: User, ticket, article, etc
  - Nouns often map closely to underlying database tables
  - The plural-form of the noun is preferred, but not required.
- Use the proper HTTP Method corresponding to the route's role in the CRUD cycle.
- To target a single resource, an additional identifier may be passed as a route parameter beyond the endpoint
  - For example: `GET /articles/123` to retrieve article number 123

The following 6 routes implement the entire CRUD cycle as resource-based URLs.

- `GET /tickets` - Retrieves a list of tickets
- `GET /tickets/12` - Retrieves a specific ticket
- `POST /tickets` - Creates a new ticket
- `PUT /tickets/12` - Updates ticket #12
- `PATCH /tickets/12` - Partially updates ticket #12
- `DELETE /tickets/12` - Deletes ticket #12

Each of these represent a unique action that can be handled by a specific controller despite sharing one of two actual routes.

## Use Nested Routes with care

A nested route has the form `/partA/partB/partC...`, where any of the parts can be parameterized. When using Resource-based URLs the most common use of nested routes is to express relationships. For example, `/blog-post/12/comments` would get all of the comments associated with blog post #12. In general, nested routes are ok to use but there are several guidelines to how they should be used:

- API endpoints **SHOULD** avoid very deep nesting. For example `/blog-post/12/comments/1/user/friends/3` would get the third friend of the user that posted the first comment on blog post 12. This makes sense, but there a lot of challenges with implementing and maintaining these types of structures. It's better to have relatively shallow routes and enable the client to make multiple, shorter requests to drill down through relationships.
- Route parameters **SHOULD NOT** be used for pagination, searching, filtering, or sorting. Query parameters are the more idiomatic approach to shape an API response.
- If an API design suggests an optional route parameter, it's **RECOMMENDED** to consider first using a query parameter. Nesting beyond optional route parameters is not possible in most cases, and such a choice up-front could hamper future development of the API.

## Use query parameters for filtering, sorting, searching, and paginating

It's likely that an API client will want to shape a response by sorting, filtering or searching the output.

- API endpoints **SHOULD** implement some default filtering or pagination of the response to prevent large amounts of data to be exchanged, potentially exhausting memory on the server or client.
- Response shaping (filtering, sorting, searching, and paginating) beyond the default **SHOULD** always be optional, and therefore **SHOULD** be implemented the query string, which is a URL's optional component.

## Envelope API Responses only when necessary

It is **RECOMMENDED** that APIs not wrap response data in an envelope containing response metadata. HTTP provides status codes to convey information about the request's success or failure and response headers to convey metadata about the request back to the client. Therefore, it is generally unnecessary to convey this information in the response body, and can lead to the API sending mixed signals back to the client (for example, a field like `success: false` in the response body alongside a 200 status code).

There are a few exceptions where enveloping is appropriate:

- The amount of metadata to send back is large (this is intentionally vague, but comparing to the size of the returned data is a good rule-of-thumb)
- A possible client of the API will not have access to HTTP headers and/or the status code

An API **MAY** consider implementing an optional envelope that can be toggled as part of the response via a query parameter.

## Version via URL if applicable

For an API that needs to exist in multiple versions to provide backward compatibility, the different versions **SHOULD** be served through different URLs. Therefore, maintaining multiple API versions can be handled at the operational level rather than the application layer. For example, `api.touchdownloyal.com/` could serve the first version of the API, and a second version could be served at `api.touchdownloyal.com/v2/`.

## Provide clear documentation for all exposed methods

REST APIs are completely useless without some form of documentation. The following parts of an API **MUST** be clearly documented for it to be of any use:

- All routes, including allowed HTTP methods, route, and query parameters
- All route inputs, for input-accepting routes
- All route outputs, for those routes that return data.

The following are guidelines for documenting a REST API

- An API's README **MAY** only be used as documentation only if it's accessible to all possible callers
- An API **MAY** expose different sets of functionality through different groups, and therefore may separate different documents to different user groups. For example, a backend API for data access may have routes that expose the data, as well as routes that expose metadata about the API's performance.
- An API **MAY** be formalized with a specification language such as [Swagger](http://swagger.io/), from which interactive documentation can be generated (amongst other things).
- Input and output objects of an API **MAY** be defined in a JSON schema to facilitate efficient data validation and code generation.

> REFERENCES
> - https://idratherbewriting.com/learnapidoc/docapis_intro_to_rest_api_doc.html

## Use Stateless Authentication when necessary

It's **RECOMMENDED** for APIs to authenticate requests with Stateless authentication. Stateless authentication states that an authorization credential must be supplied by the caller for every request, and there is no mechanism by the API to store this credential. This differs from the traditional approach of "logging into" a service and depending on the server to store the state of this authentication. Typically, in the stateless model, the authentication credential is passed to the API via a request header (good) or a query parameter (not as good, as these usually end up in web-server logs).

### Implementing secure authentication

This, stated succinctly, is a huge topic. There's a lot of things to do right to implement secure authentication and a lot of places where it can go wrong. Developers are strongly encouraged to do their own research in implementing secure authentication, but a few high level guidelines follow:

- SSL/HTTPS is required for stateless authentication to be fully secure. If SSL is not used, it's relatively trivial for an attacker to hijack the traffic and pilfer the encoded credential
- Even with SSL, credentials should never be sent as plain-text. Consider an encoding mechanism, base64 at least, and [JSON Web Tokens](https://jwt.io/) at best.
- The best authentication system is the one you [didn't write](https://blog.codinghorror.com/the-best-code-is-no-code-at-all/). Nothing personal. Consider authentication providers such as [oAuth2](http://oauth.net/2/) for large systems.

### When to use authentication

- API endpoints that return sensitive information
- API endpoints that can modify resources in an underlying data store

## Common Response Headers

The following is a list of common headers used by one or more company applications and what data they convey. It's **RECOMMENDED** for APIs to implement these headers if they intend to convey similar data.

- **Company-Served-By** - ...

## Environmental Awareness

All calls to APIs and for file assets like images and article files as well as includes like js and css files should be made to the appropriate corresponding environment and should be environmentally aware. For example, products in the HomerunLoyal Development environment should call `dev-homerunloyal-api.synapsys.us`, in qa should call `qa-homerunloyal-api.synapsys.us` and in production should call `prod-homerunloyal-api.synapsys.us`. Similarly widget calls should call `dev-w1.synapsys.us`, `qa-w1.synapsys.us`, or `prod-w1.synapsys.us` as appropriate, and so on. This applies to all calls for all APIs and assets across all products.

## References

- http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api
- http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm
