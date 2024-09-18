---
layout: post
title: 모션캡쳐 MAYA HumanIK Python 자동화 이슈 정리
subtitle: HumanIK를 활용하는 스크립트
categories: Issue
tags: [Issue]
---
해당 스크립트는 파이프라인 서버에 있는 모션캡쳐 데이터를 가져와 지정한 네이밍 스페이스의 캐릭터에게 해당 모션캡쳐를 입히는 스크립트이다.

코드: [깃헙코드](https://github.com/OvenTD/project_pipline_script/blob/main/import_motion_capture_data.py)

![image](https://github.com/user-attachments/assets/288e1fb4-dd8e-44ee-b9c2-a799ab348ec0)

- 첫번째 버튼은 선택된 오브젝트의 namespace를 가져온다.

- 두번째 버튼은 namespace를 통해 캐릭터를 구분, 서버의 Scene, Cut넘버에 맞는 캐릭터의 fbx를 가져와 아래에 보여준다.

- 세번째 버튼은 선택한 fbx를 가져오고 캐릭터에 데이터를 입힌다.

## issue

HumanIK가 진짜 까다로웠다.
python은 있는 python명령어들도 잘 안돌아간다. 
그래서 mel코드로 돌려야 한다. 

### HumanIK joint 지정
Humanik는 GUI에서는 버튼으로 클릭하면 여러 명령들이 실행되는데, 그 명령들을 찾을 수가 없다.
대표적인 예시가 이 joint를 지정하는 것이다.

![image](https://github.com/user-attachments/assets/a643bc1c-9a98-4133-88fe-07160f5a4873)

여기서 클릭 클릭해서 조인트 지정하는 거. 열심히 뜯어보니 노드가 존재했다.

![image](https://github.com/user-attachments/assets/ffde2bf9-05b7-4474-a714-fc2f86b47fa8)

이 노드이다. 

![image](https://github.com/user-attachments/assets/3c5f9678-6dc8-4d51-8dc9-6ffa3106282a)

숨겨져 있지만 input이 존재한다. 해당되는 조인트를 잘 지정해서 연결해주면 끝날 것처럼 생겼지만...
아니다.

![image](https://github.com/user-attachments/assets/22bfbd10-40e8-43fe-a3b6-bd37e6ba9909)

이 친구의 시작점은 transform 노드의 character라는 attr이다.
문제는 이 character라는 attr는 원래 없다. 

![image](https://github.com/user-attachments/assets/2042e880-0013-418a-9687-f18119e8b766)

그렇다면 오브젝트에 HIK가 원하는 형식의 attr를 만들어줘야하는데....
그걸 위한 코드가 mel의 **setCharacterObject**이다. Echo All Command로 겨우 찾아냈다.
```
mel.eval(f'setCharacterObject("{full_joint_name}","{humanIK}",{self.humanik_code},0);')
```

joint_name과 humanIK 노드이름, 그리고, HIK가 지원하는 joint식별번호를 넣어야한다.
식별번호는 Reference:0, Hips : 1... 이런 식이다.
하지만 마야는 이 번호를 어디에도 정리하지 않았다. 겨우 구글링해서 다른 사람의 [소스코드](https://forums.autodesk.com/t5/maya-programming/python-hik/td-p/4262564)를 찾아 알게되었다. 
더 깔끔한 정리는 내 코드를 참고 바란다. 이렇게 첫 관문을 넘어 섰다.

### HumanIK Source 지정

![image](https://github.com/user-attachments/assets/809d90f0-1c72-476f-aeb1-64ce97e7a929)

이렇게 클릭해서 지정하는 이 방식.
담당 Mel코드가 존재하는데, 안먹힌다. 담당 Mel코드는 **hikSetCurrentSource** 로 알고 있다.
그래서 이 코드를 못쓰고 

다른 방식을 찾아보다가 한 마야 커뮤니티 글에서 나와 같은 걸 원하는 사람이 올린 [질문글](https://forums.autodesk.com/t5/maya-animation-and-rigging/pythonic-mel-way-to-retarget-hik/td-p/7609798)이 있었다.
해당 질문글을 들어가면 어떤 선생님께서 Menu를 조작해서 특정 Menu의 목록을 찾아 Open하는 방식을 소개 시켜주셨고..
그 방식대로 해보니 돌아갔다. 

이런 방식으로 마야의 메뉴를 조작해서 동작하게 만드는 거는 처음 시도해봤는데, 신선한 접근인 것 같다.

그렇게 마지막 관문을 뚫었다. 항상 감사합니다!! 고수님들😊
