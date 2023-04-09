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
- every field in a query corresponds to a 'resolver function'
