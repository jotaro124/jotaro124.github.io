---
layout: single
title: "유니티 모바일 빌드 방법"
categories: AndroidBuild
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---





 유니티로 제작한 게임을 안드로이드 앱으로 빌드하는 방법을 알고 싶어서 조사했습니다. 

 그러던 중 유니티로 게임을 개발, 안드로이드 앱으로 빌드, 플레이 스토어에 출시, 광고 수익화하는 방법을 알려주는 강의를 찾았습니다.

 이 영상을 참고해서 안드로이드 앱으로 빌드하는 방법을 정리했습니다.



> 참고 영상

[![썸네일](http://img.youtube.com/vi/EqoU1PodQQ4/0.jpg)](https://youtu.be/EqoU1PodQQ4?t=8241)



# 안드로이드 앱 빌드를 위한 도구 설치





![2.Modul_2023-02-02 205321](..\..\images\2023-02-02-UnityMobileBuild\2.Modul_2023-02-02 205321.png)

유니티에서 안드로이드 앱으로 빌드를 위한 도구 JDK, SDK, NDK는 UnityHub에서 모듈추가를 하면 간편하게 설치할 수 있지만, 설치가 되지 않는 경우가 발생해서 수동으로 설치해야 하는 경우가 발생한다.



![1.ExternalTool_2023-02-02 204826](..\..\images\2023-02-02-UnityMobileBuild\1.ExternalTool_2023-02-02 204826.png)



## 1. JDK 설치

![3.JDK_2023-02-02 205624](..\..\images\2023-02-02-UnityMobileBuild\3.JDK_2023-02-02 205624.png)

- [해당 링크](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html)에서 JDK를 설치할 수 있다.
- 그리고 오라클 JDK를 설치하려면 오라클 계정이 필요한데 [여기](https://comclothing.tistory.com/24)를 참고하면 된다.



## 2. SDK 설치

![4.Android_Studio_2023-02-02 213210](..\..\images\2023-02-02-UnityMobileBuild\4.Android_Studio_2023-02-02 213210.png)

[Android Studio]( https://developer.android.com/studio)를 설치하고 SDK Manager에 들어가서 Android SDK를 설치하면 된다.



![5.SDKManager_2023-02-02 213655](..\..\images\2023-02-02-UnityMobileBuild\5.SDKManager_2023-02-02 213655.png)



## 3. NDK 설치

![image-20230202231553451](..\..\images\2023-02-02-UnityMobileBuild\image-20230202231553451.png)

NDK 설치는 유니티에서 요구한 버전이 필요하다.

[해당 링크](https://dl.google.com/android/repository/android-ndk-r19-windows-x86_64.zip)에서 다운로드 할 수 있다. 단, Window 버전이다.



# 안드로이드 앱 빌드 관련 설정



## 1. Comapny Name, Product Name, Version, Default Icon 설정

![image-20230202235025885](..\..\images\2023-02-02-UnityMobileBuild\image-20230202235025885.png)

- Company Name: 만든 사람 또는 만든 회사 이름을 입력
- Product Name: 스마트폰에 보여질 앱의 이름을 입력
- Version: 계속 업데이트가 되려면 버전 기입은 필수
- Default Icon: 스마트폰에 보여질 앱의 아이콘 설정

Company Name과 Product Name은 고유값이기 때문에 잘 선정해야 한다.



## 2. Orientaion 설정

![image-20230202235155977](..\..\images\2023-02-02-UnityMobileBuild\image-20230202235155977.png)

Orientaion은 스마트폰에서 화면의 방향 전환을 세팅할 수 있다.

- Default Orientation: Auto Rotation을 설정하면 스마트폰 방향에 따라 가로, 세로 자동으로 회전

![image-20230205183856931](..\..\images\2023-02-02-UnityMobileBuild\image-20230205183856931.png)

- Portrait: 홈버튼 아래쪽으로 고정
- Portrait Upside Down: 홈버튼 위쪽으로 고정
-  Landscape Right: 홈버튼 오른쪽으로 고정
- Landscape Left:  홈버튼 왼쪽으로 고정



## 3. Ohter Setting

![image-20230202235258430](..\..\images\2023-02-02-UnityMobileBuild\image-20230202235258430.png)

### Identification

- Packgae Name: 위에서 설정한 Company Name, Product Name
- Bundle Version Code: 업데이트 할 때 숫자를 올리면서 업테이트 (동일한 버전의 APK 파일 또는 App bundle을 등록하면 오류가 발생)
- Target API Level: Android 11 (API Level 30) 이상

### Configuraton

- Scripting Backend: IL2CPP로 설정
- 2019 이후 앱을 배포하기 위해서는 64비트를 지원해야 하기 때문에 ARM64 항목 체크



## 4. Publishing Settings

앱을 빌드하기 위해서는 Keystore을 요구한다.

![image-20230205190256165](..\..\images\2023-02-02-UnityMobileBuild\image-20230205190256165.png)

사용중인 Keystore가 없다면 KeyStore Manager-> Keystore->In Dedicated Location에 들어가서 생성



![image-20230205190552192](..\..\images\2023-02-02-UnityMobileBuild\image-20230205190552192.png)

New Key Values

- Alias: 키 별칭
- Password: 비밀번호 (반드시 어딘가에 기록해야 함)
- Validity (years): 키 유효 기간
- First and Last Name: 이름
- Organization: 회사
- City or Locality: 도시
- state or Province: 지역
- Country Code (XX): 국가 코드



# 안드로이드 빌드

![image-20230205191604208](..\..\images\2023-02-02-UnityMobileBuild\image-20230205191604208.png)

Player Settting이 완료되면 Build Settings으로 돌아가 Build 한다.

- Bundle App Bundle (GooglePlay): 구글 출시에 사용되므로 항목을 체크한다.



# 참고자료

1. [모바일 빌드 방법 영상1](https://youtu.be/EqoU1PodQQ4?t=8507)
2. [모바일 빌드 방법 영상2](https://youtu.be/aKSsXg3vHPg?t=2283)
3. [모바일 빌드 도구 세팅 방법](https://dooding.tistory.com/11)
4. [오라클 계정 만드는 법](https://comclothing.tistory.com/24)
5. [유니티 안드로이 빌드 플레이어 셋팅](https://notyu.tistory.com/16)
