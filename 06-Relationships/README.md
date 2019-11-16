# Relationships

## Thinking in Graphs

Our API is no longer a predefined list of operations that always return the same shapes. Instead, our API is a set of Nodes that know how to resolve themselves and have links to other Nodes. This allows a client to ask for Nodes and then follw those links to get related Nodes.

## Adding Relationships

  * Add a Type as a field value on another Type

  * Create resolvers for those fields on the Type

```graphql
type User {
  email: String!
  avatar: String
  shoes: [Shoe]!
}

interface Shoe {
  brand: ShoeType!
  size: Int!
  user: User!
}
```

Above, we are saying that a User has shoes. We can also say that a shoe has a User.

#### Relationship Resolver

```javascript
shoes(_,{input}) {
  return [
    {brand: 'NIKE', size: 12, sport: 'basketball', user: 1},
    {brand: 'ADIDAS', size: 14, sport: 'tennis', user: 2}
  ]
}
```

```graphql
type Pet {
  id: ID!
  createdAt: String!
  name: String!
  type: String!
  img: String
  owner: User!
}
```

A pet doesn't have an owner associated with it stored in the database, no relationship exists, etc.

Adding a user id to the pets resolver:

```javascript
"pet": [
  {
    "id": 1,
    "createdAt": "45425g4oeuhnu",
    "name" : "Moose",
    "type": "DOG",
    "user" : "jBWMVGjm5016LGwepDoty"
  }
]
```

### Our job on the server side is to define the schema and write the resolvers.

Our Pet type has a field with the name owner that has to be of the Type User. At this point, our resolver doesn't handle this field, so we have to resolve it in our resolvers file:

```javascript
Pet: {
  owner(pet, _, ctx) {
    ctx.models.User.findOne()
  }
}
```

owner() is a field level resolver. 

### A Field Level Resolver is any resolver for a field that's on a Type other than a Mutation or a Query.

#### Query and Mutation are top level resolvers, nothing resolves before them. They happen first, everything else comes after.