---
permalink: /aws-account-setting/
title: "[AWS] AWS 루트 계정 설정 (Access Key / MFA) "
date: 2023-05-19 13:00:00
toc: true
toc_sticky: true
toc_label: "AWS 계정 설정 ( Access Key / MFA )"
categories:
- AWS
tags:
- 카카오 클라우드 스쿨
- AWS
---
<br><br>

## ✅ AWS 계정 루트 사용자

AWS 계정을 처음 생성한다면 계정의 모든 AWS 서비스 및 리소스에 대한 전체 엑세스 권한을 지닌 **루트 사용자(Root User)**를 가지게 된다. 루트 사용자는 AWS 계정 내의 모든 리소스와 서비스에 대한 최대 권한을 지니고 있기 때문에 이 계정을 사용하면 모든 읽기, 쓰기. 삭제 작업을 수행할 수 있다. 따라서, 루투 계정이 노출되면 계정 전체가 위험해 질 수 있다.

때문에 AWS에서는 루트 사용자를 거의 사용하지 않을 것을 강력히 권장하고 있다. AWS를 처음 접하는 사람이라면 아마 대부분 루트 계정을 통해 프리티어 EC2를 생성해보거나 S3를 만들어보는 등 간단한 실습을 진행해 볼텐데, 이때가 사실 가장 위험하다. 아무런 정보 없이 그저 구글에 나와있는 실습을 따라서 진행하다가 제대로 된 보안규칙 없이 혹은 루트 사용자에 접근할 수 있는 Access Key를 노출하게 된다면 당신의 계정은 어느새 비트코인 채굴용 서버로 이용되고 있을지도 모른다. 

그래서 AWS를 좀 쓸 줄 안다 하는 사람들과 초보자들을 가장 먼저 구별할 수 있는 대표적인 방법이 바로 루트 계정에 대한 보안작업을 수행해주느냐 안해주느냐 라고 볼 수도 있다. 오늘은 루트 계정에 대한 두가지 보안작업인 **AWS 액세스 키(Access Key) 비활성화 및 삭제**, 그리고 **MFA 활성화** 두가지 작업을 알아보도록 하겠다.


<br><br><br><br><br><br>

## ✅ AWS 루트 사용자 Access Key 비활성화 및 삭제


### 📌 Access Key란?
<hr>

AWS의 액세스 키는 AWS 서비스에 접근하기 위해 사용되는 인증 정보라고 보면 된다. 크게 두 부분으로 구성된다.

1. **액세스 키 ID** : 사용자나 프로그램이 AWS API 요청을 보낼 때 사용하는 공개 식별자
2. **비밀 액세스 키** : 액세스 키 ID와 함께 사용되며 AWS 인증을 완료하는데 필요하다.

 




<br><br>

### 📌 Access Key 비활성화 방법
<hr>


1. Root user로 로그인 후 IAM 서비스 화면으로 이동한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws2.png?raw=true">
</p>

<br><br><Br>

2. 루트 사용자의 Access Key를 선택한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws3.png?raw=true">
</p>

<br><Br><Br>

3. 비활성화 버튼을 클릭한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws4.png?raw=true">
</p>

<br><br><br>

4. 비활성화 버튼을 한번 더 클릭한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws5.png?raw=true">
</p>

<br><br><br>

5. 삭제를 위해 키를 입력하고 삭제버튼을 클릭한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws6.png?raw=true">
</p>

<br><br><br>

6. 루트 사용자의 Access Key 삭제가 완료되면 다음과 같이 나온다. ( MFA 활성화도 이루어지고 난 후의 이미지)
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws7.png?raw=true">
</p>


<br><br><br><br><br><br>


## ✅ AWS 루트 사용자 MFA 활성화


### 📌 MFA란?
<hr>

MFA(Multi-Factor Authentication, 다중 인증)는 사용자가 시스템에 접근할 때 두가지 이상의 인증을 요구하는 보안 프로세스이다. 쉽게 말해 1차,2차,3차.. 등의 비밀번호를 다중으로 설정한다고 생각하면 된다.

AWS에서는 MFA를 사용하여 보안을 강화할 수 있다. 여러 유형의 MFA가 있기 때문에 루트 사용자에 대해 최소 2개 이상의 MFA 디바이스를 활성화 하는 것이 보안상 좋다.

이 포스팅에서는 루트 계정에 대해 MFA 설정하는 방법을 알아보도록 하겠다.

<br><br>

### 📌 루트 사용자 MFA 활성화
<hr>

1. 루트 유저로 로그인 후 IAM > Security credentials 로 이동한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/mfa1.png?raw=true">
</p>

<br><Br><Br>

2. MFA 활성화를 할 디바이스를 선택한다. 여기서는 Authenticator app을 선택했다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/mfa2.png?raw=true">
</p>

<br><br><Br>

3. 사용중인 스마트폰 기기의 OS 유형에 따라 Google Play Store 또는 Apple App Store에서 Google OTP 어플을 다운받아 설치한다.

<br><br><br>

4. 2번 Show QR code를 클릭하면 QR코드가 생성되는데 이를 Google OTP 앱으로 스캔하여 MFA 할당을 설정한다. Google OTP 앱에 표시되는 MFA 코드 2개 ( OTP하나 입력후, 기다리면 새로운 OTP 번호가 발급된다)를 각각 입력후 하단 MFA 할당을 클릭하면 설정이 완료된다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/mfa3.png?raw=true">
</p>


<br><br><br>

