---
layout:            post
title:             "Relational reasoning"
date:              2017-08-10 13:10:00 +0300
tags:              A simple neural network module for relational reasoning ( Adam Santoro et al., 2017 )
category:          Deep Learning paper review
author:            kimjeyoung
math:              true
---

오늘 리뷰할 논문은 딥마인드에서 2017년에 발표한 Relatinal Network 입니다. 모델의 architecture 를 살펴보기 전에 관계추론이 무엇인지에 대해 가볍게
살펴보겠습니다. 우리가 분류문제를 해결하고자 할 때와 달리 관계추론(Relational reasoning)은 상호 object들과의 관계를 살피고 답을 유추하는 것이 목적입니다.

<figure>
   <img src="{{ "/media/img/CLEVER.jpg" | absolute_url }}" />
   <figcaption>[CLEVER dataset](https://cs.stanford.edu/people/jcjohns/clevr/)</figcaption>
</figure>

위의 이미지는 관계추론의 모델에서 쓰이는 CLEVER 데이터셋의 일부입니다. 우리가 일반적인 분류문제나 Detection 을 풀고자 한다면 원통은 몇 개, 빨간 색 구의 여부,
등을 파악할 수 있을 것입니다. 하지만 이러한 경우 "파란색 정육면체 뒤에 있는 구는 어떠한 색을 가지고 있니?" 라는 질문은 해결 할 수 없습니다.
하지만 관계추론 모델의 경우 어떠한 환경(이미지) 에서 존재하는 객체들(ex, 구, 직육면체, 정육면체)과의 상호적인 관계를 따져 답을 유추하는 모델입니다.
