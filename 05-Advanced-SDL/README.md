## Enums

A set of discrete values that can be used in place of Scalars. An enum field must resolve to one of the values in the Enum. Great for limiting a field to only a few different options.

```javascript
const typeDefs = gql`
  enum ShoeType {
    JORDAN
    NIKE 
    ADIDDAS
  }

  type Shoe {
    brand: ShoeType!
    size: Int
  }
`
```

By default, the values of the enums resolve to strings of the same name.

Using Enums

```javascript
mutation {
  newShoe(input: {size:12, brand: JORDAN}) {
    size
  }
}
```

## Interfaces

Abstract Types that can't be used as field values, but instead used as foundations for explicit Types. Great for when you have Types that share common fields, but differ slightly.

If we have two or more types that share common fields, but have 2-3 fields that are slightly different, we can use an interface to share those fields. This isn't to write less on the server, this is to make things more convenient for the client. They don't have to remember all of the individual fields, they can just use the interface.

```javascript
interface Shoe {
  brand: ShoeType!
  size: Int!
}

type Sneaker implements Shoe {
  brand: ShoeType!
  size: Int!
  sport: String!
}

type Boot implements Shoe {
  brand: ShoeType!
  size: Int!
  hasGrip: Boolean!
}
```

The Sneaker and the Boot both share brand and size from the interface. The interface describes the common fields.

#### Anything that implements the interface has to put all of the common fields in their definition. In Sneaker and Boot, we have the brand and size fields.

We can never query for a Shoe, it is an abstract type that will eventually resolve to a Sneaker or a Boot.

From the client-side, we can ask for Shoe to get the common fields, but if we want something specific, we tell GraphQL, "for a sneaker, we want this..."

Because we can't query for a Shoe, we have to write a resolver that resolves to either a Boot or a Sneaker.

```javascript
# resolvers.js

Mutation: {
  ...
},
Shoe: {
  __resolveType(shoe) {
    if (typeof shoe.sport == "Boolean") return 'Sneaker'
    return 'Boot'
  }
}
```

Using __resolveType is how we tell GraphQL what to resolve a shoe to. In this case we check to see if shoe.sport is a present. If it is not, we return 'Boot'

Using an Interface

```javascript
{
  shoes {
    brand
    size
    ... on Sneaker {
      sport
    }
    ... on Boot {
      hasGrip
    }
  }
}
```

Interfaces are useful for items that are very similar but differ slightly. Without interfaces, we wouldn't have a way to get those different types of things in the same query. We couldn't query for Boots and Sneakers in the same query. Shoes is our intermediary.

## Unions

Like interfaces, but without any defined common fields amongst the Types. Useful when you need to access more than one disjoint Type from one Query, like a search.

#### Where You Would Use a Union Instead of an Interface is For Something Like a Search

Search for many different types of entities that have nothing to do with each other. No relations, no common fields, but we want to do one query to get all of them back.

javascript```
union Footwear  = Sneaker | Boot
```

We treat a union the same way that we treat an interface, we create a resolver for it that resolves the type. We use the ... syntax in the query.