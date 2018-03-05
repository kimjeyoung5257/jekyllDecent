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
간단하게 사진으로 설명을 해보면
<figure>
   <img src="{{ "/media/img/CLEVER.jpg" | absolute_url }}" />
   <figcaption>https://cs.stanford.edu/people/jcjohns/clevr/</figcaption>
</figure>

