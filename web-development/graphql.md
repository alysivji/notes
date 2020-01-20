# GraphQL

#### Table of Contents

<!-- TOC -->

- [Queries and Mutations](#queries-and-mutations)
- [Schemas and Types](#schemas-and-types)
  - [GraphQL Schema Language](#graphql-schema-language)
  - [Types](#types)
  - [Interfaces](#interfaces)
- [Validation](#validation)
- [Execution](#execution)
- [Introspection](#introspection)
- [Best Pratcies](#best-pratcies)
  - [Introduction](#introduction)
  - [Thinking in Graphs](#thinking-in-graphs)
  - [Pagination](#pagination)
  - [Global Object Identification](#global-object-identification)
  - [Caching](#caching)

<!-- /TOC -->

## Queries and Mutations

https://graphql.org/learn/queries/

> In a system like REST, you can only pass a single set of arguments - the query parameters and URL segments in your request. But in GraphQL, every field and nested object can get its own set of arguments, making GraphQL a complete replacement for making multiple API fetches. You can even pass arguments into scalar fields, to implement data transformations once on the server, instead of on every client separately.

> While query fields are executed in parallel, mutation fields run in series, one after the other.

## Schemas and Types

https://graphql.org/learn/schema/

> Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service. Then, when queries come in, they are validated and executed against that schema.

### GraphQL Schema Language

> The most basic components of a GraphQL schema are object types, which just represent a kind of object you can fetch from your service, and what fields it has.

### Types

GraphQL comes with a set of default scalar types out of the box:

- `Int`: A signed 32‐bit integer.
- `Float`: A signed double-precision floating-point value.
- `String`: A UTF‐8 character sequence.
- `Boolean`: true or false.
- ID: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an ID signifies th`at` it is not intended to be human‐readable.
- `Enums`: special kind of scalar that is restricted to a particular set of allowed values
- `List`: `[]`
- `Non-Null` (aka required): `!`

### Interfaces

An Interface is an abstract type that includes a certain set of fields that a type must include to implement the interface.

## Validation

https://graphql.org/learn/validation/

> By using the type system, it can be predetermined whether a GraphQL query is valid or not. This allows servers and clients to effectively inform developers when an invalid query has been created, without having to rely on runtime checks.

> a fragment cannot refer to itself or create a cycle, as this could result in an unbounded result

## Execution

https://graphql.org/learn/execution/

> You can think of each field in a GraphQL query as a function or method of the previous type which returns the next type. In fact, this is exactly how GraphQL works. Each field on each type is backed by a function called the resolver which is provided by the GraphQL server developer. When a field is executed, the corresponding resolver is called to produce the next value.
>
> If a field produces a scalar value like a string or number, then the execution completes. However if a field produces an object value then the query will contain another selection of fields which apply to that object. This continues until scalar values are reached. GraphQL queries always end at scalar values.

> At the top level of every GraphQL server is a type that represents all of the possible entry points into the GraphQL API, it's often called the Root type or the Query type.

## Introspection

https://graphql.org/learn/introspection/

> We designed the type system, so we know what types are available, but if we didn't, we can ask GraphQL, by querying the __schema field, always available on the root type of a Query. Let's do so now, and ask what types are available.

## Best Pratcies

### Introduction

https://graphql.org/learn/best-practices/

- `Accept-Encoding: gzip`
- While there's nothing that prevents a GraphQL service from being versioned just like any other REST API, GraphQL takes a strong opinion on avoiding versioning by providing the tools for the continuous evolution of a GraphQL schema.
- in a GraphQL type system, every field is nullable by default

### Thinking in Graphs

https://graphql.org/learn/thinking-in-graphs/

- model your business domain as a graph by defining a schema; within your schema, you define different types of nodes and how they connect/relate to one another
- Your business logic layer should act as the single source of truth for enforcing business domain rules

### Pagination

https://graphql.org/learn/pagination/

### Global Object Identification

https://graphql.org/learn/global-object-identification/

> The GraphQL schema is formatted to allow fetching any object via the node field on the root query object. This returns objects which conform to a "Node" interface.

### Caching

https://graphql.org/learn/caching/

> In the same way that the URLs of a resource-based API provided a globally unique key, the id field in this system provides a globally unique key.

> If the backend uses something like UUIDs for identifiers, then exposing this globally unique ID may be very straightforward! If the backend doesn't have a globally unique ID for every object already, the GraphQL layer might have to construct this. Oftentimes, that's as simple as appending the name of the type to the ID and using that as the identifier; the server might then make that ID opaque by base64-encoding it.
