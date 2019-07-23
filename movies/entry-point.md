# API entrypoint

## Story

As an API client I want to start with some URI to be able to discover the API capabilities.

## API behaviour

```http
GET https://hydra-movies.herokuapp.com/
```

Response headers (excerpt):

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, Link
Link: </doc>; rel="http://www.w3.org/ns/hydra/core#apiDocumentation"
```

Response body (excerpt):

```json
{
  "@context": "https://hydra-movies.herokuapp.com/context.jsonld",
  "@id": "/",
  "@type": "schema:EntryPoint"
}
```
### Explanation

```http
GET https://hydra-movies.herokuapp.com/
```

The example API is hosted at https://hydra-movies.herokuapp.com/, so most commonly this is the URI we will give to clients initially. Clients that use the API for the first time will start by dereferencing this URI.

```http
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, Link
```

The API will be used by a variety of clients. To allow the access of browser applications, we need to ensure a proper setup for [Cross Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS). The example API allows access from any Origin and exposes some relevant headers. Exposing the `Link` header is a **very important** for Hydra APIs, because the API documentation is discovered this way. If you do not setup CORS correctly, browser clients will not be able to use the API.

```http
Link: </doc>; rel="http://www.w3.org/ns/hydra/core#apiDocumentation"
```

Hydra uses the [HTTP `Link` header](http://webconcepts.info/concepts/http-header/Link) to refer to the API documentation. `</doc>` is a relative Link to the documentation and `rel="http://www.w3.org/ns/hydra/core#apiDocumentation"` is a fixed value that describes the type of the link. Every API resource (not only the entrypoint) must provide this header with every response. This applies also to error responses. The client knows now, that the API documentation can be found at https://hydra-movies.herokuapp.com/doc.

```
"@context": "https://hydra-movies.herokuapp.com/context.jsonld"
```

The example API uses [JSON-LD](https://json-ld.org) as serialization format. This is not a necessity, as Hydra supports any RDF serialization, but it is an elegant way to extend tradtional JSON-APIs. The line above links to the JSON-LD context the API uses. The context will be described in more detail in the next recipe.

```
"@id": "/"
```

This is the ID of the entrypoint. All IDs in JSON-LD expand to full URIs, so this is equivalent to the URI the client dereferenced: https://hydra-movies.herokuapp.com/.

```
"@type": "schema:EntryPoint"
```

Any resource (of any type) can be used as the entry point of a Hydra API. If your domain and API design allows it, you should use a more specific type. But sometimes the entry point has no further purpose on its own, and a [schema.org EntryPoint](https://schema.org/EntryPoint) can express this.
