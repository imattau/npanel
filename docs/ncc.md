# NCC Browser

Npanel can serve any enabled nsite through an NCC Browser service endpoint.
Set that service's endpoint URL to the Npanel dashboard endpoint with the
site's Nostr address as a path prefix:

```text
https://<npanel-host>/nsite/<nsite-npub-or-naddr>/
```

For example, configure a locator service named `docs` with endpoint
`https://panel.example.com/nsite/naddr1.../`, then open:

```text
ncc://<npanel-service-npub>/docs/
```

The selected address must match an enabled host in Npanel. The route accepts
the same nsite address forms as the dashboard (including `npub` for a root
site, and `naddr` for a root or named site). NCC Browser appends the requested
path to the endpoint's path prefix, so root-relative links and assets remain
inside the selected nsite. A page can be opened directly too:

```text
ncc://<npanel-service-npub>/docs/about?lang=en
```

Create one locator service per nsite, each with its endpoint path set to that
site's `/nsite/<address>/` prefix. Each endpoint uses the dashboard's normal
HTTP(S) hostname and port. Npanel resolves the site from the path and serves
it using the configured hostname's normal site policies. The dashboard itself
and existing hostname-based nsite serving keep their usual routes.

Npanel does not create or publish the NCC service locator. Configure the NCC
service's locator event and endpoint in the NCC sidecar/browser setup. This
path selector lets one such service expose multiple enabled nsites.

## Administrators

Npanel keeps its own `users` table, so an NCC node administrator is not
automatically an Npanel administrator. A deployment that wants the two lists
in step can drive the [users API](api.md#users--admin) with NIP-98 from a key
that is already an Npanel admin: `POST /api/users` creates the user as staff,
then `PATCH /api/users/<pubkey>` promotes it to `admin`. The operator key is
made an admin at install time with `npanel make-admin`, which is what makes
the first such call possible.
