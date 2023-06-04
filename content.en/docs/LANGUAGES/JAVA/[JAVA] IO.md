---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
---

> https://school.programmers.co.kr/learn/courses/9/lessons/266#note

![](/images/[JAVA]%20IO_40.png)

## IO

- byte 단위 입출력
- char 단위 입출력

### byte 단위 입출력

- InputStream
- OutputStream

### char 단위 입출력

- Reader
- Writer

<br>

## byte 단위 입출력

Inputstream, OutputStream postfix 가 붙는다.

예를 들면,
- 파일 : FileInputStream, FileOutputStream
- Jar: JarInputStream, JarOutputStream
- 데이터 타입 (primitive Java data types) : DataInputStream, DataOutputStream


### InputStream

```java
/**
 * This abstract class is the superclass of all classes representing
 * an input stream of bytes.
 *
 * Applications that need to define a subclass of <code>InputStream</code>
 * must always provide a method that returns the next byte of input.
 */
public abstract class InputStream implements Closeable {
    ...
}
```

### OutputStream

```java
/**
 * This abstract class is the superclass of all classes representing
 * an output stream of bytes. An output stream accepts output bytes
 * and sends them to some sink.
 * 
 * Applications that need to define a subclass of
 * <code>OutputStream</code> must always provide at least a method
 * that writes one byte of output.
 */
public abstract class OutputStream implements Closeable, Flushable {
    ...
}
```

<br>

## char 단위 입출력

Reader, Writer postfix 가 붙는다.

예를 들면,

- InputStream, OutputStram : InputStreamReader, OutputStreamReader
- Buffer : BufferedReader, BufferedWriter
- 파일 : FileReader, FileWriter
