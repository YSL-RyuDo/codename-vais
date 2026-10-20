# CodeName VAIS

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

A team-built multiplayer FPS. I connect the C++ server, match state and Unity player experience.

Public portfolio: project overview, my contribution and development videos. The team's source repositories remain private.

**Mar 2026 - Present, 10 members: 5 software, 5 graphics**

**Server development & Unity multiplayer integration**

C++, Windows IOCP, C#, Unity

[Project page](https://ysl-ryudo.github.io/en/work/vector/)

[![CodeName VAIS — Lobby UI](https://i.ytimg.com/vi/WqRBn-yWVdo/hqdefault.jpg)](https://www.youtube.com/watch?v=WqRBn-yWVdo)

## My work

I worked across the IOCP server and Unity integration: sessions and packet dispatch, match/profile/shop flows, and reconnect presentation-state recovery. ReconnectJournal retains persistent state while excluding damage, rewards and player-command replay.

| Area | Implementation |
|---|---|
| Transport | IOCP / sessions / packet dispatch |
| Game services | Rooms, match lifecycle, career, profile, shop and social data |
| Unity integration | Gameplay packets, lobby/results, Sentinel abilities and voice permissions |
| Recovery boundary | Latest persistent presentation state; gameplay actions excluded |

## ReconnectJournal

1. **Choose recoverable state** — Observe accepts an explicit set of persistent presentation packets. Damage, rewards and player commands do not enter the recovery journal.
2. **Keep the latest version** — Entries are keyed by packet type and entity ID. A subsequent update replaces that entry instead of appending a replayable action history.
3. **Respect lifecycle changes** — New-match/round packets clear the journal. Removal packets erase the corresponding object state, and mutually exclusive states replace earlier alternatives.
4. **Restore remaining time** — Snapshot subtracts elapsed steady-clock time from stored durations and clamps at zero. Zero-duration status packets are retained to clear stale client state.

CodeName VAIS is the official title. Project Vector was its working title.

## Development videos

- [Lobby UI](https://www.youtube.com/watch?v=WqRBn-yWVdo)
- [Store system](https://www.youtube.com/watch?v=OBFWkZE2rzo)
- [Profile system](https://www.youtube.com/watch?v=A3CkQvnQEGI)
- [Earlier Project Vector demo](https://www.youtube.com/watch?v=oga4ai_1Ugg)

## Source access and credits

- [ProjectVectorServer](https://github.com/YSL-RyuDo/ProjectVectorServer) — Private, access required
- [projectVector](https://github.com/YSL-RyuDo/projectVector) — Private, access required

The scope above describes my programming contribution. Art, audio, third-party assets and other team members' work retain their original credits. Recorded demos show the project at the time of recording; they do not establish current load-test results.
