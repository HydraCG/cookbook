# The API documentation

## Story

As a client developer, I want to find human-readable documenation of the API.

As a client, I want to find a machine-readable description of the API capabilities.  

## API behaviour

```http
GET https://hydra-movies.herokuapp.com/
```

Response headers (excerpt):

```http
Link: </doc>; rel="http://www.w3.org/ns/hydra/core#apiDocumentation"
```

```http
GET https://hydra-movies.herokuapp.com/doc
```

Response body (excerpt):

```json
{
  "@context": "https://hydra-movies.herokuapp.com/context.jsonld",
  "@id": "/doc",
  "@type": "ApiDocumentation",
  "title": "Hydra Movies API",
  "description": "This API shows how a basic CRUD (Create, Read, Update, Delete) API can be designed using Hydra Core Vocabulary.",
  "entrypoint": "/",
  "supportedClass": [
    {
      "@id": "schema:EntryPoint",
      "@type": "Class",
      "title": "API entrypoint",
      "description": "Every capability of the API can be reached from here by following links."
    }
  ]
}
```

### Explanation

```http
Link: </doc>; rel="http://www.w3.org/ns/hydra/core#apiDocumentation"
```

```http
GET https://hydra-movies.herokuapp.com/doc
```

As described in ["API entrypoint"](./1.entry-point.md), every API response has to contain a Link to the documentation. This allows a client to dereference and process the documentation, even if it does not start from the entrypoint (e.g. because of a "deep link" or a bookmark from a former visit).

```
"@id": "/doc",
"@type": "ApiDocumentation"
```

The API documenation is just another web rescoure, with of course an `@id` (i.e. URI) and the type `http://www.w3.org/ns/hydra/core#ApiDocumentation`, which is abbreviated to `ApiDocumentation` here thanks to the JSON-LD context.

```
"title": "Hydra Movies API",
"description": "This API shows how a basic CRUD (Create, Read, Update, Delete) API can be designed using Hydra Core Vocabulary.",
```

`title` and `description` are hydra terms that allow to describe the API in a human-readable matter on a high level. The values provided here, could for example appear in generic clients, API catalogs or API search engine results. They allow humans to get a first impression of what the API is good for.

```
"entrypoint": "/",
```

Since a client may get to the API by any URL, not only the entry point, it is important to make clear what the entry point of the API actually is. 

{% hint style='info' %}
The entry point may vary by the client or server state. An authenticated client could for example be sent to a user profile, while anonymous clients get a generic entry point. A server could initially provide an "install" entry point and change it after being set up properly.
{% endhint %}


```
"supportedClass": [
    {
      "@id": "schema:EntryPoint",
      "@type": "Class",
      "title": "API entrypoint",
      "description": "Every capability of the API can be reached from here by following links."
    }
  ]
```

The hydra term `supportedClass` describes all classes the API provides or is able to handle in some way. The most basic thing we can describe here, is again a human-readble `title` and `description`. More possibilities will be shown in later recipes.

{% hint style='info' %}
The supported classes that are shown in the API doc may vary depending on client or server state. Unauthenticated clients or specific user roles may e.g. see different subsets, or additional classes get supported after prerequisite resources have been created.
{% endhint %}
