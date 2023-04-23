---
description: >-
  On this page you will learn how to fetch data from your subgraph to your
  frontend.
---

# Fetching data to your frontend

We will break this process in two

1. Fetch the array of data
2. Display it

Here we are going to use nextjs, but you can use any framework of javascript you want.

## Create a nextjs project

You can do this by running

```
// create a nextjs application
npx create-next-app name-of-project
```

## Installing apollo and graphql

You can do this by running

```
// install apollo and graphql
npm i @apollo/client graphql
```

## Create client

Create a utils folder and create a file named **apollo-client.ts** in it paste the following code

```
// client code
import { ApolloClient, InMemoryCache } from "@apollo/client";

const client = new ApolloClient({
  uri: "https://api.thegraph.com/subgraphs/name/[YOUR_GITHUB]/[YOUR_SUBGRAPH]",
  cache: new InMemoryCache(),
});

export default client;
```

## Wrap the entire application with the apollo provider

To make sure you can query data properly from anywhere in you application open your \_app.tsx and wrap your app with the apollo provider, your code should look something like

```typescriptreact
//how the imports and wrapping looks like
import { ApolloProvider } from "@apollo/client";
import client from "../utils/apollo-client";

<ApolloProvider client={client}>
    <Component {...pageProps} /> 
</ApolloProvider>

```

## Fetching data from your subgraph

Here is an example code on how to fetch data from your subgraph

```
// Code snippet
import { useEffect, useState } from "react"
import { gql } from "@apollo/client";
import client from "../utils/apollo-client";

export default function Home() {
    const [posts, setPosts] = useState([])
    useEffect(() => {
        async function fetchData() {
            const { data: posts } = await client.query({
                query: gql`
                    {
                        posts {
                            id,
                            name,
                            age,
                            country
                        }
                    }
                `
            })
            setPosts(posts.posts)
        }
        fetchData()
    }, [])
    return(
        <div>
            {
                posts.map(post => <p>Hi my name is {post.name} 
                    I am {posts.age} years old and I live in
                    {post.country}</p>)
            }
        </div>
    )
}
```

## Hurray!!! You just fetched data to your frontend🎉🎉🎉
