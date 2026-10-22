# Proprietary Vendor Blobs for Motorola Edge 40 (`lyriq`)

Pre-extracted and patched **Android 15 (`V1TLS35.73-60-3-10`)** proprietary vendor blobs for **Motorola Edge 40 (`lyriq`)** (`lineage-22.1`).

## Included Binary Patches & Customizations

1. **Trustonic TEE Daemon (`proprietary/vendor/bin/mcDriverDaemon`):**
   - **Binary Patch Applied:** `test-keys` string patched to `test-xxxx`.
   - **Why:** On custom ROM builds (`ro.build.tags=test-keys`), stock `mcDriverDaemon` restricts trustlet search paths exclusivamente to `/mnt/vendor/tzapp`, ignoring factory-provisioned trustlets (such as `030b` SPI & Goodix/Egis FOD trustlets) stored in `/data/vendor/mcRegistry` and `/vendor/app/mcRegistry`. Patching `test-keys` $\rightarrow$ `test-xxxx` forces the release-keys registry search path so TEE/KeyMint and Fingerprint trustlets load properly on every boot.

2. **Sensors SubHALs (`sensors.moto.so` & `android.hardware.sensors@2.X-subhal-mediatek.so`):**
   - Pure Stock Lyriq Android 15 64-bit/32-bit binaries (`proprietary/vendor/lib64/sensors.moto.so` `89,688 B` & `proprietary/vendor/lib64/hw/android.hardware.sensors@2.X-subhal-mediatek.so` `44,048 B`) linking cleanly against native Android 15 libraries without legacy shims.
   - Configured in `Android.bp` to install `sensors.moto.so` directly to `/vendor/lib64/` and `/vendor/lib/` matching `/vendor/etc/sensors/hals.conf`.

3. **GNSS Daemon (`proprietary/vendor/bin/mnld`):**
   - Patched to link against `android.hardware.sensors@1.0-convert-shared.so` (built via `PRODUCT_PACKAGES` in device tree) for A15 sensor compatibility.

4. **Trustonic KeyMint 3.0 & Gatekeeper:**
   - Ships stock `android.hardware.security.keymint@3.0-service.trustonic_norkp` and `android.hardware.gatekeeper-service.trustonic` with their respective VINTF manifest fragments.
