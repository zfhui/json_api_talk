slidenumbers: false
slidecount:true
autoscale: false
build-lists: true

[.slidenumbers: false]
[.slidecount: false]
# [fit] _An Introduction to_
# [fit] { json:api }

---
![right](fei.jpg)

# [fit] _Zhuo-Fei Hui_
# __@zfhui__
# Blinkist

^
- Idea initiated at DaWanda
- taking Ruby monoliths apart into micro services
- build 1st micro service with json-api specs

---
## Problem

**RESTful APIs using JSON**
We don't have a shared understanding about the structure.

^
- snake case / camel case
- plural / singular
- endpoint design
- associations (links or embedding)
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


^
- a group of people (Yahuda Katz, Steven Klabnik)

---

## HTTP Request & Response Header

`Content-Type: application/vnd.api+json`

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
* resource object
* `data`: the root
* a identifier objects: ID, TYPE, (ATTRIBUTES, ...)

---
[.autoscale: false]

`POST /users`

```json
{
  "data": {
    "type": "users",
    "attributes": { "name": "Yehuda Katz" }
  }
}
```

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
[.autoscale: false]

`PATCH /users/2`

```json
{
  "data": {
    "type": "users",
    "id": "2",
    "attributes": { "name": "Dan Gebhardt" }
  }
}
```

---
[.autoscale: false]

`DELETE /users/2`

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

---
[.autoscale: false]

`GET /articles/2`

```json
{
  "data": {
    "id": "1",
    "type": "articles",
    "attributes": {
      "title": "Intro to JSON API",
      "content": "Lorem Opossum ..."
    },
    "relationships": {
      "user": {
        "data": { "id": "1", "type": "users" }
      }
    }
  }
}
```

---
[.autoscale: false]

`GET /users/1?include=articles`

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
    },
    "included": [
      {
        "id": "2",
        "type": "articles",
        "attributes": { "title": "Intro to JSON API", "content": "Lorem Opossum..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Lorem Opossum..." }
      }
    ]
  }
}
```

^
Side Loading

---
[.autoscale: false]

`GET /users/1?include=articles.title`

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
    },
    "included": [
      {
        "id": "2",
        "type": "articles",
        "attributes": { "title": "Intro to JSON API" }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding" }
      }
    ]
  }
}
```

^
Compound Documents

---

[.autoscale: false]

`POST /users/1`

```json
{
  "data": {
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
* when referred articles already exist
* creating more than one resource with one POST request is currently not suported
* but [will be supported in the future](https://github.com/json-api/json-api/issues/795)

---

[.autoscale: false]

`POST /users/1/articles/new`

```json
{
  "data": [
    {
      "type": "articles",
      "attributes": { "title": "Intro to JSON API", "content": "Lorem Opossum..." }
    },
    {
      "type": "articles",
      "attributes": { "title": "Intro to JSON API", "content": "Lorem Opossum..." }
    }
  ]
}
```

---
[autoscale: false]

`PATCH /users/1`

```json
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Steve Klabnik" },
    "relationships": {
      "articles": {
        "data": [
          { "id": "3", "type": "articles" },
          { "id": "6", "type": "articles" }
        ]
      }
    }
  }
}
```

^
Replaces all article relationships for the user.

---

## More things ...

... error objects, meta objects, links objects, pagination, versioning, ...

---

## Implementation

---

## gem "jsonapi-resources"

```shell
rails g jsonapi:resource user
  create  app/resources/user_resource.rb
```

```Ruby
class UserResource < JSONAPI::Resource; end
```

```
rails g jsonapi:resource article
  create  app/resources/user_resource.rb
```

```Ruby
class ArticleResource < JSONAPI::Resource; end
```

---

```Ruby
# config/routes.rb


Rails.application.routes.draw do
  # resources :articles
  # resources :users

  jsonapi_resources :articles
  jsonapi_resources :users
end
```

---

## Versioning

JSON API is stricly additive

---
## Authorization

---
## Testing

---
[.autoscale: true]
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
## References

- __Talk__ ["The JSON API Spec" by Marco Otto-Witte](https://www.youtube.com/watch?v=RSv-Yv3cgPg)

---
## json:api vs GraphQL

- GraphQL is protocol agnostic
- JSON API leverages HTTP functionalities, such as:

---
## json:api vs GraphQL

```
GET /users/1` HTTP/1.1
Accept: application/vnd.api+json
```

```
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
ETag: "bf3291afe28105e12b9ff5941a3cf6d7"
```

---
## json:api vs GraphQL

```
GET /users/1` HTTP/1.1
Accept: application/vnd.api+json
If-None-Match: "bf3291afe28105e12b9ff5941a3cf6d7"
```

```
HTTP/1.1 305 Not Modified
```

^
* HTTP caching mechanism
* with HTTP/2: more parallel requests, server push, ...
