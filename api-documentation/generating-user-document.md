# Generating user document

## Story

> As an application user<br>
>I want to be presented with some user readable API documentation<br>
>So I can link it to my project's documentation for business and project
unrelated people.

<!-- -->

> As an API consumer<br>
> I want to be able to fetch entry-point's API documentation<br>
> So I can build an nice looking API documentation.


## Usage

```typescript
const apiDocumentation = await hydraClient.getApiDocumentation("http://temp.uri/");
for (const supportedClass of apiDocumentation.supportedClasses) {
    docs.classes[supportedClass.iri] = generateDocumentationFor(supportedClass);
    for (const supportedProperty of supportedClass.supportedProperties) {
        for (const operation of supportedProperty.supportedOperations) {
            docs.operations[operation.target] = generateDocumentationFor(operation);
        }
    }
}

const entryPoint = await apiDocumentation.getEntryPoint();
for (const link of entryPoint.hypermedia.links) {
    docs.links[link.iri] = generateDocumentationFor(link);
}
```


## Details

It is common to create project documentations, including end point details.
By providing detailed description of the entry point API, it is possible to 
create automated documentation generators.

Communication would look like those described in [this](./entry-point.md), 
[this](./overview.md) and [this](./data-structures.md)
documents.

Resulting documentation should contain these details:
- classes (obtained from API documentation):
  - name
  - display name (label)
  - description
  - properties:
    - name
    - display name (label)
    - description
    - type (datatype or class IRI)
    - cardinality (optional)
    - operations:
      - URL
      - display name (label)
      - description
      - HTTP method
      - expected body:
        - type
        - description
- links (obtained from entry point):
  - URL
  - display name (label)
