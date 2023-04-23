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
- resume at https://www.howtographql.com/advanced/2-more-graphql-concepts/
