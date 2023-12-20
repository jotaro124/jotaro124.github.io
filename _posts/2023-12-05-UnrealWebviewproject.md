---
layout: single
title: "언리얼 엔진 WebBrowser Plugin 다루는 법 정리"
categories: UnrealStudy
toc: true
sidebar:
     nav: "docs"
---

<br>

이 글은 언리얼 엔진과 웹페이지를 통신하는 기능을 구현하는 방법을 정리했습니다. 여러가지 정보를 조사하고 코드를 정리했습니다.

또한, 공부와 기록을 위해서 쓴 글이기도 해서 틀린 점과 수정해야 하는 부분이 있는 점 양해 부탁드립니다.

<br>

# 개념 정리

![0_Graph](..\..\images\2023-12-05-UnrealWebviewproject\0_Graph.png)

1. 언리얼에 많은 클래스의 함수들을 JSManager의 **SetData()**에 등록한다. (예시로 SetResultII, SetResultIF, SetResultSS를 사용)
2. 웹에서 언리얼 함수를 접근할 수 있게 WebBrowser에 JSManager를 **BindUObject()**한다.
3. 웹에 SendMsgUnreal()로 WebBrowser에 바인딩된 JSManager의 **SearchData()**를 호출한다.
4. WebBrowser의 ExcuteJavascript()로 웹에 접근해서 **UnrealCall(**)를 호출한다.

<br>

# Web Browser 플러그인 설치

<br>

![1.setWebbrowserplugin](..\..\images\2023-12-05-UnrealWebviewproject\1.setWebbrowserplugin.png)

- Settings -> Plugins를 선택해서 "Web Browser"를 검색해서 활성화하고 프로젝트를 제시작한다.

<br>

~~~c#
PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "Json", "JsonUtilities", "UMG", "WebBrowser", "WebBrowserWidget" });

~~~

- 프로젝트 경로 -> Source/ProjectName/ProjectName.Build.cs에 들어가서
- Json", "JsonUtilities", "UMG", "WebBrowser", "WebBrowerWidget"을 추가한다.
- ProjectName.uproject 파일을 우클릭하고, Generate Visual Studio project Files를 선택해서 Visual Studio 프로젝트 파일을 갱신하거나 Visual Studio를 컴파일한다.

<br>

## 새로운 Webbrower C++ 생성 

![2.Image2](..\..\images\2023-12-05-UnrealWebviewproject\2.Image2.png)

- 새로운 C++ 클래스를 선택해서 새로운 클래스를 생성한다.
- "WebBrowser" 검색하고 선택한다.

<br>

![3.Image3](..\..\images\2023-12-05-UnrealWebviewproject\3.Image3.png)

- MyWebBrower라고 클래스 이름을 입력하고 클래스를 생성한다.

<br>

~~~c++
#include "CoreMinimal.h"
#include "WebBrowser.h"
#include "MyWebBrowser.generated.h"

/**
 * 
 */
UCLASS()
class SCROLLGAME1_API UMyWebBrowser : public UWebBrowser
{
	GENERATED_BODY()

public:

	// 자바스크립트에서 접근할 수 있게 해주는 함수
	UFUNCTION(BlueprintCallable, Category = "Web Browser")
		void BindUObject(const FString& Name, UObject* Object, bool bIsPermanent);

};
~~~

- MyWebBrower.h에 들어가서 BindUObject 함수를 선언한다.

<br>

~~~c++
#include "MyWebBrowser.h"
#include "SWebBrowser.h"

void UMyWebBrowser::BindUObject(const FString& Name, UObject* Object, bool bIsPermanent)
{
	if (WebBrowserWidget.IsValid())
		WebBrowserWidget->BindUObject(Name, Object, bIsPermanent);
}

~~~

- MyWebBrowser.cpp에 들어가서 BindUObject 함수를 구현한다.

<br>

## 웹페이지 생성 및 자바스크립트 구현

- 프로젝트에 들어가서 Web 폴더를 생성하고, index.html과 lib.js를 생성한다.

<br>

~~~html
<!DOCTYPE html>
<html>
    <head>
        <title>Webview 테스트용 웹페이지</title>
        <script src ="lib.js"></script>
    </head>
    <body>
        <h1 id="msg">언리얼 메시지를 보여주는 영역</h1>
        <hr>
        <br>
        <form>
            
            Arg1: <input type="text" id="1arg1" value="3">
            Arg2: <input type="text" id="1arg2" value="5">
            <input type="button" value="Atomic 함수" onclick="ClickButton1()"><br><br>

            Arg1: <input type="text" id="2arg1" value="1">
            Arg2: <input type="text" id="2arg2" value="3.14">
            <input type="button" value="Atom 함수" onclick="ClickButton2()"><br><br>

            Arg1: <input type="text" id="3arg1" value="a">
            Arg2: <input type="text" id="3arg2" value="b">
            <input type="button" value="Atomy 함수" onclick="ClickButton3()">
            
        </form>
    </body>
</html>
~~~

<br>

~~~javascript
// 언리얼에 메시지를 전달하는 함수
// 언리얼 함수를 호출하는 함수
function sendMsgUnreal(Argument)
{
    var jsonData =JSON.stringify(Argument);

    if(window.ue && window.ue.obj){
        window.ue.obj.searchdata(jsonData);  
    }
    else
    alert('Not Found Message');

}

// 언리얼에 있는 webui_SetResultII_II 함수를 호출하는 함수
// Atomic(int, int);
// 키: webui_SetResultII_II
function ClickButton1()
{
    var arg1 = document.getElementById("1arg1");
    var arg2 = document.getElementById("1arg2");

    sendMsgUnreal({key: "webui_SetResultII_II",values:[arg1.value, arg2.value]});
}

//  언리얼에 있는 webui_SetResultIF_IF 함수를 호출하는 함수
// Atom(int,float);
// 키: webui_SetResultIF_IF
function ClickButton2()
{
    
    var arg1 = document.getElementById("2arg1");
    var arg2 = document.getElementById("2arg2");
    
    sendMsgUnreal({key: "webui_SetResultIF_IF",values:[arg1.value, arg2.value]});
}

//  언리얼에 있는 webui_SetResultSS_SS 함수를 호출하는 함수
// Atomy(string,string);
// 키: webui_SetResultSS_SS
function ClickButton3()
{
    var arg1 = document.getElementById("3arg1");
    var arg2 = document.getElementById("3arg2");

    sendMsgUnreal({key: "webui_SetResultSS_SS",values:[arg1.value, arg2.value]});
}

//언리얼에서 호출될 함수
function UnrealCall(arg1, arg2)
{
    var msg = "언리얼에서 "+arg1+"와 "+arg2+"를 입력했습니다."
    var H1 = document.getElementById("msg");
    H1.innerHTML = msg;
}

~~~

<br>



# Webbrower를 사용할 WebUI 생성

<br>

![4._Image4](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4.png)

- 위젯 블루프린트를 클릭해서 UI 에셋을 생성하고 WebUI라고 한다.

<br>

## 기본적인 UI 배치 

<br>

![4._Image4_1](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1.png)

- Vertical Box에 Border 위젯을 추가하고 Vertical Alignment와 Brush Color를 수정한다.

<br>

![4._Image4_1_1](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_1.png)

- Border에 Text 위젯을 추가하고, ResultText로 이름을 변경한다.
- 변수인자에 체크하고, Color and Opacity, Font, Justification를 수정한다.
- Text 값을 "웹페이지  메시지를 보여주는 영역"으로 변경한다.

<br>

![4._Image4_1_2](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_2.png)

- Border에 MyWebBrower 위젯을 추가하고, Padding과 Size를 수정한다.

<br>

![4._Image4_1_3](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_3.png)

- Border에 Horizontal Box 위젯을 추가하고 UnrealInput으로 이름을 변경한다.
- Vertical Alignment를 수정한다.

<br>

![4._Image4_1_4](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_4.png)

- UnrealInput에 Vertical Box 위젯을 추가하고 InputPanel으로 이름을 변경한다.
- Size를 변경한다.

<br>

![4._Image4_1_5](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_5.png)

- InputPanel에 Border 위젯을 추가하고 Input_A으로 이름을 변경한다
- Pading, Size를 수정한다.

<br>

![4._Image4_1_5_1](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_5_1.png)

- Input_A에 Editable Text 위젯을 추가하고, Text_A로 이름을 변경한다
- Font size와 Color and Opacity를 수정한다.

<br>

![4._Image4_1_5_2](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_5_2.png)

- Input_A를 복사하고 각각 Input_B, Text_B로 이름을 변경한다
- padding을 수정한다.

<br>

![4._Image4_1_6](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_6.png)

- UnrealInput에 Button 위젯을 추가하고 SendBtn으로 이름을 변경한다
- Padding을 수정한다

<br>

![4._Image4_1_6_1](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_1_6_1.png)

- Text 위젯을 추가하고 Text, Color and Opacity, Size를 수정한다.

<br>

![4._image4_1_7](..\..\images\2023-12-05-UnrealWebviewproject\4._image4_1_7.png)

- UI 배치 결과 화면

<br>

## UI에 사용할 함수 구현

<br>

![4._Image4_2_1](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_2_1.png)

- int형 변수 arg1과 arg2를 더한 값을 ResultText에 반영하는 SetResultII 함수를 구현한다.

<br>

![4._Image4_2_2](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_2_2.png)

- int형 변수 arg1과 float형 변수 arg2를 더한 값을 ResultText에 반영하는 SetResultIF 함수를 구현한다.

<br>

![4._Image4_2_3](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_2_3.png)

- string형 변수 arg1과 arg2를 더한 값을 SetResultSS 함수를 구현한다.

<br>

## WebUI 화면에 띄우기

<br>

 ![5.Image5](..\..\images\2023-12-05-UnrealWebviewproject\5.Image5.png)

- GameMode 블루프린트 생성하고 BP_WebbrowserGM으로 이름을 변경한다.

<br>

![5.Image5_1](..\..\images\2023-12-05-UnrealWebviewproject\5.Image5_1.png)

- BeginPlay 이벤트에 WebUI르 띄우는 기능을 구현한다.

<br>

![5.Image5_1_1](..\..\images\2023-12-05-UnrealWebviewproject\5.Image5_1_1-170171239277742.png)

- 추가로 Show Mouse Cursor를 활성화한다.

<br>

![8.Image8](..\..\images\2023-12-05-UnrealWebviewproject\8.Image8.png)

- 월드세팅 -> 게임모드 오버라이드에 BP_WebbrowserGM으로 변경한다..

<br>

# C++로 JSManager 생성

<br>

![6._Image6_0](..\..\images\2023-12-05-UnrealWebviewproject\6._Image6_0.png)

- Actor를 상속받은 JSManager 클래스를 생성한다.

<br>

## JS와 통신할 함수들을 구현

~~~cpp
#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "JSManager.generated.h"

DECLARE_DYNAMIC_DELEGATE_OneParam(FOnJSCallFunctionDelegate, const TArray<FString>&, args);


UCLASS()
class SCROLLGAME1_API AJSManager : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	AJSManager();

private:
	TMap<FString, FOnJSCallFunctionDelegate> database; // 딕셔너리 자료구조로 만든 데이터베이스

public:

	UFUNCTION(BlueprintCallable)
		// 데이터베이스에 키와 이벤트 함수를 등록하는 함수 
		void setdata(FString key, FOnJSCallFunctionDelegate func);

	UFUNCTION(BlueprintCallable)
		// 데이터베이스를 검색하는 함수
		void SearchData(FString data);

};
~~~

- JSManager.h에 들어가서 델리게이트 FOnJSCallFuctionDelegate를 선언한다.
- 딕셔너리 자료구조인 <Fstring, FOnJSCallFunctionDelegate> database를 선언한다.
- 키와 함수를 데이터베이스에 추가하는 setdata를 선언한다.
-  데이터베이스를 검색하는 SearchData 함수를 선언한다.

<br>

~~~cpp
#include "JSManager.h"

// Sets default values
AJSManager::AJSManager()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = false;

}

void AJSManager::setdata(FString key, FOnJSCallFunctionDelegate func)
{
	database.Add(key, func);
}

void AJSManager::SearchData(FString data)
{
	TSharedPtr<FJsonObject> JsonParsed;
	TSharedRef<TJsonReader<TCHAR>> JsonReader = TJsonReaderFactory<TCHAR>::Create(data);

	//문자열로 저장된 Json 데이터를 Json 형식으로 바꾸는 작업
	// 작업이 성공하지 못하면 리턴한다
	bool checkDes = FJsonSerializer::Deserialize(JsonReader, JsonParsed) && JsonParsed.IsValid();
	if (!checkDes)
		return;

	// Json에 저장된 키와 매개변수 값들을 가져오는 작업
	FString key = JsonParsed->GetStringField("key");
	TArray<FString> ArrayJson;
	JsonParsed->TryGetStringArrayField("values", ArrayJson);

	if (database.Contains(key))
		database[key].ExecuteIfBound(ArrayJson);
}
~~~

- JSManager.cpp에 들어가서 setData함수와 SearchData 함수를 구현한다.

<br>

## JSManager를 블루프린트로 생성

<br>

![6.Image6](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6-170171047976523.png)

- JSManager를 상속받은 블루프린트 BP_JSManager를 생성한다.

<br>

![6.Image6_1](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6_1.png)

- DefaultSetting 함수를 선언하고 구현한다.
- BP_WebbrowerGM에 WebUI를 새로운 변수로 승격한다.

<br>

## UI에 구현된 함수들을 데이터베이스에 등록

<br>

![6.Image6_1_1](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6_1_1.png)

- WebUI_SetResultII_II 함수를 선언하고 구현한다.
- WebUI에 SetResultII 함수와 연결한다.

<br>

![6.Image6_1_2](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6_1_2.png)

- WebUI_SetResultIF_IF 함수를 선언하고 구현한다.
- WebUI에 SetResultIF 함수와 연결한다.

<br>![6.Image6_1_3](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6_1_3.png)

- WebUI_SetResultSS_SS 함수를 선언하고 구현한다.
- WebUI에 SetResultSS 함수와 연결한다.

<br>

![6.Image6_1_4](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6_1_4.png)

- SettingDatabase 함수를 선언하고 구현한다.
- WebUI_SetResultII_II, WebUI_SetResult_IF, WebUI_SetResult_SS를 Setdata 함수에 등록한다.

<br>

![6.Image6_1_5](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6_1_5.png)

- DefulatSetting 함수에 들어가서 SettingDatabase 함수를 호출한다.

<br>

![6.Image6_2](..\..\images\2023-12-05-UnrealWebviewproject\6.Image6_2.png)

- BeginPlay 이벤트에 UI 생성 시간을 위해 0.2초 딜레이 후 DefalutSetting 함수를 호출한다.

<br>

![8.Image8_1](..\..\images\2023-12-05-UnrealWebviewproject\8.Image8_1.png)

- 레벨에 BP_JSManager를 배치한다.

<br>

# Webbrowser와 웹페이지를 연결하기

![7.Image7_1](..\..\images\2023-12-05-UnrealWebviewproject\7.Image7_1.png)

- string 타입의 FileURL 변수를 선언한다.
- 디폴트 값으로 index.html의 주소를 넣는다.

<br>

![7.Image7](..\..\images\2023-12-05-UnrealWebviewproject\7.Image7.png)

- Construct 이벤트에 들어가서 레벨에 있는 BP_JSManager를 변수로 승격해서 JSManager를 생성한다.

<br>

![7.Image7_1_1](..\..\images\2023-12-05-UnrealWebviewproject\7.Image7_1_1.png)

- 다시 Construct 이벤트로 돌아간다
- MyWebBrowser에 LoadURL함수와 BindUObject 함수를 호출한다.
- BindUObject에 JSManager를 연결하고, Name에 obj를 넣는다.

<br>

![4._Image4_2](..\..\images\2023-12-05-UnrealWebviewproject\4._Image4_2.png)

- 자바스크립트 lib.js에 UnrealCall 함수를 호출하는 GetCallJS 함수를 선언하고 구현한다.
- 퓨어 항목에 체크한다.

<br>

![7.Image7_2](..\..\images\2023-12-05-UnrealWebviewproject\7.Image7_2.png)

- SendBtn에 OnClicked 이벤트를 생성하고 MyWebBrower의 ExcuteJavascript 함수를 호출한다.
- Text_A의 Text와 Text_B의 Text 값을 GetCallJS 함수를 호출해서 ScriptText에 전달한다.

<br>

# 실행화면

[![썸내일](http://img.youtube.com/vi/8X8YNM13fHw/0.jpg)](https://youtu.be/8X8YNM13fHw)

<br>


# 참고자료

1. [We Browser Plugin 기본 사용법](https://definelife.tistory.com/m/88)
2. [Web Browser Plugin 로컬 파일 사용법](https://unrealcommunity.wiki/web-browser-widget-13f406)
3. [UWebBrowser 클래스 설명](https://docs.unrealengine.com/4.27/en-US/API/Plugins/WebBrowserWidget/UWebBrowser/)
4. [SWebBrowser 클래스 설명](https://docs.unrealengine.com/4.27/en-US/API/Runtime/WebBrowser/SWebBrowser/)
5. [BindUObject 함수 설명](https://docs.unrealengine.com/4.27/en-US/API/Runtime/WebBrowser/SWebBrowser/BindUObject/)
6. [UWebBrowser 클래스 구조 설명](https://zhuanlan.zhihu.com/p/535722624)
7. [Execute Javascript 함수 설명](https://docs.unrealengine.com/4.26/en-US/BlueprintAPI/WebBrowser/ExecuteJavascript/)
8. [Execute Javascript 함수 사용법](https://blog.csdn.net/weixin_47428955/article/details/124949326)
9. [WebBrower와 웹브라우저 통신 방법1](https://goulandis.github.io/2021/05/14/%E3%80%90UE4%E3%80%91UE4%E5%86%85%E5%B5%8CWeb%E5%8F%8A%E4%B8%8EWeb%E9%80%9A%E4%BF%A1/)
10. [WeBrowser와 웹브라우저 통신 방법2](https://zhuanlan.zhihu.com/p/453923540?utm_id=0)
11. [FJsonObject 클래스 설명](https://docs.unrealengine.com/4.27/en-US/API/Runtime/Json/Dom/FJsonObject/)
12. [언리얼에서 JSON 사용법1](https://gist.github.com/hanzochang/07eba255ce0d1695582a92e98e973200)
13. [언리얼에서 JSON 사용법2](https://youtu.be/VOQaEGzB-cc?si=_uLIRsmEHAGg_qga)
14. [언리얼에서 JSON 사용법3](https://youtu.be/0YCHPX4jc3A?si=yUdJzHfpUfemP0iO)