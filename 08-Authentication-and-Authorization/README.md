## Authentication & Authorization

#### Authentication: Who You Are

Used to identify a user. To determine if they are who they say they are.

#### Authorization: What You Are Allowed To Do

Used to determine if a user is allowed to perform certain operations on certain resources.

## A Good Auth System in GraphQL

#### Authorization
* Should *NOT* be coupled to a resolver
* Can provide field level custom rules
* Can authorzie some of our schema and not all

#### Authentication
* Provides the user access to resolvers
* Should *NOT* be coupled to a resolver
* Can protect some of our Schema and not all of it
* Can provide field level protection

*Ways to Authenticate:*
* Outside of GraphQL
* When Creating Context
* Inside the Resolvers

## Outside of GraphQL

* Use something like Express middleware before the GraphQL Server executes
* Completely locks down all GraphQL queries and mutations
* Extra complexity of passing auth info to GraphQL

*Downside*
Completetly locks down all GraphQL queries and mutations. The complexity of passing auth info to GraphQL usually isn't worth it.

## When Creating the Context Object

* Use context creation function when creating our Apollo server
* Can access the incoming request to determine authentication
* No extra work to pass to GraphQL resolvers

## Inside of Resolvers

* Business logic tied up with authentication logic
* The simplest to implement
* The hardest to reuse

