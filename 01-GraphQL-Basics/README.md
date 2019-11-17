# What is GraphQL?

GraphQL is a spec that describes a declarative query language that our clients can use to ask an API for the exact data that they want. This is achieved by creating a strongly typed Schema for our API.

This gives us ultimate flexibility in how our API can resolve data, and client queries validated against our Schema.

##### GraphQL is just a spec. There are several implementations of GraphQL.


# Server Side GraphQL - What Do We Need?
  * Type Definitions
  * Resolvers
  * Query Definitions
  * Mutation Definitions
  * Composition
  * Schema

# Client Side - What Do We Need?
  * Queries
  * Mutations
  * Fragments

## Where Does GraphQL Fit In?
  * A GraphQL server with a connected DB (most greenfields)
  * A GraphQL server as a layer in front of many 3rd part services and connects them all with one GraphQL API
  * A hybrid approach where a GraphQL server has a connected DB and also communicates with 3rd party services

### Node GraphQL Tools
  * Servers
    * Apollo Server
    * GraphQL Yoga

  * Services
    * Amplify
  
  * Tools
    * Prisma

# Schemas

## Creating a Schema
  * Using Schema Definition Language (SDL)
  * Programmatically Creating a Schema using language constructs

SDL is the syntax that we use to create a schema.

### Using the SDL to Create Schemas

#### Basic Parts
  * *Types* - a construct defining a shape with fields
  * *Fields* - keys on a Type that have a name and a value type
  * *Scalars* - primitive value types built into GraphQL
  * *Query* - type that defines how clients can access data
  * *Mutation* - type that defines how clients can modify or create data

### Building a Server

Here, we are going to use graphql template tags to create our Schema. To do this, we are using a package called graphql-tag.

Anything that we put within the template literal string, graphql tag is going to compile into a GraphQL AST that can be accessed by the server.

Using graphql template tags allows us to create a schema declaratively.

We could also use a .graphql file, put them in the file stream.

```javascript
demo.js

var gql = require('graphql-tag');
var { ApolloServer } = require('apollo-server');

var typeDefs = gql`

// Here we are defining a new type called User. The user has three different fields. 

// The first field, has a name of email and a Scalar type of String. The ! delimits that this value is required. 

// The second field has a name of avatar and a Scalar type of String. This is also a required value.

// The third field has a name of friends and a Scalar type of an array. Empty or not, an array must be present as a value. Inside of the array, we are expecting the type of User.

  type User {
    email: String!
    avatar: String!
    friends: [User]!
  }

# By default, graphql is expecting the Query type, if we want to call our query something else, we have to tell graphql what the name of our query is.

# Here we have defined a query and given it one field. The name of the field is me and the value that it must return is the User type that we defined above.

  type Query {
    me : User!
  }
`;
# Here we are creating some resolvers, we define an object that will contain them.

# We have to name our resolvers the same thing that we called them in the type definitions, so in this case, we are using Query. Then we are passing the resolver a function called me() that correlates to the Query type that we defined.

# A resolvers' job is to return data that matches the shape of the query. In this case, we passed in the User type to me inside of our query, so we are expecting to be returned data that matches the shape of the User type (email, avatar, and friends fields).

var resolvers = {
  Query: {
    me() {
      return {
      email: "yoda@masters.com",
      avatar: "http://yoda@yoda.com",
      friends: [
        {
          email: "joe@stalin.com",
          avatar: "http://bigandbushy.com/1.jpeg"
        },
        {
          email: "doc@takeaholiday.com",
          avatar: "http://imyourhuckleberry.com"
        }
      ]
    };
    }
  }
}

# Creating the Server
const server = new ApolloServer({
  typeDefs,
  resolvers
})

server
  .listen(4000)
  .then(() => {
    console.log("Server listening on port 4000.");
  })
  .catch(err => {
    console.log(`Error starting server: ${err}`);
  });
```

Scalar Types
  * String
  * Int
  * Float
  * Boolean
  * ID

#### At Minimum, a Schema Needs a Query
We can make a type, but if we don't make a query, no one can access the type.

A Query type is really not much different than our defined User type: it has fields that resolve to values.

## Querying for Data in the GraphQL Playground

```javascript
query {
  me {
    email
    avatar
    friends {
      email
    }
  }
}

// RETURNS:
// {
//   "data": {
//     "me": {
//       "email": "yoda@masters.com",
//       "avatar": "http://yoda@yoda.com",
//       "friends": [
//         {
//           "email": "joe@stalin.com"
//         },
//         {
//           "email": "doc@takeaholiday.com"
//         }
//       ]
//     }
//   }
// }
```

