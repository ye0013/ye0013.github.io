---
title: Pattern Recognition - 1.Introduction
description: Introduction to Pattern Recognition
categories:
 - pattern recognition
tags: pattern recognition
---

> 패턴 인식의 핵심적인 내용은 `특징`과 `분류`라는 두 가지로 구성된다.

## 패턴 인식 처리과정
패턴 -> `특징` -> `분류` -> 부류

이제부터 박보검 얼굴을 인식하는 모델을 만듭시다. 우리는 사진에서 쳐진 눈, 날렵한 코, 갸름한 얼굴형 등을 보고 박보검이라 판단할 수 있겠죠. 이 때에는 눈, 코, 입 외에도 여러가지 `특징` <sup>`feature`</sup> 을 사용할 수 있습니다.


그럼 사진을 보았을 때 "박보검은 눈이 쳐지고, 얼굴이 갸름하다는 패턴이 있는데 이 사진의 사람을 눈이 찢어졌고 얼굴이 퉁퉁하네? 그럼 박보검이 아니네" 와 같은 판단을 할 수 있겠죠. 이렇게 특징들에 주목하고, 각 특징이 갖는 값의  패턴을 이미 알고 있으니 의사 결정, 즉 `분류` <sup>`classification`</sup> 를 할 수 있습니다.

