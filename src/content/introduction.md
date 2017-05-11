# GraphQL Workshop (Part 1)

# Rangle.io

#### by Rob Brander

---

## Structure

- Slides
- Demo
- Exercise

---

# What is GraphQL?
- Query language and server-side query processor
- A substitute for REST
- Query-based data fetching (as opposed to Entity-based)

NOTE: Today we'll only be covering the query language

---

# GraphQL Concept

Querying
- We can get data in complex shapes
- This workshop will cover the basics for querying

Mutating
- With one mutation we can change multiple things
- A later workshop will cover the basics of mutating

---

# What is a Graph database?

- Data resides in "nodes"
- Connections are made via "edges"
- Connections are first-class entities
- Extremely fast for joining data
- Not dependant on structure, like No-SQL

---

# Pros/Cons

- Pros
  - Decouples your data layer
  - Great for data mocking
  - Caching (Server/Client)
    - Server can cache DB hits
    - Client can cache request hits
  - One end-point
  - Custom data fetching (take what you need, leave the rest)
- Cons
  - More code to manage
  - Duplicating type declarations (PropTypes, ORM)

Notes:

Whenever you are trying to determine what tools to use,
you shoudl evaluate the pros an cons.

---

# Graph-i-QL

- A graphical interactive in-browser GraphQL IDE
- Pronounced "Graphical"
- Great for building/testing queries
- Useful for exploring schema and types

---

# What is SWAPI

- Star Wars Application Programming Interface (API)
  - located at https://swapi.co/
  - A RESTful API
- GraphQL.org created a GraphQL server to interface with the SWAPI
  - located at http://graphql.org/swapi-graphql

---

# Exploring the schema in GraphiQL

- query/mutation
- Root: nodes vs edges
- params: types and required/optional

---

# Writing a query

- There are two types: Query and Mutation
  - only one can be used in a query
- Query == HTTP GET
- Mutation == HTTP POST
- Look like JSON
- Syntax: `queryType [queryName] { entity { relatedEnties/data } }`

---

# Query
### Demo

List all star wars movies with their title and episode number

+++

# Query
### Exercise

List all Star Wars characters, for all films

http://graphql.org/swapi-graphql

---

# Arguments
### Demo
- Work like filters; to reduce resulting dataset
- Can be applied to edges and nodes
- When applied to nodes, server can apply logic (i.e. formatting numbers)
- Required arguments are indicated with an exclamination mark

Notes:

Fetch the top 6 species
then show pagination
query {
  allSpecies(first: 3, after: "YXJyYXljb25uZWN0aW9uOjI=") {
    totalCount
    species {
      name
    }
    pageInfo {
      hasNextPage
      hasPreviousPage
      startCursor
      endCursor
    }
  }
}

+++

# Arguments
### Exercise

1. Find the gravity of Luke Skywalker's homeworld

2. When listing all species with a page size of 3, what are the names of the species on page 2?

---

# Aliases
### Demo

- Useful for providing custom names for result sets
- Improves the readability and intention of query
- Makes it easier to access your data in Javascript
- Return different data from same node

Notes:

Show how you can only request one person
query {
  LukeSkywalker: person(id: "cGVvcGxlOjE=") {
    name
    homeworld {
      name
      gravity
    }
  }
  LeiaOrgana: person(id: "cGVvcGxlOjU=") {
    name
    homeworld {
      name
      gravity
    }
  }
}

+++

# Aliases
### Exercise

Write a query that returns the Luke's homeworld name and Leia's birth year

Notes:

Answer
query {
  LukeSkywalker: person(id: "cGVvcGxlOjE=") {
    name
    homeworld {
      name
    }
  }
  LeiaOrgana: person(id: "cGVvcGxlOjU=") {
    name
    birthYear
  }
}


---

# Directives
### Demo

- `@include` and `@skip`
- accept an `if` argument
- applies to edges and fields
- passed in values only; no conditional statements

Notes:

Use this query:
query ($includeHomeworld:Boolean!) {
  LukeSkywalker: person(id: "cGVvcGxlOjE=") {
    name
    homeworld @include(if:$includeHomeworld){
      name
    }
  }
  LeiaOrgana: person(id: "cGVvcGxlOjU=") {
    name
    birthYear
  }
}

Pass in variabels using the bottom left pane in graphiql
{ "includeHomeworld": true }

Then change the query to apply @include or @skip on person (edge)

+++

# Directives
### Exercise

- Write a query call getLuke with an required parameter `$withDetails`
- Fetch the details (birthYear, eyeColor, gender, and homeworld name) for Luke Skywalker
- when `$withDetails` is false the query should only include his name

Notes:

query getLuke($withDetails:Boolean!) {
  LukeSkywalker: person(id: "cGVvcGxlOjE=") {
    name @skip(if:$withDetails)
    birthYear @include(if:$withDetails)
    eyeColor @include(if:$withDetails)
    gender @include(if:$withDetails)
    homeworld @include(if:$withDetails){
      name
    }
  }
}

OR

query getLuke($withDetails:Boolean!) {
  LukeSkywalker: person(id: "cGVvcGxlOjE=") @include(if:$withDetails) {
    birthYear
    eyeColor
    gender
    homeworld
      name
    }
  }
  LukeSkywalker: person(id: "cGVvcGxlOjE=") @skip(if:$withDetails) {
    name
  }
}

Arguments:
{ "withDetails": true }

---

# That's it!

- You can learn more at http://graphql.org/learn/
- Part 2 will walk you through making a GraphQL server
- SWAPI: https://swapi.co/
- SWAPI GraphQL: http://graphql.org/swapi-graphql
