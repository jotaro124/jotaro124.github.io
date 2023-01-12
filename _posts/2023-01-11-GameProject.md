---
layout: single
title: "홀짝 게임 포트폴리오"
categories: assignment
toc: true
author_profile: true
sidebar:
     nav: "docs"
---



# 프로젝트 소개

기말고사 대체과제로 제작한 홀짝 게임

- 게임명: Odd&Even RPG
- 엔진: Unity 엔진 (2020.1.4f1)
- 에디터: Microsoft Visual Studio Community 2019(16.7.3 버전)
- 개발기간: 2022.05.31 ~ 2022.06.08
- 개발규모: 1인 개발



# 게임 기획



## 1. 기획 의도

![image-20230112154755868](..\..\images\2023-01-11-GameProject\image-20230112154755868.png)

  운으로 승패를 결정하는 게임은 단순하지만 스릴과 긴장감을 준다. 그래서 유저에게 간단하지만 게임에 몰입을 주는 홀짝 게임을 제작함.



## 2. 흐름도

![image-20230111171726282](..\..\images\2023-01-11-GameProject\image-20230111171726282.png)

- 랜덤으로 적 캐릭터가 선택된다.
- 홀짝을 선택해서 같은 수가 나오면 적이 피해를 입고, 다른 수가 나오면 플레이어가 피해를 입는다.
- 적의 체력이 0이 되면 플레이어의 승리로 다음 적과 대전을 한다.
- 플레이어 체력이 0이 될 때까지 게임을 반복한다.



# 구현

## 1. GameManager

- 플레이어 선택이 정답인지 오답인지 판단
- 플레이어나 적의 사망 처리

<details>
<summary>전체코드</summary>
<div markdown="1">


```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{

    public GameObject ClearPanel; // 게임 클리어 화면
    public GameObject OverPanel; // 게임 오버 화면
    public GameObject SelectButton; // 홀짝 버튼
    public GameObject ResultDraw; // 홀짝 결과 화면
    public GameObject ResultWin; // 플레이어  총 승리 횟수 표시
    public GameObject HitEffect; // 데미지 이펙트

    public Text PlayerHPText; // 플레이어 체력 텍스트
    public Text EnemyHPText; // 에너미 체력 텍스트
    public Text WinCountText; // 플레이어 승리 횟수 텍스트

    Text ResultWinCount; // 플레이어 총 승리 횟수

    Player player; // Player 클래스
    Enemy enemy; // Enemy 클래스

    // 플레이어 승리 횟수
    int WinCount = 0;

    // Start is called before the first frame update
    void Start()
    {
        player = GameObject.Find("Player").GetComponent<Player>();
        enemy = GameObject.Find("Enemy").GetComponent<Enemy>();
        ResultWinCount = ResultWin.GetComponentsInChildren<Text>()[1];
    }

    // Update is called once per frame
    void Update()
    {
        PlayerHPText.text = player.GetHP().ToString();
        EnemyHPText.text = enemy.GetHP().ToString();
        WinCountText.text = GetWinCount().ToString();

    }

    // OddBtn을 클릭했을 때 발동하는 함수
    public void OddClicked()
    {
        // 홀수이면 1
        Draw(1);
    }

    // EvenBtn을 클릭했을 때 발동하는 함수
    public void EvenClicked()
    {
        // 짝수이면 0
        Draw(0);
    }

    
    void Draw(int input)
    {
        // 랜덤으로 나온 숫자
        int result = -1;

        // 랜덤으로 나온 숫자가 홀수인지 판별
        result = ResultOddEven();

        // input와 result가 같으면 주인공 승리
        if (input == result)
        {
            Invoke("PlayerCurrect",1f);
        }
        else
        {
            Invoke("PlayerFail", 1f);
        }

    }


    void OnTurnResult()
    {
        ResultDraw.SetActive(true);
        SelectButton.SetActive(false);

    }

    void OffTurnResult()
    {
        ResultDraw.SetActive(false);
        SelectButton.SetActive(true);
    }

    // 플레이어가 정답을 맞췄을 때 발동하는 함수
    void PlayerCurrect()
    {
        // 적이 대미지를 입는다.
        enemy.Hurt(1);
        OffTurnResult();

    }

    // 플레이어가 정답을 틀렸을 때 발동하는 함수
    void PlayerFail()
    {
        // 주인공이 대미지를 입는다.
        player.Hurt(1);
        OffTurnResult();
    }

    // 플레이어 사망
    public void GameOver()
    {
        OverPanel.SetActive(true);
        ResultWinCount.text = GetWinCount().ToString();

    }

    // 에너미 사망
    public void GameClear()
    {
        WinCount = WinCount + 1;
        ClearPanel.SetActive(true);
        ClearPanel.GetComponent<ClearPanel>().NextEnemy();

        player.Recovery();
        enemy.Recovery();

    }

    // 플레이어 승리 횟수를 얻는 함수
    public int GetWinCount() { return WinCount; }

    // 홀수인지 짝수인지 표기하는 함수
    int ResultOddEven()
    {
        if (Random.Range(0, 6) % 2 != 0)
        {
            ResultDraw.GetComponent<Text>().text = "홀";
            OnTurnResult();
            return 1;
        }
        else
        {
            ResultDraw.GetComponent<Text>().text = "짝";
            OnTurnResult();
            return 0;
        }

    }

    // 메인 화면으로 이동하게 하는 함수
    public void LoadTitle()
    {
        SceneManager.LoadScene("TitleScene");
    }

    // 게임을 다시 동작시키는 함수
    public void RetryGame()
    {
        SceneManager.LoadScene("GameScene");
    }


    // 적의 선택이 확인되면 발동하는 함수
    public void NextGame()
    {
        ClearPanel.SetActive(false);
        ClearPanel.GetComponent<ClearPanel>().NextBtnOff();
        enemy.SetEnemyImage(ClearPanel.GetComponent<ClearPanel>().GetSprite());

    }

    public void ShowHitEffect()
    {

        HitEffect.SetActive(true);
        Invoke("HitEffectExe", 0.1f);
    }

    void HitEffectExe()
    {
        HitEffect.SetActive(false);
    }


}
```

</div>
</details>

> 플레이어 선택이 정답인지 오답인지 판단하는 코드

```c#
	// 홀 버튼을 클릭했을 때 발동하는 함수
    public void OddClicked()
    {
        // 홀수이면 1
        Draw(1);
    }

    // 짝 버튼을 클릭했을 때 발동하는 함수
    public void EvenClicked()
    {
        // 짝수이면 0
        Draw(0);
    }

	void Draw(int input)
    {
        // 랜덤으로 나온 숫자
        int result = -1;

        // 랜덤으로 나온 숫자가 홀수인지 판별
        result = ResultOddEven();

        // input와 result가 같으면 주인공 승리
        if (input == result)
        {
            Invoke("PlayerCurrect",1f);
        }
        else
        {
            Invoke("PlayerFail", 1f);
        }

    }

	// 플레이어가 정답을 맞췄을 때 발동하는 함수
    void PlayerCurrect()
    {
        // 적이 대미지를 입는다.
        enemy.Hurt(1);
        OffTurnResult();
    }

    // 플레이어가 정답을 틀렸을 때 발동하는 함수
    void PlayerFail()
    {
        // 주인공이 대미지를 입는다.
        player.Hurt(1);
        OffTurnResult();
    }
```



## 2. EnemySelector

- 적 스프라이트들을 랜덤으로 선택한다.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class EnemySelector : MonoBehaviour
{

    private Image rend; // 에너미 이미지

    // 랜덤 사이드 인덱스
    int randomSide = 0;

    public Sprite[] EnemySides; // 에너미들의 스프라이트를 저장
    public GameObject NextButton; // 게임으로 넘어가는 버튼



    // Start is called before the first frame update
    void Start()
    {
        rend = GetComponent<Image>();
        rend.sprite = EnemySides[0];
        StartCoroutine("RollTheSide");
    }

    public void NextBattle()
    {
        StartCoroutine("RollTheSide");
    }

    // 적의 스프라이트를 움직이게 하는 함수
    IEnumerator RollTheSide()
    {

        for (int i =0; i<=15; i++)
        {
            randomSide = Random.Range(0, 12);
            rend.sprite = EnemySides[randomSide];
            yield return new WaitForSeconds(0.2f);
        }

        NextButton.SetActive(true);

    }

    public Sprite GetSprite()
    {

        return EnemySides[randomSide];

    }

}
```



## 3. Player

- 플레이어의 체력을 관리

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player : MonoBehaviour
{

    // 최대 체력
    public int MaxHP = 10;

    // 현재 체력
    int currentHP = 0;

    // 게임 매니저
    GameManager mng;

    // 현재 체력 반환
    public int GetHP()
    {
        return currentHP;
    }

    // 현재의 체력을 회복하는 함수
    public void Recovery()
    {
        currentHP = MaxHP;
    }

    // 데미지를 받는 함수
    public void Hurt(int Damage)
    {
        currentHP = currentHP - Damage;
        mng.ShowHitEffect();
        if (currentHP <= 0)
        {
            // 플레이어 사망 처리
            mng.GameOver();
        }
    }

    // Start is called before the first frame update
    void Start()
    {
        // 게임 메니저를 불러온다.
        mng = GameObject.Find("GameManager").GetComponent<GameManager>();

        currentHP = MaxHP;

    }

}
```



# 참고자료

1. 모리 요시나오, 유니티를 몰라도 만들 수 있는 유니티 2D 게임 제작, 영진닷컴, 2021
2. 지국환,  C# 초보자를 위한 유니티 게임개발 스타트업. 비엘북스, 2014
3. 유니티 UI: [골드메탈 유튜브](https://youtu.be/N4PLRkupABM)
4. 적 스프라이트 랜덤으로 표시하는 기능: [https://youtu.be/W8ielU8iURI](https://youtu.be/W8ielU8iURI)
