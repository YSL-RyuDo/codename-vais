# CodeName VAIS

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

A team-built multiplayer FPS. I connect the C++ server, match state and Unity player experience.

Public portfolio: project overview, my contribution and development videos. The team's source repositories remain private.

| Project | Details |
|---|---|
| Period and team | Mar 2026 ~ Present, 10 members: 5 software, 5 graphics |
| My role | Server development & Unity multiplayer integration |
| Stack | C++, Windows IOCP, C#, Unity |

[Project page](https://ysl-ryudo.github.io/en/work/vector/)

[![CodeName VAIS, Lobby UI](https://i.ytimg.com/vi/WqRBn-yWVdo/hqdefault.jpg)](https://www.youtube.com/watch?v=WqRBn-yWVdo)

## My work

### ProjectVectorServer

Implemented IOCP connection handling, sessions and packet dispatch for the game client.

Worked on room/match lifecycle, profile and career data, social features, shop services, and their client contracts.

Added reconnect presentation-state recovery and party/team voice authorization. ReconnectJournal stores persistent state instead of replaying damage, rewards or player commands.

### projectVector

Connected character abilities to packet handling and gameplay, including Sentinel ability integration.

Worked on matchmaking and match/result presentation, career/profile data, friends, shop integration, and reconnect/voice client flows.

Integrated team-provided controller models/motion, adjusted ADS alignment, expanded the practice range, and added editor verification tools. Integration does not mean authorship of the models or animations.

## Implementation and design

A multiplayer FPS has to connect transport, match lifecycle and player-facing state. Rejoining a session also requires deciding what state to restore and what actions must not run again.

I worked across the IOCP server and Unity integration: sessions and packet dispatch, match/profile/shop flows, and reconnect presentation-state recovery. ReconnectJournal retains persistent state while excluding damage, rewards and player-command replay.

| Area | Implementation |
|---|---|
| Transport | IOCP / sessions / packet dispatch |
| Game services | Rooms, match lifecycle, career, profile, shop and social data |
| Unity integration | Gameplay packets, lobby/results, Sentinel abilities and voice permissions |
| Recovery boundary | Latest persistent presentation state; gameplay actions excluded |

## ReconnectJournal

**Choose recoverable state**

Observe accepts an explicit set of persistent presentation packets. Damage, rewards and player commands do not enter the recovery journal.

**Keep the latest version**

Entries are keyed by packet type and entity ID. A subsequent update replaces that entry instead of appending a replayable action history.

**Respect lifecycle changes**

New-match/round packets clear the journal. Removal packets erase the corresponding object state, and mutually exclusive states replace earlier alternatives.

**Restore remaining time**

Snapshot subtracts elapsed steady-clock time from stored durations and clamps at zero. Zero-duration status packets are retained to clear stale client state.

CodeName VAIS is the official title. Project Vector was its working title.

## Development videos

[Lobby UI](https://www.youtube.com/watch?v=WqRBn-yWVdo)

[Store system](https://www.youtube.com/watch?v=OBFWkZE2rzo)

[Profile system](https://www.youtube.com/watch?v=A3CkQvnQEGI)

[Earlier Project Vector demo](https://www.youtube.com/watch?v=oga4ai_1Ugg)

## Source access and credits

[ProjectVectorServer](https://github.com/YSL-RyuDo/ProjectVectorServer), Private, access required

[projectVector](https://github.com/YSL-RyuDo/projectVector), Private, access required

The following links identify the recorded source revisions. Access requires permission to the original private repositories.

| Repository | Implementation record |
|---|---|
| ProjectVectorServer | [IOCP](https://github.com/YSL-RyuDo/ProjectVectorServer/blob/3066fe6d90fb4a56a6c440af56c4e119188e73fc/ProjectVectorServer/ProjectVectorServer/IocpCore.cpp) |
| ProjectVectorServer | [ReconnectJournal](https://github.com/YSL-RyuDo/ProjectVectorServer/blob/3066fe6d90fb4a56a6c440af56c4e119188e73fc/ProjectVectorServer/ProjectVectorServer/ReconnectJournal.h) |
| ProjectVectorServer | [Validation sources](https://github.com/YSL-RyuDo/ProjectVectorServer/tree/3066fe6d90fb4a56a6c440af56c4e119188e73fc/Tests) |
| projectVector | [Sentinel](https://github.com/YSL-RyuDo/projectVector/commit/756c9a09f32ed4a073d5d3d60008b1aaa44f0f60) |
| projectVector | [Practice & ADS](https://github.com/YSL-RyuDo/projectVector/commit/f4457e2d77c251b9e0da5d976927303d04a64b43) |
| projectVector | [Reconnect & voice](https://github.com/YSL-RyuDo/projectVector/commit/b62cbeeb16e72738d2dd7c491c8d42ec31afcd18) |
| projectVector | [Additional YSL work](https://github.com/YSL-RyuDo/projectVector/commit/5e5c3c53b010fb2807f6b53c0eb0f5ceec8d1350) |

The scope above describes my programming contribution. Art, audio, third-party assets and other team members' work retain their original credits. Recorded demos show the project at the time of recording; they do not establish current load-test results.
