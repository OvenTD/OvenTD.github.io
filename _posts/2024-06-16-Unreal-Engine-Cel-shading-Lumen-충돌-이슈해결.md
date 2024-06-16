---
layout: post
title: 언리얼 Cel shading 과 Lumen 충돌 해결
subtitle: 언리얼 Cel Shading R&D 과정 중 발생한 Cel Shading과 Lumen 충돌에 대한 고민
categories: issue
tags: [Issue]
---

작품에 사용될 언리얼엔진의 셀 쉐이딩(카툰렌더링)을 리서치 해보다 우리 작품에 적용가능한 상용 에셋이 없다고 판단함.
하여 Post Process Material을 제작하기 시작함.

아래는 제작하던 도중 발생한 문제이다.

### 상황: Lumen을 키면 셀쉐이딩에 노이즈가 생김.
- Lumen
![image](https://github.com/OvenTD/OvenTD.github.io/assets/155340997/7816d644-81de-43da-b6e9-e0df926331d6)

- None
![image](https://github.com/OvenTD/OvenTD.github.io/assets/155340997/54b98905-3fe6-4036-8b4a-116643f99767)

해당 이슈는 루멘 라이팅이 적용된 라이팅 데이터를 가지고 Cel Shader로 분리하면서 생기는 듯함.

### 해결방법

Lumen으로 만들어지는 라이팅 퀄리티를 포기할 수 없다면, Lumen을 킨 상태로 배경 렌더를 진행하고, 캐릭터만 Lumen을 끈 상태로 렌더.

그 후 합성과정에서 합침

Lumen을 포기한다면 Lumen을 끈 상태로 진행.
