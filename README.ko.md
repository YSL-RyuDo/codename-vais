# CodeName VAIS

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

팀으로 개발 중인 멀티플레이 FPS. C++ 서버와 경기 상태를 Unity의 플레이 경험에 연결합니다.

프로젝트 소개, 담당 작업과 개발 시연을 정리한 공개 포트폴리오입니다. 팀의 원본 코드 저장소는 비공개로 유지합니다.

| 프로젝트 정보 | 내용 |
|---|---|
| 기간과 팀 구성 | 2026.03 ~ 현재, 10명, 소프트웨어 5명, 그래픽 5명 |
| 내 역할 | 서버 개발, Unity 멀티플레이 연동 |
| 사용 기술 | C++, Windows IOCP, C#, Unity |

[프로젝트 상세](https://ysl-ryudo.github.io/ko/work/vector/)

[![CodeName VAIS, Lobby UI](https://i.ytimg.com/vi/WqRBn-yWVdo/hqdefault.jpg)](https://www.youtube.com/watch?v=WqRBn-yWVdo)

## 담당 작업

### ProjectVectorServer

게임 클라이언트를 위한 IOCP 연결 처리, 세션과 패킷 디스패치를 구현했습니다.

방, 경기 수명주기, 프로필, 커리어 데이터, 소셜 기능, 상점 서비스와 클라이언트 규격을 작업했습니다.

재접속 시 표시 상태 복원과 파티, 팀 음성 권한 처리를 추가했습니다. ReconnectJournal은 피해, 보상, 플레이어 명령을 재실행하지 않고 지속 상태를 보관합니다.

### projectVector

센티널을 포함한 캐릭터 스킬의 패킷 처리와 게임플레이 연동을 구현했습니다.

매칭과 경기, 결과 연출, 커리어, 프로필 데이터, 친구, 상점 연동, 재접속, 음성 클라이언트 흐름을 작업했습니다.

팀원이 제공한 컨트롤러 모델, 모션 통합, ADS 정렬, 훈련장 확장과 에디터 검증 도구를 작업했습니다. 통합 작업을 모델, 애니메이션 원작 제작으로 주장하지 않습니다.

## 구현과 설계

멀티플레이 FPS에서는 통신뿐 아니라 경기 수명주기와 플레이어에게 보이는 상태가 연결되어야 합니다. 재접속에서는 복구할 상태와 다시 실행하면 안 되는 행동을 구분해야 합니다.

IOCP 서버의 세션, 패킷 처리부터 Unity의 경기, 프로필, 상점 연동, 재접속 시 표시 상태 복원까지 작업했습니다. ReconnectJournal에는 지속 상태를 보관하고 피해, 보상, 플레이어 명령 재실행은 제외했습니다.

| 영역 | 구현 |
|---|---|
| 연결, 통신 | IOCP, 세션, 패킷 디스패치 |
| 게임 서비스 | 방, 경기 수명주기, 커리어, 프로필, 상점, 소셜 데이터 |
| Unity 연동 | 게임플레이 패킷, 로비, 결과, Sentinel 능력과 음성 권한 |
| 복원 경계 | 최신 지속 표시 상태 보관, 게임 행동 재실행 제외 |

## ReconnectJournal

**복원할 상태 선택**

Observe는 명시된 지속 표시 패킷을 수집합니다. 피해, 보상, 플레이어 명령은 복원 기록에 포함하지 않습니다.

**최신 상태로 교체**

패킷 종류와 개체 ID를 키로 저장합니다. 후속 갱신은 같은 항목을 교체하므로 행동 이력을 순서대로 재생하는 방식과 구분됩니다.

**수명주기 변경 반영**

새 경기, 라운드 패킷은 기록을 초기화합니다. 제거 패킷은 대응하는 개체 상태를 지우고, 서로 배타적인 상태는 이전 항목을 정리합니다.

**남은 시간으로 복원**

Snapshot은 저장된 지속 시간에서 steady clock 경과 시간을 빼고 0 이상으로 제한합니다. 0초 상태 패킷도 전달해 클라이언트의 오래된 상태를 해제합니다.

CodeName VAIS가 정식 명칭이며 Project Vector는 이전 임시 이름입니다.

## 개발 시연 영상

[Lobby UI](https://www.youtube.com/watch?v=WqRBn-yWVdo)

[Store system](https://www.youtube.com/watch?v=OBFWkZE2rzo)

[Profile system](https://www.youtube.com/watch?v=A3CkQvnQEGI)

[Earlier Project Vector demo](https://www.youtube.com/watch?v=oga4ai_1Ugg)

## 소스 접근과 크레딧

[ProjectVectorServer](https://github.com/YSL-RyuDo/ProjectVectorServer), 비공개, 접근 권한 필요

[projectVector](https://github.com/YSL-RyuDo/projectVector), 비공개, 접근 권한 필요

아래 링크는 설명의 근거가 되는 코드 버전과 변경 기록입니다. 원본 비공개 저장소의 접근 권한이 필요합니다.

| 저장소 | 구현 기록 |
|---|---|
| ProjectVectorServer | [IOCP](https://github.com/YSL-RyuDo/ProjectVectorServer/blob/3066fe6d90fb4a56a6c440af56c4e119188e73fc/ProjectVectorServer/ProjectVectorServer/IocpCore.cpp) |
| ProjectVectorServer | [ReconnectJournal](https://github.com/YSL-RyuDo/ProjectVectorServer/blob/3066fe6d90fb4a56a6c440af56c4e119188e73fc/ProjectVectorServer/ProjectVectorServer/ReconnectJournal.h) |
| ProjectVectorServer | [Validation sources](https://github.com/YSL-RyuDo/ProjectVectorServer/tree/3066fe6d90fb4a56a6c440af56c4e119188e73fc/Tests) |
| projectVector | [스킬](https://github.com/YSL-RyuDo/projectVector/commit/756c9a09f32ed4a073d5d3d60008b1aaa44f0f60) |
| projectVector | [훈련장, 조준](https://github.com/YSL-RyuDo/projectVector/commit/f4457e2d77c251b9e0da5d976927303d04a64b43) |
| projectVector | [재접속, 음성](https://github.com/YSL-RyuDo/projectVector/commit/b62cbeeb16e72738d2dd7c491c8d42ec31afcd18) |
| projectVector | [YSL 추가 작업](https://github.com/YSL-RyuDo/projectVector/commit/5e5c3c53b010fb2807f6b53c0eb0f5ceec8d1350) |

위 내용은 제가 담당한 프로그래밍 범위입니다. 그래픽, 음향, 외부 에셋과 다른 팀원의 작업은 원래 크레딧을 유지합니다. 영상은 촬영 당시의 개발 기록이며 현재 버전의 부하 시험 결과를 의미하지 않습니다.
