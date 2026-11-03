<img src="logo.svg" width="96" alt="MilkFlint logo: a flint rock, half full of milk">

# MilkFlint packages

Kernel module packages (kmods) for
[MilkFlint](https://github.com/OddBlueDog/openwrt), served with Cloudflare
Pages.

MilkFlint runs its own kernel, so OpenWrt's kmod packages don't install on
it: each kmod names the exact kernel it was built for, and `apk` refuses it
on any other. This repository holds a feed for each MilkFlint release, built
from the same tree as that release's images, so its kmods match that
release's kernel exactly. Everything else (programs, LuCI apps) still comes
from OpenWrt's own feeds.

No feeds have been published yet. The first comes with a coming release.

## Using it

Images that have a feed here already list it in
`/etc/apk/repositories.d/kmods.list` and trust its key, so installing a
kmod works as on OpenWrt:

```sh
apk update
apk add kmod-usb-net-rndis
```

Each feed only fits the release it was built with. After upgrading
MilkFlint, the new image points at its own feed: install any kmods you had
added again. Releases without a feed here can't install kmods; upgrade to
one that has one.

## Layout

The same as downloads.openwrt.org's, one folder per kernel:

```
targets/<target>/<subtarget>/kmods/<kernel>/packages.adb   the signed index
targets/<target>/<subtarget>/kmods/<kernel>/*.apk          the kmods
```

`<kernel>` is `<version>-<release>-<vermagic>`, for example
`targets/mediatek/filogic/kmods/6.12.115-1-<hash>/`, and serves the Flint 2
and the Zyxel EX5601-T0, which share a target and kernel. MilkFlint's
vermagic covers the kernel's patches as well as its config, so a folder
only ever fits one kernel. `feeds.tsv` lists the folders kept, newest first,
with the release each came with; the page lists them too.

## Licences and source

Each package keeps its own licence, which its metadata names
(`apk info -a <package>`); most kmods are GPL-2.0-only, the Linux kernel's
licence. Each feed's folder says where its source is, in `SOURCE.txt`:

- the kernel's patches, the mt76 Wi-Fi driver fork and MilkFlint's own
  packages are in [MilkFlint's OpenWrt tree](https://github.com/OddBlueDog/openwrt),
  at the commit (and release tag) `SOURCE.txt` names;
- Linux itself is kernel.org's release that tree pins;
- OpenWrt's package feeds are at the commits in `feeds.buildinfo`.

`config.buildinfo` is the build's configuration and `version.buildinfo`
OpenWrt's revision. This repository's own files are under the GPL-2.0
(`LICENSE`).

## How it is published

The feeds are made by MilkFlint's builds, signed with MilkFlint's key, and
are never edited by hand. From time to time this repository's history is
replaced with a single commit holding the feeds still kept, so it doesn't
grow with every release; older releases' feeds may be removed once few
routers still run them.
