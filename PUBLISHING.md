# Publishing to the Official MCP Registry

The registry is the upstream that directory aggregators scrape (PulseMCP is
explicit about it: while their own submissions are paused, they say publishing
to the Official Registry "is the best first step even when we are not paused",
and they ingest from it once resumed). So one publish here propagates, rather
than submitting the same server to a dozen catalogues by hand.

## Namespace

`app.quantcalc/retirement-engine` — the reverse-DNS form of `quantcalc.app`,
which requires proving we own the domain. The alternative, GitHub-based auth,
would have given `io.github.<user>/...`; the branded namespace is worth the
extra step and cannot be claimed by anyone else.

## Authentication: DNS

A TXT record on the **apex** of `quantcalc.app` (not under a selector — MCP DNS
auth follows SPF-style placement, and a record under `_mcp-auth.` is simply not
seen, failing with a generic signature error):

```
quantcalc.app.  IN TXT  "v=MCPv1; k=ed25519; p=<base64 public key>"
```

The private key lives at `~/agents_team/.credentials/mcp-registry-quantcalc.ed25519.pem`
(git-crypt encrypted, mode 600). It is NOT in this repository. If it is ever
rotated, delete the old apex TXT record — a stale one is tried first and breaks
verification.

## Republishing a new version

```bash
# from a directory containing server.json
KEY=~/agents_team/.credentials/mcp-registry-quantcalc.ed25519.pem
HEX="$(openssl pkey -in "$KEY" -outform DER | tail -c 32 | xxd -p -c 64)"
mcp-publisher login dns --domain quantcalc.app --private-key "$HEX"
mcp-publisher validate      # catches schema problems before the registry does
mcp-publisher publish
```

Bump `version` in `server.json` first; the registry treats versions as
immutable.

## Gotchas hit the first time

- `description` is capped at **100 characters**. The first attempt was rejected
  with a 422 naming the field, which is at least an honest error.
- `mcp-publisher login dns` wants the private key as **hex**, not a PEM path:
  the raw 32 bytes are the tail of the DER encoding.

## Checking the listing

```bash
curl -s "https://registry.modelcontextprotocol.io/v0/servers?search=quantcalc"
```
