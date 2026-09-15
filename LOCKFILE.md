# Why there is no pnpm-lock.yaml

Scans ea49f928 and 65da9fa1 (agent 26.9.2.1, ghc_dev_dp) both PASSED with
`totalFail: {}` and never reproduced TKA-11076, because Mend resolved the
lockfile STATICALLY:

    Lockfile is up to date, resolution step is skipped

No install ran, so no tarball was fetched, so the Azure credential was never
used, so there was nothing to reject. The `ne ERR_PNPM_FETCH_401` assertion
passed vacuously.

Without a lockfile the resolver cannot enumerate transitive dependencies
statically and must install -- which fetches from the registry in .npmrc and
therefore exercises the credential Mend injected from the org-level host rule.

`pnpm-lock.yaml.reference` (gitignored) keeps the original for regenerating a
lockfile-based variant later.
