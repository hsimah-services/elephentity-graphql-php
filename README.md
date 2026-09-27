# elephentity-graphql-php

Experimental framework-independent GraphQL integration for Elephentity's PHP
runtime, extracted from Clog's standalone backend. It builds a `webonyx/graphql-php`
schema without WordPress or WPGraphQL.

The Composer package name is `elephentity/graphql`, preserving Clog's existing
namespace and package layout. It requires PHP 8.3+, `elephentity/runtime ^0.10` and
`webonyx/graphql-php ^15.0`. No published release is assumed; use a Composer path or
VCS repository during development.

## Components

- `SchemaBuilder`: objects, inputs, interfaces, enums, connections and mutations.
- `Manifest`: the compiled entity, query, field and mutation descriptions.
- `Registration`: maps those descriptions to resolver configurations.
- `Relay` and `Resolver`: global IDs and connection results.

Storage remains behind the Elephentity runtime, so GraphQL does not depend on the
SQLite package. The same generated PHP entities can be exposed through this package
with an appropriate runtime and storage adapter.

## Status and validation

This is a prototype imported from Clog's uncommitted `feature/standalone-sqlite`
work. [Provenance](docs/clog-source.json) identifies the actual source by file hashes.
The inherited source license is preserved in [CLOG-LICENSE](CLOG-LICENSE).

Clog's integration and HTTP suites pass with this package and the extracted SQLite
package substituted into a disposable fixture. With sibling `elephentity-sqlite`
and Clog checkouts, rerun them using:

```sh
python3 ../elephentity-sqlite/tools/test-clog.py ~/Projects/clog
python3 ../elephentity-sqlite/tools/clog-status.py ~/Projects/clog
```

See [extraction notes](docs/extraction.md) for behavior still implemented in Clog
and the work required before a general release.
