---
layout: single
title: "위상 정렬 공부 (java)"
categories: Graph
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# 위상 정렬 알고리즘
- **순서가 정해져있는 작업을 차례로 수행해야 할 때** 그 순서를 결정해주기 위해 사용하는 알고리즘입니다.
- **사이클이 없는 방향 그래프**의 모든 노드를 **방향성에 거스르지 않도록 순서대로 나열**하는 것을 의미합니다.
- **모든 원소를 방문하기 전에 큐가 비게 된다면 사이클이 존재**한다고 판단할 수 있습니다.
  - 사이클이 존재하는 그래프는 차입차수가 0이 되지 않는 원소가 발생하서 모든 원소를 방문하기 전에 큐가 비게됩니다.

<br>

# 진입차수와 진출차수
- **진입차수 (Indegree)**
    - 특정한 노드로 들어오는 간선의 개수
- **진출차수 (OutDegree)**
  - 특정한 노드에서 나가는 간선의 개수

![sample](\images\2026-02-25-Study_TopologySort\01.png)

<br>

# 동작 과정
1. 진입차수가 0인 모든 노드를 큐에 넣습니다.
2. 큐가 빌 때까지 다음의 과정을 반복합니다.
   1. 큐에서 원소를 꺼내 해당 노드에서 나가는 간선을 그래프에서 제거합니다.
   2. 새롭게 진입차수가 0이 된 노드를 큐에 넣습니다.

## 예시

![sample](\images\2026-02-25-Study_TopologySort\02.png)
- 진입차수가 0인 모든 노드를 큐에 삽입합니다.
- 현재 1번 노드의 진입차수만 0이므로 큐에 1번 노드만 삽입하게 됩니다.

![sample](\images\2026-02-25-Study_TopologySort\03.png)
- 큐에서 1번 노드를 제거하고, 1번 노드와 연결되어 있는 간선들을 제거합니다.
- 그러면 2번 노드의 진입차수가 0이 되므로 2번 노드를 큐에 삽입합니다.


![sample](\images\2026-02-25-Study_TopologySort\04.png)
- 큐에서 2번 노드를 제거하고, 2번 노드와 연결되어 있는 간선들을 제거합니다.
- 그러면 3번 노드의 진입차수가 0이 되므로 3번 노드를 큐에 삽입합니다.


![sample](\images\2026-02-25-Study_TopologySort\05.png)
- 큐에서 3번 노드를 제거합니다.
- 더이상 큐에 정점이 없기 떄문에 종료합니다.

<br>

# 코드 구현

~~~java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;
import java.util.StringTokenizer;

public class Solution {

	public static void main(String[] args) throws NumberFormatException, IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();

		int T = Integer.parseInt(br.readLine().trim());

		// tc 수행
		for (int tc = 1; tc <= T; tc++) {

			// 학생의 수 N, 순서의 개수 M 입력
			StringTokenizer st = new StringTokenizer(br.readLine());
			int N = Integer.parseInt(st.nextToken());
			int M = Integer.parseInt(st.nextToken());

			// 차입 배열 초기화
			int inDegree[] = new int[N + 1];

			// 인접 리스트 초기화
			List<List<Integer>> graph = new ArrayList<>();
			for (int i = 0; i <= N; i++) {
				graph.add(new ArrayList<>());
			}

			// 간선 입력
			for (int i = 0; i < M; i++) {
				st = new StringTokenizer(br.readLine());
				int a = Integer.parseInt(st.nextToken());
				int b = Integer.parseInt(st.nextToken());

				graph.get(a).add(b);
				inDegree[b]++;

			}

			Deque<Integer> q = new ArrayDeque<>();
			StringBuilder result = new StringBuilder();

			// 진입 차수가 0이면 큐에 먼저 삽입한다.
			for (int i = 1; i <= N; i++) {
				if (inDegree[i] == 0) {
					q.offer(i);
				}
			}

			// 더 이상 반복할 곳이 없을 때까지 반복
			while (!q.isEmpty()) {
				int cur = q.poll();
				result.append(cur).append(" ");

				for (int next : graph.get(cur)) {
					inDegree[next]--;

					if (inDegree[next] == 0) {
						q.offer(next);
					}
				}

			}

			// 출력 내용 추가
			sb.append("#" + tc + " " + result + "\n");
		} // tc 종료

		// 출력
		System.out.println(sb);

	}

}
~~~

<br>

# 참조
- [위상 정렬(Topology Sort)](https://m.blog.naver.com/ndb796/221236874984)
- [[알고리즘] 위상 정렬 (Topological Sorting)](https://velog.io/@kimdukbae/%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC-Topological-Sorting)
- [위상 정렬(Topological sort) 개념 및 구현](https://yoongrammer.tistory.com/86)

