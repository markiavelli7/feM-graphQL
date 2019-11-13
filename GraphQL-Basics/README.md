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
  * 