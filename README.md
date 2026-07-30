# Carrier

**A direct line between two or three browsers.** No account, no install, no server holding your
video. One HTML file.

### → [Open a room](https://sudo-cthulhufhtagn.github.io/p2p_webrtc_call/)

---

Somebody opens a room and gets an 8-character code, a link and a QR. Everyone else scans or
types it. The browsers then talk **straight to each other** over WebRTC — the signalling server
only introduces them and then stops mattering. Your camera feed never lands on anyone's disk.

## What's in it

- **2–3 people**, full mesh — everyone connected to everyone, no mixing server.
- **Camera and mic off before you enter.** Join dark, join muted, or both — the same toggles are
  in the call bar.
- **A live mic meter** in the check-yourself screen, so you find out your input is dead before
  the call, not during it.
- **Mute everyone else** with one button when the room gets loud.
- **A status lamp** — red for no signalling, amber for room open and waiting, green for a direct
  encrypted link. Hover it and it tells you what it means.
- **Failures say so.** Broker down, code taken, camera blocked, clipboard denied — all of it
  surfaces as a toast instead of a button that does nothing.
- **Desktop and mobile**, one layout that actually reflows. QR is there so the phone never types
  the code.
- **Codes that survive being read aloud** — no `0`/`O`, no `1`/`I`.
- **Zero build.** No npm, no bundler, no framework. Two CDN scripts and about 400 lines.

## Run it

```bash
git clone https://github.com/sudo-Cthulhufhtagn/p2p_webrtc_call.git
cd p2p_webrtc_call
python3 -m http.server 8000     # https or localhost — getUserMedia refuses plain http
```

Then open `http://localhost:8000` in two tabs. Push to `main` and the included GitHub Actions
workflow publishes to Pages.

## How it holds together

```
host                                 guest
 │  new Peer("carrier-AB3K9XZQ")       │
 │◄────── data channel ────────────────┤   "I'm here"
 ├─────── list of peers ──────────────►│
 │◄══════ media call ═════════════════►│   ...and the guest calls
 │                                     │      every other guest too
```

The room code *is* the host's peer id, so there's nothing to look up. Guests get the roster from
the host and dial the rest themselves — which is why three people work and thirty don't.

## The honest limitations

- **No TURN server.** Connections use free STUN only, so a minority of mobile and corporate
  networks (symmetric NAT) will hang on a black tile. Pass your own `iceServers` to `new Peer()`
  if you need to survive those.
- **Signalling is PeerJS's free public broker.** Fine for a call between friends, not an SLA.
- **Mesh math.** Every extra person is another upstream copy of your video. Four is pushing it on
  a phone; that's why the door says two or three.
- **Anyone with the code is in.** There's no waiting room. Codes are single-use in practice —
  they die with the tab.

## License

MIT.
