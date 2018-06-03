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
<!-- ![right](fei.jpg) -->

### _Zhuo-Fei Hui_
### __@zfhui__
### Blinkist

^
- Fei
- China /  Germany
- Came to Berlin study CS at TU Berlin
- I currently work at Blinkist at a backend developer

---
![](bikeshed.jpg)
## Problem #1

__RESTful APIs using JSON__

^
- I maintaine several APIs during my daily work,
- I assume that's something most of you do too.
- modern APIs are RESTful APIs using JSON
- = we can use CRUD operations with JSON formed payloads to manipulate data, which are sitting behind some endpoints
- PROBLEM: This setup not tell us that much about HOW to design our APIs

^
- RESTful: Representational State Transfer

---
![](bikeshed.jpg)
## Problem #1

__RESTful APIs using JSON__
> We don't have a shared understanding about the structure.

^
When we start building a new API, we discuss a bunch of things:
- camel Case over snake case
- plural / singular
- endpoint design
- resource nesting
- pass in params for filtering, sorting, pagination, ...

^
- Bike-Shedding
- instead of concentrating on more crucial things (e.g. feature, performance, architecture)
- we wast lots of time discussing these very marginal questions

<!-- ---
![](bikeshed.jpg)

## Problem

__Bikeshedding__
> Futile investment of time and energy in discussion of marginal technical issues.
-- Wiktionary

^ -->

---
![](clients.jpg)
## Problem #2

__Overloading__
> Different clients prefer different structures.

^
- sometimes we start building our API for one client
- then we have to adapt it for other clients
- Android: several requests with small responses
- iOS: good with fewer requests and large responses
- When we start to cater for all of these needs
- we often times also start to overload the endpoints
- it grows and reflects less and less the underlying data structure
- This is bad!

^
__You've probably experienced this already:__
- clients get data, they don't need
- one change in data structure -> bunch seemingly unrelated endpoints breaks
- caching and invalidating cache becomes more complicated

---
## Solution

<br/>

## [fit] { json:api }
## [fit] _A Specification for Building APIs in JSON_

<br/>

---
<!-- ---
## Solution

<br/>

# [fit] { "json": "api" }

<br/>

--- -->
## { json:api }

<br/>

^
if you go to the website of the specification: ...

---
## { json:api }

How __a client__ should request for resources to be fetched or modified.

---
## { json:api }
### _Shared Conventions_

How __a client__ should request for resources to be fetched or modified.
How __a server__ should respond to those requests.

---
<!-- ## jsonapi.org

> By following __Shared Conventions__, you can increase productivity, take advantage of generalized tooling, and focus on what matters: your application.

--- -->
[.autoscale: true]
## { json:api }

- __2013-05-03__ Yehuda Katz released initial the draft
- __2013-07-21__ registration of the media type: _application/vnd.api+json_
- __2015-05-29__ `v1.0stable` released
- __Today__ `v1.1` still in draft

^
- Katz met Klabnik at the RailsConf
- client + server can use the media type in their respective request/response header, to tell each other that they are using JSON API

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
[.autoscale: true]
## A Simple Resource Object

```ruby
User(id: integer, name: string)
```

---
## Fetching a User

```http
GET /users/1 HTTP/1.1
Accept: application/vnd.api+json
```

---
## Fetching a User

```http
GET /users/1 HTTP/1.1
Accept: application/vnd.api+json
```

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json,
```

---
## Fetching a User

```http
GET /users/1 HTTP/1.1
Accept: application/vnd.api+json
```

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json,
```

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

<br/>

---
## Creating a User

```http
POST /users HTTP/1.1
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json
```

---
## Creating a User

```http
POST /users HTTP/1.1
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json,
```

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

```http
POST /users HTTP/1.1
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json,
```

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

```http
POST /users HTTP/1.1
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json,
```

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

```http
POST /users HTTP/1.1
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json,
```

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
- and the information, with which we want to update the user

---
## Updating a User

`PATCH /users/2`,

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
- by now familiar with
- id, type and attributes

---
## Fetching a List of Users

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
      "attributes": { "name": "Dan Gebhardt" }
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
      "attributes": { "name": "Dan Gebhardt" }
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
      "attributes": { "name": "Dan Gebhardt" }
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
      "attributes": { "name": "Dan Gebhardt" }
    }
  ]
}
```

---
## Deleting a User

`DELETE /users/1`

---
## Relationships

<br/>

^
- next thing I wanna show you is how on-to-many relationship looks like

---
## Relationships

```ruby
User(id: integer, name: string)
```

---
## Relationships

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

<!-- ---
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
``` -->

---
## Compound Documents

<br/>

---
## Compound Documents

__n+1 requests__ | <br/>
-----------------|----------------------------------
GET /users/1     |
                 |
GET /articles/2  |
GET /articles/5  |
...              |
                 |


^
Remember what I told you at the beginning about the different preferences of iOS and Android client?


---
## Compound Documents

__n+1 requests__ | __1 request__
-----------------|----------------------------------
GET /users/1     |
                 |
GET /articles/2  |
GET /articles/5  |
...              |
                 |

---
## Compound Documents

__n+1 requests__ | __1 request__
-----------------|----------------------------------
GET /users/1     | GET /users/1_?include=articles_
                 |
GET /articles/2  |
GET /articles/5  |
...              |
                 |

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
`GET /users/1`

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

^
Skipping the POST request, because looks similar to Updating.

<!-- ## Creating a User with Relationships

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
- ... by sending the relationship info along your POST request -->

---
## Updating a User with Relationships

`PATCH /users/1`

---
☝️ `PATCH /users/1`

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
A caveat, if you use "relationships" field in your request's body, it replaces a user's relationships completely!

---
☝️ `PATCH /users/1`

```json, [.highlight: 9-10]
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
☝️ `PATCH /users/1`

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
☝️ `PATCH /users/1`

```json, [.highlight: 8]
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

<!-- ---

`PATCH /users/1`

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
## Updating To-One Relationship

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
-->

---
## Manipulating a User's Relationships

<br/>

---
## Manipulating a User's Relationships

 __POST__ and __DELETE__ on __Relationship Links__:

---
## Manipulating a User's Relationships

 __POST__ and __DELETE__ on __Relationship Links__:

</br>

`/users/1/relationships/articles`

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

^
you don't replace existing relationships!

---
## Adding Relationships to a User

`POST /users/1/relationships/articles`

```json, [.highlight: 3, 4]
{
  "data": [
    { "id": "3", "type": "articles" },
    { "id": "7", "type": "articles" }
  ]
}
```

^
you don't replace existing relationships!

---
## Deleting a User's Relationships

`DELETE /users/1/relationships/articles`

```json
{
  "data": [
    { "id": "3", "type": "articles" },
    { "id": "7", "type": "articles" }
  ]
}
```

---

![](hamster_band.gif)

^
That was a lot!
You've earned yourself a small break!
^
Drink some water!

---
[.autoscale: true]
## More Things ...

* __-__ meta objects, links objects
* __-__ pagination, sorting, filtering, sparse fieldset
* __-__ error objects
* __-__ n:m relationships
* __-__ does not support creating nested resources

^
- links object, allows you to write hypermedia APIs with JSON API
- nested resources: but [will be supported in the future](https://github.com/json-api/json-api/issues/795)

---
[.autoscale: true]
## To Summarise ...

- __-__ ultimate anti-bikeshedding tool
- __-__ one endpoint to serve different client needs
- __-__ tight coupling between your API and the underlying data structure
- 🍰 leverages HTTP content negotiation mechanism

^
- let the client choose the granularity & how
- one endpoint: without overloading it
- reflects the underlying data structure, instead of being loosely related or even arbitrarily merged
- well-defined resources can improve cacheability
- HTTP: caching mechanism

---
[.autoscale: true]
### Where to go from here?

- __gems codifying { json:api }__
  - 👀 [`active_model_serializers`](https://github.com/rails-api/active_model_serializers)
  - 👀 [`fast_jsonapi`](https://github.com/Netflix/fast_jsonapi)
  - 💓 [`jsonapi_resources`](http://jsonapi-resources.com/)
  - 🦄 [`jsonapi_suite`](https://jsonapi-suite.org)

^
jsonapi-resources:
* most compliant with { json:api }
* maintained by Larry Gebhardt
* relies heavily on Active Record Models

^
A collection of ruby libraries that facilitate the jsonapi.org specification.
- serializer, deserializer
- specs helpers
- helpers for sweagger documentation

---
[.autoscale: true]
## Where to go from here?

- __Tests:__ [`jsonapi-rspec`](https://github.com/jsonapi-rb/jsonapi-rspec)
- __Authorization:__ [`jsonapi-authorization`](https://github.com/venuu/jsonapi-authorization)

<br/>

- __Client:__ [`json-api-client`](https://github.com/JsonApiClient/json_api_client) / [`jsonapi-consumer`](https://github.com/jsmestad/jsonapi-consumer)

<!-- ---
[.autoscale: true]
[.build-lists: false]
## Reference Implementations

- for [`jsonapi-resource`] (https://github.com/zfhui/json-api-talk-example-1-jsonapi-resources)
- for [`jsonapi-suite`] (https://github.com/zfhui/json-api-talk-example-1-jsonapi-suite)

^
Based on the example use here in this talk!
 -->
---
[.autoscale: true]
[.build-lists: false]
## References

- __Talk__ [JSON API: convention driven API design](https://youtu.be/FpS_E90-6O8) by Steve Klabnik
- __Talk__ [Past, Present and Future of JSON API](https://youtu.be/Foi54om6oGQ) by Steve Klabnik
- __Talk__ [The Road to JSON API 1.0](https://www.infoq.com/presentations/json-api-1) by Steve Klabnik
- __Website__ [Media Type Specs](https://www.iana.org/assignments/media-types/application/vnd.api+json)
- __Talk__ ["The JSON API Spec"](https://youtu.be/RSv-Yv3cgPg) by Marco Otto-Witte

- __Talk__ ["Pragmatic JSON API Design"](https://www.youtube.com/watch?v=3jBJOga4e2Y&index=4&list=PLNLa8ejyRhUt3TjSjvr2T4rE1kGbIfJc1&t=0s) by Jeremiah Lee

- __Podcast__ ["Dan Gebhard - json-api, jsonapi-resources, orbit.js & Ember Data"](http://5by5.tv/rubyonrails/187) by Byle Daigle

- __Podcast__ ["Data Loading Patterns with the JSON API with Balint Erdi"](https://frontsidethepodcast.simplecast.fm/65) by The Frontside Podcast

- __Podcast__ ["JSON API and API Design"](https://changelog.com/podcast/189) by The Changelog

- __Image__ [Devices](https://www.maplewoodlibrary.org/main/uploads/Digital-devices.jpg)

---
![](hamster_party.gif)
# Thanks__!__

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

```http
GET /users/1 HTTP/1.1
Accept: application/vnd.api+json
```

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
ETag: "bf3291afe28105e12b9ff5941a3cf6d7"
```

---
## json:api vs GraphQL

```http
GET /users/1 HTTP/1.1
Accept: application/vnd.api+json
If-None-Match: "bf3291afe28105e12b9ff5941a3cf6d7"
```

```http
HTTP/1.1 305 Not Modified
```

^
* HTTP caching mechanism
* with HTTP/2: more parallel requests, server push, etc.
