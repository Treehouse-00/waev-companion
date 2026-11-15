# Changelog

## 1.0.13063 (2026-11-15)

Built from Waev 1.0.5 (261).

- **The status holds while the radio attaches.** On a companion this
  repeater hosts, Waev takes the radio only when an action needs it. That
  short wait used to turn the status light blue and say "Connecting", as if
  the link had dropped. Now the status keeps its state, and the row or
  header you used says "Waiting for the radio" until the action runs.
- **A busy channel no longer drops the link.** A path trace, or a ping to a
  repeater, waits up to 15 seconds for the radio to take it (it was 5). The
  repeater answers only once the packet is on the air, so a busy channel or
  a spent duty cycle could outlast the old wait, which dropped the link and
  dialled again.
- With a handheld linked over TCP or Bluetooth, a message send gets the same
  15 seconds.

## 1.0.13043 (2026-11-07)

Built from Waev 1.0.5 (260).

- Settings ends with the Waev apps, just above the version line: the app
  icon, "Close the tab. Keep the mesh." and the App Store and Google Play
  badges. The badges are pictures for now, not links.

## 1.0.13042 (2026-11-07)

Built from Waev 1.0.5 (260).

- Message history is on by default. A new companion starts with it, and one
  that never had it set is turned on while a browser is open; a companion
  set off stays off. Settings › Connection has the switch, says when a
  change waits for the repeater to restart, and offers the restart.
- On openHop Repeater's development branch from November 2026, a direct
  message's retries go out as one message with one timestamp, and a
  channel's region goes with each message. On a repeater without them, a
  channel send that needs its region is refused rather than sent outside it.
- Quick messages from the composer's leading button.
- A MeshCore link in a message, or pasted into Add contact, opens Waev's own
  flow; a contact link adds the contact after you confirm.
- A retrying message shows "Retrying · n of N" with Stop Retrying.
- A direct message the companion refuses reads failed, not unconfirmed.
- The resend offer wraps a long link, a menu's button never submits its
  form, and a pen's long press opens menus as a finger's does.

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
