## Query Type

A special type that the GraphQL server is expecting us to have.

  * Create a Query Type in the Schema using SDL
  * Add fields to the Query Type
  * Create Resolvers for the fields

#### Queries don't care what we do or how we get the data, they just expect the exact shape that we say.

## Resolvers

Resolvers are functions that are responsible for returning values for fields that exist on Types in a Schema. Resolver execution is dependent on the incoming client Query.

#### GraphQL only has one endpoint, there are not many endpoints.

*The query that the client sends up determine which resolvers get executed.*

The only resolvers that get executed are the ones in the fields that we ask for in the query.

### As the developer, we are responsible for creating Nodes and determining how they connect with each other. We are also responsible for resolving those nodes. 

We are not responsible for telling the client how to access them.

## Creating Resolvers
  * Resolver names must match the exact field name on our Schema's Types
  * Resolvers must return the value type declared for the matching field
  * Resolvers can be async
  * Because resolvers can be async, we can retrieve data from any source

##### We want our Resolvers to respect our Schema, not the other way around.

### Schema + Resolvers => Server

To create a server, at minimum, we need a Query Type with a field, and a Resolver for that field.

### Resolver Arguments
  ```javascript
  module.exports = {
    Query: {
      demo(_, __, ctx) {
        return ctx.models.Pet.findMany();
      }
    }
  }
  ```
  * First argument - Initial Value. This is a top level resolver. inside of a resolver, if our current resolver is dependent on the results from some other resolver, that's where this would go.

  * Second argument - Arguments. This allows clients to send up variables with their queries, allowing for filtering, pagination, etc.

  * Third argument - Context object. This is shared context amongst all of our resolvers.

### Setting Up Context
1. Set up context in the server:

```javascript
const server = new ApolloServer({
  context() {
    return {models, db}
  }
})
```
Inside of the call to new ApolloServer, we pass in an object with a function as a property. The function is called context() and will return the context. Here we are returning the models and db that we passed in to our server file.

2. Inside of the resolvers file, set up context

```javascript
module.exports = {
    Query: {
      demo(_, __, {models, db})
    }
  }
```

Passing models and db as context inside of our resolvers gives us complete control of testing, because we can mocking all of the arguments out.