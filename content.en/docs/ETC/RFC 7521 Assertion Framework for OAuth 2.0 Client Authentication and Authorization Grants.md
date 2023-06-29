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

# [Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants](https://datatracker.ietf.org/doc/html/rfc7521)

## 3. Framework

The entity that creates and signs or integrity-protects the assertion is typically known as the "Issuer", and the entity that consumes the assertion and relies on its information is typically known as the "Relying Party".  

**In the context of this document, the authorization server acts as a relying party.**

Although this document does not define the processes by which the client obtains the assertion (prior to sending it to the authorization server), there are two common patterns described below.

In the first pattern, depicted in Figure 1, the client obtains an assertion from a third-party entity capable of issuing, renewing, transforming, and validating security tokens.

Typically, such an entity is known as a "security token service" (STS) or just "token
service", and **a trust relationship (usually manifested in the exchange of some kind of key material) exists between the token service and the relying party(authorization server).**

The token service is the assertion issuer; its role is to fulfill requests from clients, which present various credentials, and mint assertions as requested, fill them with appropriate information, and integrity-protect them with a signature or message authentication code.  
- WS-Trust [OASIS.WS-Trust] is one available standard for requesting security tokens (assertions).

```

     Relying
     Party                     Client                   Token Service
       |                          |                         |
       |                          |  1) Request Assertion   |
       |                          |------------------------>|
       |                          |                         |
       |                          |  2) Assertion           |
       |                          |<------------------------|
       |    3) Assertion          |                         |
       |<-------------------------|                         |
       |                          |                         |
       |    4) OK or Failure      |                         |
       |------------------------->|                         |
       |                          |                         |
       |                          |                         |

                Figure 1: Assertion Created by Third Party
```

In the second pattern, depicted in Figure 2, the client creates assertions locally.  

**To apply the signatures or message authentication codes to assertions, it has to obtain key material: either symmetric keys or asymmetric key pairs.**

> 서명 또는 메시지 인증 코드를 어설션에 적용하려면 대칭 키 또는 비대칭 키 쌍 중 하나의 키 자료를 얻어야 합니다.

The mechanisms for obtaining this key material are beyond the scope of this specification.

Although assertions are usually used to convey identity and security information, self-issued assertions can also serve a different purpose.

They can be used to demonstrate knowledge of some secret, such as a client secret, without actually communicating the secret directly in the transaction. 

In that case, additional information included in the assertion by the client itself will be of limited value to the relying party, and for this reason, only a bare minimum of information is typically included in such an assertion, such as
information about issuing and usage conditions.

```
     Relying
     Party                     Client
       |                          |
       |                          | 1) Create
       |                          |    Assertion
       |                          |--------------+
       |                          |              |
       |                          | 2) Assertion |
       |                          |<-------------+
       |    3) Assertion          |
       |<-------------------------|
       |                          |
       |    4) OK or Failure      |
       |------------------------->|
       |                          |
       |                          |

                      Figure 2: Self-Issued Assertion
```

Deployments need to determine the appropriate variant to use based on the required level of security, the trust relationship between the entities, and other factors.

From the perspective of what must be done by the entity presenting the assertion, there are two general types of assertions:

1.  **Bearer Assertions**: Any entity in possession of a bearer assertion (the bearer) can use it to get access to the associated resources (without demonstrating possession of a cryptographic key).  
   1.  To prevent misuse, bearer assertions need to be protected from disclosure in storage and in transport. Secure communication channels are required between all entities to avoid leaking the assertion to unauthorized parties.
2.  **Holder-of-Key Assertions**: To access the associated resources, the entity presenting the assertion must demonstrate possession of additional cryptographic material.  
    1.  The token service thereby binds a key identifier to the assertion, and the client has to demonstrate to the relying party that it knows the key corresponding to that identifier when presenting the assertion.