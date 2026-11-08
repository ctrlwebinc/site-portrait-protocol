# Changelog

Versions follow the portrait schema version (SemVer). The protocol version is
`1` for every `1.x.y` release; minor versions only add optional members, values
or scopes.

## 1.1.0

- `platform` member in the token request, `/status` and the portrait
  (`name`, `version`; for WordPress, best-effort `distribution`,
  `distribution_version`, `composer_managed`, `acorn`). Absent means
  `wordpress`. `plugin_version` / `plugin.version` designate the version of the
  site-side implementation.
- Scopes depend on the platform; sites ignore the scopes they do not
  implement. New Laravel scopes `laravel`, `packages` and `storage`
  (`storage.files` optional).
- `database.engine` also accepts `pgsql` and `sqlsrv`; `database.options`,
  `database.transients_count` and `database.tables.site_count` are WordPress
  only and `null` elsewhere.
- Specification text made platform-independent ("site-side implementation"),
  WordPress being the reference implementation used in examples.
- Test vectors published as `test-vectors.json`.
- Reference service: configurable scope list (`--scopes-supported`,
  `scopesSupported`), every schema scope by default.
- Moved out of the WordPress plugin repository into this repository.

## 1.0.0

- Initial version, published with the WordPress plugin *ctrlweb Site Portrait*
  1.0.0.
