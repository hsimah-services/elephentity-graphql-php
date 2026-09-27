# Generic GraphQL extraction

## Source and boundaries

Source: Clog's `server/standalone/vendor/elephentity/graphql` on
`feature/standalone-sqlite`, captured while still uncommitted. See `clog-source.json`.
The package uses `GraphQL\Type` classes directly, with no WordPress registration
hooks. Some inherited comments still describe WPGraphQL configuration conventions.
Global IDs retain the existing Elephentity loader/type/id encoding for compatibility.

`server/standalone/src/GraphQL.php` in Clog assembles the complete application schema.
It installs Node and PageInfo, validates ID types, wraps connection arguments and
adds inventory totals. These application responsibilities have not all moved into
the generic package yet.

## Work before a general release

1. Provide generic Node and PageInfo registration from the manifest and runtime.
   Remove Clog-specific class-name and type-name assumptions from the proposed API.
2. Make type-aware ID validation reusable for root fields, nodes, relationship inputs
   and mutations. The imported `GlobalId::entityId()` unwraps IDs without verifying
   the requested entity type; Clog performs additional validation in its wrapper.
3. Centralize connection validation, including malformed cursors, first limits and
   explicit rejection of unsupported backward pagination. Clog's root wrappers
   currently enforce these rules; the generic connection resolver still clamps first
   and ignores last/before. Apply validation to nested connections as well.
4. Allow query-only schemas. `SchemaBuilder::build()` currently always includes
   RootMutation, even when no mutation fields have been registered.
5. Define standalone manifest generation beside the PHP target. Clog currently uses
   a manually adapted manifest. A generic GraphQL builder must emit this namespace
   without depending on the WPGraphQL runtime or requiring WordPress configuration.
6. Add package-owned tests for minimal/query-only schemas, custom scalars, Node
   resolution, wrong-type IDs, nested pagination, errors and policy enforcement.
   Verify dependency resolution and runtime/webonyx compatibility independently of
   Clog's bundled dependency tree before releasing.

Keep HTTP transport, authentication/session management, CSRF, application policies,
stock totals and custom inventory queries in Clog. The generic schema must continue
to call the runtime so entity policies apply consistently.

## Evidence

Clog's standalone integration and HTTP suites passed with the two extracted packages.
They exercise pagination, Node identity, mutation client IDs, wrong-entity root IDs,
read/write authorization, sessions and CSRF. Some guarantees are supplied by Clog's
wrapper; passing those tests does not mean this bare package supplies them itself.

## Source commit verified

Clog committed the standalone implementation as `fb57b3c`. Every imported library
file matches that commit. The disposable integration and HTTP suites also passed
with Clog's updated manifests and viewer. Development continues on the same branch.
