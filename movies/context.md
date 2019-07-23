# The JSON-LD context

## Story

As an API provider, I want to describe the semantics in a JSON-LD context, in order to simplify the actual JSON responses.

## API behaviour

```http
GET https://hydra-movies.herokuapp.com/
```

Response body (excerpt):

```json
{
  "@context": "https://hydra-movies.herokuapp.com/context.jsonld",
  "@id": "/",
  "@type": "schema:EntryPoint",
  "collection": {
    "@id": "movies",
    "@type": [
      "Collection",
      "mov:Movies"
    ]
  }
}
```

```http
GET https://hydra-movies.herokuapp.com/context.jsonld
```

Response headers (excerpt):

```http
Last-Modified: Tue, 07 May 2019 08:24:11 GMT
Etag: W/"a9-16a91649bf8"
```

Response body (excerpt):

```json
{
  "@context": [
    "https://www.w3.org/ns/hydra/core",
    {
      "@base": "https://hydra-movies.herokuapp.com",
      "mov": "https://hydra-movies.herokuapp.com/doc#",
      "schema": "http://schema.org/"
    }
  ]
}
```

### Explanation

```
"@context": "https://hydra-movies.herokuapp.com/context.jsonld"
```

Since the example API uses JSON-LD as serialization format, we need to add a context to the response body to enrich the JSON properties with RDF semantics. A context might get large, so it is a good idea to move it to it's own document and link to it, like shown above.

```http
GET https://hydra-movies.herokuapp.com/context.jsonld
```

JSON-LD libraries will automatically fetch the context document, before processing.

```http
Last-Modified: Tue, 07 May 2019 08:24:11 GMT
Etag: W/"a9-16a91649bf8"
```

The API should therefore ensure headers that allow proper caching, so that the client does not have to fetch the context over and over again.

```
{
  "@context": [
    "https://www.w3.org/ns/hydra/core",
```

Using the official hydra context as a base simplifies the usage of the hydra core vocabulary. Everything from the vocabulary is mapped to simple terms, like `collection` seen above. Further, `"@type": "@id"` or `"@type": "@vocab"` is applied to terms where this usage is most common.

```
"@base": "https://hydra-movies.herokuapp.com#",
```

Setting a base URI allows us to use relative links in the resource representations.

```
"mov": "https://hydra-movies.herokuapp.com/doc#",
"schema": "http://schema.org/"
```

These lines define the prefixes, that are used in the JSON response. We are referring to an API specific vocabulary (`mov`) as well as a well-known vocabulary (`schema`). Try not to reinvent the wheel: map your API domain to well-known vocabularies where possible. Within this example API we will use the custom vocabulary only to describe specific API capabilities while [schema.org](http://schema.org/) is used to express the domain. For details about JSON-LD contexts take a look at [the JSON-LD specification](https://w3c.github.io/json-ld-syntax/#the-context).

Given the context, let's take another look at the entrypoint response:

```
"@type": "schema:EntryPoint",
"collection": {
  "@id": "movies",
  "@type": [
    "Collection",
    "mov:Movies"
  ]
}
```

 We are now able to use simple property names like `collection` within the actual resource representation. Prefixes defined in the context allow short-hand names like `schema:EntryPoint` and `mov:Movies`, that still resolve to fully-qualified URIs when expanded.
