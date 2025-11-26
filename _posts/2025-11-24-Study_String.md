---
layout: single
title: "string 클래스 공부"
categories: C_CPP
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

# string 클래스 개념
- C++ STL에서 제공하는 클래스로 문자열을 다루는 클래스 입니다.
- char*, char[]과 다르게 **문자열의 끝에 '\0'문자가 들어가지 않으며, 문자열의 길이를 동적으로 변경 가능합니다.**

<br>

# string 클래스 생성

|생성자|설명|
|:---:|:---:|
|**string()**|빈 문자열을 가진 string 객체 생성|
|**string(const string& str)**|str을 복사한 새로운 string 객체 생성|

## 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {

	string str;
	if (str.empty()) cout << "빈 문자열\n";
	else cout << str << "\n";

	string str2("ABC");
	cout << str2 << "\n";

	return 0;
}
~~~

- 결과

~~~
빈 문자열
ABC
~~~

<br>

# string 객체에 문자열 입력
- **cin>>str**은 **공백 이전**까지 문자열을 입력받습니다.
- **getline()** 함수는 **공백을 포함**한 문자열을 입력받습니다.

## 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {

	string str;
	cin >> str;
	cout << "cin과 >>으로 문자열 입력: ";
	cout << str << "\n";
	cin.ignore(); // Enter 키를 제거하기 위한 코드
	
	string str2; 
	getline(cin, str2);
	cout << "getline()으로 문자열 입력: ";
	cout << str2 << "\n";


	return 0;
}
~~~

- 입력

~~~
ABC
I am
~~~

- 결과

~~~
cin>>으로 문자열 입력: ABC
getline()으로 문자열 입력: I am
~~~


<br>

# string 클래스의 멤버 함수

## string의 특정 원소 접근

|멤버 함수|설명|
|:---:|:---:|
|**str.at(index)**|index 위치의 문자 반환|
|**str[index]**|index 위치의 문자 반환, at 함수보다 상대적으로 빠름|
|**str.front()**|문자열의 가장 앞 문자 반환|
|**str.back()**|문자열의 가장 뒤 문자 반환|

### 예제

~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {
	string str = "abcdefgh";

	cout << "at() 함수로 2번째 원소 접근: " << str.at(2) << "\n";
	cout << "str[2]으로 2번째 원소 접근: " << str[2] << "\n";
	cout << "제일 앞 원소 접근: " << str.front() << "\n";
	cout << "제일 뒤 원소 접근: " << str.back() << "\n";

	return 0;

}

~~~

- 출력

~~~
at() 함수로 2번째 원소 접근: c
[]으로 2번째 원소 접근: c
제일 앞 원소 접근: a
제일 뒤 원소 접근: h
~~~

## string의 크기

|멤버 함수|설명|
|:---:|:---:|
|**str.length()**|문자열 길이 반환|
|**str.size()**|문자열 길이 반환|
|**str.capacity()**|문자열이 사용 중인 메모리 크기 반환|
|**str.resize(n)**|string을 n의 크기로 만듧|
|**str.empty()**|str이 빈 문자열인지 확인|

### 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {

	string str = "abcdefgh";

	cout << "문자열 길이: " << str.length() << "\n";
	cout << "문자열 크기: " << str.size() << "\n";
	cout << "문자열 메모리 크기: " << str.capacity() << "\n";

	str.resize(10);
	cout << "resize() 후 문자열 길이: " << str.length() << "\n";
	cout << "resize() 후 문자열 크기: " << str.size() << "\n";
	cout << "resize() 후 문자열 메모리 크기: " << str.capacity() << "\n";

	cout << "문자열 비었는지 확인: ";
	if (str.empty()) cout << "O\n";
	else cout << "X\n";

	return 0;
}
~~~

- 결과

~~~
문자열 길이: 8
문자열 크기: 8
문자열 메모리 크기: 15
resize() 후 문자열 길이: 10
resize() 후 문자열 크기: 10
resize() 후 문자열 메모리 크기: 15
문자열 비었는지 확인: X
~~~

## string에 삽입, 추가, 삭제

|멤버 함수|설명|
|:---:|:---:|
|**str.append(str2)**|str 뒤에 str2 문자열을 이어 붙임|
|**str.append(str2,n,m)**|str2 문자열 내 n 위치에서 m개의 문자를 str 뒤에 덧붙임|
|**str.append(n,'a')**|str 뒤에 n개의 'a'를 이어 붙여줌|
|**str.insert(n,str2)**|n번째 index 앞에 str2 문자열을 삽입|
|**str.replace(n,k,str2)**|n번째 index부터 k개의 문자를 str2로 대체|
|**str.clear()**|저장된 문자열을 모두 지움|
|**str.erase(n,m)**|n번째 index부터 m개의 문자를 지움|
|**str.erase()**|clear와 같은 동작|
|**str.push_back(c)**|str의 맨 뒤에 c 문자를 붙여줌|
|**str.pop_back()**|str의 맨 뒤에 문자를 제거|
|**str.assign(str2)**|str에 str2 문자열을 할당|

### 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {

	string str = "abcdefgh";
	string str2 = "ABCD";

	str.append(str2);
	cout << "append() 함수 사용1: " << str << "\n";
	str.append(str2, 1, 3);
	cout << "append() 함수 사용2: " << str << "\n";
	str.append(3, 'Z');
	cout << "append() 함수 사용3: " << str << "\n";

	str.clear();
	cout << "clear() 함수 사용: " << str << "\n";

	str.assign("abcdefgh");
	cout << "assign() 함수 사용: " << str << "\n";

	str.insert(2, str2);
	cout << "insert() 함수 사용: " << str << "\n";

	str.replace(3, 3, "TY");
	cout << "replace() 함수 사용: " << str << "\n";

	str.push_back('a');
	cout << "push_back() 함수 사용: " << str << "\n";

	str.pop_back();
	cout << "pop_back() 함수 사용: " << str << "\n";
	
	str.erase(0, 3);
	cout << "erase() 함수 사용1: " << str << "\n";
	str.erase();
	cout << "erase() 함수 사용2: " << str << "\n";

	return 0;
}
~~~

- 결과

~~~
append() 함수 사용1: abcdefghABCD
append() 함수 사용2: abcdefghABCDBCD
append() 함수 사용3: abcdefghABCDBCDZZZ
clear() 함수 사용:
assign() 함수 사용: abcdefgh
insert() 함수 사용: abABCDcdefgh
replace() 함수 사용: abATYcdefgh
push_back() 함수 사용: abATYcdefgha
pop_back() 함수 사용: abATYcdefgh
erase() 함수 사용1: TYcdefgh
erase() 함수 사용2:
~~~

## 부분 문자, 비교, 복사, 찾기

|멤버 함수|설명|
|:---:|:---:|
|**str.substr()**|str 전체를 반환|
|**str.substr(n)**|str의 n번째 index부터 끝까지의 문잘ㄹ 부분문자열로 반환|
|**str.substr(n,k)**|str의 n번째 index부터 k개의 문자를 부분문자열로 반환|
|**str.compare(str2)**|str과 str2를 비교햐여 같으면 0, 사전 순으로 str이 앞에 오면 음수, 뒤에 오면 양수 반환|
|**str.find(str2)**|str의 처음부터 str2을 검색하여 발견한 처음 index 반환, 없으면 -1 반환|
|**str.find(str2,n)**|str의 n 위치부터 str2을 검색하여 발견한 처음 index 반환, 없으면 -1 반환|

### 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {

	string str = "abcdefgh";
	string str2 = "ABCD";

	cout << "전체 복사: " << str.substr() << "\n";
	cout << "3번째 부터 끝까지 복사: " << str.substr(3) << "\n";
	cout << "3번째 부터 3개의 문자열: " << str.substr(3, 3) << "\n";

	cout << "두 문자열 같은지 확인: ";
	if (str.compare(str2) == 0) cout << "O\n";
	else cout << "X\n";

	cout << "문자열 찾기: " << str.find("de") << "\n";
	cout << "2번째부터 문자열 찾기: " << str.find("de", 2) << "\n";

	return 0;
}
~~~

- 결과

~~~
전체 복사: abcdefgh
3번째 부터 끝까지 복사: defgh
3번째 부터 3개의 문자열: def
두 문자열 같은지 확인: X
문자열 찾기: 3
2번째부터 문자열 찾기: 3
~~~

## string 클래스의 연산자

|연산자|설명|
|:---:|:---:|
|**str = str2**|str2를 str에 치환|
|**str1 + str2**|str1과 str2를 연결한 새로운 문자열|
|**str1 += str2**|str1에 str2 문자열 연결|
|**str1 == str2**|str1과 str2가 같은 문자열이면 true|
|**str1 != str2**|str1과 str2가 다른 문자열이면 true|
|**str1 < str2**|str1이 사전 순으로 str2보다 앞에 오면 true|
|**str1 > str2**|str1이 사전 순으로 str2보다 뒤에 오면 true|
|**str1 <= str2**|str1이 str2와 같거나 앞에 오면 true|
|**str1 >= str2**|str1이 str2와 같거나 뒤에 오면 true|

### 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {

	string str1 = "abcd";
	string str2 = "ABCD";
	string str3 = str1;

	cout << "str1 + str2: " << str1 + str2 << "\n";

	str1 += str2;
	cout << "str1 += str2: " << str1 << "\n";

	str1 = str3;
	cout << "str1 = str3: " << str1 << "\n";

	cout << "str1 == str2: " << (str1 == str2) << "\n";
	cout << "str2 != str2: " << (str1 != str2) << "\n";
	cout << "str2 < str2: " << (str1 < str2) << "\n";
	cout << "str2 > str2: " << (str1 > str2) << "\n";
	cout << "str2 <= str2: " << (str1 <= str2) << "\n";
	cout << "str2 >= str2: " << (str1 >= str2) << "\n";

	return 0;
}
~~~

- 결과

~~~
str1 + str2: abcdABCD
str1 += str2: abcdABCD
str1 = str3: abcd
str1 == str2: 0
str2 != str2: 1
str2 < str2: 0
str2 > str2: 1
str2 <= str2: 0
str2 >= str2: 1
~~~


## 문자열 숫자 변환

|멤버 함수|설명|
|:---:|:---:|
|**to_string(n)**|숫자 n을 문자열로 변환|
|**stoi(str)**|문자열을 숫자로 변환(int)|
|**stof(str)**|문자열을 숫자로 변환(float)|
|**stol(str)**|문자열을 숫자로 변환(long)|
|**stod(str)**|문자열을 숫자로 변환(double)|

### 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {

	float num = 4.1;
	string str = to_string(num);
	cout << "to_string: " << str << "\n";

	string str1 = "4";
	int numi = stoi(str1);
	cout << "숫자(int)로 변환: " << numi << "\n";

	string str2 = "4.1";
	float numf = stof(str2);
	cout << "숫자(float)로 변환: " << numf << "\n";

	string str3 = "5";
	long numl = stol(str3);
	cout << "숫자(long)로 변환: " << numl << "\n";

	string str4 = "5.1";
	double numd = stod(str4);
	cout << "숫자(double)로 변환: " << numd << "\n";

	return 0;
}
~~~

- 결과

~~~
to_string: 4.100000
숫자(int)로 변환: 4
숫자(float)로 변환: 4.1
숫자(long)로 변환: 5
숫자(double)로 변환: 5.1
~~~

## 문자 다루기

|함수|설명|
|:---:|:---:|
|**isspace(c)**|**cctype 헤더 포함**, c가 공백인지 확인|
|**isdigit(c)**|**cctype 헤더 포함**, c가 숫자인지 확인, 숫자이면 0이 아닌 숫자 반환|
|**isalpha(c)**|**cctype 헤더 포함**, c가 알파벳인지 확인, 대문자는 1 반환, 소문자는 2 반환, 알파벳이 아니면 0 반환|
|**isupper(c)**|**cctype 헤더 포함**, c가 대문자인지 확인|
|**islower(c)**|**cctype 헤더 포함**, c가 소문자인지 확인|
|**toupper(c)**|**cctype 헤더 포함**, c문자를 대문자로 변환|
|**tolower(c)**|**cctype 헤더 포함**, c문자를 소문자로 변환|

### 예제
~~~c++
#include <iostream>
#include <string>
#include <cctype>

using namespace std;

int main() {

	string str = "1234 Aa";

	for (char iter : str) {

		if (isspace(iter) != 0) {
			cout << iter << "은 공백 입니다.\n";
			continue;
		}
			
		if (isdigit(iter) != 0) {
			cout << iter << "은 숫자 입니다.\n";
			continue;
		}
			
		if (isalpha(iter) != 0) {
			cout << iter << "은 알파벳 ";

			if (isupper(iter) != 0)
				cout << "대문자 입니다.\n";

			if (islower(iter) != 0)
				cout << "소문자 입니다.\n";

			continue;

		}
	}

	string str2 = str;
	cout << str2 << "을 대문자로 변환: ";
	for (int i = 0; i < str2.size(); i++)
		str2[i] = toupper(str2[i]);
	cout << str2 << "\n";

	string str3 = str;
	cout << str3 << "을 소문자로 변환: ";
	for (int i = 0; i < str3.size(); i++)
		str3[i] = tolower(str3[i]);
	cout << str3 << "\n";

	return 0;
}
~~~

- 결과

~~~
1은 숫자 입니다.
2은 숫자 입니다.
3은 숫자 입니다.
4은 숫자 입니다.
 은 공백 입니다.
A은 알파벳 대문자 입니다.
a은 알파벳 소문자 입니다.
1234 Aa을 대문자로 변환: 1234 AA
1234 Aa을 소문자로 변환: 1234 aa
~~~

## 기타 유용한 함수

|멤버 함수|설명|
|:---:|:---:|
|**str.c_str()**|string을 c스타일의 문자열로 변경|
|**str.begin()**|string의 시작 iterator 반환|
|**str.end()**|string의 끝 iterator 반환|
|**swap(str1,str2)**|str1과 str2를 바꿔줌|

### 예제
~~~c++
#include <iostream>
#include <string>

using namespace std;

int main() {
	string str1 = "abcde";
	string str2 = "ABCDE";

	const char* cstyle = str1.c_str();
	cout << "c 스타일: " << cstyle << "\n";

	cout << "iterator로 전체 원소 출력: ";
	for (string::iterator iter = str1.begin(); iter != str1.end(); iter++)
		cout << *iter;
	cout << "\n";

	swap(str1, str2);
	cout << "swap() 후 str1: " << str1 << ", str2: " << str2 << "\n";

	return 0;
}
~~~

- 결과

~~~
c 스타일: abcde
iterator로 전체 원소 출력: abcde
swap() 후 str1: ABCDE, str2: abcde
~~~

<br>

# 참조
- 명품 C++ programming, 황기태, 생능출판, 2013
- [[C++] string (문자열) 클래스 정리 및 사용법과 응용](https://rebro.kr/53)
- [알고리즘 - C++에서 문자열(string) 다루기](https://chanhuiseok.github.io/posts/algo-37/)
- [[C++] std::string 클래스 문자열 완벽 총정리 -string 확장 함수 erase find append stoi to_string front replace substr](https://m.blog.naver.com/dorergiverny/223046924132)
- [C++ isupper, islower, isdigit - 문자 대소문자, 숫자 판별](https://notepad96.tistory.com/64)
- [C++ String to int, int to String - 문자열 숫자 형변환](https://notepad96.tistory.com/67)
