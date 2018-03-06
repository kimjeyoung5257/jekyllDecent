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
   <figcaption>CLEVER dataset</figcaption>
</figure>

위의 이미지는 관계추론의 모델에서 쓰이는 [CLEVER](https://cs.stanford.edu/people/jcjohns/clevr/) 데이터셋의 일부입니다. 우리가 일반적인 분류문제나 Detection 을 풀고자 한다면 원통은 몇 개, 빨간 색 구의 여부,
등을 파악할 수 있을 것입니다. 하지만 이러한 경우 "파란색 정육면체 뒤에 있는 구는 어떠한 색을 가지고 있니?" 라는 질문은 해결 할 수 없습니다.
하지만 관계추론 모델의 경우 어떠한 환경(이미지) 에서 존재하는 객체들(ex, 구, 직육면체, 정육면체)과의 상호적인 관계를 따져 답을 유추하는 모델입니다.
그럼 이제부터 모델의 구조를 보면서 하나씩 살펴보겠습니다!

<figure>
   <img src="{{ "/media/img/RN.png" | absolute_url }}" />
   <figcaption>출처: https://arxiv.org/pdf/1706.01427.pdf</figcaption>
</figure>

위의 구조가 논문에서 제시한 모델의 구조입니다. 이 구조는 크게 CNN, LSTM, RN 이며 RN 을 이용해서 object 들의 관계를 나타냅니다. RN 에 대한 설명은 잠시 뒤로 미루고 인풋레이어부터 살펴보죠!
visual QA 의 경우 input 으로 필요한 것은 무엇일까요? 모델의 구조에서 볼 수 있듯이 이미지와 이미지에서 물어보고 싶은 질문이 될 것입니다. <br>

### CNN

논문에 따르면 CNN 은 총 4개의 convolutional layer를 가지는데 각각의 convolutional layer 는
convolution + ReLu non-linearity + batch normalization 으로 이루어져 있습니다. Max-pooling 은 사용하지 않았는데 논문에서 언급하지는 않았지만 이유를 생각해보자면
이미지에서 존재하는 각각의 object 간의 positional relation 를 가지고 가야 하기 때문이고 Max-pooling 을 이용하게 되면 FOV(Field Of View)를 넓힐 수 는 있지만
그에따른 object 의 다양한 정보들의 손실이 불가피하기 때문에 pooling을 사용하지 않은 것 같습니다. 다시 모델의 구조를 보면 4번의 convolutional layer를 거치게 될 경우
$$ d \times d $$ 의 크기를 가지는 $$ k $$ 개의 feature map 이 형성되어집니다. <br>
그렇다면 이 feature map 에서 어떻게 object 를 선택할까요.
모델구조중 Final CNN feature maps 에서 볼 수 있듯이 하나의 sub-object 는 feature map 의 같은 위치의 값들의 벡터로 이루어 집니다. 위의 구조에서는 빨, 파, 노 로 이루어진 벡터들이겠죠?
각각의 object 들은 texture, conjunction of physical objects, background 등을 나타낼 수 있습니다. 이렇게 object 들을 선택하게 되면 각각의 object 들간의 상호 공간적인 관계는 표현하지 못하더라도
object 의 location 정보와 좀 더 다양한 object 의 특성들을 가지고 갈 수 있게 됩니다. <br>

### LSTM

<figure>
   <img src="{{ "/media/img/lstm.png" | absolute_url }}" />
   <figcaption>lstm 구조</figcaption>
</figure>

LSTM 의 경우 논문에서는 final state 만을 이용합니다. 여기서는 LSTM 에 대해서 설명하지는 않겠습니다. LSTM 의 인풋으로는 lookup table 의 trainable word vector 입니다. 그림에서 알 수 있듯이 time-step 마다
word 단위의 vector 가 input으로 이용이 되며 최종적인 state 의 output 값과 CNN 의 구조의 object vector 의 조합으로 RN 네트워크를 형성합니다.

### RN

이제 이 논문의 핵심인 RN 에 대해서 알아보겠습니다. RN 의 구조는 굉장히 simple 하지만 좋은 성능을 보였습니다. 제가 생각하기에 RN을 간단히 나타내자면 object 조합에 대한 fully-connected 라고 생각합니다.
CNN을 통해서 나온 object의 invariant properties 와 LSTM을 통해서 나온 질문에 대한 output 값을 사용해서 object 들간의 관계추론을 학습합니다. 그럼 RN 의 수식을 한 번 볼까요?

$$ RN(O) = f_\phi (\sum_{i,j} g_\theta(o_i, o_j) ) $$



