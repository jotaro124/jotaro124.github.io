---
layout: single
title: "유니티 기본 교육 과정 제작"
categories: work2lern
toc: true
sidebar:
     nav: "docs"
---



# 프로젝트 소개

 현장 실습으로 들어간 회사에서 유니티 역량을 알아보기 위해 정해진 기간 안에 샘플과 사양서를 참고해서 샘플과 유사하게 구현하는 업무를 받아서 수행함.

- 프로젝트명: Unity Engine으로 루빅스 큐브 만들기
- 엔진: Unity 엔진 (2020.3.22f1)
- 에디터 :Microsoft Visual Studio Community 2019(16.7.3 버전)
- 개발기간: 2022.09.01~2022.09.06
- 개발규모: 1인 개발
- Demo: [[https://splendorous-biscochitos-ae02f7.netlify.app](https://splendorous-biscochitos-ae02f7.netlify.app/)]([https://splendorous-biscochitos-ae02f7.netlify.app](https://splendorous-biscochitos-ae02f7.netlify.app/))



# 사양서

![image-20230117013612690](..\..\images\2023-01-16-RubiksCube\image-20230117013612690.png)

- U, F, R, D, B, L키를 누르면 해당 텍스트가 녹색으로 변하며, 해당하는 면이 시계방향으로 회전 (회전 기능은 코루틴을 이용하여 개발할 것)
- Shift를 누르면 안내 텍스트가 변경되며, 회전 방향이 반시계 방향으로 변함
-  마우스 좌클릭 후 화면을 드래그하여 큐브를 임의 방향으로 회전시켜 볼 수 있음
- 스페이스 바를 누르면 임의의 면이 임의 방향으로 1회 회전
- 백스페이스를 누르면 이전 명령 1회를 되돌림 (백스페이스 기능은 Command 패턴을 사용하여 개발할 것)



# 구현

## 1. 큐브 회전 기능 구현

### 1.1 OriginalRotate.cs

- 오브젝트를 시계, 반시계 방향으로 회전을 하는 기능
- 중심 큐브를 기준으로 주변 큐브를 찾는 기능
- 각 면의 중심 큐브를 기준으로 주변 큐브를 연결하는 기능

<details>
<summary>전체코드</summary>
<div markdown="1">

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class OriginalRotate : MonoBehaviour{

     // 회전축 오브젝트
    public GameObject RotateObj;

    // 회전 방향
    int clockwise = 1;
    int anti_clockwise = -1;

    // 회전 관련 변수
    bool rotating = false;
    float rotateTime = 0.1f;

    // 회전 각도
    float angle = 0f;

    public float Angle { get { return angle; } set {angle = value; } }
    public bool Rotating { get { return rotating; } set { rotating = value; } }
    public float RotateTime { get { return rotateTime; } set { rotateTime = value; } }

    public int CLOCKWISE { get { return clockwise; }  }
    public int ANTI_CLOCKWISE { get { return anti_clockwise; } }


    char _CurrentSide = ' ';
    bool _CurrentWise = true;

    public char CurrentSide { get { return _CurrentSide; } set { _CurrentSide = value; } }
    public bool CurrentWise { get { return _CurrentWise; } set { _CurrentWise = value; } }

    // 부모 오브젝트
    GameObject Parent;

    // 자식 오브젝트
    GameObject[] childs = new GameObject[8];

    protected virtual void Start()
    {
        Parent = GameObject.Find("RubicCube");
    }

    // 코루틴 종료 후, 90º에 가장 가까운 값을 찾도록 하는 함수
    // 반환값 float
    public float round90(float f)
    {
        float r = f % 90;
        return (r < 45) ? f - r : f - r + 90;
    }

    // 오브젝트를 90도 회전하는 코루틴
    public IEnumerator moveBlockTime(int idx, int wise)
    {
        rotating = true;

        float elapsedTime = 0.0f;

        // 현재 회전값
        Quaternion currentRotation = RotateObj.transform.rotation;

        angle += 90 * wise; // 방향 전환

        
        // 타겟 회전값
        Quaternion targetRotation = GetTargetRotation(idx, angle, wise);

        while (elapsedTime < rotateTime)
        {
            RotateObj.transform.rotation = Quaternion.Slerp(currentRotation, targetRotation, elapsedTime / rotateTime);


            elapsedTime += Time.deltaTime;
            yield return null;
        }

        // 회전각도 90도로 유지
        angle = round90(angle);
        RotateObj.transform.rotation = GetTargetRotation(idx, angle, wise);

        // 언바인딩 한다.
        UnBindChild();

        rotating = false;

    }


    public void RotateSide(bool clockwise){
        if (rotating == false)
        {
            int index = GetSideIndex(_CurrentSide);

            //바인딩 한다. 
            BindChild(index);

            CurrentWise = clockwise;

            // 회전에 필요한 함수를 코루틴을 사용해서 호출
            if (clockwise)
                StartCoroutine(moveBlockTime(index, CLOCKWISE));
            else
                StartCoroutine(moveBlockTime(index, ANTI_CLOCKWISE));
        }
    }

    // Side에 해당하는 인덱스 값을 얻는다.
    // 반환타입 int
    public int GetSideIndex(char Side) {
        int result = -1;

        switch (Side)
        {
            case 'U':
                result = 0;
                break;

            case 'D':
                result = 1;
                break;

            case 'F':
                result = 2;
                break;

            case 'B':
                result = 3;
                break;

            case 'R':
                result = 4;
                break;

            case 'L':
                result = 5;
                break;
        }
        
        
        return result; 
    }

    // 인덱스에 해당하는 회전값을 구한다.
    // 반환값 Quaternion
    // U:0, D: 1, F:2, B:3. R:4, L:5
    Quaternion GetTargetRotation(int idx, float Angle, int wise)
    {
        Quaternion result = Quaternion.identity;

        switch (idx)
        {
            case 0:
            case 1:
                result = Quaternion.Euler(0,Angle,0);
                break;

            case 2:
            case 3:
                result = Quaternion.Euler(0, 0, Angle);
                break;

            case 4:
            case 5:
                result = Quaternion.Euler(Angle, 0, 0);
                break;
        }

        return result;
    }

    // 인덱스에 해당하는 오브젝트의 위치값을 반환
    // 반환값 int
    // U:0, D: 1, F:2, B:3. R:4, L:5
    int GetXYZPos(GameObject obj, int idx)
    {

        int result = 0;

        switch (idx)
        {
            case 0:
            case 1:

                result = Mathf.RoundToInt(obj.transform.position.y);
                break;

            case 2:
            case 3:
                result = Mathf.RoundToInt(obj.transform.position.z);
                break;

            case 4:
            case 5:
                result = Mathf.RoundToInt(obj.transform.position.x);
                break;

        }

        return result;

    }

    // 조건에 맞는 자식을 찾은 함수
    void FindChild(int index)
    {
        // 회전축의 위치
        int R_pos = GetXYZPos(RotateObj, index);


        int j = 0;
        for (int i = 0; i < Parent.transform.childCount; i++)
        {
            // 회전축에 포함될 자식 후보들
            GameObject child = Parent.transform.GetChild(i).gameObject;

            int Rc_pos = GetXYZPos(child, index);

            if (R_pos == Rc_pos)
            {
                if (child.transform.position != RotateObj.transform.position)
                {
                    childs[j] = child;
                    j++;
                }


            }

        } // 반복문 종료

    }

    // 자식과 연결하는 함수
    public void BindChild(int index)
    {
        FindChild(index);

        for (int i = 0; i < 8; i++)
        {
            GameObject tmp = childs[i];
            tmp.transform.SetParent(RotateObj.transform);
        }
    }

    // 자식과 연결을 해제하는 함수
    public void UnBindChild()
    {
        for (int i = 0; i < 8; i++)
        {
            GameObject tmp = childs[i];
            tmp.transform.SetParent(Parent.transform);
        }

    }

}
```
</div>
</details>



> 오브젝트를 시계, 반시계 방향으로 회전하는 기능

```c#
     // 코루틴 종료 후, 90º에 가장 가까운 값을 찾도록 하는 함수
     // 반환값 float
    public float round90(float f)
    {
        float r = f % 90;
        return (r < 45) ? f - r : f - r + 90;
    }

     // 오브젝트를 90도 회전하는 코루틴
    public IEnumerator moveBlockTime(int idx, int wise)
    {
        rotating = true;

        float elapsedTime = 0.0f;

        // 현재 회전값
        Quaternion currentRotation = RotateObj.transform.rotation;

        angle += 90 * wise; // 방향 전환

        
        // 타겟 회전값
        Quaternion targetRotation = GetTargetRotation(idx, angle, wise);

        while (elapsedTime < rotateTime)
        {
            RotateObj.transform.rotation = Quaternion.Slerp(currentRotation, targetRotation, elapsedTime / rotateTime);


            elapsedTime += Time.deltaTime;
            yield return null;
        }

        // 회전각도 90도로 유지
        angle = round90(angle);
        RotateObj.transform.rotation = GetTargetRotation(idx, angle, wise);

        // 언바인딩 한다.
        UnBindChild();

        rotating = false;

    }

     public void RotateSide(bool clockwise){
        if (rotating == false)
        {
            int index = GetSideIndex(_CurrentSide);

            //바인딩 한다. 
            BindChild(index);

            CurrentWise = clockwise;

            // 회전에 필요한 함수를 코루틴을 사용해서 호출
            if (clockwise)
                StartCoroutine(moveBlockTime(index, CLOCKWISE));
            else
                StartCoroutine(moveBlockTime(index, ANTI_CLOCKWISE));
        }
    }

```



## 2. Command 패턴을 사용해서 Undo 기능

### 2.1 Command.cs

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Command
{
    public virtual void Execute() { }
    public virtual void Undo() { }
}

public class CommandUndo:Command
{
    private OriginalRotate _orign;
    private char _side;
    private bool _wise;
    private char _pre_side;
    private bool _pre_wise;

    public CommandUndo(OriginalRotate orign, bool wise)
    {
        _orign = orign;
        _wise = wise;
    }

    public override void Execute()
    {
        _pre_side = _side;
        _pre_wise = !_wise;

        _orign.RotateSide(_wise);
    }

    public override void Undo()
    {
        _orign.RotateSide(_pre_wise);
    }


}
```



### 2.2 ControllCube.cs

- 일부 코드 생략

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ControllCube : MonoBehaviour{
    /*일부 코드 생략*/
    
    // Undo 키 체크용도 Flag
    bool isPushUndoKey = false;
    
    // 액터의 Command를 담는 스택 선언
    Stack<Command> stack = new Stack<Command>();
    
    Command GetCommand()
    {
        // Undo 키를 눌렀을 때 처리
        if (Input.GetKeyUp(KeyCode.Backspace))
        {
            isPushUndoKey = true;

            if (stack.Count > 0)
            {
                return stack.Pop();
            }
        }

        // Undo 키가 눌리지 않았다면

        // 위쪽
        if (Input.GetKeyUp(KeyCode.U))
        {
            Command command = new CommandUndo(OrignArr[0], !IsDownShiftKey);
            stack.Push(command);

            return command;
        }

        // 아래쪽
        if (Input.GetKeyUp(KeyCode.D))
        {
            Command command = new CommandUndo(OrignArr[1], !IsDownShiftKey);
            stack.Push(command);

            return command;
        }

        // 앞쪽
        if (Input.GetKeyUp(KeyCode.F))
        {
            Command command = new CommandUndo(OrignArr[2], !IsDownShiftKey);
            stack.Push(command);

            return command;
        }

        // 뒤쪽
        if (Input.GetKeyUp(KeyCode.B))
        {
            Command command = new CommandUndo(OrignArr[3], !IsDownShiftKey);
            stack.Push(command);

            return command;
        }

        // 오른쪽
        if (Input.GetKeyUp(KeyCode.R))
        {
            Command command = new CommandUndo(OrignArr[4], !IsDownShiftKey);
            stack.Push(command);

            return command;
        }

        // 왼쪽
        if (Input.GetKeyUp(KeyCode.L))
        {
            Command command = new CommandUndo(OrignArr[5], !IsDownShiftKey);
            stack.Push(command);

            return command;
        }

        // 랜덤 셔플
        if (Input.GetKeyUp(KeyCode.Space))
        {
            // 임의의 면
            // U:0, D:1, F:2, B:3, R:4, L:5
            int RandSideNum = Random.Range(0, 6);
            OriginalRotate RandSide = GetRandomSide(RandSideNum);

            //임의 방향
            // true: 시계 방향, false: 반시계 방향
            bool RandWise = (Random.value > 0.5f);

            Command command = new CommandUndo(RandSide, RandWise);
            stack.Push(command);

            return command;
        }

            return null;
    }
    
    void Update(){
        isPushUndoKey = false;
        Command command = GetCommand();
        if (command != null)
        {
            if (isPushUndoKey)
            {
                command.Undo();
            }
            else
            {
                command.Execute();
            }
        }
    }
}
```





# 참고자료

1. [오브젝트 회전 시키는 방법](https://bloodstrawberry.tistory.com/841)
2. [카메라 회전 시키는 방법](https://bloodstrawberry.tistory.com/604)
3. [Command 패턴으로 Undo 구현 방법](https://mentum.tistory.com/580)
4. [Command 패턴으로 Undo 구현 방법2](http://rapapa.net/?p=3143)
5. [형식 문자열 format()에 대해서](https://slaner.tistory.com/92)

