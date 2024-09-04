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

# SCIM (System for Cross-domain Identity Management)

SCIM 사양은 시스템 간 사용자 정보 교환을 위한 표준을 정의한다.

### Model (SCIM 2.0 기준)

SCIM 2.0 은 Resource 라는 공통 분모(모델)를 사용한다. Resource 하위에 User, Group 등의 개체가 존재하게 된다. 이들은 id, externalId, meta 등의 속성을 가진다. RFC 7643 에서 정의됐다고 한다.

### Example of User

```json
{
  "schemas": [
    "urn:ietf:params:scim:schemas:core:2.0:User"
  ],
  "id": "2819c223",
  "externalId": "dschrute",
  "meta": {
    "resourceType": "User",
    "created": "2011-08-01T21:32:52Z",
    "lastModified": "2011-08-01T21:32:52Z",
    "location": "https://example.com/v2/Users/2819c223"
    // ...
  }
  // ...
}
```

### Example of Group

```json
{
  "schemas": [
    "urn:ietf:params:scim:schemas:core:2.0:Group"
  ],
  "id": "e9e30dba",
  "displayName": "Sales Reps",
  "members": [
    {
      "value": "2819c223",
      "$ref": "https://example.com/v2/Users/2819c223",
      "display": "Dwight Schrute"
    }
    // ...
  ],
  "meta": {
    "resourceType": "Group",
    "created": "2011-08-01T21:32:52Z",
    "lastModified": "2011-08-01T21:32:52Z",
    "location": "https://example.com/v2/Groups/2819c223"
    // ...
  }
  // ...
}
```

### Operations

| Operation | Endpoint                                                                                                          |
|-----------|-------------------------------------------------------------------------------------------------------------------|
| Create    | POST /{version}/{resource}                                                                                        |
| Read      | GET /{version}/{resource}/{id}                                                                                    |
| Replace   | PUT /{version}/{resource}/{id}                                                                                    |
| Update    | PATCH /{version}/{resource}/{id}                                                                                  |
| Delete    | DELETE /{version}/{resource}/{id}                                                                                 |
| Search    | Get /{version}/{resource}?filter={attribute}{operator}{value}&sortBy={attribute}&sortOrder={ascending/descending} |
| Bulk      | POST /{version}/Bulk                                                                                              |

### Discovery

상호 운용성을 단순화하기 위해, SCIM 은 지원되는 기능과 특정한 속성 세부 정보(attribute details)를 검색할 수 있는 3가지의 Endpoint 를 정의한다.

| Description                                                    | Endpoint                             |
|----------------------------------------------------------------|--------------------------------------|
| Service Provider Config <br/>: 컴플라이언스 정보, 인증 스키마, 데이터 모델       | GET /{version}/ServiceProviderConfig |
| Resource Types <br/>: 지원되는 리소스 타입 조회                           | GET /{version}/ResourceTypes         |
| Schemas <br/>: 리소스, 속성 확장(attribute extensions) 조사(introspect) | GET /{version}/Schemas               |

# SCIM 1.1 vs SCIM 2.0

### SCIM 1.1

SCIM 1.1 은 2011년 릴리즈되었다. SCIM 1.1 은 SCIM 1.0(deprecated) 의 업데이트 버전이다. SCIM 1.1 은 사용자 정보 교환을 위한 많은 표준적 기반을 마련했다. SCIM 1.1 은 RESTful API 를 정의했고 users,
groups 등의 표준 스키마를 정의했다. 이로써 시스템 간 사용자 정보 교환이 쉽게 이루어질 수 있었다.

### SCIM 2.0

SCIM 2.0 은 2015년 릴리즈되었다. **서로 다른 시스템 간 사용자 정보를 교환하기 위해 User Management API와 Schema를 정의한다.주로 SP 와 IDP 사이에서 사용될 수 있다. 이렇게 되면 SP 는 IDP 로부터 사용자 정보를 가져와
사용자 정보를 프로비저닝 할 수 있다.** ⭐

SCIM 2.0 의 주요 목표는 SCIM 1.1의 스키마를 더 유연하게 만들고, 시스템 간 상호 운용성을 높이는 것이다.

### Key Differences

| SCIM 1.1                                | SCIM 2.0                                                     |
|-----------------------------------------|--------------------------------------------------------------|
| XML, JSON                               | JSON                                                         |
| TLS 1.0 ~ 1.2                           | TLS 1.2                                                      |
| /Users, /Groups, /ServiceProviderConfig | /Users, /Groups, /ServiceProviders, /ResourceTypes, /Schemas |
| -                                       | New Attributes : immutability, returned, uniqueness          |
| -                                       | More filtering options : ne, ew, not, []                     |
| -                                       | PATCH method for partial updates                             |

더 많은 정보는 [scim2-vs-scim1](https://workos.com/blog/scim2-vs-scim1)에서 확인할 수 있다.

# References

* https://scim.cloud/
* https://datatracker.ietf.org/doc/html/rfc7642
* https://datatracker.ietf.org/doc/html/rfc7643
* https://datatracker.ietf.org/doc/html/rfc7644
* https://workos.com/blog/scim2-vs-scim1