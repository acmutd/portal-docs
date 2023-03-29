---
sidebar_position: 4
---

# Querying GraphQL from Next.js page

GraphQL queries are currently made using `react-query` in this project. The following guide will explain how one can create a query that is needed for their component and how they can generate the appropriate typing system for the corresponding query.

Even though this guide will walk you through how to create a GraphQL query, this same guide can also be used to create mutation.

## Specify a query for your component.

Consider the following query specifying all the data it needs for its component:

```graphql
query getUserInfo {
  me {
    name
    isOfficer
  }
}
```

To get started, create a file with the `.graphql` extension in the `src/graphql` directory. The name of this file will be dependent on how the developers want to group their queries together. For example, a developer can create a file called `events.graphql` that will contain all necessary queries related to the Event page.

For the purpose of this example, let's say the query above will be stored in `user.graphql` file.

## Generate typing system for the queries

To start generting the typing system, boot up the server on the terminal with the following command:

```
$ npm run dev
```

After the application is up, open another terminal and run the following command:

```
$ npx graphql-codegen
```

:::note
The second command would only work if there is a running instance of the GraphQL server, thus requiring the first command to be executed first.
:::

Upon finishing execution of the second command, the GraphQL typing system should be available in `src/api.ts` file.

## Use the generated query in Next.js pages

Consider a page with the routing `pages/users` that requires the data that we specify above. To be able to fetch those data from the GraphQL server, we will use `react-query` to fetch the data in both server-side and client-side as follow:

### Server-side

Inside `pages/users.tsx`, create a `getServerSideProps` function with the following structure:

```ts
export const getServerSideProps: GetServerSideProps = async (ctx) => {
  await queryClient.prefetchQuery("userData", () => gqlQueries.getUserInfo());
  return {
    props: {
      dehydratedState: dehydrate(queryClient),
    },
  };
};
```

Notice that `userData` is the key for the query that will be matched with the `() => gqlQueries.getUserInfo()`.

`queryClient` is a global object used to interact with the GraphQL server that can be imported into the file as follow:

```ts
import { queryClient } from "src/api";
```

`gqlQueries` is an object containing all the queries specified in the `.graphql` file in form of a callable async function. `gqlQueries` can be imported as follow:

```ts
import { gqlQueries } from "src/api";
```

### Client-side

Inside the page component, create the following `useQuery` hook:

```ts
const { data, error, isLoading } = useQuery(["userData"], () =>
  gqlQueries.getUserInfo()
);
```

Notice that `userData` key is being used again in client-side.

## Additional Resource

The GraphQL system for frontend is being created based on the following video, which can be useful resource to better understand how to work with this system. [Video Link](https://www.youtube.com/watch?v=XzE-PzALyDc)
