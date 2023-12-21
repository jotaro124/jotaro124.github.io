---
layout: single
title: "[C#] 중복을 제외하고 랜덤으로 숫자를 뽑는 기능 구현"
categories: CSharp
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---



<br/>

C#으로 중복을 제외하고 숫자를 랜덤으로 뽑는 기능을 구현합니다.



```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace MyProject{
    class Program{
        
        // 0~lastIndex 까지의 숫자를 중복없이 Count 만큼 뽑는 함수
        static List<int> RandomNumber(int lastIndex, int count)
        {
            List<int> result = new List<int>();
            int element;

            while(result.Count < count)
            {
                element = new Random().Next(lastIndex);

                result.Add(element);
                result = result.Distinct().ToList(); // 중복제거 작업

            }

            return result;
        }
        
        static void Main(string[] args)
        {
            List<int> list = RandomNumber(5,3);
            // 리스트를 한 줄로 출력
            Console.WriteLine(string.Join(",",list));
        }
        
    }
}
```

![image-20230209183434454](..\..\images\2023-02-09-CSharpRandomNumber\image-20230209183434454.png)

<br/>

이제 코드에 대해서 자세히 설명하자면 result 리스트의 크기가 Count와 같아질 때까지 무한 반복한다.

```csharp
 while(result.Count < count)
```



element에 0~lastIndex까지의 임의의 숫자를 뽑아내고, result에 Add한다. 마지막으로 Distinct()으로 중복을 제거한 리스트를 생성한다.

```csharp
element = new Random().Next(lastIndex);
result.Add(element);
result = result.Distinct().ToList();
```

<br/>

# 참고자료

1. [Random 클래스 사용법](https://developer-talk.tistory.com/212)
2. [리스트 사용법](https://codingcoding.tistory.com/383)
3. [리스트 출력하는 밥법](https://pinggoopark.tistory.com/312)
4. [[파이썬] 중복되지 않은 무작위의 3자리 숫자 생성하기](https://opentutorials.org/module/2980/28196)
5. [배열 중복 제거 방법](https://developer-talk.tistory.com/215)

