# DiPlay 0.2.19 upstream integration

Current merge source: upstream `9aa9c549ee2726770ad55ec997beeeff642990fe` (0.2.19 release merge; tag `v0.2.19` = `e06a250219b2c2288948d8b4ad7203b417db0de1`).
Upstream releases included: 0.2.17 = `391c90f80b4d54b2732dbbb3e51bf56910768ceb`, 0.2.18 = `e8d23603ccf5aa9c32ad5c999656c5fb492c83e6`.
Previous merge source: upstream `6ebb58218f90b0c0853b78013cb8431765ca77bb` (0.2.16 release merge, PR #533; tag `v0.2.16` = `bac419695ca26b7e67d204a279333ca52132c2cb`).
Previous connection reset: upstream `7887bb7bf2b52258e663a2a4ea1332382ed80ad8` (0.2.14).
Previous feature baseline: `8f53b27b3168aedb661e9f9bb7122ea344b75ada`.
App version is `0.2.19`, aligned with upstream at the user's request.
Version code increases to `42` so installed fork builds can upgrade.
Fork merge commit: `721a0814abb5e168e80b4b36508dec7de2a9de17`.

## Upstream 0.2.17-0.2.19 integration scope

Three upstream releases are merged in one integration. Retained fork behaviour: Geely
factory-hotspot routing (ECARX client route) and the separate GD bridge, steering
interception and wheel learning, HUD and cluster projection, navigation/media output
hooks, vehicle scanning, the fork's single advertised AP address policy and app-wide
immersive display mode. Adopted upstream fixes: the 5 GHz companion hotspot network,
report saving into the visible public Downloads/DiPlay folder, Harman fullscreen flags,
album-artwork recovery, the call-popup guard, retryable startup failures, car-button icon
sizing, and the Japanese, Traditional Chinese and phone-page work.
This integration remains the required path: `sync-upstream.yml` keeps refusing an
automatic merge while this file exists, because upstream changes must be applied
selectively against the fork's connection, audio and projection work.

## Full upstream merge

This integration records both fork and upstream parents. Upstream settings, appearance,
updates, video pacing, audio processing, Bluetooth and USB fixes are merged while retaining
vehicle scanning, steering interception, projection and navigation/media output hooks.
In-app updates use the fork release channel and its published APK checksums. The base
app and scan client follow upstream Android 7.1 minimum; the separate GD bridge remains
Android 9+. Factory-hotspot SDK and routed-interface experiments remain exclusively on
`fix/geely-auto-connect-apk-20261008` and are not included in this main merge.

## Previous connection reset scope

Changed network files and connection transport files are copied from the pinned upstream
revision: IPv4-first wireless selection, interface Bonjour, manual hotspot readiness and
configuration, Android 9 P2P channels, Bluetooth RFCOMM ownership, USB reads/queues and
wireless iAP2 handoff. Controller connection hunks retain projection, HUD and audio hooks.
The prior Geely interface scorer, extra LAN fanout, independent probe workers and
multi-address iAP2 endpoint are removed. The 2026-10-08 reset also restores upstream
hotspot mode persistence and runtime configuration, the complete AirPlay protocol directory,
pairing crypto and session auxiliary socket/timing behavior with matching receive statistics.
The existing renderer and its private decode queue/statistics stay paired; the upstream
media interface forwards timestamped frames to that renderer through its compatible method. The auto-connect entry uses the upstream last-used transport
preference. Local navigation/media routing and vehicle projection hooks remain separate. The upstream UI/persistence
default is MANUAL (the system/car hotspot), not P2P. The previous integration incorrectly
mapped the custom AUTOMATIC default to P2P. AUTOMATIC is retired: upstream persistence
migrates that unknown stored value to MANUAL, preserving hotspot credentials and all
explicit supported modes, including P2P on Android 9. No fallback chain is introduced.

Upstream prefers usable IPv4 for every model and retains scoped IPv6 fallback. USB NCM's
IPv6 endpoint is retained: supplied reports include working USB sessions. Upstream's AP
evidence policy is restored exactly; hidden OEM Ethernet AP ownership cannot be inferred.

## Evidence and limits

The five reports contain different stages and versions. Two G636 reports are 0.2.13.1.
G636 and KX11 failures already use IPv4, while sa8155 selects IPv6 first. Several attempts
complete Bluetooth authentication and StartSession but have no discovery or AirPlay TCP.
Others fail earlier at P2P, Bluetooth or USB. Neither universal IPv6 incompatibility nor
one common root cause is proven. Unchanged version numbers do not mean unchanged APKs.
The custom working APK has a different package/signing context; upstream code does not
reproduce factory grants. Signing, authentication secrets and package identity are retained.
Compile and source review precede submission; actual operation needs in-car confirmation.

## Features and sound

Bridge/steering interception, scanning/profile application, navigation editor, HUD and
instrument displays, three-finger projection, full map, rotary zoom, adaptive geometry and
fullscreen preferences are retained. Post-session music handoff does not change discovery.
Sound settings show navigation and media only. Non-navigation audio shares media output,
usage and legacy stream selection, retaining logical call/voice focus priorities. Old
per-call output/microphone overrides are ignored. Factory output renders CarPlay locally.
Bluetooth output retains native A2DP sink connections and observes actual playback for the
session's phone. Only confirmed Bluetooth music suppresses local media music and its focus.
Calls, voice, ringing, navigation and microphones remain negotiated and rendered normally.
Connection alone never mutes media; unsupported playback state keeps CarPlay fallback.
Neither mode forces the phone's selected destination. All non-navigation output uses media
attributes/device/stream preferences without forcing communication mode or microphone device.

## Reports

Scans use DiPlay-Vehicle timestamps, separate fallback folders and a header with model/source,
hardware/firmware, Android, ABI/screen and app/bridge versions. Unknown models stay unidentified.
Raw schema/rows remain importable. A fresh scan supplies metadata absent from old reports.
Downloads retains the DiPlay directory with distinct filenames. Cloud intake classifies
validated scans into data/vehicle-scans, accepting legacy filenames and unchanged partial
upload metadata. Total report limit is 10 MB, with at most 640 chunks. Historical scan
archives are separated with a recoverable migration manifest, without altering their content.

## Phone comparison on 2026-10-08

The user reports that upstream 0.2.14 connects immediately on the same Android phone.
Custom build logs show a system hotspot in use while the app requests a new P2P group
and receives BUSY. Earlier attempts created P2P but stopped with Bluetooth disabled.
Default mode divergence explains why the two packages may take different startup paths;
it does not prove every BUSY response is caused by that divergence. Authentication,
crypto and RTP were not reached in the latest failing attempt. Those protocol files
are restored to remove further local differences, not presented as proven causes.
The exact-upstream file manifest and source archive are kept with the repair artifacts.
Projection and audio hooks run outside hotspot creation; independent review found no
additional Wi-Fi operation in them. Physical connection after this reset still requires
a device report; compilation and CI success alone cannot establish it.
