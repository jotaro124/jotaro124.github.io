---
layout: single
title: "Vector 기본 사용법 및 예제"
categories: C_CPP
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---

# Vector의 개념
- 자동으로 메모리가 할당되는 배열입니다.
- vector를 생성하면 메모리 heap에 생성되며 동적할당됩니다.
- 배열처럼 최대 크기가 정해져 있는 것이 아닌, 필요에 따라 유동적으로 확장되는 배열입니다.
- 배열에 비해 속도가 떨어지지만 메모리를 효율적으로 관리하고 예외처리가 쉽다는 장점이 있습니다.

<br>

# Vector 사용법
- vector 헤더 파일을 포함해야 합니다.
- vector<[데이터 타입]> 이름; 으로 vector를 선언합니다.
- using namespace std를 추가하면 편리하게 사용할 수 있습니다.
~~~c++
#include <deque> 
using namespace std;
vector<int> v;
~~~

<br>

# Vector 기본 함수
- **v.at(n)**
    - vector의 n번째 원소를 참조합니다.
    -  v[n]보다 상대적으로 속도가 느립니다.

- **v[n]**
    - vector의 n번째 원소를 참조합니다.
    - v.at(n)보다 상대적으로 속도가 빠릅니다.

- **v.assign(n,i)**
    - i의 값을 n번 반복해서 할당합니다.

- **v.front()**
    - vector의 첫 번째 원소를 참조합니다.

- **v.back()**
    - vector의 마지막 원소를 참조합니다.

- **v.begin()**
    - 첫 번째 원소를 가리킵니다.

- **v.end()**
    -마지막 원소의 다음을 를 가리킵니다.

- **v.push_back(i)**
    - vector의 마지막 원소 뒤에 원소 i를 삽입합니다.

- **v.pop_back()**
    - vector의 마지막 원소를 삭제합니다.

- **v.clear()**
    - vector의 모든 요소를 삭제합니다.

- **v.reserve(n)**
    - n개의 데이터를 저장할 수 있는 공간을 동적할당으로 예약합니다.

- **v.resize(m)**
    - v의 크기를 m으로 변경합니다.
    - 원래 크기보다 작아지는 경우, 뒤에서부터 지워집니다.
    - 원래 크기보다 커지는 경우, 빈자리를 0으로 초기화합니다.

- **v.size()**
    - vector의 원소 수를 반환합니다.

- **v.capcity()**
    - 할당된 공간 크기를 반환합니다.

- **v2.swap(v1)**
    - v1과 v2의 모든 것을 교환합니다.

- **v.insert(i,k)**
    - i번째 위치에 k를 삽입합니다.

- **v.erase(iterator)**
    - iterator가 가리키는 위치의 요소를 삭제합니다.

- **v.empty()**
    - vector가 비어있으면 true 아니면 false를 반환합니다.

<br>

# Vector 기본 사용 예시
~~~c++
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = { 1,2,3 };
    cout << "v[1] : " << v[1] << "\n";  // v[1] : 2
    cout << "v.back() : " << v.back() << "\n";  // v.back() : 3
    v.push_back(10);
    cout << "v.back() <added> : " << v.back() << "\n";  // v.back() <added> : 10

    cout << "v.size() : " << v.size() << "\n";  // v.size() : 4
    v.push_back(12);
    v.push_back(14);
    v.push_back(16);
    v.push_back(18);
    cout << "v.size() <added> : " << v.size() << "\n";  // v.size() <added> : 8

    v.pop_back();
    cout << "v.size() <pop-1> : " << v.size() << "\n";  // v.size() <pop-1> : 7
    cout << "v.back() <pop-1> : " << v.back() << "\n";  // v.back() <pop-1> : 16
    v.pop_back();
    cout << "v.size() <pop-2> : " << v.size() << "\n";  // v.size() <pop-2> : 6
    cout << "v.back() <pop-2> : " << v.back() << "\n";  // v.back() <pop-2> : 14
    v.pop_back();
    cout << "v.size() <pop-3> : " << v.size() << "\n";  // v.size() <pop-3> : 5
    cout << "v.back() <pop-3> : " << v.back() << "\n";  // v.back() <pop-3> : 12
}
~~~

- 결과

~~~
v[1] : 2
v.back() : 3
v.back() <added> : 10
v.size() : 4
v.size() <added> : 8
v.size() <pop-1> : 7
v.back() <pop-1> : 16
v.size() <pop-2> : 6
v.back() <pop-2> : 14
v.size() <pop-3> : 5
v.back() <pop-3> : 12
~~~

<br>

# Vector 중복 제거
- 기본 형태

~~~ c++
sort(vec.begin(), vec.end());
vec.erase(unique(vec.begin(),vec.end()), vec.end());
~~~

- 진행 과정

~~~c++
#include <iostream>
#include <vector>
#include <algorithm> 

using namespace std; 

void print_vec(vector<int> vec){    
	for(int i=0; i<vec.size(); i++)    {        
		cout<<vec[i]<<"  ";    
	}     
	cout<<endl;
}

int main(){    
	vector<int> vec;    
	int arr[] = {7,2,1,2,7,5};    
	vec.assign(arr, arr+6); //벡터 원소 할당 
	print_vec(vec);

	sort(vec.begin(), vec.end()); //정렬     
	print_vec(vec);

	vector<int>::iterator uit= unique(vec.begin(), vec.end()); //중복원소 (쓰레기값으로 변환 후) 뒤로 보냄      
	print_vec(vec);     
	
	vec.erase(uit, vec.end()); //삭제     
	print_vec(vec);     
	return 0;
}
~~~

- 결과

~~~
7  2  1  2  7  5
1  2  2  5  7  7
1  2  5  7  7  7
1  2  5  7
~~~

<br>

# 참조
- [C++ vector사용법 및 설명 (장&단점)](https://hwan-shell.tistory.com/119)
- [[C++ STL : vector] 벡터 중복원소 제거](https://kkaeruk.tistory.com/19)
- [[C++ vector] 사용법](https://dense.tistory.com/entry/cpp-stl-vector)
- [[C++]벡터 중복제거(sort,unique,erase)](https://dpdpwl.tistory.com/39)
- [[C++] vector container 정리 및 사용법](https://blockdmask.tistory.com/70)