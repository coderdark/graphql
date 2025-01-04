# Graphql

## Resources
+ https://graphql.org/
+ https://studio.apollographql.com/public/star-wars-swapi/variant/current/home

Its a way to query your api service unlike rest where you have multiple uri for each of the resources.  With GraphQL you only have one uri and a query to select the data you need. Good for mobile since you can only select the fields you need to avoid latancy.  

## Queries
+ Query - reads
+ Mutation - writes
+ Can have a name or be anonymous
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
+ Everything is a POST request - Unlike REST where you have POST, GET, UPDATE, DELETE
+ Everything is a 200 Status Code
+ Check payload of data to see if is a success or an error
+ We are not using Fetch but a GraphQL client (there are many of them).  There is Apollo for Client and Server.  The Apollo client is hard and confusing at times.  There is an simpler tool, Urql (https://github.com/urql-graphql/urql)
+ What is a GraphQL client? It is a wrapper for fetch. Similar to SWR (NextJS) or React-Query.
+ ! in GraphQL means not null.  The value is expected.
