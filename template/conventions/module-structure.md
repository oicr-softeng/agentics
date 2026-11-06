# Module structure

Read this when you create, move, split or rename a module, or edit an index file or a package's `exports` field. It covers JavaScript and TypeScript packages; `code-style.md` § Module format covers file extensions and ESM.

## A concept with two or more source files gets a folder

A concept in one file stays a file. Turning a file into a folder is a commit of its own, since every import path naming the file changes.

## An index file holds the module's public surface, as named re-exports only

**`index.ts` lists every export by name.** No `export *`, `export * as name` or `export type *`, and no default export. A compatibility re-export of another package lists its names too. Implementation lives in the module's other files, so reading the index shows the whole surface.

**The exception is a default export required by a framework or tool**, such as a Next.js page or a tool's config file.

Seen in a GraphQL router: eight index files held implementation and no re-exports, and about 25 imports reached past an index.

## Inside a module, files import their siblings, never their own index

Importing the index closes a cycle, since the index re-exports the importing file. Tests are exempt, since nothing imports a test.

## From outside, code imports only a module's outermost index

At the package edge, the `exports` field is the boundary, and each subpath points at a module's index, never at a single file. Enforce it mechanically. Offer an existing tool first, per `code-style.md` § Library awareness: a lint rule restricting import paths does it. A test scanning import paths does it too, and under `node:test` adds no dependency.

## A module with types in its public surface has a `types.ts`, holding no runtime code

Where a type is inferred from a runtime schema, such as a zod schema, `types.ts` imports the schema as a type only and infers from it. An interface belongs to the module calling through it, not to the module implementing it.

## `index.test.ts` pins the module's runtime exports

It checks the index's runtime exports against a written list, so adding or removing an export means changing the test too. The module's other tests sit beside their source, per `testing.md` § Test file placement.

## Files and folders are named for what they hold

A file built around one main export is named after that export; otherwise, after its concept. A folder is named for its contents, never for the code using it. This extends `code-style.md` § Naming conventions from identifiers to files and folders.

## Line count raises a question, not a split

If the file still holds one concept, it stays one file.
