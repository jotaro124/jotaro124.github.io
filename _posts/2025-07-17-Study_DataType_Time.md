---
layout: single
title: "데이터 타입의 범위와 시간 제한 공부"
categories: AlgorithmStudy
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

<style>
 table{ border-collapse : collapse; }
 th,td{
      border: 1px solid #000;

    }
</style>

코딩 테스트하면서 `틀렸습니다`와 `시간 초과`라는 메시지를 받는 경우가 많았습니다.
여러가지 이유가 존재하지만 데이터가 표현할 수 있는 범위를 벗어나서 오답 처리되는 경우가 있었습니다.
- 예를들어 [나무 자르기](https://www.acmicpc.net/problem/2805) 문제에서 int형으로 코드를 구현했는데 int형의 범위를 넘어서는 데이터였기 때문에 `틀렸습니다` 메시지를 받았습니다. 
- 또, 주어진 N의 범위를 고려하지 않고 시간 복잡도 O(N^2)로 코드를 구현해서 `시간 초과` 메시지를 받았습니다.
그래서 이러한 경우를 방지하기 위해 데이터 타입의 범위와 시간 제한 공부 내용을 정리했습니다.

# 데이터 타입의 범위
## 정수형

|자료형|크기|범위|
|:---:|:---:|:---:|
|char|1바이트, 8비트|-128 ~ 127|
|short|2바이트, 16비트|-32768 ~ 32767|
|int|4바이트, 32비트|**-2147483648 ~ 2147483647**|
|long|4바이트, 32비트|-2147483648 ~ 2147483647|
|long long|8바이트, 64비트|**-9223372036854775808 ~ 9223372036854775808**|
|unsigned char|1바이트, 8비트|0~ 255|
|unsigned short|2바이트, 16비트|0 ~ 65535|
|unsigned int|4바이트, 32비트|0 ~ 4294967295|
|unsigned long|4바이트, 32비트|0 ~ 4294967295|
|unsigned long long|8바이트, 64비트|0 ~  18446744073709551615|

- int형의 범위는 약 -21억 ~ 21억 입니다.
- 즉, 문제에서 주어진 N과 M의 **최대 범위를 더하거나 곱했을 때 결과값이 약 21억을 넘기면** 변수의 타입을 **long long으로 사용**합니다.

## 실수형

|자료형|크기|범위|
|:---:|:---:|:---:|
|float|4바이트, 32비트|-3.4X10^38 ~ 3.4X10^38|
|double|8바이트, 8비트|-1.79X10^308 ~ 1.79X10^308|
|long double|8바이트 이상|double이상의 크기|

<br>

# 시간 제한과 시간 복잡도
- 코딩 테스트에서 문제의 **제한 시간은 1~5초** 정도입니다.
- CPU 기반의 개인 컴퓨터나 채점용 컴퓨터에서 **1초**에 실행할 수 있는 최대 연산 횟수는 **약 1억번(10^8)**입니다.

|시간 복잡도|N이 1000일 때 연산 횟수|
|:---:|:---:|
|O(N)|10^3|
|O(NlogN)|10^4|
|O(N^2)|10^6|
|O(N^3)|10^9|

- N의 범위가 **500**: 시간 복잡도가 `O(N^3)`이하인 알고리즘 설계
- N의 범위가 **2000**: 시간 복잡도가 `O(N^2)`이하인 알고리즘 설계
- N의 범위가 **100000**: 시간 복잡도가 `O(NlogN)`이하인 알고리즘 설계
- N의 범위가 **10000000**: 시간 복잡도가 `O(N)`이하인 알고리즘 설계

<br>

# C++ 입출력 시간 단축 방법 
~~~c++
ios::sync_with_stdio(false); 
cin.tie(NULL); 
cout.tie(NULL);
~~~
- cout,cin의 성능을 printf, scanf만큼 빠르게 만들어주는 코드입니다.
- 싱글 쓰레드 환경인 **코딩 테스트에서는 문제가 되지 않지만**, 멀티 쓰레드 환경일 수 있는 **협업에선 문제가 발생**할 수 있습니다.

<br>

# 참조
- [시간 제한과 시간 복잡도](https://velog.io/@jiyaho/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%8B%9C%EA%B0%84-%EC%A0%9C%ED%95%9C%EA%B3%BC-%EC%8B%9C%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84)
- [[코딩테스트 문제 풀이 전략] 시간 복잡도 / 시간 복잡도 줄이는 Tip](https://woohyun-king.tistory.com/352)
- [코테 시간초과 분석 방법](https://kr-ddubbu.tistory.com/138)
- [c언어 기본 자료형의 종류와 데이터의 표현 범위](https://go-hard.tistory.com/112)
- [[C개념] :: 자료형(Data type) 별 크기 및 범위](https://chartworld.tistory.com/23)
- [정수 자료형의 크기와 범위](https://almond0115.tistory.com/entry/%EC%A0%95%EC%88%98-%EC%9E%90%EB%A3%8C%ED%98%95%EC%9D%98-%ED%81%AC%EA%B8%B0%EC%99%80-%EB%B2%94%EC%9C%84)
- [C++ scanf 와 cin 의 입출력 속도 비교, 입출력 속도 높이기](https://ansohxxn.github.io/cpp/iospeed/)
- [(C++) - cout,cin 실행 속도 높이기(시간초과 해결법)](https://codecollector.tistory.com/381)
- [ios::sync_with_stdio , cin.tie , cout.tie 사용법과 설명, 속도 비교](https://hegosumluxmundij.tistory.com/54)
- [ios_base::sync_with_stdio(false); cin.tie(NULL); 의 의미](https://codingfriend.tistory.com/21)