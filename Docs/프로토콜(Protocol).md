>[!question]
>GQ1. 프로토콜은 무엇일까?
>GQ2. 프로토콜은 클래스나 구조체와는 어떻게 다를까?
>GQ3. 프로토콜은 상속과 어떻게 다를까?

## Description

> A _protocol_ defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality.

프로토콜은 메소드, 프로퍼티 등 특정한 작업이나 기능을 위해 필요한 요구사항들의 청사진이다.


## 주요 기능
+ 클래스, 구조체, 열거형에 채택될 수 있다.
```
struct SomeStructure: FirstProtocol, AnotherProtocol { 
	// structure definition goes here 
} 

class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol { 
	// class definition goes here 
}
```


## 코드 예시

```
protocol SomeProtocol { 
	// protocol definition goes here 
}
```


## Keywords
+ 파생된 키워드들을 작성

## References
- [Swift.org - Protocols](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols/)
- [티스토리 - 코딩하는 체대생](https://mini-min-dev.tistory.com/304)