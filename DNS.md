# Pointing mapomo.org at this site

The site is served by GitHub Pages from `main` of this repository. As of
5 August 2026 it answers at
[mapomo-africa.github.io/site](https://mapomo-africa.github.io/site/), and
`mapomo.org` still serves a GoDaddy parking page.

DNS for `mapomo.org` is hosted at GoDaddy (`ns43.domaincontrol.com`,
`ns44.domaincontrol.com`). The records below go in the GoDaddy DNS manager.

## Order matters

Set the DNS records **first**, wait for them to resolve, and only then add the
custom domain on the GitHub side. Doing it the other way round takes the site
offline: as soon as GitHub knows about the custom domain, the `github.io` URL
redirects to `mapomo.org`, which will not serve anything until DNS points here.

## Step 1: replace the existing A records

Delete the two current A records on `@` (`15.197.148.33`, `3.33.130.190`, both
GoDaddy parking) and add the four GitHub Pages addresses.

| Type | Name | Value | TTL |
|---|---|---|---|
| A | @ | 185.199.108.153 | 600 |
| A | @ | 185.199.109.153 | 600 |
| A | @ | 185.199.110.153 | 600 |
| A | @ | 185.199.111.153 | 600 |

IPv6, recommended rather than required:

| Type | Name | Value | TTL |
|---|---|---|---|
| AAAA | @ | 2606:50c0:8000::153 | 600 |
| AAAA | @ | 2606:50c0:8001::153 | 600 |
| AAAA | @ | 2606:50c0:8002::153 | 600 |
| AAAA | @ | 2606:50c0:8003::153 | 600 |

These are the current addresses published by GitHub at
`https://api.github.com/meta` under `pages`. Check them there rather than
copying them from memory if you are reading this much later.

## Step 2: point www at the org, not at the apex

`www.mapomo.org` is currently a CNAME to `mapomo.org`. Repoint it:

| Type | Name | Value | TTL |
|---|---|---|---|
| CNAME | www | mapomo-africa.github.io | 600 |

Note the value has no `/site` and no trailing dot beyond what GoDaddy adds
itself. GitHub resolves the repository from the custom domain configuration, not
from the CNAME target.

## Step 3: check that it resolves

```bash
dig +short A mapomo.org          # expect the four 185.199.x.153 addresses
dig +short CNAME www.mapomo.org  # expect mapomo-africa.github.io
```

Wait until both answer correctly. GoDaddy usually propagates within minutes at a
600 second TTL, but do not move on while the old parking addresses are still
being returned.

## Step 4: add the custom domain on GitHub

Either commit a `CNAME` file at the root of this repository containing exactly:

```
mapomo.org
```

or set it under Settings, Pages, Custom domain. The two are the same mechanism:
the setting writes the file.

Then wait for the certificate. GitHub provisions a Let's Encrypt certificate for
the apex and for `www` once it can verify the domain, which usually takes a few
minutes and occasionally up to an hour. **Do not enable Enforce HTTPS until the
certificate is issued**, or the site answers with a certificate error in the
meantime.

## Step 5: verify

```bash
curl -sIL https://mapomo.org | head -3
curl -sIL https://www.mapomo.org | head -3
```

Both should return 200 and serve this site rather than the parking page.

## Domain verification, worth doing

Under the organization settings, GitHub offers domain verification. Verifying
`mapomo.org` prevents anyone else from claiming it on GitHub Pages if a record
is ever left dangling. It costs one TXT record and closes a real takeover route.
