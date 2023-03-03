---
layout: single
title: "유니티 웹 뷰 사용하기"
categories: UnityStudy
toc: true
sidebar:
     nav: "docs"
---



<br>

Unity에서 웹 브라우저를 사용할 때 Webview를 사용하면 구현이 가능하다.

이 글은 Webview를 사용하기 위해 여러가지 정보를 검색해서 조사 및 정리한 내용이다.



또한, 공부와 기록을 위해서 쓴 글이기도 해서 틀린 점과 수정해야 하는 부분이 있는 점 양해 부탁드립니다.

<br>



# Unity Webview 플러그인 설치 방법

## 1. Gree Unity Webview 플러그인 설치

[Gree Unity Webview plugin](https://github.com/gree/unity-webview)

![image-20230302223028023](..\..\images\2023-03-02-UnityWebviewGree\image-20230302223028023.png)

* 위의 링크를 통해서 플러그인을 설치할 수 있음.
* Download Zip을  클릭해서 압축파일을 다운로드 받고 압축을 해제한다.



![image-20230302223813707](..\..\images\2023-03-02-UnityWebviewGree\image-20230302223813707.png)

* dist 폴더에 들어가서 unity-webview.unitypackage를 선택해서 Unity 프로젝트에 설치한다.
* 만약, File Input Field 기능을 사용하지 않는다면 unity-webview-nofragment.unitypackage를 선택해서 설치한다.

![image-20230302233653252](..\..\images\2023-03-02-UnityWebviewGree\image-20230302233653252.png)

* 유니티 프로젝트 하나를 생성한 후 플러그인을 선택해서 Import한다.



# Gree Unity Webview 플러그인 사용법

## 1. 웹 페이지 작성

~~~html
<!DOCTYPE html>
<html>
    <head>
        <title>Webview 테스트용 웹 페이지</title>
        <script src ="lib.js"></script>
    </head>
    <body>
        <h1>테스트</h1>
        <hr>
        <br>
        <form>
            <input type="button" id= "btn1" value="click" onclick="ClickButton1()">
        </form>
    </body>
</html>
~~~

* 로컬 웹페이지를 부르는 방법을 적는다.
* 예시용 사용할 웹 페이지 index.html 작성한다.
* 버튼을 클릭하면 웹 뷰 화면을 안보이게 한다.



~~~javascript
// 유니티로 메시지를 전달하는 함수
function sendMsgUnity(msg)
{
    if(typeof Unity !=="undefined")
			Unity.call(msg);
}

function ClickButton1()
{
    var value1 = document.getElementById("btn1").value;
    sendMsgUnity(value1);
}

function f(a,b)
{
    var sum = parseInt(a)+parseInt(b);
    alert(sum);
}
~~~

* 자바스크립트 함수를 가지고 있는 lib.js 작성한다.
* id가 btn1인 값을 sendMsgUnity에 전달한다.
* Unity.call(string)으로 유니티로 메시지 전달한다.
* Unity에서 보낸 데이터들의 합을 출력한다.



## 2. StartWebView()

~~~csharp
void StartWebView()
{
    // 웹뷰 오브젝트 생성 및 초기화
    webviewobj = (new GameObject("WebViewObject")).AddComponent<WebViewObject>();
    webviewobj.Init(cb:ReciveMsg);

    StartCoroutine(LoadLocalFile("lib.js", false));
    StartCoroutine(LoadLocalFile("index.html", true));
    webviewobj.SetVisibility(true);
    webviewobj.SetMargins(0,250,0,260);
}
~~~

* 웹 뷰 오브젝트 생성 및 초기화를 한다.
* lib.js, index.html을 Load한다.
* 웹 뷰 화면을 띄우고 Margin을 설정한다.



## 3. ReciveMsg()

~~~csharp
// 웹뷰로 로드한 html에서 보낸 메시지를 받는 함수
void ReciveMsg(string msg)
{
    if(msg.Equals("click"))
    {
        webviewobj.SetVisibility(false);
    }
}
~~~

* msg가 "click"과 같으면 웹 뷰를 안보이게 한다.



## 4. LoadLocalFile()

~~~csharp
// 스트리밍 폴더에 있는 로컬파일을 불러오는 함수
IEnumerator LoadLocalFile(string FileName, bool IsHtml)
{
     string filepath = Path.Combine(Application.streamingAssetsPath, FileName); // StreamingAsset 경로
     string storagePath = Path.Combine(Application.persistentDataPath, FileName); // 저장 경로
     byte[] result; // 데이터 다운로드 결과

     if (filepath.Contains("://"))
     {
         UnityWebRequest www = UnityWebRequest.Get(filepath);
         yield return www.SendWebRequest();
         result = www.downloadHandler.data;
     }
     else result = File.ReadAllBytes(filepath);

     File.WriteAllBytes(storagePath,result);

     if (IsHtml)
         webviewobj.LoadURL("file://" + storagePath.Replace(" ", "%20"));
}
~~~

* index.html와 lib.js 파일을 StreamingAssets 폴더에 넣은 다음 해당 주소를 filepath에 넣는다.
* 데이터를 저장할 수 있는 디렉토리 경로를 storagePath에 넣는다.
* UnityWebRequest 클래스로 StreamingAssets에 있는 데이터를 불러와서 storagePath에 저장한다.
* UnityWebRequest와 Path 및 File은 각각 UnityEngine.Networking, System.IO 네임스페이스를 선언해야 한다.
* html 파일이라면 WebView에 Load한다.



## 5. Show()

~~~csharp
// 자바스크립트 함수 호출
public void Show()
    {
        //자바스크립트에 있는  f(1,5) 함수 호출
        webviewobj.EvaluateJS(string.Format("f({0},{1})", 1, 5));
    }
~~~

* 자바스크립트의 함수 f(a,b)를 호출하는 함수
* 유니티 버튼 이벤트에 바인딩해서 사용

# 실행결과

![image-20230303223949134](..\..\images\2023-03-02-UnityWebviewGree\image-20230303223949134.png)

<span style="color:red">주의)</span> Unity 에디터나 Winodw 빌드 세팅으로 실행하게 되면 해당 플랫폼에는 Webview가 지원하지 않는다는 에러 메시지가 발생함.

그래서 모바일 빌드로 세팅하고, 모바일 기기에서 실행해야 정상적으로 웹 뷰 화면이 나타남.

### 실행 화면

![image-20230303225659928](..\..\images\2023-03-02-UnityWebviewGree\image-20230303225659928.png)





# 참고자료

1. [웹 뷰 샘플 코드](https://github.com/gree/unity-webview/blob/master/sample/Assets/Scripts/SampleWebView.cs)
2. [웹 뷰 사용법1](https://dragontory.tistory.com/407)
3. [웹 뷰 사용법2](https://loumo.jp/archives/5335)
4. [유니티에서 자바스크립트 함수 호출 방법1](https://github.com/gree/unity-webview/issues/334)
5. [유니티에서 자바스크립트 함수 호출 방법2](https://github.com/gree/unity-webview/issues/652)
6. [자바스크립트에서 유니티로 메시지 보내는 방법1](https://202psj.tistory.com/1331)
7. [자바스크립트에서 유니티로 메시지 보내는 방법2](https://tsubakit1.hateblo.jp/entry/20130523/1369316363)