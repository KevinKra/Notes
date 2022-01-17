# Apollo Federations

- To get the most out of GraphQL, your organization should expose a single graph that provides a unified interface for querying any combination of your backing data sources. However, it can be challenging to represent an enterprise-scale graph with a single, monolithic GraphQL server. **To remedy this, you can use Apollo Federation to divide your graph's implementation across multiple back-end services (called subgraphs).**

- Unlike other distributed GraphQL architectures (such as schema stitching), _Apollo Federation uses a declarative programming model_ that enables each subgraph to implement only the part of your composed supergraph that it's responsible for.

#### Apollo Managed Mode

> Apollo Federation also supports a free managed mode with Apollo Studio, which enables you to add, remove, and refactor your subgraphs without requiring any downtime for your production graph.

In addition to providing its supergraph schema on startup, Apollo Gateway can operate in managed federation mode, where Apollo Studio acts as the source of truth for each subgraph's schema.

This mode (Managed Mode) enables multiple teams working on a graph to coordinate when and how underlying subgraphs change. It's recommended for all federated graphs. For more information, read Managed federation overview.

## Federated Schemas

> In a federated architecture, multiple GraphQL APIs are composed into a single federated graph. The individual APIs are called subgraphs, and they're composed into a supergraph

A federated graph uses multiple "types" of GraphQL schemas:

- **Subgraph schemas**. Each of your subgraphs has a distinct schema that indicates which types and fields of your composed supergraph it's responsible for resolving.

- **Supergraph schema**. This schema is the result of performing _composition_ on your collection of subgraph schemas. It combines all of the types and fields from your subgraph schemas, _plus some federation-specific directives that tell your gateway which subgraphs are responsible for resolving which fields._

- **API schema**. This schema is like the supergraph schema, **but it omits types, fields, and directives that are considered "machinery" and are not part of your public API** (this includes federation-specific directives).
  - **This is the schema that your gateway exposes to your GraphQL API's consumers, who don't need to know any internal implementation details about your graph.**

### Subgraph schemas

> Accounts Subgraph

```
extend type Query {
  me: User
}

type User @key(fields: "id") {
  id: ID!
  username: String!
}
```

> Products Subgraph

```
extend type Query {
  topProducts(first: Int = 5): [Product]
}

type Product @key(fields: "upc") {
  upc: String!
  name: String!
  price: Int
}
```

> Reviews SubGraph

```
type Review {
  body: String
  author: User @provides(fields: "username")
  product: Product
}

extend type User @key(fields: "id") {
  id: ID! @external
  username: String! @external
  reviews: [Review]
}

extend type Product @key(fields: "upc") {
  upc: String! @external
  reviews: [Review]
}
```

- A subgraph can reference a type that's defined by _another_ subgraph. For example, the `Review` type includes a product field of type `Product`, even though the Product type is defined in a different subgraph.
- A subgraph can also _extend_ a type that's defined by another subgraph. For example, the `reviews` subgraph extends the User type by adding a `reviews` field to it.
- A subgraph must add the `@key` **directive** to an object type's definition in order for other subgraphs to be able to reference or extend that type. This **directive** makes an object type an entity.

### Supergraph schema

- To create our supergraph schema, we perform **composition** on our collection of subgraph schemas. With managed federation, **Apollo performs composition automatically whenever one of your subgraphs registers an updated schema.**

- You can also perform composition manually with the Rover CLI.
- `rover supergraph compose --config ./supergraph.yaml`

### API Schema

- The gateway uses its **supergraph schema** to produce an **API schema**, _which is what's exposed to clients as your actual GraphQL API._

## Gateway Example

Your federated supergraph is represented by a **gateway** which routes each incoming query to the appropriate combination of subgraphs and returns the combined result.

The supergraph's schema is the combination of each subgraph's schema, plus some special federation-specific directives. Each subgraph's schema can even reference and extend types that originate in a different subgraph.

This architecture enables clients to query data from multiple services simultaneously, just by querying the gateway!

```
const supergraphSdl = readFileSync('./supergraph.graphql').toString();

const gateway = new ApolloGateway({
  supergraphSdl
});

const server = new ApolloServer({ gateway });
server.listen();
```

With Apollo Federation, **resolvers live in your subgraphs.** The **gateway** serves _only_ to plan and execute GraphQL operations across those subgraphs.

#### Entities

As part of our federated architecture, we want _other_ subgraphs to be able to extend the `User` type the following subgraph defines. To enable this, we add the `@key` **directive** to the `User` type's definition to designate it as an **entity**.

The `@key` **directive** tells other subgraphs which field(s) of the User type to use to uniquely identify a particular instance. In the case below, subgraphs should use the single field `id`.

```
  type User @key(fields: "id") {
    id: ID!
    username: String
  }
```

---

# Questions

- What is a subgraph?
- What is a supergraph?
- What term is used to describe the combining of subgraphs?
- Describe Apollo managed mode.
- What is supergraph gateway?
- What is the API schema?
- Which schema variant is exposed to the clients as the GraphQL API?
- Can subgraphs reference types defined in other subgraphs?
- What is an entity?
- What is a directive?
