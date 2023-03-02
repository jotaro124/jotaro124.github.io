---
layout: single
title: "JSON형식으로 작성한 함수데이터로 Unity 함수 호출하기"
categories: UnityStudy
toc: true
sidebar:
     nav: "docs"
---



<br>

 자바스크립트에서 Unity의 함수를 호출하는 메시지를 보내는 방법을 찾다가 둘 다 JSON을 사용할 수 있어서 JSON을 통한 메시지 전송 방법을 찾아냈다. 이 글은 JSON 형식으로 작성한 함수 정보를 유니티가 해석해서 해당 함수를 호출하는 내용을 담고 있다. 또 공부와 기록 목적으로 작성한 내용이기 때문에 틀린 부분과 자주 수정할 사향이 많은 점 이해해주시면 감사하겠습니다.

<br>

# JSON 데이터 처리 방법

## 1. JSON형식으로 작성한 함수 데이터

~~~json
{"key":"AtomISF","values":["5","dg","3.15"]}
~~~

* 키와 매개변수 값으로 구성한다.
* 키는 함수명_매개변수 타입 이니셜(숫자)로 작성한다.
  - 함수명은 오버로딩을 제외하면 이름이 중복이 되는 경우가 없기 때문에 자연스럽게 고유키가 정해진다.
  - 매개변수 타입들이 Int, String, Float인 경우 이니셜은 ISF가 된다.
  - 매개변수가 없는 함수는 숫자 0을 넣는다.



## 2. JSON형식 데이터 처리 클래스 작성

~~~csharp
[System.Serializable]
public class JFunctionData
{
    public string key; // 함수명 (키) 
    public string[] values; // 함수 매개변수 값
}
~~~

* JSON형식을 처리하기 위해서 키와 값을 선언한다.



## 3. 예시로 사용할 함수 선언

~~~csharp
// Atom();
void Atom_0(string[] arg) => Debug.Log("매개변수 없는 함수 호출");
~~~

* 매개변수가 없는 함수 Atom()는 그냥 호출하면 된다.



~~~csharp
// Atom(int,string,Float)
void Atom_ISF(string[] arg)
{
    try
    {
        int arg1 = Convert.ToInt32(arg[0]);
        string arg2 = arg[1];
        float arg3 = Convert.ToSingle(arg[2]);

        // Atom(arg1, arg2, arg3)
        Debug.Log(string.Format("매개변수 1: {0}",arg1));
        Debug.Log(string.Format("매개변수 2: {0}",arg2));
        Debug.Log(string.Format("매개변수 3: {0}",arg3));
    }
    catch(Exception e)
    {
        Debug.Log(e.Message);
    }
    
}
~~~

* 매개변수 형이 int, string, float인 함수 Atom(int,string,Float)은  string 타입으로 받은 값들을 해당 타입으로 변환하는 작업을 거쳐야 한다.

* 타입으로 변환하는 과정에서 맞지 않는 타입일 경우 예외처리를 해서 오류를 방지했다. 예를들어 bool a = Convert.ToInt32("A")이면 에러가 발생한다.



# 데이터베이스 구축 및 검색

## 1. 데이터베이스 구축

~~~csharp
// Call From Js Message Manager
public class CFSManager: MonoBehaviour {
    
    /*일부 코드 생략*/
    
    // 델리게이트 선언
    public delegate void FunctionDelegate(string[] value);

    // 딕셔너리 자료구조로 만든 데이터베이스
    Dictionary<string, FunctionDelegate> database;
    
    // 데이터베이스에 데이터를 넣는 함수
    void SettingDatabase()
    {
        database = new Dictionary<string, FunctionDelegate>()
        {
            {"Atom_0",(value)=>{Atom_0_(value); } },
            {"Atom_ISF",(value)=>{Atom_ISF(value); } }
        };
    }

}
~~~

* 호출할 함수를 등록하기 위해서 delegate void FunctionDelegate(string[] value)를 선언한다.
* 딕셔너리 자료구조로 만든 Dictionary<string, FunctionDelegate> database를 선언한다.
  - 데이터베이스는 키는 함수명_이니셜, 값은 호출할 함수로 구성했다.
  - Dictionary<키 값, 호출할 함수>
  - 호출할 함수는 람다식으로 등록한다.



## 2. 데이터베이스 검색함수 구현

~~~csharp
// 데이터베이스 검색 함수
    public void SearchDatabase(string json) 
    {
        JFunctionData data = new JFunctionData();
        data = JsonConvert.DeserializeObject<JFunctionData>(json);

        if (database.ContainsKey(data.key))
        {
            database[data.key](data.values);
        }
        else
            Debug.Log("해당 함수를 찾지 못했습니다.");
    }
~~~

* json 데이터를 JFunctionData로 역직렬화 한다.
* data의 키가 데이터베이스에 포함되어 있는지 확인한다.
  - 포함되어 있는 않으면 "해당 함수를 찾지 못했습니다." 메시지가 나오게 한다.
  - 포함되어 있으면 해당 키의 델리게이트를 호출한다.



## 3. 실행결과

![image-20230302183936590](..\..\images\2023-03-02-JsonDataFuncionCall\image-20230302183936590.png)



# 참고자료

1. [[C언어] 실습: 이름을 검색하여 해당하는 연락처 출력하기](https://sweetnew.tistory.com/235)
2. [[C#] 델리게이트 개념1](https://blog.hexabrain.net/151)
3. [[C#] 델리게이트 개념2](https://seonbicode.tistory.com/27)
4. [[C#] 가변 배열](https://learn.microsoft.com/ko-kr/dotnet/csharp/programming-guide/arrays/jagged-arrays)
5. [[c#] 문자열 숫자 변환 방법](https://codingcoding.tistory.com/789)
6. [[c#] 딕셔너리 사용법1](https://codingcoding.tistory.com/380)
7. [[c#] 딕셔너리 사용법2](https://developer-talk.tistory.com/697)
8. [[유니티] Newtonsoft.json 사용법](https://postiveground.com/etc/%EC%9C%A0%EB%8B%88%ED%8B%B0-json-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95/)
9. [json 사용법1](http://www.tcpschool.com/json/json_use_js)
10. [json 사용법2](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)
11. [json 사용법3](https://java119.tistory.com/54)
