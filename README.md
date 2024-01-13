# 산업재해 모니터링 시스템

**산업현장**에서 **작업자**의 **넘어짐**, **지정된 작업자 수**, **금지구역 침범**을 **객체검출**을 이용해 **낮은 비용**으로 **산업재해**를 **모니터링**하고 **예방**을 기대하는 **프로젝트**입니다.

**라즈베리파이**를 이용한 **영상 송출**과 **iOS** 기기에서 **기기 추론**을 바탕으로 해당 **상황**이 **발생**하게 되면 **설정된** 관리자의 **iOS** 기기로 **APNs**을 이용해 신속하게 **알림**을 보냅니다.

### 사용된 기술 및 라이브러리

- **Swift, UIKit**
- **Firebase**( RealtimeDatabase, Storage, Messaging, GoogleSignIn )
- **Nginx Media Server** ( RTMP → Nginx → HLS )
- **Google Colab** ( Train, Create Logic, Object detection Testing  )
- **HaishinKit** ( RTMP → Nginx )
    
    > https://github.com/shogo4405/HaishinKit.swift
    > 
- **CoreML Tools** ( PyTorch → CoreML )
    
    > https://github.com/apple/coremltools
    > 
- **Roboflow** ( Train dataset )
    
    > [KeepWalking DataSet](https://app.roboflow.com/keepwalking/falldown-rqrk8/deploy/2)
    >

## 구성도




전체 시스템 구성도 | iOS 구성도
---|---|
![시스템 구성도](https://github.com/Seok231/-/assets/97385742/6770adeb-ce05-4805-8cc9-a98c3524a2bf)|![image](https://github.com/Seok231/-/assets/97385742/42f78ca2-f840-4a72-bf23-06e8518d3f92)



본 프로잭트의 CCTV는 라즈베리파이에서 실시간 영상을 받아와 추론하는 방식과 iOS기기를 활용해 추론하는 방식을 가지고 있습니다.

Python 모델은 서버에서 추론을 하지만, iOS기기는 YOLO모델을 CoreML로 변환하여 기기에서 추론을 합니다.

외부에서도 실시간으로 영상을 확인하기 위해 RTMP 프로토콜로 영상을 Nginx로 송출합니다.

Nginx에서 받아올 HLS 주소와 검출된 상세 내용을 Firebase에 저장합니다.


# iOS App

Google 로그인으로 각 유저별 정보를 나눠 관리가 가능합니다.

다른 iOS기기, 라즈베리파이에서 송출된 영상을 Nginx를 통해 실시간으로 확인이 가능합니다.  

![image](https://github.com/Seok231/-/assets/97385742/01e71e20-6aba-42df-8d35-5a9624450b04)



구글 로그인 | Live
---|---|
![구글로그인2](https://github.com/Seok231/-/assets/97385742/fe3bc716-3c61-4362-975e-a89c986391c8) | ![Live확인](https://github.com/Seok231/-/assets/97385742/a4e2fb27-da89-4d01-90a1-0c84f08e8309)  



<br/>
<br/>


CCTV로 설치한 iOS기기를 외부에서 손쉽게 최소 작업자 수 감지, 넘어짐 감지 기능을 키고 끌 수 있습니다.

해당 기능으로 일정 작업자 수 이하, 넘어짐이 감지 된다면, App이 종료된 상태에도 신속하게 알림을 전달합니다.

Live -> YOLO | 알림
---|---|
![Live -  YOLO + person](https://github.com/Seok231/-/assets/97385742/196450a6-82da-4f05-a67e-45c217d96e2e) |![넘어짐 알림](https://github.com/Seok231/-/assets/97385742/ef341da3-bea3-407f-a3df-bc6bef4935c9)



<br/>
<br/>

본 모델에서 사용하는 사람 수 검출은 Input 데이터로 들어온 영상을 일정 frame으로 나눠 추론을 진행하게 됩니다. 각 frame 별 추론된 Class Label을 각
각 list에 넣고 넣은 리스트 안에 ‘person’의 개수가 사람의 수로 사용됩니다.

![image](https://github.com/Seok231/-/assets/97385742/04e817f1-1f1d-41a2-80ad-f3a203990b6e)



넘어짐 감지 | 사람 수 감지
---|---|
![넘어짐2](https://github.com/Seok231/-/assets/97385742/a404d99d-1d34-44b5-b764-c3fcfed1b0d0) | ![사람 수](https://github.com/Seok231/-/assets/97385742/d154f52b-1b60-4e19-b016-2f801bec27bf)







