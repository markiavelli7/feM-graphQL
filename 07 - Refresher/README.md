## The Main Parts

* Type Definitions
* Resolvers
* Schema
* Data Sources
* Query
* Mutation

### Basic GraphQL Implementation:

```javascript
var gql = require('graphql-tag')
var { ApolloServer } = require('apollo-server')

var typeDefs = gql`
  type User {
    id: ID!
    username: String!
    createdAt: Int!
  }

  type Settings {
    user: User!
    theme: String!
  }

  input NewSettingsUnput {
    user: ID!
    theme: String!
  }

  type Query {
    me: User!
    settings(user: ID!): Settings!
  }

  type Mutation {
    settings(input: NewSettingsUnput!): Settings!
  }
`

const resolvers = {
  Query: {
    me() {
      return {
        id: '1234',
        username: 'coder123',
        createdAt: 123456
      }
    },
    settings(_, { user }) {
      return {
        user,
        theme: 'Light'
      }
    }
  },
  Mutation: {
    settings(_, { input }) {
      return input
    }
  },
  Settings: {
    user(settings, {input}) {
      return {
        id: '1234',
        username: 'coder123',
        createdAt: 123456
      }
    }
  }
}

const server = new ApolloServer({
  typeDefs,
  resolvers
})

server.listen(3005).then((url) => {
  console.log(`Server listening on ${JSON.stringify(url.port)}`);
})
```