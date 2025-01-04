# Graphql

## Resources
+ https://graphql.org/
+ https://studio.apollographql.com/public/star-wars-swapi/variant/current/home

Its a way to query your api service unlike rest where you have multiple uri for each of the resources.  With GraphQL you only have one uri and a query to select the data you need. Good for mobile since you can only select the fields you need to avoid latancy. 

## GraphQL
+ Everything is a POST request - Unlike REST where you have POST, GET, UPDATE, DELETE
+ Everything is a 200 Status Code
+ Check payload of data to see if is a success or an error
+ We are not using Fetch but a GraphQL client (there are many of them).  There is Apollo for Client and Server.  The Apollo client is hard and confusing at times.  There is an simpler tool, Urql (https://github.com/urql-graphql/urql)
+ What is a GraphQL client? It is a wrapper for fetch. Similar to SWR (NextJS) or React-Query.
+ You can use Apollo Server to create your queries or mutations and then copy paste in your code.  If using `urql` client, you can use their utility `gql` to interpolate the copied query or mutation from the Apollo Server.

## Queries and Mutations
+ Query - reads
  + Query example. Queries can have a name or be anonymous
```
//With Name
query User {
  user {
    id
    created
    title
    name
  }
}

//Without Name
query {
  user {
    id
    created
    title
    name
  }
}
```
+ Mutation - writes
  + Mutation Example. The `$input` is an argument (can be named whatever you want) passed in to the mutation. `!` in `AuthInput!` means not null, the value is expected.  The request name has to be named the same as the argument passed to the mutation.  In this case the argument to the mutation is `$input` the variable/request name has to be `input`.
```
mutation Mutation($input: AuthInput!) {
  createUser(input: $input) {
    token
    id
  }
}

// Variables/Request
{
  "input": {
    "email": "admin@admin.com",
    "password": "password"
  }
}
```
+ Using GraphQL in React
  + Use gql to interpolate (template tag) your GraphQL query or mutation, see below.
```
import { gql } from '@urql/next'

export const MyMutation = gql`
  mutation Mutation($input: AuthInput!) {
    createUser(input: $input) {
      token
    }
  }
`
```
  +
