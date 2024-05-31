---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

인증서 관련 표준, 인코딩 및 확장자에 대해 알아봅니다.

# PKCS#8, PKCS#12, X.509

## PKCS#8

Public Key Cryptography Standards 표준 중 하나로, **개인 키를 저장하는 방법**을 정의한다.

> Public Key Cryptography Standards (PKCS)에 대해서 살펴보자. ✔︎

## PKCS#12

Public Key Cryptography Standards 표준 중 하나로, 하나 이상의 개인 키와 인증서를 포함하는 **키 저장소**를 정의한다.

## X.509

PKI(Public Key Infrastructure) 표준으로 **인증서**를 정의한다.

> [RFC 5280 - Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile](https://datatracker.ietf.org/doc/html/rfc5280) 를 살펴보자. ✔︎

# Encoding

## DER (Distinguished Encoding Representation)

바이너리 형태로 인코딩된 인증서다.

## PEM (Privacy Enhanced Mail)

Base64 형태로 인코딩된 인증서다.

인증서, 비밀키, CSR 등을 위한 포맷으로 자주 사용되고 있다.

`-----BEGIN XXX-----` 와 `-----END XXX-----` 로 시작과 끝이 표현된다.

기존에는 secure email을 위해 개발되었으나, 현재는 주로 인증서를 저장하는데 사용된다고 한다.

# Extension

## .key

개인키, 공개키를 표현한다.

## .cer .crt

인증서를 표현한다.

## .p12 .pfx

PKCS#12 형식으로, 하나 이상의 인증서와 개인키를 포함하고 있는 '키 스토어' 파일을 표현한다.

패스워드로 암호화되어 있다. 

# References

- https://www.letmecompile.com/certificate-file-format-extensions-comparison/