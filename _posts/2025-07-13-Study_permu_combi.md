---
layout: single
title: "순열과 조합 공부"
categories: AlgorithmStudy
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

이 글은 순열과 조합을 재귀로 구현한 것을 정리한 내용입니다.


- 순열과 조합을 구현할 때 **트리 구조**로 생각하면 구현하는데 도움을 줍니다.


# 순열
~~~c++
#include <iostream>
#include <vector>

using namespace std;

int R; // 선택한 데이터 수
vector<int> v; // 주어진 데이터 모음
vector<int> result; // 선택한 데이터 모음
int visited[4]; // 방문 체크

// 결과 출력 함수
void print() {
	cout << "[ ";
	for (int i = 0; i < result.size(); i++) {
		cout << result[i]<<" ";
	}
	cout << "]\n";
}

// dfs를 통해 순열 처리
void dfs(int level) {
	if (level == R) {
		print();
		return;
	}

	for (int i = 0; i < v.size(); i++) {
		if(!(visited[i]==0)) continue;

		visited[i] = 1;
		result[level] = v[i];
		dfs(level + 1);
		visited[i] = 0;

	}

}

int main() {

	// 주어진 데이터 초기화
	v = {1,2,3};

	// 선택할 데이터 수 입력
	R=2;
    result = vector<int>(R);

	dfs(0);

	return 0;
}
~~~
- 주어진 1, 2, 3 중에서 2개의 수를 선택해서 나열하는 코드입니다.
- 순열은 **DFS 트리**와 **방문 체크 리스트**를 사용합니다.
- 해당 함수의 매개변수는 **트리 깊이(level)**를 받습니다.
-  선택한 수(R)와 트리 깊이(level)가 같으면 결과를 출력하고 재귀를 종료하기 위해 리턴합니다.
- for문으로 **0 ~ 배열 크기-1**까지 탐색합니다.
    - i번 배열에 방문하지 않았다면 방문 체크합니다.
    - result의 현재 깊이에 해당하는 위치에 값을 넣습니다.
    - 다음 자식 노드에 방문하기 위해 재귀함수 호출합니다.
    - 재귀가 종료되면 방문 체크를 해제한다.

## 코드 풀이 과정
- 다음 설명은 주어진 v = {1, 2, 3} 중에서 2개의 수를 선택해서 나열하는 과정을 보여줍니다.

![sample](\images\2025-07-13-Study_permu_combi\permu.png)
- 초기 상태

![sample](\images\2025-07-13-Study_permu_combi\permu0.png)
- f(0)을 호출하면 트리 깊이가 0이기 때문에 재귀를 진행합니다.
- for문으로 0 ~ 2까지 탐색합니다.
- <span style="color:red"> 0번 배열</span>은 방문하지 않았기 때문에 **방문체크** 합니다.
- result의 <span style="color:blue">현재 깊이인 0번째</span>에 주어진 수 <span style="color:red"> v[0]</span> 을 넣습니다.
- 다음 자식 노드에 방문하기 위해 f(<span style="color:blue">level+1</span>) 즉, **f(1)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\permu1.png)
- f(1)을 호출하면 트리 깊이가 1이기 때문에 재귀를 진행합니다.
- for문으로 0~2까지 탐색합니다.
- 0번 배열은 이미 방문했기 때문에 넘어갑니다.
- <span style="color:red">1번 배열</span>은 방문하지 않았기 때문에 **방문체크** 합니다.
- result의 <span style="color:blue">현재 깊이인 1번째</span>에 주어진 수 <span style="color:red">v[1]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(2)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\permu2.png)
- f(2)를 호출하면 트리 깊이가 2이기 때문에 재귀를 종료합니다.
- result의 내용을 **[1 2]**로 출력합니다.
- **f(1)로 돌아갑니다.**

![sample](\images\2025-07-13-Study_permu_combi\permu3.png)
- f(1)로 돌아오면 **1번 방문 기록을 해제**합니다.
- <span style="color:red">2번 배열</span>은 방문하지 않았기 때문에 **방문체크** 합니다.
- result의 <span style="color:blue">현재 깊이인 1번째</span>에 주어진 수 <span style="color:red">v[2]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(2)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\permu4.png)
- f(2)를 호출하면 트리 깊이가 2이기 때문에 재귀를 종료합니다.
- result의 내용을 **[1 3]**로 출력합니다.
- **f(1)로 돌아갑니다.**

![sample](\images\2025-07-13-Study_permu_combi\permu5.png)
- f(1)로 돌아오면 **2번 방문 기록을 해제**합니다.
- 더이상 방문할 곳이 없어서 **f(0)으로 돌아갑니다.**

![sample](\images\2025-07-13-Study_permu_combi\permu6.png)
- f(0)으로 돌아오면 **0번 방문 기록을 해제**합니다.
- <span style="color:red">1번 배열</span>은 방문하지 않았기 때문에 **방문체크** 합니다.
- result의 <span style="color:blue">현재 깊이인 0번째</span>에 주어진 수 <span style="color:red">v[1]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(1)을 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\permu7.png)
- f(1)을 호출하면 트리 깊이가 1이기 때문에 재귀를 진행합니다.
- for문으로 0~2까지 탐색합니다.
- <span style="color:red">0번 배열</span>은 방문하지 않았기 때문에 **방문체크** 합니다.
- result의 <span style="color:blue">현재 깊이인 1번째</span>에 주어진 수 <span style="color:red">v[0]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(2)를 호출**합니다.
- 이 과정을 **f(0)이 더이상 방문할 수 없을 때까지 반복**합니다.

<br>

# 조합
~~~c++
#include <iostream>
#include <vector>

using namespace std;

vector<int> v; // 주어진 데이터 모음
vector<int> result; // 선택한 데이터 모음
int R; // 선택한 데이터 수

// dfs를 통해 조합 처리
void dfs(int level, int idx) {
	if (level == R) {
        cout<<"[";
		for (int i=0; i < result.size(); i++) {
			cout << result[i]<<" ";
		}
		cout << "]\n";
		return;
	}

	for (int i = idx; i < v.size(); i++) {
		result[level] = v[i];
		dfs(level + 1, i + 1);

	}

}

int main() {

	// 주어진 데이터 초기화
	v = { 1,2,3};

	// 선택한 데이터 수 입력
	R=2;
	result = vector<int>(R);

	dfs(0, 0);

	return 0;
}
~~~
- 주어진 1, 2, 3 중에서 2개의 수를 선택하는 코드입니다.
- 조합은 **DFS 트리**를 이용해서 풀고, 트리를 탐색할 때 **다음 시작점**을 가지고 갑니다.
- 해당 함수의 매개변수는 **트리 깊이(level)**와 **다음 시작점(idx)**을 받습니다.
-  선택한 수(R)와 트리 깊이(level)가 같으면 결과를 출력하고 재귀를 종료하기 위해 리턴합니다.
- for문으로 **다음 시작점 ~ 배열 크기-1**까지 탐색합니다.
    - result의 현재 깊이에 해당하는 위치에 값을 넣습니다.
    - 다음 자식 노드에 방문하기 위해 재귀함수 호출합니다.

## 코드 풀이 과정
- 다음 그림은 주어진 v = {1, 2, 3} 중에서 2개의 수를 선택하는 과정을 보여줍니다.

![sample](\images\2025-07-13-Study_permu_combi\combi1.png)
- 초기 상태

![sample](\images\2025-07-13-Study_permu_combi\combi2.png)
- f(0,0)을 호출하면 트리 깊이가 0이기 때문에 재귀를 진행합니다.
- 시작점이 **0**이기 때문에 for문으로 **0** ~ 2까지 탐색합니다.
- <span style="color:red">i = 0</span>이 됩니다.
- result의 <span style="color:blue">현재 깊이인 0번째</span>에 주어진 수 <span style="color:red">v[0]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 f(<span style="color:blue">level+1</span>, <span style="color:red">i+1</span>) 즉, **f(1,1)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\combi3.png)
- f(1,1)을 호출하면 트리 깊이가 1이기 때문에 재귀를 진행합니다.
- 시작점이 **1**이기 때문에 for문으로 **1** ~ 2까지 탐색합니다.
- <span style="color:red">i = 1</span>이 됩니다.
- result의 <span style="color:blue">현재 깊이인 1번째</span>에 주어진 수 <span style="color:red">v[1]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(2,2)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\combi4.png)
- f(2,2)를 호출하면 트리 깊이가 2이기 때문에 재귀를 종료합니다.
- result의 내용을 **[1 2]**로 출력합니다.
- **f(1,1)로 돌아갑니다.**

![sample](\images\2025-07-13-Study_permu_combi\combi5.png)
- f(1,1)로 돌아오면 <span style="color:red">i = 2</span>가 됩니다.
- result의 <span style="color:blue">현재 깊이인 1번째</span>에 주어진 수 <span style="color:red">v[2]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(2,3)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\combi6.png)
- f(2,3)를 호출하면 트리 깊이가 2이기 때문에 재귀를 종료합니다.
- result의 내용을 **[1 3]**로 출력합니다.
- **f(1,1)로 돌아갑니다.**

![sample](\images\2025-07-13-Study_permu_combi\combi7.png)
- f(1,1)로 돌아오면 더이상 방문할 곳이 없어서 **f(0,0)로 돌아갑니다.**

![sample](\images\2025-07-13-Study_permu_combi\combi8.png)
- f(0,0)로 돌아오면 <span style="color:red">i=1</span>이 됩니다.
- result의 <span style="color:blue">현재 깊이인 0번째</span>에 주어진 수 <span style="color:red">v[1]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(1,2)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\combi9.png)
- f(1,2)를 호출하면 트리 깊이가 1이기 때문에 재귀를 진행합니다.
- 시작점이 **2**이기 때문에 for문으로 **2** ~ 2까지 탐색합니다.
- <span style="color:red">i = 2</span>이 됩니다.
- result의 <span style="color:blue">현재 깊이인 1번째</span>에 주어진 수 <span style="color:red">v[2]</span>을 넣습니다.
- 다음 자식 노드에 방문하기 위해 **f(2,3)를 호출**합니다.

![sample](\images\2025-07-13-Study_permu_combi\combi10.png)
- f(2,3)를 호출하면 트리 깊이가 2이기 때문에 재귀를 종료합니다.
- result의 내용을 **[2 3]**로 출력합니다.
- **f(1,2)로 돌아갑니다.**
- 이 과정을 **f(0,0)이 더이상 방문할 수 없을 때까지 반복**합니다.

<br>

# 응용
## 응용1 - 다른 변수 타입 활용
~~~c++
vector<string> v2; // 주어진 데이터 모음 
vector<string> result2; // 선택한 데이터 모음
int R; // 선택한 데이터 수
~~~
- 변수 타입을 문자열로 바꿔서 사용할 수 있습니다.
- 다른 변수를 사용해도 응용이 가능합니다.

~~~c++
#include <iostream>
#include <vector>

using namespace std;

vector<string> v2; // 주어진 데이터 모음 
vector<string> result2; // 선택한 데이터 모음
int R; // 선택한 데이터 수

// dfs 함수
void dfs(int level, int idx) {
	if (level == R) {
		for (int i=0; i < result2.size(); i++) {
			cout << result2[i]<<" ";
		}
		cout << "\n";
		return;
	}

	for (int i = idx; i < v2.size(); i++) {
		result2[level] = v2[i];
		dfs(level + 1, i + 1);

	}

}

int main() {

	// 주어진 데이터 초기화
	v2 = {"aa", "bb", "cc"};

	// 선택한 데이터 수 입력
	R = 2;
	result2 = vector<string>(R);

	dfs(0, 0);

	return 0;
}
~~~

## 응용2 - 지도 활용
- [백준 14502 연구소](https://www.acmicpc.net/problem/14502)와 같은 문제에서 연구소에 3개의 벽을 세우는 방법이 조합을 활용하는 것 입니다.
- 자세히 설명하면 연구소의 **빈 칸 중에서 3개를 선택**해서 벽을 세우는 것 입니다.

![sample](\images\2025-07-13-Study_permu_combi\sample0.png)

- 그림과 같이 2차원 배열의 구조를 A, B, C, D로 구역을 나누어 생각하면 **응용1**과 같은 방식으로 문제를 해결할 수 있습니다.

# 참조
- [알고리즘 뽀개기 - 순열 - 재귀이용](https://youtu.be/78MHQ9s7kAc?si=TmLc9KfLMXHuG77b)
- [알고리즘 뽀개기 - 조합 - 재귀이용](https://youtu.be/HYKpunR1Nto?si=yfXHDDy9n6xpTMFe)
- [순열, 조합 알고리즘 - DFS로 구하기(백트래킹), C++, Python](https://paris-in-the-rain.tistory.com/35)
- [순열과 조합](https://velog.io/@abc2752/%EC%88%9C%EC%97%B4%EA%B3%BC-%EC%A1%B0%ED%95%A9#-%EB%A0%88%ED%8D%BC%EB%9F%B0%EC%8A%A4)

