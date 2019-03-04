## Assumptions

For simplification, some assumptions are made so the main topic of each of the use-cases are not distracted with side details.

These would be:

- client-server communication uses JSON-LD RDF serialization using HTTP headers:
  `Content-Type: application/ld+json` for server responses
  `Accept: application/ld+json` client requests
- JSON-LD context is described later on in this document and is referenced in further example
  payloads with this property:
  ```json
  {
      "@context": "/api/context.jsonld"
      // further example payload goes here
  }
  ```
- to simplify, the host was dropped from all URLs. We can assume an example like http://example.com/.


### JSON-LD context

```json
{
    "@context": {
        "hydra": "http://www.w3.org/ns/hydra/core#",
        "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
        "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
        "xsd": "http://www.w3.org/2001/XMLSchema#",
        "owl": "http://www.w3.org/2002/07/owl#",
        "schema": "http://schema.org/",
        "title": {
            "@id": "hydra:title",
            "@type": "xsd:string"
        },
        "operation": {
            "@id": "hydra:operation",
            "@type": "@id"
        },
        "method": {
            "@id": "hydra:method",
            "@type": "xsd:string"
        },
        "returns": {
            "@id": "hydra:returns",
            "@type": "@vocab"
        },
        "expects": {
            "@id": "hydra:expects",
            "@type": "@vocab"
        },
        "supportedClass": {
            "@id": "hydra:supportedClass",
            "@type": "@vocab"
        },
        "supportedProperty": {
            "@id": "hydra:supportedProperty",
            "@type": "@vocab"
        },
        "property": {
            "@id": "hydra:property",
            "@type": "@vocab"
        },
        "subject": {
            "@id": "hydra:subject",
            "@type": "@id"
        },
        "object": {
            "@id": "hydra:object",
            "@type": "@id"
        },
        "member": {
            "@id": "hydra:member",
            "@type": "@id"
        },
        "collection": {
            "@id": "hydra:collection",
            "@type": "@id"
        },
        "manages": {
            "@id": "hydra:manages",
            "@type": "@id"
        },
        "totalItems": {
            "@id": "hydra:totalItems",
            "@type": "xsd:int"
        },
        "search": {
            "@id": "hydra:search",
        },
        "Event": {
            "@id": "schema:Event",
            "@type": "@id"
        },
        "eventName": {
            "@id": "schema:name",
            "@type": "xsd:string"
        },
        "eventDescription": {
            "@id": "schema:description",
            "@type": "xsd:string"
        },
        "startDate": {
            "@id": "schema:startDate",
            "@type": "xsd:dateTime"
        },
        "endDate": {
            "@id": "schema:endDate",
            "@type": "xsd:dateTime"
        }
    }
}
```
