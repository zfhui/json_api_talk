slidenumbers: false
slidecount:true
autoscale: false
build-lists: true

[.slidenumbers: false]
[.slidecount: false]
# [fit] _An Introduction to_
# [fit] { json:api }

<!-- ^
- Greeting!
- idea of this talk came to me while I was working on a project at my previous company DaWanda
- taking ruby monoliths apart into micro services
- that project should become a template for other micro services
- build 1st micro service with json-api specs
- this talk was meant as an internal talk
- if you are using it, might be repetative -->

---
![right](fei.jpg)

# [fit] _Zhuo-Fei Hui_
# __@zfhui__
# Blinkist

^
- Fei
- China /  Germany
- Came to Berlin study CS at TU Berlin
- I currently work at Blinkist

---
## Problem

__RESTful APIs using JSON__

^
- maintaining one or probably several APIs during our daily work
- modern APIs are RESTful APIs sending JSON payloads
- PROBLEM: This does not tell us about HOW to structure our APIs

---
## Problem

__RESTful APIs using JSON__
> We don't have a shared understanding about the structure.

^
- If you give several individual developers the same data structure and some use cases and ask each of them to build an API, everyone will come back with a different solution.
- It you put them then into the same room to discuss their solutions, they will tell you why prefer
- camel Case over snake case
- plural / singular
- endpoint design
- resource nesting
- filters, sorting params

---
![](bikeshed.jpg)

## Problem

__Bikeshedding__
> Futile investment of time and energy in discussion of marginal technical issues.
-- Wiktionary

^
- instead of concentrating on more crucial things (e.g. logic, architecture, performance)
- we spent lots of time discussing the same questions over and over again

---
## Problem

Different clients prefer different structures.

^
- iOS: good with fewer requests and large responses
- Android: several requests with small responses
- When we start to cater for all of these needs
- we often times overload the API, it grows and reflects less and less the underlying data structure
- This is bad!
- one change in data structure -> bunch seemingly unrelated endpoints breaks
- caching and invalidating cache becomes more complicated
- clients get data, they don't need

---
## Solution

<br/>

## [fit] { json:api }

<br/>

<!-- ---
## Solution

<br/>

# [fit] { "json": "api" }

<br/> -->

---
[.autoscale: true]
## History

- __2013-05-03__ Yehuda Katz released initial the draft
- __2013-07-21__ media type _application/vnd.api+json_ registration with IANA completed
- __2014-07-05__ `v1.0rc` released
- __2015-05-29__ `v1.0stable` released
- __Today__ `v1.1` still in draft

^
- Katz was building a generic Ember API client
- IANA: International Assigned Numbers Authority
- 3 yrs old specification
- maintainers: Steve Klabnik / Yehuda Katz / Dan Gebhard

---
## jsonapi.org
### _A Specification for Building APIs in JSON_

<br/>

^
URL

---
## jsonapi.org
### _A Specification for Building APIs in JSON_

How __a client__ should request for resources to be fetched or modified.

---
## jsonapi.org
### _A Specification for Building APIs in JSON_

How __a client__ should request for resources to be fetched or modified.
How __a server__ should respond to those requests.

<!-- ---
## jsonapi.org

> By following __Shared Conventions__, you can increase productivity, take advantage of generalized tooling, and focus on what matters: your application.

^
- the promiss
 -->
<!-- ---
## Header

<br/>

---
## Header

__Request__

```http
GET /users/1 HTTP/1.1
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json
```

---
## Header

__Request__

```http
GET /users/1 HTTP/1.1
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json
```

__Response__

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
``` -->

---
## A Simple Resource Object

<br/>

---
## A Simple Resource Object

```ruby
User(id: integer, name: string)
```

---
## Fetching a User

```ruby
User(id: integer, name: string)
```

`GET /users/1`

---
## Fetching a User

```ruby
User(id: integer, name: string)
```

`GET /users/1`

```json
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Steve Klabnik" }
  }
}
```

---
## Fetching a User

```ruby
User(id: integer, name: string)
```

`GET /users/1`

```json, [.highlight: 2, 6]
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Steve Klabnik" }
  }
}
```

^
`data`: the root

---
## Fetching a User

```ruby
User(id: integer, name: string)
```

`GET /users/1`

```json, [.highlight: 3-4]
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Steve Klabnik" }
  }
}
```

^
mandatory: identifier objects: ID, TYPE

---
## Fetching a User

```ruby
User(id: integer, name: string)
```

`GET /users/1`

```json, [.highlight: 5]
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Steve Klabnik" }
  }
}
```

^
optional

---
## Creating a User

`POST /users`

---
## Creating a User

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
## Creating a User

`POST /users`

```json, [.highlight: 2, 5]
{
  "data": {
    "type": "users",
    "attributes": { "name": "Yehuda Katz" }
  }
}
```

---
## Creating a User

`POST /users`

```json, [.highlight: 3]
{
  "data": {
    "type": "users",
    "attributes": { "name": "Yehuda Katz" }
  }
}
```

---
## Creating a User

`POST /users`

```json, [.highlight: 4]
{
  "data": {
    "type": "users",
    "attributes": { "name": "Yehuda Katz" }
  }
}
```

---
## Updating a User

`PATCH /users/2`

^
- send PATCH to endpoint /users/2
- and the information, with which we want to update the user

---
## Updating a User

`PATCH /users/2`

```json
{
  "data": {
    "id": "2",
    "type": "users",
    "attributes": { "name": "Dan Gebhardt" }
  }
}
```

^
- looks almost extactly like creating a user
- id, type and attributes

---
## Updating a User

`PATCH /users/2`

```json, [.highlight: 3]
{
  "data": {
    "id": "2",
    "type": "users",
    "attributes": { "name": "Dan Gebhardt" }
  }
}
```

^
- the payload is hopefully something we are familiar by now
- id, type and attributes

---
## Getting a List of Users

`GET /users`

^
- now we already have 2 users in our DB, let fetch them
- we make a GET request to /users endpoint

---

`GET /users`

```json
{
  "data": [
    {
      "id": "1",
      "type": "users",
      "attributes": { "name": "Steve Klabnik" }
    },
    {
      "id": "2",
      "type": "users",
      "attributes": { "name": "Yehuda Katz" }
    }
  ]
}
```

^
and we get this payload

---
`GET /users`

```json, [.highlight: 2, 13]
{
  "data": [
    {
      "id": "1",
      "type": "users",
      "attributes": { "name": "Steve Klabnik" }
    },
    {
      "id": "2",
      "type": "users",
      "attributes": { "name": "Yehuda Katz" }
    }
  ]
}
```

^
- we have the data key in our root
- this time with an array as its value

---
`GET /users`

```json, [.highlight: 3-7]
{
  "data": [
    {
      "id": "1",
      "type": "users",
      "attributes": { "name": "Steve Klabnik" }
    },
    {
      "id": "2",
      "type": "users",
      "attributes": { "name": "Yehuda Katz" }
    }
  ]
}
```

^
- each of the element in the array is an individual user resource object
- id, type, attributes
- this is our 1st user Steve Klabnik

---
`GET /users`

```json, [.highlight: 8-12]
{
  "data": [
    {
      "id": "1",
      "type": "users",
      "attributes": { "name": "Steve Klabnik" }
    },
    {
      "id": "2",
      "type": "users",
      "attributes": { "name": "Yehuda Katz" }
    }
  ]
}
```

^
- this is our 2nd user Yehuda Katz

---
## Deleting a User

`DELETE /users/2`

^
We get a 204 back.

---
## 1:n Relationship

<br/>

^
- next thing I wanna show you is how on-to-many relationship looks like

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
## Fetching a User

`GET /users/1`

---
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
`GET /users/1`

```json, [.highlight: 2-5]
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
`GET /users/1`

```json, [.highlight: 6-13]
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
`GET /users/1`

```json, [.highlight: 6, 13]
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
`GET /users/1`

```json, [.highlight: 7, 12]
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
`GET /users/1`

```json, [.highlight: 8, 11]
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
`GET /users/1`

```json, [.highlight: 9, 10]
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
- be aware that we only get the article's id and type, no attributes!
- if we are interested the attributes, we can GET each article respectively

---
`GET /articles/2`

```json
{
  "data": {
    "id": "2",
    "type": "articles",
    "attributes": {
      "title": "Intro to JSON API",
      "content": "Lorem opossum ..."
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
`GET /articles/2`

```json, [.highlight: 5-8]
{
  "data": {
    "id": "2",
    "type": "articles",
    "attributes": {
      "title": "Intro to JSON API",
      "content": "Lorem opossum ..."
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
`GET /articles/2`

```json, [.highlight: 9-13]
{
  "data": {
    "id": "2",
    "type": "articles",
    "attributes": {
      "title": "Intro to JSON API",
      "content": "Lorem opossum ..."
    },
    "relationships": {
      "user": {
        "data": { "id": "1", "type": "users" }
      }
    }
  }
}
```

<!-- ---
`GET /articles/5`

```json
{
  "data": {
    "id": "5",
    "type": "articles",
    "attributes": {
      "title": "Anti-Bikeshedding",
      "content": "Marsupial fur trees ..."
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
`GET /articles/5`

```json, [.highlight: 5-8]
{
  "data": {
    "id": "5",
    "type": "articles",
    "attributes": {
      "title": "Anti-Bikeshedding",
      "content": "Marsupial fur trees ..."
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
`GET /articles/5`

```json, [.highlight: 9-13]
{
  "data": {
    "id": "5",
    "type": "articles",
    "attributes": {
      "title": "Anti-Bikeshedding",
      "content": "Marsupial fur trees ..."
    },
    "relationships": {
      "user": {
        "data": { "id": "1", "type": "users" }
      }
    }
  }
}
``` -->

---
[.autoscale: true]
## Compound Documents

- __Instead of 3 requests:__
- `GET /users/1`
- `GET /articles/2` __and__ `GET /articles/5`
- __We can do 1 request:__
- `GET /users/1?include=articles`

^
- remember what I told you at the beginning about the different preferences of iOS and Android client?
- to reduce number of requests

---
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```

^
- don't worry, this is the most complex response we are going to get into today!

---
`GET /users/1?include=articles`

```json, [.highlight: 2-13]
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```

---
`GET /users/1?include=articles`

```json, [.highlight: 14-25]
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```

---
`GET /users/1?include=articles`

```json, [.highlight: 14, 25]
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```

---
`GET /users/1?include=articles`

```json, [.highlight: 15-19]
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```

---
`GET /users/1?include=articles`

```json, [.highlight: 20-24]
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```

---
`GET /users/1?include=articles`

```json, [.highlight: 18, 23]
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```
^
- if you are not interested in all of articles attributes
- e.g. show a list of user's articles = titles
- JSON API allows you to only include titles

---
`GET /users/1?include=articles.title`

```json, [.highlight: 18, 23]
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
        "attributes": { "title": "Intro to JSON API", "content": "Lorem opossum ..." }
      },
      {
        "id": "5",
        "type": "articles",
        "attributes": { "title": "Anti-Bikeshedding", "content": "Marsupial fur trees ..." }
      }
    ]
  }
}
```
---
`GET /users/1?include=articles.title`

```json, [.highlight: 18, 23]
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

---
## Creating a User with Relationships

`POST /users`

---
`POST /users`

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
- you can create a user with relationships to articles ...

---
`POST /users`

```json, [.highlight: 5-12]
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
- ... by sending the relationship info along your POST request

---
## ☝️ Updating a User with Relationships

`PUT /users/1`

---

☝️ `PUT /users/1`

```json, [.highlight: 6-13]
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Dan Gebhardt" },
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
"relationships" field replaces relationships completely!

---

☝️ `PUT /users/1`

```json, [.highlight: 6-13]
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Dan Gebhardt" },
    "relationships": {
      "articles": {
        "data": []
      }
    }
  }
}
```

^
Empties all relationships to articles.

---

`PUT /users/1`

```json
{
  "data": {
    "id": "1",
    "type": "users",
    "attributes": { "name": "Dan Gebhardt" }
  }
}
```

---
<!-- ## Updating To-One Relationship

`PUT /articles/5/relationships/user`

---
## Updating To-One Relationship

`PUT /articles/5/relationships/user`

```json
{
  "data": { "id": "3", "type": "users" }
}
```

---
## Replacing To-One Relationship

`PUT /articles/5/relationships/user`

```json
{
  "data": { "id": "3", "type": "users" }
}
```

---
## Updating To-One Relationship

`PUT /articles/5/relationships/user`

---
## Updating To-One Relationship

`PUT /articles/5/relationships/user`

```json
{
  "data": null
}
```

---
## Remove To-One Relationship

`PUT /articles/5/relationships/user`

```json
{
  "data": null
}
```

---
## Updating To-Many Relationship

`PUT /users/1/relationships/articles`

#☝️

__Replaces relationships completely!__

---
## Updating To-Many Relationships

☝️ `PUT /users/1/relationships/articles`

```json
{
  "data": [
    { "id": "3", "type": "articles" },
    { "id": "7", "type": "articles" }
  ]
}
```

---
## Replace To-Many Relationships

☝️ `PUT /users/1/relationships/articles`

```json
{
  "data": [
    { "id": "3", "type": "articles" },
    { "id": "7", "type": "articles" }
  ]
}
```

^
Depends on server side implementation:
```html
HTTP/1.1 403 Forbidden
```
```json
{
  "errors": [
    {
      "title": "Complete replacement forbidden",
      "detail": "Complete replacement forbidden for this relationship",
      "code": "FORBIDDEN",
      "status": "403"
    }
  ]
}
```

---
## Remove To-Many Relationship

☝️ `PUT /users/1/relationships/articles`


```json
{
  "data": []
}
```

^
Depends on server side implementation:
```http
HTTP/1.1 403 Forbidden
```
```json
{
  "errors": [
    {
      "title": "Complete replacement forbidden",
      "detail": "Complete replacement forbidden for this relationship",
      "code": "FORBIDDEN",
      "status": "403"
    }
  ]
}
```

--- -->
## Manipulating relationships

Use __POST__ and __DELETE__!

---
## Adding Relationships to a User
<br/>

---
## Adding Relationships to a User

`POST /users/1/relationships/articles`

---
## Adding Relationships to a User

`POST /users/1/relationships/articles`

```json
{
  "data": [
    { "id": "3", "type": "articles" },
    { "id": "7", "type": "articles" }
  ]
}
```

---
## Deleting a User's Relationships

`DELETE /users/1/relationships/articles`

```json
{
  "data": [
    { "id": "3", "type": "articles" },
    { "id": "7", "type": "articles"}
  ]
}
```

---

![](hamster_band.gif)

^
You've earned yourself a small break!

---
[.autoscale: true]
## Things we didn't discuss

* __-__ error objects, meta objects, links objects
* __-__ n:m relationships
* __-__ pagination, sorting, filtering, sparse fieldset
* __-__ does not support creating nested resources

^
- but [will be supported in the future](https://github.com/json-api/json-api/issues/795)

^

---
[.autoscale: true]
## To summarise ...

- __-__ ultimate anti-bikeshedding tool
- __-__ one endpoint to serve different client needs
- __-__ tight coupling between the data and underlying data structure
- __-__ leverages HTTP content negotiation mechanism

^
- let the client choose the granularity & how
- reflects the underlying data structure, instead of being loosely related or even arbitrarily merged
- one endpoint: without overloading it
- well-defined resources can improve cacheability
- HTTP: caching mechanism

---
[.autoscale: true]
## [fit] __Implementation__ _with_ JSONAPI::Resources

* __-__ most compliant with { json:api }
* __-__ maintained by Larry Gebhardt
* __-__ relies heavily on Active Record Models
* __-__ currently at `v0.9` and `v0.10beta`

^
- brother of Dan Gebhardt - primary author & maintainer of { json:api }
- Active Record Models: functionalities/logic seems obvious/familiar
- but at the same time inflexible, lots of overwriting
- you know what to expect, before writing a line of code
- the clients can already start mocking

---
[.autoscale: true]
## Create your new Rails API

- `rails new my-new-json-api --api`
- `rails g scaffold user name:string`
- `rails g scaffold article title:string content:text`
- `rails db:setup`
- Add `gem "jsonapi-resource"` to your Gemfile
- `bundle install`

---
## Generate JSONAPI::Resources

<br/>

---
## Generate JSONAPI::Resources

```shell
rails g jsonapi:resource user
```

---
## Generate JSONAPI::Resources

```shell
rails g jsonapi:resource user
  create  app/resources/user_resource.rb
```

---
## Generate JSONAPI::Resources

```shell
rails g jsonapi:resource user
  create  app/resources/user_resource.rb
```

```ruby
class UserResource < JSONAPI::Resource; end
```

---
<!-- ## Generate JSONAPI::Resources

```shell
rails g jsonapi:resource user
  create  app/resources/user_resource.rb
```

```ruby
class UserResource < JSONAPI::Resource; end
```

<br/>

```shell
rails g jsonapi:resource article
  create  app/resources/article_resource.rb
```

```ruby
class ArticleResource < JSONAPI::Resource; end
```

--- -->
## Generate JSONAPI::Resources

```shell
rails g jsonapi:resource user
  create  app/resources/user_resource.rb
```

```ruby
class UserResource < JSONAPI::Resource



end
```

---
## Generate JSONAPI::Resources

```shell
rails g jsonapi:resource user
  create  app/resources/user_resource.rb
```

```ruby
class UserResource < JSONAPI::Resource
  attribute :name


end
```

---
## Generate JSONAPI::Resources

```shell
rails g jsonapi:resource user
  create  app/resources/user_resource.rb
```

```ruby
class UserResource < JSONAPI::Resource
  attribute :name
  has_many :articles,
           always_include_linkage_data: true
end
```

---
## <br/>

```ruby
# app/resources/article_resource.rb

class ArticleResource < JSONAPI::Resource
  attributes :title, :content
  has_one :author,
          always_include_linkage_data: true,
          foreign_key: :user_id
end
```

---
## <br/>

```ruby, [.highlight: 5, 7]
# app/resources/article_resource.rb

class ArticleResource < JSONAPI::Resource
  attributes :title, :content
  has_one :author,
          always_include_linkage_data: true,
          foreign_key: :user_id
end
```

---
## <br/>

```ruby
# config/routes.rb

Rails.application.routes.draw do
  resources :articles
  resources :users
end
```

---
## <br/>

```ruby, [.highlight: 4,5]
# config/routes.rb

Rails.application.routes.draw do
  resources :articles
  resources :users
end
```

---
## <br/>

```ruby, [.highlight: 4,5]
# config/routes.rb

Rails.application.routes.draw do
  jsonapi_resources :articles
  jsonapi_resources :users
end
```

---
## <br/>

```ruby
# app/controllers/application_controller.rb

class ApplicationController < ActionController::API

end
```
---
## <br/>

```ruby
# app/controllers/application_controller.rb

class ApplicationController < ActionController::API
  include JSONAPI::ActsAsResourceController
end
```
---
## <br/>

```ruby
# app/controllers/users_controller.rb

class UsersController < ApplicationController
  def index
    ...
  def create
    ...
  def update
    ...
    ...
end
```

---
## <br/>

```ruby
# app/controllers/users_controller.rb

class UsersController < ApplicationController; end
```

---
## <br/>

```ruby
# app/controllers/users_controller.rb

class UsersController < ApplicationController; end
```

<br/>

```ruby
# app/controllers/articles_controller.rb

class ArticlesController < ApplicationController; end
```

---
![filter](hamster_party.gif)
## [fit] You've just created a JSON API_!_
<br/>

---
[.autoscale: true]
## Where to go from here?

- __Only Readable API__: [`fast_jsonapi`](https://github.com/Netflix/fast_jsonapi) /  [`active_model_serializers`](https://github.com/rails-api/active_model_serializers)
- __Tests:__ [`jsonapi-rspec`](https://github.com/jsonapi-rb/jsonapi-rspec)
- __Authorization:__ [`jsonapi-authorization`](https://github.com/venuu/jsonapi-authorization)
- __Client:__ [`json-api-client`](https://github.com/JsonApiClient/json_api_client) / [`jsonapi-consumer`](https://github.com/jsmestad/jsonapi-consumer)

---

## jsonapi-suite.org

__A collection of ruby libraries that facilitate the jsonapi.org specification.__

- __-__ serialiser and deserialiser for { json:api }
- __-__ spec helpers
- __-__ helpers for swagger documentation

---
[.autoscale: true]
## References

- __Talk__ [JSON API: convention driven API design](https://youtu.be/FpS_E90-6O8) by Steve Klabnik
- __Talk__ [Past, Present and Future of JSON API](https://youtu.be/Foi54om6oGQ) by Steve Klabnik
- __Website__ [Media Type Specs](https://www.iana.org/assignments/media-types/application/vnd.api+json)
- __Talk__ ["The JSON API Spec"](https://youtu.be/RSv-Yv3cgPg) by Marco Otto-Witte

- __Talk__ ["Pragmatic JSON API Design"](https://www.youtube.com/watch?v=3jBJOga4e2Y&index=4&list=PLNLa8ejyRhUt3TjSjvr2T4rE1kGbIfJc1&t=0s) by Jeremiah Lee

- __Podcast__ ["Dan Gebhard - json-api, jsonapi-resources, orbit.js & Ember Data"](http://5by5.tv/rubyonrails/187) by Byle Daigle

- __Podcast__ ["Data Loading Patterns with the JSON API with Balint Erdi"](https://frontsidethepodcast.simplecast.fm/65) by The Frontside Podcast

---

# Thanks_!_

---
```ruby
# app/resources/user_resource.rb

class UserResource < JSONAPI::Resource
  attributes :name, :first_name, :last_name

  has_many :articles, always_include_linkage_data: true

  def name
    "#{@model.first_name} #{@model.last_name}"
  end

  def fetchable_fields
    %i(name articles)
  end

  def self.creatable_fields(_context)
    %i(first_name last_name articles)
  end

  def self.updatable_fields(_context)
    %i(first_name last_name articles)
  end
end
```

^
Final version of UserResource

---
## Yehuda Katz

__{ json:api }__ is a wire protocol for incrementally fetching and updating a graph over HTTP.

^
- wire protocol: it includes both a format for resources and a specification of operation that you can do on those resource
- incrementally: clients can fetch data as they need it at the appropriate granularity.
- fetching and updating, which means there is a defined way to modify resources that you fetched
- graph structure: because data are linked together in a well-defined way
- HTTP: it uses HTTP protocol, including it's caching semantics, as its backbone

[.footer: https://twitter.com/wycats/status/925847356532125696]

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
* with HTTP/2: more parallel requests, server push, etc.
