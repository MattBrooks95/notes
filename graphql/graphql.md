## GraphQl

### What it is
- a structured way to query an API, using a query language
- backend/database agnostic

### It solves
- overfetching. In REST api's, the JSON payload likely contains more info than the client needs
- n+1 problem: in REST api's, the JSON payload will often contain ids that are to be used in subsequent requests to fetch more data
    - getting nested information could take several requests in a REST api
    - a graphQL request will instead gather up the necessary information as demanded by the client, and return it in one request

### Differences with REST
- a REST api has many endpoints, whereas graphql usually has only one
    - the client's request defines the information that it needs, so the endpoint can do the work necessary to grab that information and return it, all from a single endpoint

### Parts
- Schema Definition Language (SDL)
    - this is used to describe the endpoints, and the return types of their results
    - an example from [howtographql](https://www.howtographql.com/basics/2-core-concepts/)
        ```
        type Person {
            name: String!
            age: Int!
        }

        type Post {
            title: String!
            author: Person!
        }

        type Person {
            name: String!
            age: Int!
            posts: [Post!]!
        }
        ```
        - the first Person is a simple example, with two required (suffixed with '!') fields; 'name' and 'age'
        - Post has a one-to-one relationship with a required person
        - the second Person type has a one-to-many relationship with Posts, because it is an array type
- query language
    - 'allPersons' is the query 'root', and everything within it is the 'payload'
    ```
    {
        allPersons {
            name
        }
    }
    ```
    - because the payload only contains the 'name' field, only the 'name' field of each person is returned; the age or posts are not
    - if the query were modified to also contain the 'age' field, then the ages of every person would also be returned
    ```
    {
        allPersons {
            name
            age
        }
    }
    ```
    - graphql is good for querying nested information, this query would return only the 'title' field of every post (of every person)
    ```
    {
        allPersons {
            name
            age
            posts {
                title
            }
        }
    ```
    - each field can have 0 or more arguments (depending on what is specified in the schema) that filter the returned results
        ```
        {
            allPersons(last: 2) {
                name
            }
        }
        ```
        - if this query had a 'last' argument in the schema, it could be used to only return the specified number of persons
- mutations (insertions, updates)
    ```
    mutation {
        createPerson(name: "Bob", age: 36) {
            name
            age
        }
    }
    - the mutation keyword is used to designate a query as being a mutation
    - 'createPerson' is the root field
    - here, the 'name' and 'age' arguments are used to specify the data for the new entry
    - because within the brackets we are asking for 'name' and 'age', the information from the new record will be returned to us. In this case it's not very helpful because the client already knows the name and age, but it's possible that we could ask for other fields that the client doesn't know yet
        - the example from howtographql has a mutation that asks for the auto-generated ID of the person, which would be new information to the client
- subscriptions: a client subscribes to an event, and gets a steady connection to the server. When an event occurs, the server sends the data to the client immediately. This is in contrast to the request-response cycle like the examples from earlier.
    - subscriptions use the same syntax as the queries:
    ```
    subscription {
        newPerson {
            name
            age
        }
    }
    ```
    - if a new mutation that adds a person occurs, the client that used the above subscription to subscribe to the event will be informed by the server as soon as the event occurs

### Schemas
- a schema is a contract between a server & a client. Usually, schemas are just collections of GraphQL types.
- there are some special 'root' types, that are the entry points for requests from the client:
    - type Query { ... }
    - type Mutation { ... }
    - type Subscription { ... }
- an example Query root field that enables the 'allPersons' query example from before looks like:
    ```
    type Query {
        allPersons: [Person!]!
    }
    ```
    - this is called a 'root field' of the API
- similarly, we'd need to define a mutation as well:
    ```
    type Mutation {
        createPerson(name: String!, age: Int!): Person!
    }
    ```
    - notice how we specified the arguments for 'name' and 'age', that would need to be provided by the client
- lastly, we add the subscription
    ```
    type Subscription {
        newPerson: Person!
    }
    ```
- I assume in these root fields we can declare several subscriptions, mutations and queries?
