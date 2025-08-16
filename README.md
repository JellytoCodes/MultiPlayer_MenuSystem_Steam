# Multiplayer Menu System (Steam)

> 간단 멀티플레이 메뉴 샘플.  
> HOST / JOIN / QUIT 3버튼으로 세션 생성·참여·종료
---

<br>

## TL;DR
 - 레벨 시작 시 WBP_Menu 생성 → MenuSetup(NumPublicConnections, MatchType, LobbyPath) 호출
 - Host: 세션 생성 → ServerTravel(LobbyPath)
 - Join: 세션 검색 → 매칭되는 세션 접속 → ClientTravel
 - Quit: 게임 종료
<img width="1543" height="865" alt="image" src="https://github.com/user-attachments/assets/c656adfb-ef74-4ef9-8596-9a135382ecbc" />

<br>

## 요구사항
 - Unreal Engine 5.4.4
 - OnlineSubsystemSteam 활성화
 - 테스트용 steam_appid.txt(예: 480)와 Steam 클라이언트 실행

<br>

## DefaultEngine.ini 최소 설정 예:
```
[OnlineSubsystem]
DefaultPlatformService=Steam

[OnlineSubsystemSteam]
bEnabled=true
bUseLobbiesIfAvailable=true
bInitServerOnClient=true

[PacketHandlerComponents]
+Components=OnlineSubsystemSteam.SteamAuthComponentModuleInterface
```

<br>

## 주요 구성
```
Plugins/
  MPMenuSystem/
    Source/
      MPMenuSystem/            // 플러그인 모듈
        Public/
          MenuSubsystem.h      // 세션 생성/검색/조인 래퍼
          MenuWidget.h         // UUserWidget 기반 메뉴
        Private/
          MenuSubsystem.cpp
          MenuWidget.cpp
Content/
  UI/WBP_Menu.uasset           // 메뉴 위젯
Maps/StartMap                   // 로비/시작 맵
```

<br>

## 기능 요약
 - 세션 생성/검색/조인 래핑
 - ServerTravel / ClientTravel 흐름 분리
 - 매치 타입 필터링(MatchType)
 - 에러/완료 델리게이트 바인딩으로 UI 피드백
