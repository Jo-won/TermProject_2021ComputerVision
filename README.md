# TermProject_2021ComputerVision
This is the purpose of the term project of class Computer Vision in Sejong Univ.

## Challenge Overview

  본 챌린지는 Temporal Action Proposal task를 다룹니다.
  Temporal Action Proposal이란 video에서 사람의 action이 나타나는 영역을 찾아내는 task로, 아래 그림과 같이 특정 action이 비디오에서 나타나는 시작 시간과 끝 시간을 찾아내는 것이 목표입니다.
  <br>
  <br>
  <br>
  ![image](https://user-images.githubusercontent.com/46413594/137617481-0a73c4c1-c492-4127-b672-5eb9ff268cb1.png)


## Dataset

  데이터 셋으로는 ActivityNet-v1.3을 사용합니다.   
  ActivityNet-v1.3 데이터 셋은 Google에서 매년 관련 task로 ActivityNet Challenge를 열 때 사용하는 데이터 셋이며, 여러 종류의 action을 포함하고 있습니다.  
  ActivityNet-v1.3 데이터 셋은 train, valid, test 셋으로 구분되어 있으며, 본 챌린지에서는 학습 데이터를 train set, 평가데이터를 valid set으로 선정하였습니다.  
  또한 개개인마다 리소스가 한정되어 있기에 _**베이스라인 저자가 ActivityNet-v1.3 의 video 별로 feature 추출 및 rescale 하여 제공한 데이터를 본 챌린지의 학습 & 평가 데이터로 제한 합니다. (데이터 셋 링크는 아래 참고)**_  
  제한된 데이터 이외에는 사용하실 수 없습니다.  
  또한 ActivityNet-v1.3은 현재 valid set 에 대해 GT가 공개되어져 있기에, _**valid set GT를 활용한 학습이나 cheating을 금지합니다.**_  
  이를 판단하기 위해 본 챌린지의 참가자들은 챌린지가 끝난 후, _**코드를 제출해야하며 성능이 원복되어야 순위를 인정받을 수 있습니다.**_  
  
## Baseline
  베이스라인으로는 BSN(Boundary Sensitive Network)를 사용하였습니다.
  Video에서 action이 있을 것 같은 위치의 start end time을 예측하여 action proposal을 생성하는 방법으로 아래 그림과 같습니다.
  <br>
  <br>
  <br>
  ![image](https://user-images.githubusercontent.com/46413594/137618348-f2106cce-d248-448a-9ac6-ada4e9a051e4.png)
  <br>
  <br>
  <br>
  평가지표로는 AR@AN(AR with Average Number of proposals)이 사용되며, AN=100 기준으로 평가됩니다.  
  평가 과정으로는 각 video 별로 AN=100개 만큼의 proposal을 반환했을 때 GT와 기준 tIoU 이상 겹치는 proposal을 True Positive로 두고 Recall을 계산하며, tIoU가 0.5부터 0.9까지 0.05 interval로 변경하며 계산한 Recall의 평균 값을 AR@100의 결과로 사용합니다.  
  베이스라인 코드를 따라하시다보면 predict과 AR@100의 결과를 확인할 수 있으니 차근차근 따라해보시길 바랍니다.

## Result
  Dictionary 형태의 json으로 제출하셔야하며, 아래와 같이 저장하시면됩니다. 
  편의를 위해 sample submit.json을 아래 제공하며, 베이스라인 코드를 따라하시다보면 포맷에 맞는 result json을 얻으실 수 있습니다. 
  -  json['results'][평가비디오ID][예측한 Proposal index] -> {"score" : Proposal의 confidence, "segments" : [Proposal의 start(sec), Proposal의 end(sec)]}  
  
  
## Challenge
  - [데이터 셋] [ActivityNet-v1.3 Dataset-rescaled feature](https://drive.google.com/file/d/1ISemndlSDS2FtqQOKL0t3Cjj9yk2yznF/view) 
    - (아래 메타 데이터를 활용하여 {training-학습, validation-평가}를 나누어 사용할 것)
    - [ActivityNet-v1.3 Meta](https://github.com/Jo-won/TermProject_2021ComputerVision/files/7359457/video_info_new.csv)
  - [베이스라인 코드] [Code](https://github.com/wzmsltw/BSN-boundary-sensitive-network)
  - [베이스라인 논문] [Paper](https://arxiv.org/pdf/1806.02964.pdf)
  - [평가지표 설명] [Metric](http://activity-net.org/challenges/2019/tasks/anet_proposals.html)
  - [결과 예시] [sample_submit.json](https://drive.google.com/file/d/1bB_TMAcl_xGjRWaMD9QjJ4K31bBTlroO/view?usp=sharing)

## Caution
  - 학습 혹은 평가 시, 위 링크로 제공된 데이터 만을 사용하여야 합니다. 이외의 데이터는 사용할 수 없습니다. (ActivityNet-v1.3 Dataset-rescaled feature)
  - GT가 공개된 데이터 셋인 관계로 Cheating 여부를 판단하기 위해 리더보드 마감 이후, 리더보드의 성능이 원복 가능한 코드를 제출하여야 합니다.
    - 메일 : clockjw@rcv.sejong.ac.kr
  - 베이스라인 코드를 일부 수정하며 따라하시다보면 해당 챌린지의 베이스라인 성능에 도달하실 수 있으니 잘 읽고 따라해보시길 바랍니다.
