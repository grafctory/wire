# @grafctory/wire-sparql-oxigraph

[![npm](https://img.shields.io/npm/v/@grafctory/wire-sparql-oxigraph)](https://www.npmjs.com/package/@grafctory/wire-sparql-oxigraph)

SPARQL connector for [@grafctory/wire](https://github.com/grafctory/wire) backed by an Oxigraph in-memory WASM store.

Exposes an [Oxigraph](https://oxigraph.org/) WASM store through the common [@grafctory/wire-sparql] `Repository`
interface, running entirely in-process with no external server required.

# Installation

```shell
npm install @grafctory/wire-sparql           # the SPARQL query API
npm install @grafctory/wire-sparql-oxigraph  # this connector
npm install oxigraph                        # peer dependency
```

> [!WARNING]
>
> TypeScript consumers must use `"moduleResolution": "nodenext"/"node16"/"bundler"` in `tsconfig.json`.
> The legacy `"node"` resolver is not supported.

# Usage

Create an in-process Oxigraph store with `createOxiRepository`, then issue queries and updates through the
[@grafctory/wire-sparql] `Repository` API:

```typescript
import { createOxiRepository } from "@grafctory/wire-sparql-oxigraph";

const repository = createOxiRepository();
```

See [@grafctory/wire-sparql] for the `Repository` API: `ask`, `select` and `construct` query the store, `update`,
`merge` and `store` write to it, and `execute` and `close` manage transactions and resources.

> [!IMPORTANT]
>
> This connector provides no transaction isolation: updates apply immediately to the in-memory store, with no `execute` bracketing.

> [!WARNING]
>
> Datasets apply to queries only: an update given a dataset listing any graphs is rejected and leaves the store as it
> was, since Oxigraph offers no way to run an update against an explicit dataset.

[@grafctory/wire-sparql]: https://grafctory.github.io/wire/modules/_grafctory_wire-sparql.html

# Support

- open an [issue](https://github.com/grafctory/wire/issues) to report a problem or to suggest a new feature
- start a [discussion](https://github.com/grafctory/wire/discussions) to ask a how-to question or to share an idea

# License

This project is licensed under the Apache 2.0 License –
see [LICENSE](https://github.com/grafctory/wire?tab=Apache-2.0-1-ov-file) file for details.
