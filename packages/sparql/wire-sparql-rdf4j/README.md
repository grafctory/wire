# @grafctory/wire-sparql-rdf4j

[![npm](https://img.shields.io/npm/v/@grafctory/wire-sparql-rdf4j)](https://www.npmjs.com/package/@grafctory/wire-sparql-rdf4j)

SPARQL connector for [@grafctory/wire](https://github.com/grafctory/wire) backed by an RDF4J Server repository.

Exposes any [RDF4J Server](https://rdf4j.org/) repository through the common [@grafctory/wire-sparql] `Repository`
interface, bracketing each task in a server-managed transaction. One-off operations outside a transaction are delegated
to the [SPARQL 1.1 Protocol](https://www.w3.org/TR/sparql11-protocol/) through the companion
[@grafctory/wire-sparql-http] connector.

# Installation

```shell
npm install @grafctory/wire-sparql         # the SPARQL query API
npm install @grafctory/wire-sparql-rdf4j   # this connector
```

> [!WARNING]
>
> TypeScript consumers must use `"moduleResolution": "nodenext"/"node16"/"bundler"` in `tsconfig.json`.
> The legacy `"node"` resolver is not supported.

# Usage

Connect to an RDF4J Server repository with `createRDF4JRepository`, then issue queries and updates through the
[@grafctory/wire-sparql] `Repository` API:

```typescript
import { createRDF4JRepository } from "@grafctory/wire-sparql-rdf4j";

const repository = createRDF4JRepository({
	server: "https://example.org/rdf4j-server",
	repository: "my-repo"
});
```

The optional `infer` and `timeout` settings apply to every operation: `infer: false` restricts operations to explicit
statements, and `timeout` bounds the execution time of each operation, in seconds. RDF4J Server ignores `timeout` on
updates issued within an `execute` task, so an update that needs a time bound has to run outside a transaction.

See [@grafctory/wire-sparql] for the `Repository` API: `ask`, `select` and `construct` query the store, `update`,
`merge` and `store` write to it, and `execute` and `close` manage transactions and resources.

> [!IMPORTANT]
>
> This connector defers transaction isolation to the configured RDF4J repository: server-managed transactions bracket each task
> for atomic commit and rollback, achieving the isolation level of the underlying store (RDF4J's MemoryStore and
> NativeStore both default to `SNAPSHOT_READ`). The REST API exposes no isolation-level parameter on the
> start-transaction call.

[@grafctory/wire-sparql]: https://grafctory.github.io/wire/modules/_grafctory_wire-sparql.html

[@grafctory/wire-sparql-http]: https://grafctory.github.io/wire/modules/_grafctory_wire-sparql-http.html

# Support

- open an [issue](https://github.com/grafctory/wire/issues) to report a problem or to suggest a new feature
- start a [discussion](https://github.com/grafctory/wire/discussions) to ask a how-to question or to share an idea

# License

This project is licensed under the Apache 2.0 License –
see [LICENSE](https://github.com/grafctory/wire?tab=Apache-2.0-1-ov-file) file for details.
