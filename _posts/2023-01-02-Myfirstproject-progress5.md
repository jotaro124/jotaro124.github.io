---
layout: single
title: "BoardFighter 포트폴리오 수정5"
categories: BoardFighter
toc: true
author_profile: true
sidebar:
     nav: "docs"
---



# Unreal Portfolio

대전 액션 게임의 문제에 새로운 방안을 제시하기 위해 보드게임과 격투 게임을 혼합시킨 게임입니다.

- 게임명: BoardFighter

 - 엔진 : Unreal Engine 4 (4.25.4 버전)
 - 에디터 : Microsoft Visual Studio Community 2019 (16.7.3 버전)
 - 제작기간 : 2022.01.02 ~ 2022.08.24
 - 개발규모 : 1인개발



# 시연 영상

> Youbute
>
> * 촬영 날짜:: 2022.12.31

[![썸네일](http://img.youtube.com/vi/LiIPO0efQnY/0.jpg)](https://youtu.be/LiIPO0efQnY)



# 게임 기획



## 1. 기획 의도

 기획 의도는 PVP를 선호하는 유저가 아닌 대전 액션 게임에 관심은 있지만, PVP를 선호하지 않는 유저들이 대전 액션 게임을 즐길 수 있게 하는 것을 목표로 했다. 대전 액션 게임의 대표적인 문제점 실력 이외에는 게임의 형세를 변화시킬 변수가 없는점, 사람에 따라 게임 난이도가 극과 극으로 변하는 PVP의 특징. 이 2가지를 해소를 하면 된다. 먼저, 실력 외에 변수로 보드 게임과 캐릭터 성장요소를 추가한다. 보드 게임은 운에 따라 게임의 흐름을 변화시킬수 있고, 캐릭터 성장 요소는 캐릭터를 성장시켜서 게임의 형세를 변화 시킬수 있기 때문이다. 그리고 대전 방식은 일정한 패턴이 있어 난이도 변화가 상대적으로 낮은 PVE 방식으로 한다.



## 2. 게임 특징

* 보드 게임과 액션 게임 혼합

 보드 게임과 액션 게임을 혼합한 장르이다. 게임이 일직선 진행으로 지루함을 막기 위해 운에 따라 게임의 형세가 변하는 보드 게임을 추가했다.

* 캐릭터의 성장

 대전 액션 게임 파트에 캐릭터 성장 요소를 추가했다. 캐릭터를 성장을 시켜서 게임의 형세를 변화시킬수 있는 변수로 작용할 수 있어서 자신이 유리하거나 불리할 수 있는 상황을 만들 수 있다. 또, 일정한 비용을 소비해서 성장하는 고정적 성장과 운에 의한 성장 요소도 넣어서 게임의 형세를 변화시킬수 있게 했다.

* 대전 방식은 PVE

 대전 액션 게임 파트에 대전 상대는 PVE로 했다. 일정한 패턴이 있어 PVP와 비교했들 떄 난이도 변화가 상대적으로 없는 PVE라면 대전 액션 게임에 흥미는 있지만, PVP를 선호하지 않는 유저도 대전 액션 게임을 즐길수 있다.



## 3. 전체 게임 흐름도

![Slide2](..\..\images\2023-01-02-Myfirstproject-progress5\Slide2.PNG)

1. 게임 시작 시 메인 화면으로 이동
2. 메인 화면에서는 게임 종료, 캐릭터 선택 화면으로 이동 기능 지원
3. 캐릭터 선택 화면에서 캐릭터를 선택하고, 보드 게임으로 이동한다.
4. 보드 게임에서 지도를 보거나 주사위를 던져서 이동하고, 주어진 턴이 0이 아니면 준비 단계로 이동한다.
5. 준비 단계에서 장비 장착, 장비 구매, 능력 강화를 하고, 대전 액션 게임으로 이동한다.
6. 대전 액션 게임에서 보스 몬스터를 쓰러트리면 다시 보드 게임으로 이동한다.
7. 주어진 턴이 0이 되면 보드 게임 결과가 나오고, 메인 화면으로 이동한다.



## 4. 보드 게임 흐름도

![Slide1](..\..\images\2023-01-02-Myfirstproject-progress5\Slide1.PNG)

1. 게임 시작 시 주사위를 던저서 게임 순서를 정한다.
2. 정해진 순서대로 주사위를 던져서 이동한다.
3. 참가자 모두 이동이 끝났을 때 주어진 턴이 0이 아니면 준비 단계로 이동하고, 턴이 0이면 보드 게임 결과로 이동한다.



## 5. 대전 액션 게임 흐름도

![Slide6](..\..\images\2023-01-02-Myfirstproject-progress5\Slide6.PNG)

1. 게임 시작 시 일반 적을 소환하고, 일반 적이 사망할 떄 소환 횟수가 0이 아니면 다시 일반 적을 소환한다.
1. 소환 횟수가 0이고, 특정 시간이 0이 아니면 어나더 적을 소환, 특정 시간이 0이면 보스를 소환한다.
1. 특정 시간이 0이 돌 떄까지 어나더 적을 소환하고, 어나더 적이 사망할 때 소환 횟수가 0이 아니면 일반 적을 소환, 특정 시간이 0이면 보스를 소환한다.
1. 소환 횟수가 0이고 특정 시간이 0이면 보스를 소환하고, 보스가 사망하면 보드 게임으로 이동한다.



# 수정된 내용



## 1. 캐릭터 선택 메뉴 변경

![Change1](..\..\images\2023-01-02-Myfirstproject-progress5\1_New_SelectMenu.png)



## 2. 아이템 사용 버튼 제거

![Change2](..\..\images\2023-01-02-Myfirstproject-progress5\2_Delecte_Item_Button.PNG)



## 3. 점프 애니메이션 수정

![Change3](..\..\images\2023-01-02-Myfirstproject-progress5\3_JumpAnimation.PNG)



## 4. 원거리 공격 추가

![Change4](..\..\images\2023-01-02-Myfirstproject-progress5\4_RangeAttack.PNG)



## 5. 골드 획득 기능 구현

![Change5](..\..\images\2023-01-02-Myfirstproject-progress5\5_GetGold.PNG)



## 6. 새로운 애너미 추가

![Change6](..\..\images\2023-01-02-Myfirstproject-progress5\6_NewEnemy.PNG)



## 7. 액션 게임 결과 UI 수정

![Change7](..\..\images\2023-01-02-Myfirstproject-progress5\7_New ActionResult_UI.PNG)



# 참고 자료

1. 원거리 공격 구현 방법: [UE4 RPG tutorial for beginners MAGIC spells](https://youtu.be/MOMEf93XEkY)
