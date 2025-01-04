# Graphql

## Resources
+ https://graphql.org/
+ https://studio.apollographql.com/public/star-wars-swapi/variant/current/home

Its a way to query your api service unlike rest where you have multiple uri for each of the resources.  With GraphQL you only have one uri and a query to select the data you need. Good for mobile since you can only select the fields you need to avoid latancy.  

## Queries
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
+ Everything is a POST request
+ Everything is a 200 Status Code
