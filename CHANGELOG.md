# Changelog

## 1.0.12920 (2026-11-02)

Built from Waev 1.0.5 (259).

- Path Trace on the map: trace the path to a node and watch each leg
  answer, with its readings and the loop home. One trace runs at a time
  across the open tabs.
- A node with no kept route says how far it was heard.
- When several repeaters share a relay's short ID, the route card says so:
  One of 3 repeaters.
- A companion this repeater hosts appears at the repeater's place on the
  map. That place is used for the map only.
- A route shown on one tab's map stays on that tab.
- The map no longer draws its own repeater as heard directly.
- A trace in progress finishes even if you hide its tab, within the
  5-minute hold.
- The first load is smaller: each page's words load with that page.

## 1.0.12549 (2026-10-31)

- The bundled push module's licence names waev.app as its copyright holder.

## 1.0.12546 (2026-10-31)

The first public release of the Waev web companion, built from Waev 1.0.5
(255).

- Channels and direct messages in the browser, with each message's hops,
  size and delivery status.
- The mesh map: the nodes in view and a line to each node heard directly,
  with its signal and how recently it was heard. A message's route can be
  shown on the map.
- Settings › Connection: use a companion this repeater hosts, create one, or
  reach a handheld over the network or Bluetooth.
- Push for the Waev apps, off by default, replacing the `waev.push` plugin.
  Its data can be imported from `waev.push`.
- Light and dark appearance, desktop and phone-sized browsers.
