# Arguments

  * Allow clients to pass variables along with Queries that can be used in our Resolvers to get data

  * Must be defined in our Schema

  * Can be added to any field

  * Either have to be Scalars or Input Types

schema.js
```javascript
  type Query {
    pets(type: String): [Pet]!
  }
```
With this change, we are saying that pets takes a type, and its value is going to be a string: pets(type: String).

Then, inside of our pet resolver, we have access to an object whose keys are the arguments that we've passed in:

```javascript
module.exports = {
  Query: {
    pets(_, {type}, ctx) {
      return [...]
    }
  }
}
```

In our case, we have access to type, which was passed into the pets query from schema.js. Any arguments that we pass in to our schema are accessible on an object inside of our resolver. The keys are the names of the argument that we passed, in this case 'type', for type of animal.

# Input Types

  * Just like Types, but used for Arguments

  * All field value types must be other Input Types or Scalars
 
Because we declare inputs in the same way that we declare types, they look very similar, especially if we name our input the same name that we use for our type. for this reason, it is a good idea to specify in the input name that it is an input:

```javascript
input PetInput {
  ...
}

# Instead of

input Pet {
  ...
}
```

Input Example:

```javascript
input PetInput {
  name: String
  type: String
}
```

Then, inside of the query, we pass in our input.

```javascript
type Query {
  pets(input: PetInput): [Pet]!
}
```

A good convention is to pass the defined input with a key named input, this way, you always know what you're getting.

If we are passing more than one argument, it is a good idea to pass a predefined Input object. This makes it a lot easier for us to manage when we create our resolvers.

## Arguments in Resolvers

  * Arguments will be passed to field Resolvers as the second argument

  * The argument object will strictly follow the argument names and the field types

  * We can do whatever we want with them

## Query With Arguments

```javascript
query {
  shoes(input: {brand: ""})
}
```

When querying, we always have to use " ", not ' '. Otherwise things can break.

### Best Practice: On fields that have an array type, pass in an input to the name, this allows us to be able to filter the array instead of getting all of the nodes back:

```javascript
type Pet {
  id: ID!
  createdAt: String!
  type: String!
  img: String!
  buddies(...): [Pet]
}
```