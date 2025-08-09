---
layout: single
title: "C++ 소수 판별하기 "
categories: AlgorithmStudy
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

소수를 판별하는 방법을 정리한 내용입니다.

<br>

# 소수판별 1 (기본)

~~~c++
// 소수 판별
for (int i = 2; i < n; i++) {
	if (n % i == 0) {
		isPrime = false;
		break;
	}
}
~~~

- 소수는 **1과 자기자신을 제외하고 약수를 갖지 않는 1보다 큰 자연수**를 의미합니다.
- 소수를 판별하는 가장 간단한 아이디어는 다음과 같습니다.
    - 주어진 수 N이 있으면 **2부터 N-1**까지 그 수와 나누어 떨어지는 수가 있다면 소수가 아니고, 나누어 떨어지는 수가 없으면 소수가 됩니다.

## 전체 코드

~~~c++
#include <iostream>

using namespace std;

int main() {

	int n; // 주어진 수
	cin >> n; // 주어진 수 입력
	bool isPrime = true; // 소수 판별

	// 소수 판별
	for (int i = 2; i < n; i++) {
		if (n % i == 0) {
			isPrime = false;
			break;
		}
	}

	// 결과 출력
	if (isPrime)
		cout << n << "은 소수가 맞습니다.";
	else
		cout << n << "은 소수가 아닙니다.";

	return 0;
}
~~~

<br>

# 소수 판별 2 (제곱근)
~~~c++
// 소수 판별
for (int i = 2; i < sqrt(n); i++) {
	if (n % i == 0) {
		isPrime = false;
		break;
	}
}
~~~

- 첫번째 방법은 2 ~ N-1까지 모든 경우의 수를 판단하는 비효율적인 방법입니다.
- 제곱근을 이용해서 나누는 방법을 사용하면 **1/2 제곱만큼 검사 시간을 줄여줍니다.**
- 예를들어 81의 약수는 1,3,9,27,81입니다.
    - 81의 제곱근은 9가 됩니다.
    - 즉, 나누어 떨어지는 수를 1~9까지 찾으면 되기 때문에 모든 경우의 수를 사용할 필요가 없어집니다.

## 전체 코드

~~~c++
#include <iostream>
#include <cmath>

using namespace std;

int main() {

	int n; // 주어진 수
	cin >> n; // 주어진 수 입력
	bool isPrime = true; // 소수 판별

	// 소수 판별
	for (int i = 2; i < sqrt(n); i++) {
		if (n % i == 0) {
			isPrime = false;
			break;
		}
	}

	// 결과 출력
	if (isPrime)
		cout << n << "은 소수가 맞습니다.";
	else
		cout << n << "은 소수가 아닙니다.";

	return 0;
}
~~~

<br>

# 소수 판별 3 (에라토스테네스의 체)

~~~c++
// 소수 판별
for (int i = 2; i < n + 1; i++) {
	if (isPrime[i]) {
		for (int j = i * 2; j < n + 1; j += i) {
			isPrime[j] = false;
		}
	}
}
~~~

- 주어진 하나의 값이 소수인지 판별한다면 1,2 방법을 사용해도 상관없습니다.
- 하지만, **특정 범위에서 소수인 수들을 판별**하기 위해서는 에라토스테네스의 체를 사용하면 효율적으로 동작합니다.

## 예시
- 1~50까지의 수를 나열

![sample](\images\2025-08-05-Study_check_primeNumber\sample.png)

- 소수도 합성수도 아닌 1를 제거

![sample](\images\2025-08-05-Study_check_primeNumber\sample1.png)


- 2를 제외한 2의 배수를 모두 제거한다.

![sample](\images\2025-08-05-Study_check_primeNumber\sample2.png)

- 3을 제외한 3의 배수를 모두 제거한다.

![sample](\images\2025-08-05-Study_check_primeNumber\sample3.png)

- 5를 제외한 5의 배수를 모두 제거한다.

![sample](\images\2025-08-05-Study_check_primeNumber\sample5.png)

- 7를 제외한 7의 배수를 모두 제거한다.

![sample](\images\2025-08-05-Study_check_primeNumber\sample7.png)

- 이렇게 끝까지 제거하면 소수만 판별할 수 있게 된다.

![sample](\images\2025-08-05-Study_check_primeNumber\sample8.png)

## 전체 코드

~~~c++
#include <iostream>
#include <vector>

using namespace std;

int main() {

	int n; // 주어진 수
	cin >> n; // 주어진 수 입력
	vector<bool> isPrime(n + 1, true); // 소수 판별 배열

	// 소수 판별
	for (int i = 2; i < n+1; i++) {
		if (isPrime[i]) {
			for (int j = i * 2; j < n + 1; j += i) {
				isPrime[j] = false;
			}
		}
	}

	// 결과 출력
	cout << "소수: ";
	for (int i = 2; i < isPrime.size(); i++) {
		if (isPrime[i])
			cout << i << " ";
	}
	cout << "\n";

	return 0;
}
~~~

<br>

# 참조
- [[알고리즘] 소수판별 알고리즘 C++](https://khu98.tistory.com/227)
- [[C/C++] 소수 구하기 (for문, 재귀, 에라토스테네스의 체)](https://cocoon1787.tistory.com/88)
- [[알고리즘/C++] 소수 판별하기](https://jehunseo.tistory.com/207)
- [[Algorithm] 에라토스테네스의 체 - C++](https://donggoolosori.github.io/2020/10/16/eratos/)
- [[Algorithm/C++] 에라토스테네스의 체 - 특정 범위 수 소수 판별](https://notepad96.tistory.com/219)
- [[Algorithm] C++ - 소수 구하기 (제곱근, 에라토스테네스의 체)](https://jepilyu.tistory.com/68)