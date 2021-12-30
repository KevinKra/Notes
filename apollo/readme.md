## Apollo + Graphql

- Every GraphQL server (including Apollo Server) uses a `Schema` to define the structure of data that clients can query.

- Your GraphQL server uses a **schema** to describe the shape of your available data. This schema defines a hierarchy of **types** with **fields** that are populated from your back-end data stores. The schema also specifies exactly which **queries** and **mutations** are available for clients to execute.

- The GraphQL specification defines a human-readable schema definition language (or SDL) that you use to define your schema and store it as a string. Note that the schema is not responsible for defining where data comes from or how it's stored. It is entirely implementation-agnostic.

- The following "Book" **object type** defines the queryable fields for every book in our data source.

```
type Book {
  title: String
  author: String
}
```

- The "Query" type is special: it lists all of the available queries that clients can execute, along with the return type for each.

```
  type Query {
    books: [Book]
  }
```

- Apollo Server can fetch data from any source you connect to (including a database, a REST API, a static object storage service, or even another GraphQL server).

- `Resolvers` tell Apollo Server _how_ to fetch the data associated with a particular type. For instance, from a database, a REST API, a static object storage service, or even another GraphQL server.

- **Note:** If your server is deployed to an environment where `NODE_ENV` is set to `production`, **introspection is disabled by default**. This prevents Apollo Sandbox from working properly. To enable introspection, set `introspection: true` in the options to ApolloServer's constructor.

- One of the most important concepts of GraphQL _is that clients can choose to query only for the fields they need._

- \# Comments in GraphQL strings start with the hash (`#`) symbol.

Most of the schema types you define have one or more fields:

```
# This Book type has two fields: title and author
type Book {
  title: String  # returns a String
  author: Author # returns an Author
}
```

- Each field returns data of the type specified. A field's return type can be a **scalar, object, enum, union, or interface**.

- **List Fields:** A field can return a list containing items of a particular type. You indicate list fields with square brackets []. `[Book]` -- a list of books.

- **Field nullability:** By default, it's valid for any field in your schema to return `null` instead of its specified type. You can require that a particular field doesn't return `null` with an exclamation mark `!`. Example: `name: String!` -- can't return null. If your server attempts to return null for a non-nullable field, an error is thrown.

- If `!` appears inside the square brackets, the returned list can't include items that are `null`. If `!` appears outside the square brackets, the list itself can't be `null`. In any case, it's valid for a list field to return an empty list.

```
type Author {
  books: [Book!]! # This list can't be null AND its list *items* can't be null
}
```

- Scalar types are similar to primitive types in your favorite programming language. They always resolve to concrete data. GraphQL's default scalar types are: `Int`, `Float`, `String`, `Boolean`, `ID`.

- An **object type** contains a collection of fields, each of which has its own _type_.

- `__typename` defines the type of its object.

- The `Query` type is a special object type that defines all of the top-level entry points for queries that clients can execute against your server.

```
query GetBooks {
  books {
    title
    author {
      name
    }
  }
}
```

- The `Mutation` type is similar in structure and purpose to the `Query` type. Whereas the `Query` type defines entry points for read operations, the Mutation type defines entry points for write operations.

```
type Mutation {
  addBook(title: String, author: String): Book
}
```

- This Mutation type defines a single available mutation, addBook. The mutation accepts two arguments (title and author) and returns a newly created Book object. As you'd expect, this Book object conforms to the structure that we defined in our schema.

- `Subscriptions` (type) are long-lasting GraphQL read operations that can update their result whenever a particular server-side event occurs.
