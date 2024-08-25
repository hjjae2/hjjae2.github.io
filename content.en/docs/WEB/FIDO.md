---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
title: FIDO(w/ Passkey)
---

# 개요

FIDO Alliance 는 비밀번호에 대한 의존을 줄이기 위한 목적을 갖고 있다. 그 방법으로 인증, 기기 증명을 위한 개발을 장려하고 있다. 비밀번호를 대체하지는 않더라도 강한 의존을 줄여야 한다는 공감대는 형성되고 있다. FIDO Alliance 는 암호, 
SMS OTP 보다 더 안전하고, 쉬운 방식인 '패스키'를 통해 인증 방식을 변화시키고 있다.

현재 FIDO Alliance 는 세 가지 인증 사양을 제공하고 있다.

* FIDO U2F(FIDO Universal Second Factor)
* FIDO UAF(FIDO Universal Authentication Framework)
* FIDO 2.0(which includes WebAuthn specification, and FIDO CTAP)

# 패스키

비밀번호 대신 생체 인식(지문, 얼굴 인식), PIN, 패턴 등의 인증 수단을 사용한다. 표준이기에 다양한 브라우저(chrome, safari, ...)와 운영체제(MacOS, Windows, ...)에서 사용할 수 있다.
\* 브라우저나 OS 에서 인증을 처리한다.

패스키는 비밀번호 관리자(예: Google 의 경우 [Google 비밀번호 관리자](https://passwords.google/?hl=ko))에 저장한다. 패스키는 비밀번호 관리자 계정(예: Apple 계정, Google 계정)에 정보가 종속된다. 

노트북에서 지원하지 않는 기기라면, 휴대전화를 통해 패스키를 사용할 수 있다.