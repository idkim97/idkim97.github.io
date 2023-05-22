---
permalink: /2023-05-19-AWS 루트 사용자 Access Key 삭제, MFA 활성화/
title: "[AWS] AWS 루트 사용자 Access Key 삭제, MFA 활성화"
date: 2023-05-19 13:00:00
toc: true
toc_sticky: true
toc_label: "AWS 루트 사용자 Access Key 삭제, MFA 활성화"
categories:
- AWS
tags:
- 카카오 클라우드 스쿨
- AWS
---
<br><br>

## ✅ AWS 계정 루트 사용자

AWS 계정을 처음 생성한다면 계정의 모든 AWS 서비스 및 리소스에 대한 전체 엑세스 권한을 지닌 **루트 사용자**를 가지게 된다. 루트 사용자는 모든 권한을 지니고 있기 때문에 보안에 굉장히 취약하고 해킹을 당했을 때 큰 피해를 입을 수 있다.

때문에 AWS에서는 루트 사용자를 거의 사용하지 않을 것을 강력히 권장하고 있다. 이를 위해 AWS 엑세스 키( Access Key)를 비활성화 후 삭제하거나, MFA를 활성화 하는 등 다양한 방법을 통해 위험 요소를 미리 제거할 수 있게끔 도와준다.


<br><br><Br><br>

## ✅ AWS 루트 사용자 Access Key 비활성화 후 삭제

1. Root user로 로그인 후 IAM 서비스 화면으로 이동한다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws2.png?raw=true">
</p>

<br><br><Br>

2. 루트 사용자의 Access Key를 선택한다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws3.png?raw=true">
</p>

<br><Br><Br>

3. 비활성화 버튼을 클릭한다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws4.png?raw=true">
</p>

<br><br><br>

4. 비활성화 버튼을 한번 더 클릭한다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws5.png?raw=true">
</p>

<br><br><br>

5. 삭제를 위해 키를 입력하고 삭제버튼을 클릭한다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws6.png?raw=true">
</p>

<br><br><br>

6. 루트 사용자의 Access Key 삭제가 완료되면 다음과 같이 나온다. ( MFA 활성화도 이루어지고 난 후의 이미지)
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/aws7.png?raw=true">
</p>


<br><br><br><br>

## ✅ AWS 루트 사용자 MFA 활성화

MFA(다중 인증)를 사용하여 AWS 환경의 보안을 강화할 수 있다. 여러 유형의 MFA가 있기 때문에 루트 사용자에 대해 여러 MFA 디바이스를 활성화 하는것이 좋다. 

<br><br>

1. 루트 유저에 로그인 후 IAM > Security credentials 로 이동한다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/mfa1.png?raw=true">
</p>

<br><Br><Br>

2. MFA 활성화를 할 디바이스를 선택한다. 여기서는 Authenticator app을 선택하도록 하겠다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/mfa2.png?raw=true">
</p>

<br><br><Br>

3. 사용중인 스마트폰 기기의 OS 유형에 따라 Google Play Store 또는 Apple App Store에서 Google OTP 어플을 다운받아 설치한다.

<br><br><br>

4. 2번 Show QR code를 클릭하면 QR코드가 생성되는데 이를 Google OTP 앱으로 스캔하여 MFA 할당을 설정한다. Google OTP 앱에 표시되는 MFA 코드 2개 ( OTP하나 입력후, 기다리면 새로운 OTP 번호가 발급된다)를 각각 입력후 하단 MFA 할당을 클릭하면 설정이 완료된다.
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/mfa3.png?raw=true">
</p>


<br><br><br>

AWS 루트 사용자 Access Key 삭제와 MFA 활성화는 "이 사람이 AWS를 그래도 어느정도 써봤고 관심이 있는 사람이구나" 정도를 판단할 수 있는 척도 중 하나가 될 수 있기 때문에 반드시 적용시켜주는것이 좋다.
