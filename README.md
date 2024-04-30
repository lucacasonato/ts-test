This is a standalone reproduction for
https://github.com/microsoft/TypeScript/issues/58353.

This reproduction shows the issue when relatively importing a `.js` file (not a
`.d.ts` file), because this is what TypeScript will emit, but with other tools
that create declarations, the published package may reference between `.d.ts`
files using a `.d.ts` extension.

## Steps to reproduce

1. Clone this repository.
2. `cd library`
3. `npm install`
4. `tsc` - This will compile the TypeScript files to JavaScript and emit the
   declarations. There are no type errors.
5. `npm publish` (you can skip this, I have already published to npm as
   `@lucacasonato/typescript-issue-58353`)
6. `cd ../application`
7. `npm install`
8. `tsc` - This will type check the TypeScript files. You will encounter an
   error because TypeScript is re-type checking the `.ts` files in the
   `@lucacasonato/typescript-issue-58353` package in the `node_modules` folder,
   even though there are `.d.ts` files in the package. This fails, because there
   are some missing globals when checking the internals of the library

## Expected behavior

TypeScript should not re-type check the `.ts` files in the `node_modules` folder
when there are `.d.ts` files available.
