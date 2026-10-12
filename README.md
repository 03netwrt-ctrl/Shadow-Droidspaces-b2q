# Shadow-Droidspaces-b2q v1.0

Custom kernel for the **Samsung Galaxy Z Flip3 (SM-F711B / b2q)** that enables
[Droidspaces](https://github.com/ravindu644/Droidspaces) Linux containers —
including OpenWrt.

**Powered by 03NET** | Telegram: [@O3netwrt77](https://t.me/O3netwrt77)

---

## Why this kernel?

The stock Samsung kernel (`5.4.86-qgki`) is missing the PID and IPC namespaces,
so Droidspaces cannot run on it — its Requirements check fails on two
**MUST HAVE** items. This build enables everything Droidspaces needs:

| Feature | Status |
|---|---|
| PID namespace (`CONFIG_PID_NS`) | ✅ enabled |
| IPC namespace (`CONFIG_IPC_NS`) | ✅ enabled |
| User namespace (`CONFIG_USER_NS`) | ✅ enabled |
| devtmpfs (`CONFIG_DEVTMPFS`) | ✅ enabled |
| Network namespace, veth, bridge, OverlayFS, cgroup v2, seccomp, TUN/TAP | ✅ enabled |
| `ANDROID_PARANOID_NETWORK` | ⬜ disabled (container networking) |
| Netfilter TTL target/match (`XT_TARGET_HL` / `XT_MATCH_HL`) | ✅ enabled (tethering TTL fix) |

After flashing, the Droidspaces v6.6.0 Requirements check passes **100%**
(all MUST HAVE, RECOMMENDED and OPTIONAL items green).

## Downloads

Get the zips from the [**Releases**](https://github.com/03netwrt-ctrl/Shadow-Droidspaces-b2q/releases)
page:

- **`Shadow-Droidspaces-Kernel-Magisk-v1.0.zip`** — recommended. Magisk module:
  backs up your stock boot image automatically, then flashes the kernel during
  install. Install via Magisk → Modules → Install from storage → reboot.
- **`Shadow-Droidspaces-b2q-v1.0.zip`** — plain AnyKernel3 zip for TWRP /
  Franco Kernel Manager / manual flashing.

## Installation (Magisk module, recommended)

1. Download `Shadow-Droidspaces-Kernel-Magisk-v1.0.zip`.
2. Open the **Magisk** app → **Modules** → **Install from storage** → select the zip.
3. Wait for `Kernel flashed successfully!` (the installer checks device `b2q`
   and Android 12 first, and aborts safely without touching anything on failure).
4. **Reboot** the phone.
5. Verify: Droidspaces → Settings → Requirements → all green.

The installer automatically saves your stock boot image to
`/sdcard/Download/boot-backup-b2q-stock.img` (once — never overwritten).
**Do not delete that file**: it is your way back to the stock kernel.

## Restoring the stock kernel

If you ever want to go back:

```sh
su -c '/system/bin/dd if=/sdcard/Download/boot-backup-b2q-stock.img of=/dev/block/by-name/boot bs=4096'
```

then reboot.

## Compatibility

- Device: Samsung Galaxy Z Flip3 **SM-F711B** (`b2q`) — bootloader unlocked, Magisk rooted
- Android: **12** (One UI 4.1)
- Base: Linux `5.4.86` (kernel version string `5.4.86-ShadowOfLeaf`)

> ⚠️ Built from 2022-era sources, so the security patch level is old.
> Intended for lab / homelab use (e.g. running OpenWrt in Droidspaces),
> not as a daily-driver security update.

## Build info & source (GPL)

- Base source: [shadowofleaf96/android_kernel_samsung_b2q](https://github.com/shadowofleaf96/android_kernel_samsung_b2q)
  (Shadow Kernel for b2q)
- Changes vs. upstream defconfig: `CONFIG_PID_NS=y`, `CONFIG_IPC_NS=y`,
  `CONFIG_USER_NS=y`, `CONFIG_DEVTMPFS=y`, `CONFIG_ANDROID_PARANOID_NETWORK`
  disabled, plus the full Droidspaces container config set and
  `CONFIG_NETFILTER_XT_TARGET_HL` / `CONFIG_NETFILTER_XT_MATCH_HL`.
- The exact defconfig used for this build is in [`defconfig/`](defconfig/).
- Kernel sources are GPL-2.0; the build complies by pointing at the upstream
  source above plus the published defconfig in this repo.

## Credits

- Kernel base: **ShadowOfLeaf** (Shadow Kernel for b2q)
- AnyKernel3 backend: **osm0sis** @ xda-developers
- Droidspaces: **ravindu644**
- Build, Droidspaces config set & publishing: **03NET**

---

**Powered by 03NET** — Telegram [@O3netwrt77](https://t.me/O3netwrt77) · [03net.my.id](https://03net.my.id)
