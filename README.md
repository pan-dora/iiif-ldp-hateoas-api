## IIIF-LDP-HATEOAS-API

This repository contains an alternative client-server interaction model to the IIIF Presentation API 3.0.

- Rather than enforcing a Domain API by means of concrete monolithic JSON serializations, it provides a functional interface.

- The interface is served from an RDF dataset graph backed by Trellis LDP and Activity Stream Events.  

- The data model is pure RDF with JSON-LD being the serialization Content-Type.

- The intent is to facilitate "smart" clients that depend on LDP / HATEOAS and a modular service architecture.

- It will be built similarly to the GitHub API.

## Examples

All Objects indicated with `:some-object` are `<https://www.w3.org/ns/ldp#Resource>`

#### Summary Representation

These responses always contain an `@list`.

List all organizations

> GET /organizations

```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/organizations",
  "type": "Collection",
  "items": []
}
```

Fetch collections for an organization

Where Organization Resource IRI = `<https://api.repository.org/orgs/org-UUID>`

> GET /orgs/:org/collections

```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/orgs/org-UUID/collections",
  "type": "Collection",
  "items": []
}
```

Fetch manifests for a collection

Where Collection Resource IRI = `<https://api.repository.org/orgs/org-UUID/collections/collection-UUID>`

> GET /orgs/:org/collections/:collection/manifests

```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/orgs/org-UUID/collections/collection-UUID/manifests",
  "type": "Collection",
  "items": []
}
```
Fetch all manifests for an organization

> GET /orgs/:org/manifests

```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/orgs/org-UUID/manifests",
  "type": "Collection",
  "items": []
}
```

Fetch ranges for a manifest 

Where Manifest Resource IRI = `<https://api.repository.org/manifests/org-UUID/manifest-UUID>`

> GET /manifests/:org/:manifest/ranges


```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/ranges",
  "type": "Range",
  "items": []
}
```
    
Fetch all annotations for a manifest

> GET /manifests/:org/:manifest/annotations

```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/annotations",
  "items": []
}
```
Fetch sequences for a manifest

> GET /manifests/:org/:manifest/sequences


```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/sequences",
  "type": "Sequence",
  "items": []
}
```
Fetch objects for a sequence

Where Sequence Resource IRI = `<https://api.repository.org/manifests/org-UUID/manifest-UUID/sequences/sequence-UUID>`

> GET /manifests/:org/:manifest/sequences/:sequence/canvases

```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/sequences/sequence-UUID/canvases",
  "type": "Sequence",
  "items": []
}
```
Fetch annotations for a canvas

Where Canvas Resource IRI = `<https://api.repository.org/manifests/org-UUID/manifest-UUID/canvases/canvas-UUID>`

> GET /manifests/:org/:manifest/canvases/:canvas/annotations

```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/canvases/canvas-UUID/annotations",
  "items": []
}
```
Note here that annotations are NEVER embedded objects.  The request URI provides the criteria that allows the association to a canvas
using the `target` property of an annotation.

#### Detailed Representation

Fetch a collection for an organization

> GET /collections/:org/:collection
```apache
Status: 200 OK
Link: <https://api.repository.org/orgs/org-UUID/collections/collection-UUID/manifests>; rel="next",
      <https://api.repository.org/org-UUID/collections>; rel="collection"
```
```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/collections/org-UUID/collection-UUID",
  "type": "Collection",
  "label": "Top Level Collection for Example Organization",
  "renderingHint": "top",
  "description": "Description of Collection",
  "attribution": "Provided by Example Organization"
}
```

Fetch manifest properties 

> GET /manifests/:org/:manifest
```apache
Status: 200 OK
Link: <https://api.repository.org/manifests/org-UUID/manifest-UUID/sequences>; rel="next",
      <https://api.repository.org/orgs/org-UUID/collections/collection-UUID/manifests>; rel="collection"
```
```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID",
  "type": "Manifest",
  "label": {
    "en": [
      "Book 1"
    ]
  },
  "renderingDirection": "right-to-left",
  "renderingHint": [
    "paged"
  ],
  "navDate": "1856-01-01T00:00:00Z",
  "rights": [
    {
      "id": "https://api.repository.org/repository/binaries/binary-UUID",
      "type": "Text",
      "language": "en",
      "format": "text/html"
    }
  ],
  "attribution": {
    "en": [
      "Provided by Example Organization"
    ]
  },
  "logo": {
    "id": "https://api.repository.org/repository/binaries/binary-UUID",
    "service": {
      "id": "https://example.org/service/inst1",
      "type": "ImageService3",
      "profile": "level2"
    }
  },
  "related": [
    {
      "id": "https://api.repository.org/repository/binaries/binary-UUID",
      "type": "Video",
      "label": {
        "en": [
          "Video discussing this book"
        ]
      },
      "format": "video/mpeg"
    }
  ],
  "service": [
    {
      "id": "https://example.org/service/example",
      "type": "Service",
      "profile": "https://example.org/docs/example-service.html"
    }
  ],
  "seeAlso": [
    {
      "id": "https://api.repository.org/repository/binaries/binary-UUID",
      "type": "Dataset",
      "format": "text/xml",
      "profile": "https://example.org/profiles/bibliographic"
    }
  ],
  "rendering": [
    {
      "id": "https://api.repository.org/repository/binaries/binary-UUID",
      "type": "Text",
      "label": {
        "en": [
          "Download as PDF"
        ]
      },
      "format": "application/pdf"
    }
  ],
  "within": [
    {
      "id": "https://api.repository.org/orgs/org-UUID/collections/collection-UUID",
      "type": "Collection"
    }
  ],
  "startCanvas": {
    "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/canvases/canvas-UUID",
    "type": "Canvas"
  }
}
```

Fetch metadata for a manifest


> GET /manifests/:org/:manifest/metadata
```apache
Status: 200 OK
Link: <https://api.repository.org/manifests/org-UUID/manifest-UUID/sequences>; rel="next",
      <https://api.repository.org/orgs/org-UUID/collections/collection-UUID/manifests>; rel="collection"
```
```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID",
  "type": "Manifest",
  "metadata": [
    {
      "label": {
        "en": [
          "Author"
        ]
      },
      "value": {
        "@none": [
          "Anne Author"
        ]
      }
    },
    {
      "label": {
        "en": [
          "Published"
        ]
      },
      "value": {
        "en": [
          "Paris, circa 1400"
        ],
        "fr": [
          "Paris, environ 1400"
        ]
      }
    },
    {
      "label": {
        "en": [
          "Notes"
        ]
      },
      "value": {
        "en": [
          "Text of note 1",
          "Text of note 2"
        ]
      }
    },
    {
      "label": {
        "en": [
          "Source"
        ]
      },
      "value": {
        "@none": [
          "<span>From: <a href=\"https://example.org/db/1.html\">Some Collection</a></span>"
        ]
      }
    }
  ]
}
```

Fetch a manifest sequence

> GET /manifests/:org/:manifest/sequences/:sequence

```apache
Status: 200 OK
Link: <https://api.repository.org/manifests/org-UUID/manifest-UUID/sequences/sequence-UUID/canvases>; rel="next",
      <https://api.repository.org/manifests/:org/:manifest/sequences>; rel="collection"
```
```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/sequences/sequence-UUID",
  "type": "Sequence",
  "label": {
    "en": [
      "Current Page Order"
    ]
  },
  "renderingDirection": "left-to-right",
  "renderingHint": [
    "paged"
  ],
  "startCanvas": "https://api.repository.org/manifests/org-UUID/manifest-UUID/canvases/canvas-UUID"
}  
```
  
Fetch a canvas

> GET /manifests/:org/:manifest/canvases/:canvas

```apache
Status: 200 OK
Link: <https://api.repository.org/manifests/:org/:manifest/canvases/:canvas/annotations>; rel="next",
      <https://api.repository.org/manifests/:org/:manifest/canvases>; rel="collection"
```
```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/canvases/canvas-UUID",
  "type": "Canvas",
  "label": {
    "@none": [
      "p. 1"
    ]
  },
  "height": 1000,
  "width": 750,
  "duration": 180.0
} 
```
  
Fetch a canvas annotation

> GET /manifests/:org/:manifest/annotations/:annotation

```apache
Status: 200 OK
Link: <https://api.repository.org/manifests/org-UUID/manifest-UUID/annotations>; rel="collection"
```
```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/org-UUID/manifest-UUID/annotations/annotation-UUID",
  "type": "Annotation",
  "motivation": "painting",
  "body": {
    "id": "https://api.repository.org/repository/binaries/binary-UUID",
    "type": "Image",
    "format": "image/jpeg",
    "service": {
      "id": "https://example.org/images/book1-page1",
      "type": "ImageService3",
      "profile": "level2"
    },
    "height": 2000,
    "width": 1500
  },
  "target": "https://api.repository.org/manifests/org-UUID/manifest-UUID/canvases/canvas-UUID"
}
```

### No Recursion

A single detail will never include summary lists, but only the properties for that resource.  This supports
service encapsulation and caching. 

### HATEOAS

A response for a resource will include Link Headers with paged relations to its associated members if they exist.

> GET /manifests/:org/:manifest/canvases/:canvas

#### Response
```apache
Status: 200 OK
Link: <https://api.repository.org/manifests/:org/:manifest/canvases/:canvas/annotations>; rel="next",
      <https://api.repository.org/manifests/:org/:manifest/canvases>; rel="collection"
```
```json
{
  "@context": [
    "https://www.w3.org/ns/anno.jsonld",
    "https://iiif.io/api/presentation/3.0/context.json"
  ],
  "id": "https://api.repository.org/manifests/manifest-UUID/canvases/canvas-UUID",
  "type": "Canvas",
  "label": {
    "@none": [
      "p. 1"
    ]
  },
  "height": 1000,
  "width": 750,
  "duration": 180.0
} 
```

