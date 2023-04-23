## Architecture
- graphql is itself a specification, that is implemented by a graphql server
    - kind of like OpenGl or Vulkan, they are specifications to be implemented
- three different kinds of architectures that include a graphql server
    - with a connected database
        - I guess this is the most typical, or most similar to a traditional api?
        - most common for greenfield projects
        - the graphql server listens for queries, and then returns the data that the query requested. This is called 'resolving' a query
        - graphql is transport layer agnostic, so we could use tcp or websockets
        - graphql also doesn't care what type of DB you use
    - Wrapper
        - thin layer over third party or legacy systems
    - Hybrid approach
        - with a connected database and as an abstraction over third party or legacy systems

## Resolver Functions
- every field in a query corresponds to a 'resolver function'. A resolver function is called for each of the fields in the query, which could do any number of tasks necessary to prepare the data that is to be sent back to the client
- it looks like actually implementing and using graphql requires you to create the resolvers that can handle your schema. I guess this is where the frontend and backend teams would need to communicate (or rather, just when defining the schema)

## Client Libraries
- graphql client libraries like Appolo and Relay are useful on the frontend because they will do the heavy lifting around getting data from the server for you
    - Appolo is an open source community project
    - Relay is facebooks client-side library
- the client libraries help make the client side code more declarative
- they can be hooked up with frontend frameworks like React, and according to the tutorial work well with FRP(functional reactive programming, which I want to study anyway)
- it is not a good idea to cache the results from a graphql query as-is, because the data can be very nested
    - the article mentions a way to flatten (normalize) the data records and then reference them with an ID, and links to a post by the Apollo team on this subject
- if the build system for the client-side code is aware of the Graphql schema, it can ensure that the client side code is type safe in regards to the schema at compile time

## Server
- resolvers are processed in a breadth-first fashion
- depending on the implementation language, some resolvers can be auto-derived ("default resolvers")
    - for instance, there is no need to specify a resolver in GraphQl.js if an object has an attribute that matches the field name
- depending on the query, the same sub-field could need to be fetched several times, even if the data was the same (like the authors for blog posts on a website)
    - it looks there's some sort of solution where something resembling lazy evaluation can be used to make sure that the same data is only fetched once

## More Concepts
### Fragments
- a collection of fields for a type, that can be referenced by name
- this allows us to avoid having to type out a bunch of fields everytime we use a common pattern
```
type User {
    name: String!
    age: Int!
    email: String!
    street: String!
    zipcode: String!
    city: String!
}
```
for the above type, we can collect some of those fields together and use a shorthand notation for grabbing them from the User type
```
fragment addressDetails on User {
    name
    street
    zipcode
    city
}
```
the three dot syntax can be used to reference a fragment for a type
```
{
    allUsers {
        ... addressDetails
    }
}
```
### Arguments for Fields
the `olderThan` parameter can be used to filter users, and it can be provided a default value. In this case the default value is -1.
```
type Query {
    allUsers(olderThan: Int = -1): [User!]!
}
```
the parameter could be used as such:
```
{
    allUsers(olderThan: 30) {
        name
        age
    }
}
```
where the parameter is specified to be 30. I wonder how the resolver code gets access to these variables?

### Query Result Aliases
when the results of a query would cause a naming conflict at the same nesting level, it is necessary to specify aliases for each of the results, that way the resulting data structure can store the results in a way that they are uniquely named
```
{
  User(id: "1") {
    name
  }
  User(id: "2") {
    name
  }
}
```
here, the 'User' name would cause a conflict so, it is necessary to specify 'variable names' for each of the results:
```
{
    first: User(id: "1") {
        name
    }
    second: User(id: "2") {
        name
    }
}
```

for which the result would be:
```
{
    "first": {
        "name": "Alice"
    },
    "second": {
        "name": "Sarah"
    }
}
```

### Advanced SDL (Schema Definition Language)
There are two kinds of types: Scalar types and Object types
The default Scalar types are String, Int, Float, Boolean and ID.
Object types are named and are composed of other object and scalar types.

- scalar types
    - scalar types can be user-defined
    - a common use case for user-defined scalars would be a Date type, where the format, serialization, and deserialization need to be specified by the developer

- enums
    - enums are used to create a scalar types whose possible values are defined beforehand
    ```
    enum Weekday {
        MONDAY
        TUESDAY
        WEDNESDAY...
    }
    ```

Interfaces are abstract declarations of the fields that a type needs to define in order to satisfy the interface. A common use case for this is an interface that expects a type to have an ID field.
```
interface Node {
    id: ID!
}

type User implements Node {
    id: ID!
    name: String!
    age: Int!
}
```

Unions are like type unions in programming languages, we can define a type Z as being either a type A or a type B (as many as we want). While some fields may be shared between all the types that make up a union type, there will some fields that aren't. In order for that situtation to not become a type error, we can use what are called 'conditional fragments'.

a union type made up of two other types
```
type Adult {
  name: String!
  work: String!
}

type Child {
  name: String!
  school: String!
}

union Person = Adult | Child
```

query
```
{
    allPersons {
        name # works for `Adult` and `Child`
        ... on Child {
            school
        }
        ... on Adult {
            work
        }
    }
}
```
