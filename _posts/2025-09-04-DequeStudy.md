---
layout: single
title: "Deque 기본 사용법 및 예제"
categories: C_CPP
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# Deque의 개념
- Double Ended Queue의 줄임말로, 양쪽 끝에서 삽입과 삭제가 가능한 선형 자료구조입니다.

<br>

# Deque 사용법
- deque 헤더 파일을 포함해야 합니다.
- deque<[데이터 타입]> 이름; 으로 deque를 선언합니다.
- using namespace std를 추가하면 편리하게 사용할 수 있습니다.
~~~c++
#include <deque> 
using namespace std;
deque<int> dq;
~~~

<br>

# Deque 기본 함수
- **deque.assign(n)**
    - 요소 n개를 0으로 초기화합니다.

- **deque.assign(n,i)**
    - 요소 n개를 i로 초기화합니다.

- **deque.at(n)**
    - deque의 n번째 원소를 참조합니다.
    - deque[n]보다 상대적으로 속도가 느립니다.
    
- **deque[n]**
    - deque의 n번째 원소를 참조합니다.
    - deque.at(n)보다 상대적으로 속도가 빠릅니다.

- **deque.front()**
    - deque의 첫 번째 원소를 참조합니다.

- **deque.back()**
    - deque의 마지막 원소를 참조합니다.

- **deque.clear()**
    - deque의 모든 요소를 삭제합니다.

- **deque.empty()**
    - deque가 비어있으면 true 아니면 false를 반환합니다.

- **deque.size()**
    - deque의 원소 수를 반환합니다.

- **deque.insert(iterator,i)**
    - iterator가 가리키는 위치에 요소 i 삽입합니다.
    - 기존 원소들은 한 칸씩 밀려납니다.

- **deque.erase(iterator)**
    - iterator가 가리키는 위치의 요소를 삭제합니다.

- **deque.resize(m)**
    - deque의 메모리 크기를 m으로 늘리고 늘어난 부분은 0으로 초기화합니다.

- **deque.push_front(i)**
    - deque의 첫 번째 원소 앞에 원소 i를 삽입합니다.

- **deque.pop_front()**
    - deque의 첫 번째 원소를 삭제합니다.

- **deque.push_back(i)**
    - deque의 마지막 원소 뒤에 원소 i를 삽입합니다.

- **deque.pop_back()**
    - deque의 마지막 원소를 삭제합니다.

<br>

# Deque 기본 사용 예시
~~~c++
#include <iostream>
#include <deque>

using namespace std;

int main()
{
    deque<int> dq;

    dq.push_back(10); // 뒤에 10 삽입
    dq.push_front(20); // 앞에 20 삽입

    cout << dq.front() << '\n'; // 20 출력
    cout << dq.back() << '\n'; // 10 출력

    dq.pop_front(); // 앞에 있는 20 제거
    dq.pop_back(); // 뒤에 있는 10 제거

    dq.push_back(30);
    dq.push_back(40);

    for (int i = 0; i < dq.size(); i++) {
        cout << dq[i] << ' ';
    }
    cout << '\n'; // 30 40 출력

    return 0;
}
~~~

- 결과

~~~
20
10
30 40
~~~

<br>

# 참조
- [[C++ STL] std::deque (시퀀스 컨테이너)](https://marmelo12.tistory.com/305)
- [[C++][STL] 덱(Deque) 개념과 사용 방법](https://baehoon.tistory.com/18)
- [[C++] deque container 정리 및 사용법](https://blockdmask.tistory.com/73)