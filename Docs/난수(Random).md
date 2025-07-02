>[!question]
>GQ1. C언어에서 rand() 함수를 호출하면 왜 계속 같은 값이 나올까?
>GQ2. Swift에서도 비슷한 현상이 발생할까?

## Description

[[pwnable.kr] Toddler's Bottle - random](https://jini00.tistory.com/64)
오랜만에 블로그 글을 보던 중에 문제 풀이 방식과 풀이에 이용된 원리에 관해 궁금증이 생겼다.


> 난수(Random)는 무엇이고 C언어에서 왜 동일한 값을 리턴할까?


이는 시드(seed) 때문이다.

시드(seed)란 난수 생성기에서 사용하는 초기값으로, 이 값에 따라 생성되는 난수의 순서가 결정된다.
난수는 완전한 무작위가 아니라 사실은 컴퓨터가 미리 정해진 알고리즘에 의해 만들어진 값이라 "의사 난수"라 부르기도 한다.

몇번이고 다시 실행해도 같은 결과값이 나오는 이유는 특정 시드값을 같은 알고리즘에 넣어서 계속해서 난수를 생성하기 때문이다. (기본 seed값은 1이다.)

그렇기 때문에 난수의 값을 바꾸고 싶다면 rand() 함수의 시드값을 바꿔줘야 한다.


위의 질문과 같이 생긴 의문점이 하나 있다.


> Swift에서는 일반적으로 왜 비슷한 현상이 나타나지 않는 걸까?


Swift도 C언어와 마찬가지로 random 함수의 내부는 난수 생성기를 기반으로 되어있다. 그렇기 때문에 seed값을 지정하면 동일한 순서의 난수가 나올 수 있다.

하지만, Swift에서 제공하는 random 함수는 내부적으로 seed값을 자동으로 설정해주는 매커니즘이 있어서 일반적으로 항상 같은 값이 나오지 않는다. (다만, 사용자가 seed값을 직접 지정해줄 수는 없다.)



## 주요 기능
+ 랜덤한 수를 생성한다.
```
let random = Int.random(in: 1...100)
```


## 코드 예시
- **C언어** - seed값을 설정하지 않아 매번 같은 값이 반환됨
```
#include <stdio.h>
#include <stdlib.h>

int main() {
	int random = rand(); // 시드 설정하지 않음

	printf("%d\n", random); // 같은 결과값 출력

	return 0;
}
```


- **C언어** - 현재 시간을 seed값으로 설정하여 매번 다른 난수를 반환하도록 함
```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL)); // 시드 설정 (현재 시간 기준)
    
    int random = rand();    // 난수 생성
    
    printf("%d\n", random);
    
    return 0;
}
```


- **Swift** - 기본 함수는 seed 지정 불가
```
let random = Int.random(in: 1...100)

print(random)
```



## Keywords
+ 시드(Seed)
+ 

## References
- [Apple 공식 Swift 문서 - RandomNumberGenerator](https://developer.apple.com/documentation/swift/randomnumbergenerator)
- 