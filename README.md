# Singh Academy V60.15.2

Complete project source, not a patch. Read **LOCAL-GUIDE-V60.15.2.md** first.
See **QA-REPORT-V60.15.2.md** for executed checks and verification limits.

## Targeted correction

The original `/free-resources` and `/resources` page stubs already called `notFound()`.
They were not restored demo pages. The parent loading/Suspense path can commit a 200
response before the not-found result. They now have non-streamed HTTP Route Handler
responses returning 404, including old deep links and HEAD requests. Existing guest
login protection and the current status-page wording/appearance are retained.

The DB-backed browser launcher now uses `.next/standalone/server.js`, with the required
`public` and `.next/static` assets copied into generated standalone output. It no longer
uses `next start` against the standalone artifact. No provider, financial model or
application authentication policy is changed.

## Testing

Use a FRESH extracted folder; do not overlay this on old resource page files.
Copy only private `.env` files and reviewed backend/frontend `package-lock.json` files
from your working V60.15.1 copy. Do not copy old source, `.next` or `node_modules`.
Keep your supported Node 22.x or 24.x and working Docker installation.

```powershell
node -p "require('./package.json').version"
npm run verify:release
# Only after PASS, with Docker Engine running and normal app servers stopped:
npm run verify:integration
```

Expected version: `60.15.2`. The new candidate must pass BOTH commands end to end.
The ordinary local fixture-browser suite now declares 75 cases. The separate DB-backed
browser suite declares 46; the unchanged backend integration suite counts 77, with 12
schema-contract checks before Docker. Counts are expected targets, not reported results.

Approved public UI/CSS, original assets, certificates, videos, payment/access logic and
authentication are preserved. Full Next/Mongo/browser/online-advisory verification was not
available in the authoring environment; read the QA report rather than treating controlled
tests as full acceptance. Do not switch live keys before same-commit CI and staging approval.
