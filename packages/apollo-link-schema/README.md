---
title: apollo-link-schema
description: Assists with mocking and server-side rendering
---

## Installation

`npm install apollo-link-schema --save`


## Usage

### Server Side Rendering

When performing SSR _on the same server_ you can use this library to avoid making network calls.

```js
import { ApolloClient } from "apollo-client";
import { InMemoryCache } from "apollo-cache-inmemory";
import { SchemaLink } from "apollo-link-schema";

import schema from './path/to/your/schema';

const graphqlClient = new ApolloClient({
  ssr: true,
  cache: new InMemoryCache(),
  link: new SchemaLink({ schema })
});
```

### Mocking
```js
import { ApolloClient } from "apollo-client";
import { InMemoryCache } from "apollo-cache-inmemory";
import { SchemaLink } from "apollo-link-schema";
import { makeExecutableSchema, addMockFunctionsToSchema } from 'graphql-tools';

const typeDefs = `
  Query {
  ...
  }
`;

const mocks = {
  Query: () => ...,
  Mutation: () => ...
};

const schema = makeExecutableSchema({ typeDefs });
addMockFunctionsToSchema({
  schema,
  mocks
});

const apolloCache = new InMemoryCache(window.__APOLLO_STATE__);

const graphqlClient = new ApolloClient({
  cache: apolloCache,
  link: new SchemaLink({ schema })
});
```

