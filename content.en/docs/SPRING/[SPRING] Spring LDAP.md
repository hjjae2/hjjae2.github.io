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

# ContextSource

This is responsible for configuring and creating DirContext instances.

> 설정, DirContext 객체 생성을 책임지는 클래스다.

It is typically used from LdapTemplate to acquiring contextx for LDAP operations, but may be used standalone to perform LDAP authentication.

```java
public interface ContextSource {

	DirContext getReadOnlyContext() throws NamingException;

	DirContext getReadWriteContext() throws NamingException;

	DirContext getContext(String principal, String credentials) throws NamingException;
}
```

# [Pooling Support](https://docs.spring.io/spring-ldap/docs/2.3.8.RELEASE/reference/#pooling)

## Pool2 Configuration

|Attribute|Default|Description|
|-|-|-|
|max-total|-1|The overall maximum number of active connectiosn(for all types) that can be allocated from this pool at the same time. You can use a non-positive number for no limit.|
|max-total-per-key|8|The limit on the number of object instances allocated by the pool(checked out or idle), per key. When the limit is reached, the sub-pool is exhausted. A negative value indicates no limit.|
|max-idle-per-key|8|The maximum number of active connections of each type (read-only or read-write) that can remain idle in the pool, without extra connections being released. A negative value indicates no limit.|
|min-idle-per-key|0|The minimum number of active connections of each type (read-only or read-write) that can remain idle in the pool, without extra connections being created. You can use zero(the default) to create none.|
|max-wait|-1|The maximum number of milliseconds that the pool waits(when there are no available connections) for a connection to be returned before throwing an exception. You can use a non-positive number to wait indefinitely.|
|block-when-exhausted|true|Whether to wait until a new object is available. If max-wait is positive, a NoSuchElementException is thrown if no new object is available after th maxWait time expires.|
|test-on-create|false|Whether objects are validated before borrowing. If the object fails to validate, then borrowing fails.|
|test-on-borrow|false|The indicator for whether objects are validated before being borrowed from the pool. If the object fails to validate, it's dropped from the pool, and an attepmt to borrow another is made.|
|test-while-idle|false|The indicator for whether objects are validated by the idle object evictor (if any). If an object fails to validate, it is dropped from the pool.|
|...|...|...|
|min-evictable-time-millis|1000 * 60 * 30 (30 mins)|The minimum amount of time an object may sit idle in the pool before it is eligible for eviction by the idle object evictor (if any).|
|eviction-policy-class|`org.apache.commons.pool2.impl.DefaultEvictionPolicy`|The eviction policy implementation that is used by this pool. The pool tries to load the class by using the thread context class loader. If that fails, the pool tries to load the class by using the class loader that loaded the class.|
|fairness|false|The pool serves threads that are waiting to borrow connections fairly. `true` means that waiting threads are served as if waiting in a FIFO queue.|


## max-total vs max-total-per-key (by GPT)

### max-total

maxTotal은 풀에 있는 전체 커넥션(연결) 수의 최댓값을 나타냅니다.
이 값은 애플리케이션 전체에서 사용할 수 있는 동시 커넥션 수를 설정하는 데 사용됩니다.
예를 들어, maxTotal이 10으로 설정되면 애플리케이션은 동시에 최대 10개의 LDAP 연결을 유지할 수 있습니다.

## maxTotalPerKey

maxTotalPerKey는 풀당 특정 키(일반적으로 호스트 또는 서버 주소)에 대한 최대 커넥션 수를 의미합니다.
이 값은 풀 내에서 각 호스트 또는 서버 주소별로 유지할 수 있는 최대 연결 수를 설정하는 데 사용됩니다.
예를 들어, maxTotalPerKey가 5로 설정되면 풀 내에서 동일한 호스트나 서버 주소에 대해 최대 5개의 연결을 유지할 수 있습니다.

따라서, maxTotal은 전체적인 커넥션 수의 최댓값을 설정하고, maxTotalPerKey는 각 키(호스트 또는 서버 주소)별로 최대 커넥션 수를 설정하는 것입니다. 이러한 설정을 통해 애플리케이션은 LDAP 서버와의 연결 관리를 효율적으로 제어할 수 있습니다.