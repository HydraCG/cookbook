# Using SHACL to describe data structures

## Story

As an API publisher

I want to use SHACL to describe data structures

So that consumers can use existing tooling to consume the API

## Details

SHACL, which stands for Shapes Constraint Language is an RDF vocabulary, defined in a [W3C recommendation][shacl], which is used to describe graph shapes.

It is very expressive, more rich than `hydra:Class` et.al., and out of the box provides multiple features to provide an accurate description of request payloads.

[Many uses for SHACL have been proposed][ucr] such as, but not limited to, validation and building user interfaces dynamically.

This page shows how Shapes can be used to replace or extend Hydra Core vocabulary terms within the API Documentation and resource representations.

[shacl]: https://www.w3.org/TR/shacl/
[ucr]: https://www.w3.org/TR/shacl-ucr/

### Announce SHACL in API Documentation

An API which chooses to use SHACL in addition to the built-in `hydra:Class` `MUST` announce that fact in its API Documentation resource

```json
{
  "@context": {
    "hydra": "http://www.w3.org/ns/hydra/core#",
    "requires": { "@id": "hydra:requires", "@type": "@id" }
  },
  "@id": "/api",
  "@type": "hydra:ApiDocumentation",
  "requires": "http://www.w3.org/ns/hydra/shacl"
}
```

By adding the `requires` property with the `hydra/shacl` identifier the client will be prepared to find SHACL terms within the payloads coming from this API.

### Describing request bodies

The most obvious use for Shapes is to use them to annotate supported operations' payloads. They will simply be added alongside `hydra:Class` as objects of `hydra:expects`.

```json
{
  "@context": "/api/context.jsonld",
  "@id": "/api",
  "@type": "hydra:ApiDocumentation",
  "supportedClass": [{
    "@id": "mov:Movies",
    "supportedOperation": {
      "@type": "schema:CreateAction",
      "method": "POST",
      "expects": ["mov:Movie", "mov:NewMovieShape"]
    }
  }, {
    "@id": "mov:Movie",
    "@type": "Class",
    "supportedProperty": [{
      "property": "schema:name"
    }, {
      "property": "mov:director"
    }, {
      "property": "schema:genre"
    }]
  }]
}
```

By adding a second value to the `expects` annotation, the client will discover that the payload of this operation is expected to be not only a representation of a `mov:Movie` but also that the request body has to conform to the given SHACL Shape.

{% hint style="info" %}
Client which do not understand SHACL will ignore it and proceed with only the `hydra:Class` description.
{% endhint %}

For clarity the `mov:NewMovieShape` is presented separately as it would appear in the API Documentation resource or as its own individual resource.

```json
{
  "@id": "mov:NewMovieShape",
  "@type": "sh:NodeShape",
  "sh:targetClass": "mov:Movie",
  "sh:property": [{
    "sh:path": "schema:name",
    "sh:or": [{
      "sh:datatype": "xsd:string"
    }, {
      "sh:datatype": "rdf:langString"
    }],
    "sh:name": "Title",
    "sh:description": "Movie's title",
    "sh:minCount": 1
  }, {
    "sh:path": "mov:director",
    "sh:class": "schema:Person",
    "sh:nodeKind": "sh:IRI",
    "sh:maxCount": 1
  }, {
    "sh:path": "schema:genre",
    "sh:in": [
      "Comedy",
      "Drama",
      "Documentary"
    ]
  }]
}
```

While the Shape repeats some of the information typically found on the supported class `mov:Movie` it is clear that it can also provide much more information about the expected payload of the request to create a movie resource, which would be otherwise impossible with just Hydra Core terms:

1. About `schema:name` property:
   * At least one value is required
   * There can be multiple values
   * Values can be a plain string of string with language tag
2. About `mov:director` property:
   * Must be a resource identifier of a `schema:Person` resource
   * There can be at most one value
3. About `schema:genre` property:
   * Values should be one of those provided by the `sh:in` enumeration

{% hint style='info' %}
The only gray area is the director property, where it is up to the client to figure out the available instances of `schema:Person`
{% endhint %}

### Describing IRI templates

Another way for a client to create HTTP requests is by filling an IRI template with string values and dereferencing the resulting identifier. This works well for example for filtering a collection with query strings.

An instance of tht `mov:Movies` class above could provide such a search template to filter movies by genre.

```json
{
  "@id": "movies",
  "@type": ["Collection", "mov:Movies"],
  "search": {
    "@type": "IriTemplate",
    "template": "movies{?genre}",
    "mapping": [{
      "@type": "IriTemplateMapping",
      "variable": "slug",
      "property": "schema:genre"
    }]
  }
}
```

While this is enough to match an existing resource which has the `schema:genre` property, it is too little information to gather user input, such as with a form on a web page.

To enrich the template definition, a SHACL shape can be added to further specify the template mappings.

```diff
{
   "@id": "movies",
   "@type": ["Collection", "mov:Movies"],
   "search": {
     "@type": "IriTemplate",
     "template": "movies{?genre}",
     "mapping": [{
       "@type": "IriTemplateMapping",
       "variable": "slug",
       "property": "schema:genre"
-    }]
+    }],
+    "hashi:shape": {
+      "@type": "sh:Shape",
+      "rdfs:label": "Search movies collection",
+      "sh:property": [{
+        "sh:path": "schema:genre",
+        "sh:name": "Genre",
+        "sh:in": ["Comedy", "Drama", "Documentary"]
+      }]
+    }
   }
}
```

Just like in the shape to create a new movie, the values of `schema:genre` property would be described as an enumeration of string values to present in a select box.

<img alt="select example" src="/images/search-select.png" width="400px">
