---
layout: single
title: "커뮤니티 메타버스 프로젝트"
categories: work2lern
toc: true
toc_sticky : true
sidebar:
     nav: "docs"
---



# 프로젝트 소개

  현장 실습으로 들어간 회사 대표님이 주신 아이디어를 바탕으로 메타버스 프로젝트 개발. (포트폴리오로 사용해도 된다는 허락을 받음)

- 프로젝트명: Beyond Link My Planet
- 엔진: Unity 엔진 (2020.3.23f1)
- 에디터: Microsoft Visual Studio Community 2019(16.7.3 버전)
- 개발기간: 2022.10.05~2022.11.30
- 개발규모: 2인 개발 (같이 현장 실습 온 팀원과 같이 아이디어 기획서 제작 및 개발을 진행)



# 일정표

![image-20230131102542178](..\..\images\2023-01-30-CommunityMetaverse\image-20230131102542178.png)

- 10월 일정표 (같이 한 팀원의 이름은 비공개 처리)



![image-20230131102821653](..\..\images\2023-01-30-CommunityMetaverse\image-20230131102821653.png)

- 11월 일정표(같이 한 팀원의 이름은 비공개 처리)



# 구현

- 일단 회사 리소스랑 만들었던 것들은 전부 삭제를 해서 내가 기억하고 구현한 코드와 기능만 소개를 한다.



## 1. 행성 위로 걷게 하는 기능

> PlayerScript.cs
>
> - Awake 함수: 플레이어 메시와 카메라 피벗 정보를 가지고 온다.
> - Update 함수: 키보드 움직임 값 * 카메라 피벗의 Y축 각도 값 relativeMoveVector를 매 프레임마다 계산한다.
> - FixedUpdate 함수: Rigidbody로 플레이어를 움직이게 하고, relativeMoveVector으로 회전하게 한다.
> - RotatePlayer 함수: 플레이어 캐릭터 회전 함수
> - GetCurrentSpeed() 함수: 현제 속도를 구하는 함수


<details>
<summary>전체코드</summary>
<div markdown="1">

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Photon.Pun;
using Photon.Realtime;
using PlayFab;
using PlayFab.ClientModels;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class PlayerScript : MonoBehaviourPunCallbacks, IPunObservable
{
 /* 일부 코드 중략 */
    
    [SerializeField] private float walkSpeed = 10.0f; // 플레이어 걷는 속도
    private float runSpeed = 30.0f; // 플레이어 뛰는 속도
    
    private Transform playerMesh; // 캐릭터 모델
    private float turnSpeed = 5f; // 플레이어 회전 속도
   
    private Vector3 moveDirection; // 이동 방향
    private Quaternion RelativeMoveAngle; // 카메라 회전에 따른 회전값
    private Vector3 relativeMoveVector; // 카메라 회전에 따른 방향
    [SerializeField] public bool _IsRound = true; // 플레이어가 행성을 도는지 검사
    
    private Vector3 currPos = Vector3.zero;
    bool isRun; // 캐릭터가 달리는 상태인지 검사
    bool ismove;
    Vector3 curPos; // 리모트 플레이어의 위치 동기화를 부르럽게 하기 위해 사용
    private Quaternion currRot; // 리모트 플레이어의 회전 동기화를 부드럽게 하기 위해 사용

    private Transform cameraPivotTransform; // 카메라 피벗
    
     public bool IsRound
    {
        get { return _IsRound; }
        set { _IsRound = value; }
    }
    public bool MoveRound;
    
    #region Unity 관련 함수
    void Awake()
    {   
        // 자식으로 있는 카메라 피벗 오브젝트를 GET한다.
        cameraPivotTransform = transform.Find("CameraPivot");
        playerMesh = transform.Find("Mesh");
    }
    
    void Update()
    {
        if (IsRound && MoveRound){
            moveDirection = new Vector3(Input.GetAxisRaw("Horizontal"), 0, Input.GetAxisRaw("Vertical")).normalized;
                RelativeMoveAngle = Quaternion.AngleAxis(cameraPivotTransform.localRotation.eulerAngles.y, Vector3.up);
                relativeMoveVector = RelativeMoveAngle * moveDirection;
        }
    }
    
    void FixedUpdate()
    {
        GetComponent<Rigidbody>().MovePosition(GetComponent<Rigidbody>().position + transform.TransformDirection(relativeMoveVector * GetCurrentSpeed() * Time.fixedDeltaTime));

            if (relativeMoveVector != Vector3.zero)
            {
                RotatePlayer(relativeMoveVector);
                ismove = true;
            }
            else
                ismove = false;
    }
    #endregion
        
    // 플레이어 캐릭터 회전 함수
    void RotatePlayer(Vector3 moveVector)
    {

            Quaternion targetRotation = Quaternion.LookRotation(moveVector, Vector3.up);

            Quaternion newRotation = Quaternion.Lerp(playerMesh.localRotation, targetRotation, turnSpeed * Time.deltaTime);

            playerMesh.localRotation = newRotation;      
    }
    
    // 현재 속도를 구한다.
    // 반환값: float
    float GetCurrentSpeed()
    {
        return isRun ? runSpeed : walkSpeed;
    }
    
}
```
</div>
</details>



Awake 함수로 카메라 피벗 오브젝트를 가지고 온다.

```c#
void Awake()
    {   
        // 자식으로 있는 카메라 피벗 오브젝트를 GET한다.
        cameraPivotTransform = transform.Find("CameraPivot");
        playerMesh = transform.Find("Mesh");
    }
```



Update 함수는 플레이어가 움직인 방향 * 카메라 피벗 Y축 각도를 곱한 값 relativemovevector를 매 프레임마다 계산한다.

```c#
void Update()
    {
        if (IsRound && MoveRound){
            moveDirection = new Vector3(Input.GetAxisRaw("Horizontal"), 0, Input.GetAxisRaw("Vertical")).normalized;
                RelativeMoveAngle = Quaternion.AngleAxis(cameraPivotTransform.localRotation.eulerAngles.y, Vector3.up);
                relativeMoveVector = RelativeMoveAngle * moveDirection;
        }
    }
```



FixedUpdate 함수는 Rigidbody로 플레이어를 움직이게 하고, relativeMoveVector으로 회전하게 한다.

```c#
void FixedUpdate()
    {
        GetComponent<Rigidbody>().MovePosition(GetComponent<Rigidbody>().position + transform.TransformDirection(relativeMoveVector * GetCurrentSpeed() * Time.fixedDeltaTime));

            if (relativeMoveVector != Vector3.zero)
            {
                RotatePlayer(relativeMoveVector);
                ismove = true;
            }
            else
                ismove = false;
    }
```



RotatePlayer 함수는 플레이어를 movector 값만큼 회전하게 하는 함수이다.

```c#
// 플레이어 캐릭터 회전 함수
    void RotatePlayer(Vector3 moveVector)
    {

            Quaternion targetRotation = Quaternion.LookRotation(moveVector, Vector3.up);

            Quaternion newRotation = Quaternion.Lerp(playerMesh.localRotation, targetRotation, turnSpeed * Time.deltaTime);

            playerMesh.localRotation = newRotation;      
    }
```



GetCurrentSpeed 함수는 플레이어가 달라는 상태라면 runSpeed를 반환하고, 걷는 상태라면 walkSpeed를 반환한다.

```c#
// 현재 속도를 구한다.
    // 반환값: float
    float GetCurrentSpeed()
    {
        return isRun ? runSpeed : walkSpeed;
    }
```






## 2. JSON으로 파일 저장 및 불러오는 기능

> SaveBuilding.cs

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[Serializable]
public class PlaceObject
{
    public int index; // 오브젝트 인덱스
    public string name; // 오브젝트 이름
    public Vector3 pos; // 오브젝트 위치값
    public Quaternion rot; // 오브젝트 회전값
}


[Serializable]
public class SaveBuilding
{
    public List<PlaceObject> savePlace = new List<PlaceObject>();

}
```



> BuildingManager.cs

```c#
public List<GameObject> objectlist = new List<GameObject>(); // 스폰할 오브젝트 리스트

SaveBuilding savebuild = new SaveBuilding(); // json에 저장할 데이터
[SerializeField] protected string FileName; // 저장할 파일 이름;
protected string path; // 저장할 파일 경로
```



searchIndex 함수는 스폰할 오브젝트 리스트 중에서 target과 이름이 같은 오브젝트 인덱스를 찾는 함수이다. targetName에 (clone)을 제거한 이유는 스폰한 오브젝트의 이름에는 (Clone)이 앞에 붙기 때문이다.

```c#
// 오브젝트 인덱스를 찾는 함수
    int searchIndex(GameObject target) 
    {
        int result = -1;
        string targetName = target.name.Replace("(Clone)", "");

        foreach(var obj in objectlist)
        {
            result = result + 1;
            if (obj.name == targetName)
                break;
        }
        
        return result;
    }
```



GetSvaePosition, GetSaveRotation 함수를 가상함수로 선언한 이유는 행성 오브젝트와 가구 오브젝트끼리 저장하는 방식이 다르기 때문이다.

```c#
// 저장할 오브젝트의 Position을 반환하는 함수
    protected virtual Vector3 GetSavePosition(GameObject Go)
    {
        return Vector3.zero;
    }

    // 저장할 오브젝트의 Rotationd을 반환하는 함수
    protected virtual Quaternion GetSaveRotation(GameObject Go)
    {
        return Quaternion.identity;
    }
```



Save함수는 오브젝트의 인덱스, 이름, 위치값, 회전값을 저장한다.

```c#
// 꾸미기 오브젝트들을 저장하는 함수
    public void Save()
    {
        foreach(var go in GameObject.FindGameObjectsWithTag(ObjectTag))
        {
            PlaceObject placeobj = new PlaceObject();

            placeobj.index = searchIndex(go);
            placeobj.name = go.name;
            placeobj.pos = GetSavePosition(go);
            placeobj.rot = GetSaveRotation(go);

            savebuild.savePlace.Add(placeobj);

        }

        string json = JsonUtility.ToJson(savebuild);
        File.WriteAllText(path, json);
    }
```



Load 함수는 해당 json 파일이 있으면 저장된 데이터를 바탕으로 오브젝트를 배치하게 한다.

```c#
// 불러오기 함수
    void Load()
    {

        // 해당 파일을 불러온다.
        if (File.Exists(path))
        {
            string json = File.ReadAllText(path);
            SaveBuilding data = JsonUtility.FromJson<SaveBuilding>(json);
            savebuild = data;

            foreach(var loaddata in savebuild.savePlace)
            {
                SpawnObjectOtherPC(loaddata.index, loaddata.pos, loaddata.rot);
                photonView.RPC("SpawnObjectOtherPC", RpcTarget.OthersBuffered, loaddata.index, loaddata.pos, loaddata.rot);
            }

            savebuild.savePlace = new List<PlaceObject>();

        }

    }
```



##  3. 오브젝트 배치 기능

> BuildingManager.cs
>
> * Update 함수: 스폰된 오브젝트의 위치와 회전값을 계산하고, 오브젝트가 배치할 곳인지 체크하다.
> * FixedUpdate 함수: 마우스 커서의 위치값을 스폰한 오브젝트의 위치로 변경하도록 한다.
> * RotateObject 함수: 마우스 휠에 따라 오브젝트를 회전하게 하는 함수이다.
> * PlaceObject 함수: 오브젝트를 배치하는 함수이다.
> * SpawnObject 함수: index에 해당하는 objectlist에 있는 오브젝트를 스폰하는 함수이다.
> * SelectObject 함수: 오브젝트를 선택했을 때 호출하는 함수이고, 다른 사람 컴퓨터에는 해당 오브젝트를 제거한다.
> * DestroyObject 함수: 오브젝트를 파괴하는 함수이고, 다른 사람 컴퓨터에도 해당 오브젝트를 제거한다.
> * SetColliderChilds 함수: 해당 오브젝트의 모든 콜리더 컴포넌트의 IsTrigger 값을 설정하는 함수이다.
> * SetMaterials 함수: 해당 오브젝트의 모든 메쉬렌더러 컴포넌트의 color 값을 변경하는 함수이다.

```c#
public List<GameObject> objectlist = new List<GameObject>(); // 스폰할 오브젝트 리스트
public GameObject pendingObject; // 스폰된 오브젝트
[SerializeField] private float rotateAmount=0; //스폰할 오브젝트 회전값
private Vector3 pos; // 스폰할 위치

public bool CanPlace=true; // 해당 장소에 배치할 수 있는지 판단
protected int spawnIndex = 0; // 스폰할 오브젝트 인덱스

private RaycastHit hit;
[SerializeField] private LayerMask layermask ;
```

* Update 함수는 스폰된 오브젝트의 위치와 회전값을 계산하고, 오브젝트가 배치할 곳인지 체크하다.

```c#
	void Update()
    {
        if (pendingObject != null)
        {
            GetPosition(pos);
            pendingObject.transform.rotation = Player.transform.rotation;
            RotateObject();
            SetMaterialsChilds(CanPlace);

            if (Input.GetMouseButtonDown(0) && CanPlace)
                PlaceObject();
        }
    }
```

* FixedUpdate 함수는 마우스 커서의 위치값을 스폰한 오브젝트의 위치로 변경하도록 한다.

```c#
	private void FixedUpdate()
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);

        if (Physics.Raycast(ray, out hit, 1000, layermask))
        {
            pos = hit.point;
        }
    }
```

* RotateObject 함수는 마우스 휠에 따라 오브젝트를 회전하게 하는 함수이다.

```c#
	// 오브젝트 회전 함수
    void RotateObject()
    {
        rotateAmount -= Input.GetAxisRaw("Mouse ScrollWheel") * 45;
        Quaternion RotateObject = Quaternion.AngleAxis(rotateAmount, Vector3.up);
        GetRotation(RotateObject);
    }
```

* PlaceObject 함수는 오브젝트를 배치하는 함수이다.

```c#
	// 오브젝트를 배치하는 함수
    public void PlaceObject()
    {
        SetColliderChilds(false);
        SetMaterialsChilds(true);
        SetPlace();

        pendingObject = null;

    }
```

* SpawnObject 함수는 index에 해당하는 objectlist에 있는 오브젝트를 스폰하는 함수이다.

```c#
	// 오브젝트를 생성할 때 호출하는 함수
    public void SpawnObject(int index)
    {
        spawnIndex = index;
        pendingObject = Instantiate(objectlist[index], pos, transform.rotation);
        SetColliderChilds(true);
    }
```

* SelectObject 함수는 오브젝트를 선택했을 때 호출하는 함수이고, 다른 사람 컴퓨터에는 해당 오브젝트를 제거한다.

```c#
	// 오브젝트를 선택할 때 호출하는 함수
    public void SelectObject(GameObject selectobj)
    {
        pendingObject = selectobj;
        SetColliderChilds(true);
        photonView.RPC("DestroyObjectOtherPC", RpcTarget.OthersBuffered, GetSavePosition(pendingObject), pendingObject.name);
    }
```

* DestroyObject 함수는 오브젝트를 파괴하는 함수이고, 다른 사람 컴퓨터에도 해당 오브젝트를 제거한다.

```c#
	// 오브젝트를 파괴할 때 호출하는 함수
    public void DestroyObject(GameObject destroyobj) 
    {
        ObjectPayBack(destroyobj);
        photonView.RPC("DestroyObjectOtherPC", RpcTarget.OthersBuffered, GetSavePosition(destroyobj), destroyobj.name);

    }
```

* SetColliderChilds 함수는 해당 오브젝트의 모든 콜리더 컴포넌트의 IsTrigger 값을 설정하는 함수이다.

```c#
	// 자식들이 가지고 있는 콜리더의 IsTrigger 값을 변환 시키는 함수
    void SetColliderChilds(bool IsPlace)
    {

        Collider[] colliders = pendingObject.GetComponentsInChildren<Collider>();

        foreach(var col in colliders)
        {
            col.isTrigger = IsPlace;
        }

        colliders[0].isTrigger = true;

    }
```

* SetMaterials 함수는 해당 오브젝트의 모든 메쉬렌더러 컴포넌트의 color 값을 변경하는 함수이다.

```c#
	// 자식들이 가지고 있는 머테리얼 색상을 변환 시키는 함수
    void SetMaterialsChilds(bool Check)
    {
        MeshRenderer[] materials = pendingObject.GetComponentsInChildren<MeshRenderer>();

        foreach(var mat in materials)
        {
            mat.material.color = Check ? Color.white : Color.red;
        }

    }
```

* 행성과 가구 오브젝트에 따라 설치하는 방법이 달라서 가상함수로 설정했다.

```c#
	// 오브젝트를 설치하는 함수
    protected virtual void SetPlace() { }

    // 오브젝트 위치값을 얻는 함수
    protected virtual void GetPosition(Vector3 position) { }

    // 오브젝트 회전값을 얻는 함수
    protected virtual void GetRotation(Quaternion rotation) { }
```



# 참고자료

1. [행성 위를 걷는 방법1](https://youtu.be/cH4oBoXkE00)
2. [행성 위를 걷는 방법2](https://youtu.be/QvSC9kuK9J4)
3. [Third Person Movement - Unity](https://youtube.com/playlist?list=PLm9GTh6TKrHPUQcFdDotZknqrcZsq_Pgg)
4. [행성 및 집 꾸미기 기능 구현 방법](https://youtube.com/playlist?list=PLy354RIS_cauyVP4kRB_6JFly38DQjdCS)
5. [유니티 Json 파일 저장하거나 불러오는 방법1](https://lefthanddeveloper.tistory.com/15)
6. [유니티 Json 파일 저장하거나 불러오는 방법2](https://postiveground.com/etc/%EC%9C%A0%EB%8B%88%ED%8B%B0-json-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95/)
7. [유니티 Json 파일 저장하거나 불러오는 방법3](https://geojun.tistory.com/65)
8. [Photon 채팅 프로그램 ](https://hyokim.tistory.com/3)
9. [자주 사용하는 Photon 코드](https://wmmu.tistory.com/entry/%EC%9E%90%EC%A3%BC-%EC%93%B0%EB%8A%94-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%8F%AC%ED%86%A4-%EC%BD%94%EB%93%9C)
10. [유니티 포톤PUN2 서버개발](https://youtube.com/playlist?list=PL3KKSXoBRRW3YE4UMnRH762vOhSHLdnpK)

