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
`/etc/apk/repositories.d/distfeeds.list` and trust its key, so installing a
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

```
<release>/<target>/<subtarget>/packages.adb   the signed index
<release>/<target>/<subtarget>/*.apk          the kmods
```

For example `0.43a/mediatek/filogic/` serves the Flint 2 and the Zyxel
EX5601-T0, which share a target and kernel.

## How it is published

The feeds are made by MilkFlint's release builds, signed with MilkFlint's
build key, and are never edited by hand. Each publish replaces this
repository's history with a single commit holding the feeds still kept, so
the repository doesn't grow with every release; older releases' feeds may
be removed once few routers still run them.
