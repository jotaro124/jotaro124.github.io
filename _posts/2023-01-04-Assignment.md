---
layout: single
title: "단간론파 슈팅 게임"
categories: assignment
toc: true
sidebar:
    nav: "docs"
---



# 프로젝트 소개

기말고사 대체과제로 제작한 슈팅게임. 그리고 단간론파 게임의 특징을 넣어서 제작

- 게임명: 단간론파 슈팅 게임
- 엔진: Unity 엔진 (2020.1.4f1)
- 에디터: Microsoft Visual Studio Community 2019(16.7.3 버전)
- 제작기간: 2020.12.14 ~ 2020.12.22
- 개발 규모: 1인 개발

* Demo: [https://affectionate-raman-ead1fb.netlify.app/](https://affectionate-raman-ead1fb.netlify.app/)



# 게임 기획



## 1. 기획 의도

![OpenSouceFinalTest4](..\..\images\2023-01-04-Assignment\OpenSouceFinalTest4.png)



  위 그림같이 기본적인 게임 디자인을 받고, 이것을 바탕으로 자신의 소재를 넣어서 게임을 만드는 것이 과제의 목표이다.

그래서 소재를 찾던 중 단간론파라는 게임에 포함된 미니 게임 패닉 토크 액션(PTA)을 떠올랐고 해당 게임을 소재로 하기로 했다.



> ## 패닉 토그 액션 (PTA)

[![썸네일](http://img.youtube.com/vi/BAJmKaaQs08/0.jpg)](https://youtube.com/playlist?list=PLMv-mL-Ks7-u4r-km2S14PjE-68CzPWWL)

* 상대방의 발언들을 파괴하고 최후의 발언을 논파했을 때 클리어하는 게임



## 2. 흐름도



![Flowchart](..\..\images\2023-01-04-Assignment\FlowChart2.png)

* 주인공은 정해진 체력이 있다
* 적은 주인공을 찾아내서 공격한다.
* 공격을 받으면 체력이 줄어든다.
* 블록들을 전부 파괴하면 최후의 발언이 나온다.
* 최후의 발언을 파괴하면 게임 클리어
* 주인공 체력이 0이 되거나 최후의 발언을 파괴하지 못할 경우 게임 오버



# 구현



## 1. 블록 자동 배치

- 블록의 위치를 표시하는 Position 클래스를 선언한다.
- 블록들의 위치를 저장하는 배열을 선언한다.
- 블록을 스폰하는 함수를 선언하고, Start 함수에서 호출한다.

>## InitSetting.cs

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


public class InitSetting : MonoBehaviour
{
    
    /* 일부 코드 생략 */

    public GameObject block; // 스폰할 블록
    private int BlockCount= 12; // 블록 갯수
    
    class Position {
        public float X;
        public float Z;
        public Position(float X, float Z) {
            this.X = X;
            this.Z = Z;
        }
    }
    
    Position[,] BlockPos = new Position[3,4] {
    { new Position(-10f,15f),new Position(-4f,15f),new Position(2f,15f),new Position(8f,15f)},
    { new Position(-10f,6.5f),new Position(-4f,6.5f),new Position(2f,6.5f),new Position(8f,6.5f)},
    { new Position(-10f,-2f),new Position(-4f,-2f),new Position(2f,-2f),new Position(8f,-2f)}
    };
    
    void Start()
    {
        blockInit();
    }
    
    void blockInit() {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 4; j++)
            {
                Vector3 Pos = new Vector3(BlockPos[i, j].X, 1f, BlockPos[i, j].Z);
                GameObject obj = Instantiate(block,Pos , Quaternion.identity) as GameObject;
            }
        }
    }
    
}

```





## 2. 에너미 AI

- 블록의 갯수가 0이 되거나 플레이어 체력이 0이 될 때까지 플레이어를 공격한다.
- 움직이다가 균형을 무너지는 것에 대비해서 자세를 바로잡는 rePosition 함수를 선언한다.
-  타겟과의 위치 차가 0보다 크면 공격을 한다.
- 3초 후 움직임을 변경하게 한다.

> ## Enemy.CS

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Enemy : MonoBehaviour
{
    public int Speed = 5;
    public GameObject Bullet;
    public GameObject Target;
    public Transform FirePos;

    int movementFlag = 0;
    InitSetting ins;
    int BlockCount;
    Player player;

    private float attackCoolTime;
    private float timer;
    
    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine("ChangeMovement");
        attackCoolTime = 1f;
        timer = 0f;
        ins = GameObject.Find("SettingMap").GetComponent<InitSetting>();
        player = GameObject.Find("Player").GetComponent<Player>();
    }

    // Update is called once per frame
    void Update()
    {
        timer += Time.deltaTime;
        BlockCount = ins.getBlockCount();

        if(BlockCount!=0&&player.getHP()>0)
        {
            Move();
            rePosition();
            float difer = Target.transform.position.x - transform.position.x;
            if (difer > 0)
            {
                if (timer >= attackCoolTime)
                {
                    Shot();
                    timer = 0f;
                }
            }
        }
    }

    void rePosition()
    {
        if (transform.rotation != Quaternion.identity)
        {
            transform.localPosition = new Vector3(0.0f, 2.0f, 21.0f);
            transform.localRotation = Quaternion.identity;
        }
    }

    void Move()
    {
        if (movementFlag == 1)
        {
            transform.Translate(Vector3.right * Speed * Time.deltaTime);

        }
        else if (movementFlag == 2) {
            transform.Translate(Vector3.left * Speed * Time.deltaTime);
        }
    }

    IEnumerator ChangeMovement()
    {
        movementFlag = Random.Range(0, 3);
        yield return new WaitForSeconds(3f);
        StartCoroutine("ChangeMovement");

    }

    void Shot()
    {
        GameObject obj = Instantiate(Bullet, FirePos.transform.position, FirePos.transform.rotation) as GameObject;
        Physics.IgnoreCollision(obj.GetComponent<Collider>(), GetComponent<Collider>());
    }
}
```



# 참고자료

1. 배경음: https://youtube.com/playlist?list=PLU4ktq2pWONtJ7o5pLZyWJc88nttIzMKx
2. 모노쿠마 음성: https://youtu.be/82_XOtle9Tg
3. 나에기 음성: https://youtu.be/_TnOwKGcsyw
4. 유니티 UI: [골드메탈 유튜브](https://youtu.be/N4PLRkupABM)
5. 지국환,  C# 초보자를 위한 유니티 게임개발 스타트업. 비엘북스, 2014
