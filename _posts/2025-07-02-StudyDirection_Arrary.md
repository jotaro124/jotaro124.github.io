---
layout: single
title: "방향 배열(Direction Array)"
categories: AlgorithmStudy
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 방향 배열
- 2차원 이상의 격자 혹은 좌표 공간에서 인접 위치로 이동해야 할 때 각 방향을 일정한 규칙으로 정의해두는 배열
- 상, 하, 좌, 우 이동에 대한 조건문을 따로 처리할 필요 없이 하나의 반복문으로 현재 좌표 (x, y)에서 원하는 모든 방향의 탐색이 가능

- **BFS/DFS, 시뮬레이션, 그래프 탐색** 등에서 주로 사용

<br>

# 그림 설명
![sample](\images\2025-07-02-StudyDirction_Array\sample0.png)
- 2차원 배열 구조이다.
- 만약 좌표 순서를 (x, y)라고 할 때 (0, -1) -> **좌** , (0, 1) -> **우**, (-1, 0) -> **상**, (1,0) -> **하** 이다.
- 만약 좌표 순서를 (y, x)라고 할 때 (0, -1) -> **상** , (0, 1) -> **하**, (-1, 0) -> **좌**, (1,0) -> **우** 이다.
- 그래서 좌표의 순서가 **(y, x)인지 (x, y)인지 통일하고 코드를 작성**해야 혼동을 방지할 수 있음

<br>

# 코드
~~~c++

int dx[4] = {-1, 1, 0, 0} // 행 방향 이동
int dy[4] = {0, 0, -1, 1} // 열 방향 이동

// 상하좌우 이동
for (int i = 0; i < 4; i++) {
	int next_x = x + dx[i]; // 이동한 x좌표
	int next_y = y + dy[i]; // 이동한 y좌표
}
~~~

<br>

# 참조
- [[코딩 팁] 방향 배열(Direction Array) : 상하좌우 이동](https://kangworld.tistory.com/69)
- [[알고리즘] 방향 배열 (Direction Array)](https://devkuk.tistory.com/17#%EB%B0%A9%ED%96%A5-%EB%B0%B0%EC%97%B4%EC%9D%98-%EC%9D%BC%EB%B0%98%ED%99%94)