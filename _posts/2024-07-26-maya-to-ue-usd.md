---
layout: post
title: USD를 이용한 maya to Unreal engine 파이프라인
subtitle: USD를 이용한 maya to Unreal engine 파이프라인
categories: Pipeline
tags: [Pipeline]
---

마야에서 만든 에셋들을 UE로 가져갈때 fbx로 가져갈 경우,
텍스쳐 파일을 Full 사이즈로 못 가져간다.

그리고, 에셋 마다 다시 메테리얼을 만들어 줘야한다.

이 부분이 언리얼 엔진을 사용하면서 가장 큰 걸림돌인데, USD를 사용하면 상관 없다.

USD는 핫한 파이프라인 포멧이고, 카메라 뿐만 아니라 다양한 에셋, 캐시등을 담을 수 있다.
따라서 UE로 USD를 가져왔을 때 자동으로 메테리얼과 텍스쳐 파일을 가져오는 것이 이상하지 않다.

마야에서 테스트 해본 결과, 자주 사용하는 AI standardsurface는 안 먹는 것 같다.
그래서 lambert meterial로 사용했고,

대충 에셋을 만들어 넘겼다.

![image](https://github.com/user-attachments/assets/003ea9d3-a627-4444-88bf-24fc182e5c31)


export selection에 USD 포멧이 존재하고 별 다른 세팅없이 export 했다.

![image](https://github.com/user-attachments/assets/46a94c40-4ee3-4a8f-8ae1-a26bbd90de57)


UE에서는 USD 관련 플러그인을 설치해야한다.

![image](https://github.com/user-attachments/assets/387abfdf-555f-4d86-bb18-282e5a67ecf2)

이제 usd파일을 컨텐츠 브라우저에 드래그 드롭 하면 
경로를 설정하라는 창이 뜨고,

![image](https://github.com/user-attachments/assets/f8bf53ee-8667-4a0a-a7d1-1f8725170250)

export한 hierarchy가 뜨고, meterial 정보도 같이 뜬다.

![image](https://github.com/user-attachments/assets/e1b69748-071b-42a8-9eb7-fcfb97c485bb)

선택한 경로에 내 파일명으로 폴더가 생기고, Meterial, static Mesh, Textures(택스쳐맵) 폴더가 생기고 해당 파일들이 들어왔다.
![image](https://github.com/user-attachments/assets/c07fbddb-2efc-423d-a6da-7114ef8f5857)

![image](https://github.com/user-attachments/assets/de6b847f-4f96-4200-98ad-f0ef176b566d)
![image](https://github.com/user-attachments/assets/d01f5062-2ff8-4dbd-a01e-4a9bbfba9781)
![image](https://github.com/user-attachments/assets/76d706cf-dc01-4de4-ac02-ad99b65c6720)

생각보다 간단하게 파일을 넘길 수가 있었고, 해당 방식을 안 쓸 이유가 없어보인다.
