# Tested custom Home configurations

The `gal/unified-tv-home` branch combines the optional tabs, Close all and webOS 4
compatibility contribution from PR #271 with the maintainer's webOS 7 eligibility
change from PR #244. It retains both Home integration methods in one package.
Personal preferences, remote mappings and installed-app data are not bundled.

| Capability | LG 43UQ75006LG, webOS 7.6.0 | LG 65UK6300YVB, webOS 4.4.3-22 |
| --- | --- | --- |
| Firmware | 04.60.92 | 05.50.70 |
| Hardware-tested custom build | 3.2.1-based unified build | Earlier 3.1.0-based build |
| Home integration | Native lifecycle-v2 Home executable mount | Compositor filter; embedded OEM Home |
| Home / Apps / Settings | Verified; drawer disabled | Verified; drawer disabled |
| Back from Home | Luma Recent apps | Luma Recent apps |
| Recent apps backend | LG recent-history service | SAM running-card inventory |
| X Close all | Verified; normal apps closed through SAM | Verified; normal apps closed through SAM |
| Protected apps | Home, Live TV, AirPlay and overlays retained | Home, inputs, AirPlay and overlays retained |
| Home after opening an app | Same native Home process | Standalone Luma surface, reset to Home |
| Existing remote mapper | Preserved; Prime and night-mode actions verified | Preserved; owner confirmed Prime and Voice |

The first TV also displays the new on-TV **Settings > General > Tabbed
navigation** switch. Its preferences and mapper files were byte-identical before
and immediately after installation. Home from Settings resets to the Home page;
Down does not open the drawer when tabs are enabled.

After a software restart, the updated executable was mounted again and Luma was
foreground as `com.webos.app.home`. The early-start log records uptime 19.38
seconds; an independent status check at 56 seconds confirmed the mounted checksum,
unchanged preferences and the active original remote mapper.

The unified 3.2.1 package has not been installed on the second TV. Its earlier
hardware results establish the legacy integration's behavior on that specific
device, rather than verification of every later build or other models.

## Installation and updates

Follow [the legacy Home guide](webos4-home.md) on webOS 4. Do not bind-mount a
native Home executable there: the tested firmware has no separate Home app.

On a matching native lifecycle-v2 webOS 7 Home, use the normal root integration:

```sh
cd /media/developer/apps/usr/palm/applications/com.ecklf.luma
sh home.sh enable
sh /var/lib/luma-home/home.sh status
```

An update must refresh the copied helpers and replace the old mounted executable
inode by running `enable` from the newly installed application directory. Merely
installing a package does not refresh an existing bind mount. Confirm the mounted
Home executable and installed `luma` have the same checksum.

To restore LG's native Home:

```sh
sh /var/lib/luma-home/home.sh disable
```

Keep the custom build until the needed contributions are present in an upstream
release. A stock upstream package may omit the optional UI or firmware support.
Hard power removal and all Quick Start/standby combinations remain unverified.
