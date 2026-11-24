# @grafctory/wire-sparql-http

[![npm](https://img.shields.io/npm/v/@grafctory/wire-sparql-http)](https://www.npmjs.com/package/@grafctory/wire-sparql-http)

SPARQL connector for [@grafctory/wire](https://github.com/grafctory/wire) backed by a SPARQL 1.1 Protocol endpoint.

Exposes any [SPARQL 1.1 Protocol](https://www.w3.org/TR/sparql11-protocol/) compliant endpoint through the common
[@grafctory/wire-sparql] `Repository` interface, issuing each query and update as a single HTTP request. For [RDF4J
Server](https://rdf4j.org/) repositories with server-managed transactions, use the companion
[@grafctory/wire-sparql-rdf4j] connector.

# Installation

```shell
npm install @grafctory/wire-sparql         # the SPARQL query API
npm install @grafctory/wire-sparql-http    # this connector
```

> [!WARNING]
>
> TypeScript consumers must use `"moduleResolution": "nodenext"/"node16"/"bundler"` in `tsconfig.json`.
> The legacy `"node"` resolver is not supported.

# Usage

Connect to a SPARQL 1.1 Protocol endpoint with `createHTTPRepository`, then issue queries and updates through the
[@grafctory/wire-sparql] `Repository` API:

```typescript
import { createHTTPRepository } from "@grafctory/wire-sparql-http";

const repository = createHTTPRepository({
	query: "https://example.org/sparql",
	update: "https://example.org/sparql/statements"
});
```

The optional `graphs` setting names a [Graph Store Protocol](https://www.w3.org/TR/sparql11-http-rdf-update/) endpoint,
where `merge` and `store` upload their triples in bulk. Without it, they run as SPARQL updates on the `update` endpoint,
with the same effect on the store, but with the data parsed as SPARQL text and sent as a single update request.

See [@grafctory/wire-sparql] for the `Repository` API: `ask`, `select` and `construct` query the store, `update`,
`merge` and `store` write to it, and `execute` and `close` manage transactions and resources.

> [!IMPORTANT]
>
> This connector provides no transaction isolation: the SPARQL 1.1 Protocol exposes no transaction primitives, so each query and update is
> dispatched eagerly as a single HTTP request and `execute` runs its task directly, without bracketing. A
> multi-statement update is atomic only insofar as the endpoint applies it atomically.

[@grafctory/wire-sparql]: https://grafctory.github.io/wire/modules/_grafctory_wire-sparql.html

[@grafctory/wire-sparql-rdf4j]: https://grafctory.github.io/wire/modules/_grafctory_wire-sparql-rdf4j.html

# Support

- open an [issue](https://github.com/grafctory/wire/issues) to report a problem or to suggest a new feature
- start a [discussion](https://github.com/grafctory/wire/discussions) to ask a how-to question or to share an idea

# License

This project is licensed under the Apache 2.0 License –
see [LICENSE](https://github.com/grafctory/wire?tab=Apache-2.0-1-ov-file) file for details.
