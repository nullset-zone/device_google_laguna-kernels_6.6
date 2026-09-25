# GuardTalkOS — trunk-14072179 resolution (T-PORT-RANGO-LAYER)

`build/release/flag_values/trunk_staging/RELEASE_KERNEL_RANGO_DIR` historically
required:

    device/google/laguna-kernels/6.6/trunk-14072179/rango

## Resolution method (2026-07-25)

1. **`RELEASE_KERNEL_RANGO_DIR` (trunk_staging)** points at the real directory
   `device/google/laguna-kernels/6.6/grapheneos/rango` (akita-style durable
   aconfig fix; mirrors tokay → `…/grapheneos` and akita → `…/grapheneos`).
2. **Symlink** `trunk-14072179` → `grapheneos/` remains for any tooling that
   still looks up the old trunk path name
   (`…/trunk-14072179/rango` → `…/grapheneos/rango`).

### Why not keep the flag on the symlink?

`vendor/adevtool/.../device-common.mk` installs `init.insmod.*.cfg` via
`find-copy-subdir-files` → GNU `find` **does not traverse a symlink start
path**. With `TARGET_KERNEL_DIR=…/trunk-14072179/rango` (symlink parent),
`init.insmod.rango.cfg` can be omitted from `vendor_dlkm.img`. Belt-and-
suspenders: also `vendor/guardtalk/device/rango/guardtalk-insmod.mk`.

### Why not Google AOSP trunk-14072179 blobs?

GrapheneOS `laguna-kernels/6.6` in this tree ships only `grapheneos/`.
Inventing AOSP trunk binaries is forbidden.

### Revert symlink only

    rm device/google/laguna-kernels/6.6/trunk-14072179
