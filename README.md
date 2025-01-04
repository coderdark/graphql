# Graphql

## Resources
+ https://graphql.org/
+ https://studio.apollographql.com/public/star-wars-swapi/variant/current/home
+ https://caisy.io/blog/when-to-use-trpc-or-graphql
+ https://commerce.nearform.com/open-source/urql/docs/

Its a way to query your api service unlike rest where you have multiple uri for each of the resources.  With GraphQL you only have one uri and a query to select the data you need. Good for mobile since you can only select the fields you need to avoid latancy. 

## GraphQL
+ Everything is a POST request - Unlike REST where you have POST, GET, UPDATE, DELETE
+ Everything is a 200 Status Code
+ Check payload of data to see if is a success or an error
+ We are not using Fetch but a GraphQL client (there are many of them).  There is Apollo for Client and Server.  The Apollo client is hard and confusing at times.  There is an simpler tool, Urql (https://github.com/urql-graphql/urql)
+ What is a GraphQL client? It is a wrapper for fetch. Similar to SWR (NextJS) or React-Query.
+ You can use Apollo Server to create your queries or mutations and then copy paste in your code.  If using `urql` client, you can use their utility `gql` to interpolate the copied query or mutation from the Apollo Server.
+ When running your site that was setup with GraphQL you can go to `http://localhost:3000/api/graphql` to see the Apollo Studio Server.  This is like `Postman` or `Insomnia` clients
+ You can see the call to GraphQL in the network tab by searching and selecting `graphql`
+ Using Apollo Studio Server you can view the `schema introspection`  for more information about the types for troubleshooting or anything else. **Introspection needs to be enabled**.

## Features of GraphQL Queries and Mutations
- Fields
- Nested Objects
- Arguments

## Queries and Mutations (You can use your project url: http://localhost:3000/api/graphql to access the Apollo Server Studio)
+ Query - reads
  + Query example. Queries can have a name or be anonymous
```
//With Name
query User {
  user {
    id // <==! This names can change, for example you can do userId: id
    created: createdDate // <==! your frontend can call this field created but this wont change your backend, which it is still createdDate
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
  + Nested (Recursive) Queries - You can do nested queries but BECAREFUL with them, these can bring down your server if not careful.
```
query User {
  user {
    id
    created
    title
    name
    user {
      id
      created
      title
      name
        user {
          id
          created
          title
          name
      }
    }
  }
}
```
  + Using GraphQL Queries in React
```
//GraphQL Query
import { gql } from '@urql/next'

export const IssuesQuery = gql`
  query IssuesQuery {
    issues {
      content
      id
      name
      status
    }
  }
`

//GraphQL Query In React Code
import { useMutation, useQuery } from '@urql/next'
import { useState } from 'react'
import Issue from '../_components/Issue'
import { IssuesQuery } from '@/gql/issuesQuery'

const IssuesPage = () => {
  const [issueName, setIssueName] = useState('')
  const [issueDescription, setIssueDescription] = useState('')
  const [{ data, error, fetching }, replay] = useQuery({ query: IssuesQuery }) // Replay allows you to recall the query again

  const onCreate = async (close) => {
  }

  function handleOnOpen(){
    const issueNameAnswer = window.prompt("Issue Name");
    setIssueName(issueNameAnswer);

    const issueDescriptionAnswer = window.prompt("Issue Description");
    setIssueDescription(issueDescriptionAnswer);
  }

  return (
    <div>
      <header>All Issues</header>
      <button onClick={onOpen}>+</button>
      {fetching && <Spinner />}
      {error && <div>Error</div>}
      {data &&
        data.issues.map((issue) => (
          <div key={issue.id}>
            <Issue issue={issue} />
          </div>
        ))}
    </div>
  )
}

export default IssuesPage

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
    "username": "admin@admin.com",
    "password": "password"
  }
}
```
+ Using GraphQL Mutations in React
  + Use `gql` utility from `urql` to interpolate (template tag) your GraphQL query or mutation, see below.
```
//GraphQL Mutation
import { gql } from '@urql/next'

export const MyMutation = gql`
  mutation Mutation($input: AuthInput!) {
    createUser(input: $input) {
      token
    }
  }
`

//GraphQL Mutation in React Code
import { useState } from 'react'
import { useMutation } from 'urql'
import { SigninMutation } from './gql/signinMutation'

const SignInPage = () => {
  const [state, setState] = useState({ password: '', username: '' })
  const router = useRouter()
  const [signInResult, signIn] = useMutation(SigninMutation)

  const handleSignIn = async (e) => {
    e.preventDefault()
    const result = await signIn({ input: state })

    if(result.data.signIn){
      setToken(result.data.createUser.token) // you can set token in localstorage
      router.push('/')
    }
  }

  return (
    <div>
      <h1>Sign In</h1>
      <form onSubmit={handleSignin}>
        <div>
          <input
            value={state.email}
            onValueChange={(v) => setState((s) => ({ ...s, email: v }))}
            placeholder="Email"
          />
        </div>
        <div>
          <input
            value={state.password}
            onValueChange={(v) => setState((s) => ({ ...s, password: v }))}
            placeholder="Password"
            type="password"
          />
        </div>
        <div>
          <button type="submit">
            SignIn
          </Button>
        </div>
      </form>
    </div>
  )
}

export default SignInPage
```
+ URQL
