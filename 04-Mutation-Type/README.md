# What are Mutations

A Type on a Schema that defines operations clients can perform to mutate data (create, update, delete).

### Creating Mutations

  * Define Mutation Type on Schema using SDL

  * Add fields for the Mutation type

  * Add arguments for Mutation fields

  * Create Resolvers for Mutation fields

```javascript
# schema.js

input NewShoeInput {
  brand: String!
  size: Int!
}
type Mutation {
  newShoe(input: NewShoeInput!): Shoe!
}

# resolvers.js
const resolvers = {
  Query: {
    ...
  },
  Mutation: {
    newShoe(_, {input}) {
      return input
    }
  }
}
```

Just like Query, GraphQL is looking for a type called Mutation for mutations.

Using a Mutation From the Client

```javascript
mutation {
  newShoe(input: {brand: "jordan", size: 9}) {
    brand
    size
  }
}
```

### Best Practice: Always return the thing that you are mutating. If you create something, return the created object. If we are updating something, return the updated node. If we deleted something, at least return the id of the item that we deleted.