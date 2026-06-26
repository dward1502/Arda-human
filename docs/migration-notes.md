# annunimas-human tenant migration notes

This private tenant copy was created from `Annunimas/crates/annunimas-human` for Platform OS S2/G1.

Boundaries:

- Live `human/**` vault data was not copied.
- Runtime state and ingestion ledgers were not copied.
- The tenant remains private/local and uses path dependencies back to Annunimas core crates until a tenant-facing memory contract replaces direct path dependencies.
- The Annunimas workspace member was not removed in this step; compatibility removal remains a follow-on gate.

Verification:

- Run `cargo test --manifest-path /var/home/mythos/Eregion/annunimas-human-tenant/Cargo.toml`.
- Run `cargo test -p annunimas-human --test target_local` from Annunimas until the compatibility seam is removed.
