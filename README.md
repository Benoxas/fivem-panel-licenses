# fivem-panel-licenses

License allowlist for the FiveM dashboard panel.

`licenses.json` holds the **SHA-256 hashes** of every active customer's license key.
The panel fetches this file once an hour and allows access only if the customer's
`LICENSE_KEY` (hashed) is present and active.

## The URL to use in a customer's `.env`

```
LICENSE_MANIFEST_URL=https://raw.githubusercontent.com/Benoxas/fivem-panel-licenses/main/licenses.json
```

(Works on any public repo — no GitHub Pages needed.)

## Add a customer

1. In the panel project, hash their key:
   ```
   node scripts/license-hash.mjs THEIR-LICENSE-KEY
   ```
2. Add the printed hash to `licenses.json`:
   ```json
   { "<hash>": { "active": true, "domain": "their-panel-domain.com", "expires": "2026-12-31" } }
   ```
3. Give the customer (in their `.env`):
   ```
   LICENSE_KEY=THEIR-LICENSE-KEY
   LICENSE_MANIFEST_URL=https://raw.githubusercontent.com/Benoxas/fivem-panel-licenses/main/licenses.json
   ```

## Revoke a customer

Delete their hash line, or set `"active": false`. Their panel stops within ~1 hour
(immediately if they can't reach this file — it fails closed).

## Notes

- Never put raw keys or IP addresses here — only hashes.
- `domain` and `expires` are optional. Simplest form is just an array of hashes:
  `["hash1", "hash2"]`.
