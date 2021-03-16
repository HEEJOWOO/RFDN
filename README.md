# RFDN : https://arxiv.org/abs/2009.11551

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Residual Feature Distillation Network for Lightweight Image Super-Resolution
----------------------------------------------------------------------------

Abstract
--------
  * CNN을 기반으로한 SISR의 많은 성공에도 불구하고 많은 계산량으로 Real-Time, 저전력 컴퓨터에 적용하는데 무리
  * 위와 같은 문제를 해결하기 위해 빠르고 경량화된 네트워크들이 제안됨
  * 대표적으로 information distillation Net은 Channel split을 이용하여 distilled특징들을 추출하였음
  * 하지만 Channel Split과 같은 방법은 어떻게 효과적으로 SISR에 도움을 주는지 명백하지 못함
  * 제안하는 Net은 Channel split과 동등하게 기능적으로 발휘하는 더 경량적이고 flexible한  feature distillation connection(FDC)을 제안
  * residual feature distillation net(RFDB)을 제안하며, RFDN은 다수의 feature distillation connection을 이용해 더 많은 차별화된 특징을 표현 할 수 있게 됨 
  * 또한 RFDN의 메인 블록인 shallow residual blcok(SRB)를 제안하여 경량적이며 잔여 학습으로부터 더 효과적으로 얻을 수 있는 네트워크를 만들었음 

Introduction
------------
  * 깊은 네트워크가 좋은 성능을 만들 수는 있지만 실시간에 적용하기에 많은 무리가 있음(VDSR 20 layer, EDSR 160 layer), 빠르고 경량화된 CNN모델을 만드는게 중요
  * 재귀적인 방법을 이용하여 효과적으로 파라미터의 수를 줄임(DRCN, DRRN), 하지만 재귀의 손실을 막기 위해 네트워크의 깊이나 넓이는 늘려서 사용, 이런 모델은 연산량과 추론 시간을 늘리는 대신 모델 크기를 줄임, 실시간에서 수행된, SR 모델들은 연산량 또한 중요한 요소 중 하나임
  * cascading network 구조를 이용하여 모바일 장치에서도 이용 가능한 CARN-M모델 제안, 하지만 PSNR이 많이 하락
  * information distillation network(IDN)의 등장, 중간 특징을 이용하여 2개의 부분으로 나누어 사용, 하나는 retain 또 다른 하나는 refine에 사용, information distillation network를 업그레이드한 information multi distillation network(IMDN)는 IMDB내에서 다수의 split을 사용함, 하지만 경량화된 네트워크들 사이에서 비교하게 되면 많은 수의 파라미터의 수를 가지고 있음 
  * 기존 information distillation mechanism에 대해서 분석을 하여 더 경량화되고 효율적인 feature distillation connection(FDC)를 제안 
  * 기존 IMDN 구조를 다시 생각하여 residual feature distillation network를 제안하고 IMDN과 비교하였을 때 feature distillation connection을 사용 하게 되면 더 경량화 되는 것을 확인 할 수 있음 
  * shallow residual block을 제안하여 SR성능을 향상 시켰음, residual 구조와 하나의 Conv, 하나의 activation으로 구성, 추가적인 파라미터 없이 잔여 학습으로부터 효과를 더 받을 수 있음 
  
Contribution
------------
  * 빠름, 정확, 경량 residual feature distillation network를 제안
  * information distillation mechanism을 분석하여 feature distillation connection을 제안
  * shallow residual block(SRB)를 제안, residual을 이용하여 더 좋은 SR성능을 만듦

information multi distillation block
------------------------------------
![image](https://user-images.githubusercontent.com/61686244/111261045-2f668980-8665-11eb-9182-6c25b575037d.png)

![image](https://user-images.githubusercontent.com/61686244/111261059-355c6a80-8665-11eb-9495-6482b5b7bfae.png)

  * 그림 2의 (a)는 information distillation block, 3x3 Conv를 이용하는 다수의 후속 distillation 단계를 통해 특징을 추출
  * 각 distillation 단계에서 split을 이용, 두 개로 나눠짐, 하나는 retain, 다른 하나는 refine으로 다음 distillation 단계로 넘어감
  * block의 입력을 이라 하였을 때 수식 1과 같이 block을 수식화 할 수 있음 
  
![image](https://user-images.githubusercontent.com/61686244/111261109-473e0d80-8665-11eb-9656-305f90a87f68.png)
  
Rethinking the IMDB(IMDB-R)
---------------------------
  * IMDB의 PRM구조는 효과적으로 개선은 하였지만 split operation으로 충분한 효율을 가지고 있지 않고, 유연성이 부족함 distilled 특징들은 불필요한 파라미터를 가지고 있는  3x3 Conv를 통해서 만들어짐
  * feature refinement는 split operation으로 이루어짐, 이는 identity mapping(입력 값을 그대로 전달하는 것)을 사용하는데 한계를 가지고 있음
  * 위와 같은 문제를 해결하기 위해 기존 IMDB의 PRM에 대해서 재구성을 하여 제안
  * 3x3+split을 3x3, 3x3 두 개로 사용(DL, RL) 
  * DL : distilled features  / RL : refinement layer
  * 수식 3, 수식 4와 같은 관계를 가질 수 있음 
  
![image](https://user-images.githubusercontent.com/61686244/111261170-5cb33780-8665-11eb-8249-a52d9dde15b2.png)

![image](https://user-images.githubusercontent.com/61686244/111261182-60df5500-8665-11eb-90ed-070e620c30d5.png)
  
  * 즉 split은 동시에 작용하는 두 개의 Conv로 나타낼 수 있음(두 개의 Conv를 사용함으로 써 identity connection이 가능해지고 distilled features이 불필요한 3x3 Conv를 통과하지 않아도 됨) 

Residual feature distillation block
-----------------------------------
  * IMDB-R을 개선한 더 경량화하고 강력한 residual distillation block, RFDB를 제안
  * 기존 IMDB는 ratio를 주어 split하였음, 하지만 RFDB는 split보다 더 효율적으로 채널을 감소할 수 있는 1x1 Conv를 사용(왼쪽 라인), 파라미터를 감소하는데 도움
  * 3x3 Conv를 사용(오른쪽 라인), RFDB의 body 부분에 해당, refine 특징을 더 잘 표현 
  * shallow residual block을 제안, 3x3 conv, identity connection, activation unit으로 구성, SRB는 추가적인 파라미터 없이 잔여 학습으로부터 이득을 볼 수 있음

Framework of RFDN 
-----------------
  * 1) the first feature extraction convolution
  * 2) multiple stacked residual feature distillation blocks(RFDBs)
  * 3) the feature fusion part
  * 4) the last reconstruction block

  * 처음에 3x3 Conv를 이용하여 coarse features를 LR로부터 추출
  
![image](https://user-images.githubusercontent.com/61686244/111261292-9ab05b80-8665-11eb-884f-d7bc5f3730e0.png)

  * RFDBs는 추출된 특징을 점차 refine하는 역할을 가지고 있음 
  
![image](https://user-images.githubusercontent.com/61686244/111261318-a439c380-8665-11eb-8e2c-2c796a06314a.png)

  * 각 RFDB에서 나온 특징들은 마지막에 concat됨, 1x1, 3x3 conv 통과
  
![image](https://user-images.githubusercontent.com/61686244/111261343-adc32b80-8665-11eb-98b4-b33ef7b74fbc.png)

![image](https://user-images.githubusercontent.com/61686244/111261347-b156b280-8665-11eb-8b2e-78e7314bdc9f.png)

![image](https://user-images.githubusercontent.com/61686244/111261361-b9165700-8665-11eb-9858-3218f3556b1a.png)

Experiments
-----------
  * Train : DIV2K
  * Test : Set5, Set14, Bsd100, Urban100, Manga109
  * RFDN channel number : 48 / RFDN-L channel number :52 / RFDB 사용 횟수 : 6

Ablation study
--------------
  * 제안된 feature distillation connection(FDC), shallow residual block(SRB)의 성능을 확인 해보기 위한 실험
  
![image](https://user-images.githubusercontent.com/61686244/111261440-d64b2580-8665-11eb-89c9-532b6286da07.png)

![image](https://user-images.githubusercontent.com/61686244/111261452-da774300-8665-11eb-8575-27acdeb1a1a0.png)

![image](https://user-images.githubusercontent.com/61686244/111261462-df3bf700-8665-11eb-9fd8-eeeedb4a4046.png)

![image](https://user-images.githubusercontent.com/61686244/111261478-e400ab00-8665-11eb-8da2-1d80c2b9e065.png)

Conclusion
----------
  * 기존 information distillation mechanism의 분석을 통해 새로운 방식을 제안 
  * IMDN의 구조를 변형하여 feature distillation conneions(FDC)를 제안, 더 경량적이고 유연함
  * SR의 성능을 올리기 위해 identity connection 과 한 개의 Conv로 이루어진 shallow residual block(SRB)를 제안
  * SRB와 FDC로 구성된 residual feature distillation new(RFDN)을 제안

