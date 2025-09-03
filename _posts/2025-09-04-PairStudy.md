---
layout: single
title: "Pair 기본 사용법 및 예제"
categories: C_CPP
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# Pair의 개념
- 사용자가 지정한 2개의 타입의 데이터를 저장하는데 사용합니다.

<br>

# Pair 사용법
- utility라는 헤더 파일에 존재하는 STL입니다.
- 그러나 **algorithm, vector** 헤더파일에 utility 헤더 파일이 이미 포함되어 있기 때문에 둘 중에 하나를 선택해서 헤더 파일을 사용합니다.
- pair<[데이터 타입],[데이터 타입]> 이름; 으로 pair를 선언합니다.
~~~c++
#include <utility>
#include <algorithm>
#include <vector>
pair<int,int> p
pair<int,char> p
~~~

<br>

# Pair 기본 사용법
- **pair.first**
     - pair의 첫 번째 인자를 반환합니다.

- **pair.second**
     - pair의 두 번째 인자를 반환합니다.

- **make_pair(val1,val2)**
     - val1, val2를 한 쌍으로 하는 pair를 생성해서 반환합니다.

<br>

# Pair 기본 사용 예시
~~~c++
#include<iostream>
#include<vector>

using namespace std;

pair<int, double> p;

int main()
{
    p.first = 1; //pair의 첫번째 인자에 접근
    p.second = 2.1; //pair의 두번째 인자에 접근

    cout << "first value: " << p.first << "\n";
    cout << "second value: " << p.second << "\n";

    return 0;
}
~~~

- 결과

~~~
first value: 1
second value: 2.1
~~~

<br>

# Pair 정렬
- 기본적으로 first를 기준으로 정렬되고, first가 같으면 second를 기준으로 정렬됩니다.

~~~c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()
{
     vector <pair<int, int> > v;
	v.push_back(make_pair(1, 1));
	v.push_back(make_pair(2, 1));
	v.push_back(make_pair(3, 3));
	v.push_back(make_pair(3, 2));
	v.push_back(make_pair(3, 1));

	sort(v.begin(), v.end());

	for (int i = 0; i < v.size(); i++)
	{
		cout << v[i].first << " " << v[i].second << "\n";
	}

	return 0;
}
~~~

- 결과

~~~
1 1
2 1
3 1
3 2
3 3
~~~

<br>

참조
- [[C++ STL]pair 클래스 사용방법](https://ya-ya.tistory.com/91)
- [[C++] STL pair를 sort 함수로 내림차순 정렬하기](https://withhamit.tistory.com/195)
- [[C++] sort 함수 내림차순, 내맘대로 정렬 (+DNA, 2017 아주대학교 프로그래밍 경시대회 (Large) 풀이)](https://kau-algorithm.tistory.com/72)