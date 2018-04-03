slidenumbers: false
slidecount:true
autoscale: true
build-lists: true

[.slidenumbers: false]
[.slidecount: false]
# [fit] _An Introduction to_
# [fit] { json:api }

---
![left](fei-placeholder.jpg)

## _Zhuo-Fei Hui_
## __@zfhui__
## Blinkist

^
- Idea initiated at DaWanda
- taking Ruby monoliths apart into micro services
- build 1st micro service with JSON:API
- share the knowledge in an internal talk

---
## Problem

**RESTful APIs using JSON**
We don't have a shared understanding about the structure.

^
- snake case / camel case
- plural / singular
- endpoint design
- resource nesting
- partial / complete update
- filters, sorting, meta info?
- variations of the same thing
- new client for every API
- BIKESHEDDING

---
![](bikeshed.jpg)

## Problem

**Bikeshedding**
"Futile investment of time and energy in discussion of marginal technical issues."
_— Wiktionary_

---
## { json:api }
### _A Specification for Building APIs in JSON_

How __a client__ should request for resources to be fetched or modified.

How __a server__ should respond to those requests.

---
## { json:api }

"By following __shared conventions__, you can increase productivity, take advantage of generalized tooling, and focus on what matters: your application."
_— jsonapi.org_

---
## A Simple Resource Object

```ruby
User(id: integer, name: string)
```

`GET /users/1`

```json
{
  "data": {
    "type": "users",
    "id": "1",
    "attributes": { "name": "Steve Klabnik" }
  }
}
```

^
A Resource Object: ID, TYPE, (ATTRIBUTES, ...)
---
[.autoscale: false]

`GET /users`

```json
{
  "data": [
    {
      "type": "users",
      "id": "1",
      "attributes": { "name": "Steve Klabnik" }
    },
    {
      "type": "users",
      "id": "2",
      "attributes": { "name": "Yehuda Katz" }
    }
  ]
}
```

---
## 1:n Relationship

```ruby
User(id: integer, name: string)

Article(
  id: integer,
  title: string,
  content: text,
  user_id: integer
)
```
---
[.autoscale: false]

`GET /users/1`

```json
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Steve Klabnik" },
    "relationships": {
      "articles": {
        "data": [
          { "id": "2", "type": "articles" },
          { "id": "5", "type": "articles" }
        ]
      }
    }
  }
}
```

^
A Resource Identifier Object: ID, TYPE

---

## Compound Documents

`GET /users/1?included`

---
## Authentication

---
## Authorization

---
## Testing

---
## Documentation

---
## History

- __2013-05-03__ Yehuda Katz released initial draft
- __2013-07-21__ media type registration with IANA completed
- __2014-07-05__ `v1.0rc` released
- __2015-05-29__ `v1.0stable` released
- __Today__ `v1.1` still in draft

^
- Katz was building a generic Ember API client
- IANA: International Assigned Numbers Authority
- 3 yrs old specification
- maintainers: Steve Klabnik / Yehuda Katz / Dan Gebhard
- all have backgrounds in Rails
- @jsonapi: <300 tweets

---
## Sources

---
## JSON:API vs GraphQL
