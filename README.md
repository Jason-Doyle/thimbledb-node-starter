# ThimbleDB Node starter

This repository is a local-first authenticated notes application built with
ThimbleDB, Node.js, TypeScript, and Vite.

It includes:

- a Node authority
- loopback-only development identity
- encrypted user-scoped local storage
- typed notes
- declared title and modification-time indexes
- bounded fluent queries
- deletion and restore
- memory and encrypted IndexedDB caches

## Run locally

```powershell
npm install
npm run dev
```

Open `http://127.0.0.1:5173`.

The local authority listens on `127.0.0.1:8787`. Vite proxies `/api` requests
to that authority so browser sessions remain same-origin.

## Validate the starter

```powershell
npm run doctor
npm run check
```

## Move to a hosted Node authority

The starter defaults to local files and the development identity. Before
deploying:

1. Generate the recommended Entra entries:

   ```powershell
   npx thimbledb generate-entra-roles `
     --out ".\entra-authorization.json"
   ```

2. Merge the generated roles and scope with the existing Entra application.
3. Set `NODE_ENV=production`.
4. Remove `THIMBLE_DEV_IDENTITY`.
5. Configure Microsoft Entra or another OIDC provider.
6. Set the exact `THIMBLE_ALLOWED_ORIGIN`.
7. Set `THIMBLE_HOST` for the intended runtime interface.
8. Store `THIMBLE_MASTER_KEY` and provider credentials in the platform secret
   store.
9. Keep data and authentication storage separate.

For Amazon S3 or an S3-compatible provider, install
`@aws-sdk/client-s3` and configure the documented S3 environment variables.

For Azure Blob Storage, install `@azure/storage-blob` and configure separate
data and authentication containers.

The complete provider, authentication, migration, and operations guides are
available at [thimbledb.com/docs](https://thimbledb.com/docs/).
ThimbleDB source and releases are available in the
[main repository](https://github.com/Jason-Doyle/thimble).

For automation or a live administration tool, use an OIDC service principal
with explicit application roles. `thimble.admin` authorizes administration
endpoints but does not grant database-wide access to every data scope.

## Security boundary

The development identity is accepted only with the local provider, outside
production, on a loopback host and loopback browser origin. The authority
fails startup if those conditions are not met.

Do not put storage credentials, OIDC tokens, the master key, or scope keys in
browser code or committed files.

## Licence

Apache-2.0
