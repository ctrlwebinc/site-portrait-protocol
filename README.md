# Site Portrait protocol

Site Portrait lets a website owner connect their site to an external service
of their choice (for example a hosting provider), so that the service can read
a **read-only, privacy-minimised technical portrait** of the site: software
versions, server capacity, database size, content volumes… The typical use is
recommending suitable hosting.

The protocol is open and not tied to any service or platform. It uses an
OAuth-style pairing with PKCE, Ed25519 HTTP Message Signatures (RFC 9421) in
both directions, administrator-chosen scopes, revocation and key rotation.

| | |
|---|---|
| Specification | [`PROTOCOL.md`](PROTOCOL.md) |
| JSON schemas | [`schema/`](schema/): discovery document, portrait, network sites |
| Test vectors | [`test-vectors.json`](test-vectors.json) (also in PROTOCOL.md §15) |
| Examples | [`examples/`](examples/) |
| Reference service | [`bin/reference-client.mjs`](bin/reference-client.mjs): plays the service role, for end-to-end tests and as a starting point |
| Changes | [`CHANGELOG.md`](CHANGELOG.md) |

Current version: protocol `1`, portrait schema `1.1.0`.

## Implementations

| Platform | Implementation |
|---|---|
| WordPress | [ctrlweb Site Portrait](https://wordpress.org/plugins/ctrlweb-site-portrait/) (reference implementation) |
| Laravel | `ctrlweb/laravel-site-portrait` (in development) |

A compatible **service** must implement the checklist in PROTOCOL.md §13.

## Using this repository in an implementation's tests

Releases are tagged with the schema version (`v1.1.0`).

**Node** (reference service, schemas, vectors):

```sh
npm install --save-dev github:ctrlwebinc/site-portrait-protocol#v1.1.0
```

```js
import { createService, DEFAULT_SCOPES } from 'site-portrait-protocol/service';
import { signedFetch } from 'site-portrait-protocol/client';
import { State } from 'site-portrait-protocol/state';
import vectors from 'site-portrait-protocol/test-vectors.json' with { type: 'json' };
```

**PHP** (schemas and vectors), until the package is on Packagist:

```json
{
    "repositories": [{ "type": "vcs", "url": "https://github.com/ctrlwebinc/site-portrait-protocol" }],
    "require-dev": { "ctrlweb/site-portrait-protocol": "^1.1" }
}
```

Files are then in `vendor/ctrlweb/site-portrait-protocol/`.

## Reference service by hand

```sh
npm install
node bin/reference-client.mjs serve --port 8787 --auto-approve
node bin/reference-client.mjs serve --scopes-supported laravel,packages,storage,server,database,users_counts
node bin/reference-client.mjs status | portrait [--scopes a,b] [--validate] | sites | revoke
```

By default the service advertises the scopes of every platform; each site only
offers the ones it implements. State (keys, pairings) is kept in
`.reference-client/`.

## Contributing

Changes to the protocol are proposed as pull requests that update
`PROTOCOL.md`, the schemas, the examples and `CHANGELOG.md` together. Minor
versions may only add optional members, values or scopes; anything else needs
a new protocol version.

```sh
npm install
npm test
```

## License

- Specification, schemas, test vectors and examples:
  [CC-BY-4.0](LICENSE).
- Reference client (`bin/`): [Apache-2.0](bin/LICENSE).
