>[!question]
>GQ1. C언어에서 rand() 함수를 호출하면 왜 계속 같은 값이 나올까?
>GQ2. Swift에서도 비슷한 현상이 발생할까?

## Description

[[pwnable.kr] Toddler's Bottle - random](https://jini00.tistory.com/64)
오랜만에 블로그 글을 보던 중에 문제 풀이 방식과 풀이에 이용된 원리에 관해 궁금증이 생겼다.


> 난수(Random)는 C언어에서 왜 동일한 값을 리턴할까?


이는 시드(seed) 때문이다.

시드(seed)란 난수 생성기에서 사용하는 초기값으로, 이 값에 따라 생성되는 난수의 순서가 결정된다.
난수는 완전한 무작위가 아니라 사실은 컴퓨터가 미리 정해진 알고리즘에 의해 만들어진 값이라 "의사 난수"라 부르기도 한다.

몇번이고 다시 실행해도 같은 결과값이 나오는 이유는 특정 시드값을 같은 알고리즘에 넣어서 계속해서 난수를 생성하기 때문이다. (기본 seed값은 1이다.)

그렇기 때문에 난수의 값을 바꾸고 싶다면 rand() 함수의 시드값을 바꿔줘야 한다.


위의 질문과 같이 생긴 의문점이 하나 있다.


> Swift에서는 왜 비슷한 현상이 나타나지 않는 걸까?


Swift도 C언어와 마찬가지로 random 함수의 내부는 난수 생성기를 기반으로 되어있다. 그렇기 때문에 seed값을 지정하면 동일한 순서의 난수가 나올 수 있다.

하지만, Swift에서 제공하는 random 함수는 내부적으로 seed값을 자동으로 설정해주는 매커니즘이 있어서 일반적으로 항상 같은 값이 나오지 않는다. 사용자가 seed값을 직접 지정할 수도 없다.


만약, Swift에서도 seed값을 설정하여 나오는 난수값을 통제하고 싶다면 GameplayKit의 GKMersenneTwisterRandomSource를 사용할 수 있다.

GameplayKit를 import 해야 사용할 수 있는건 다음과 같은 게임 개발과 관련한 특징들 때문이다. 
- 특정한 결과를 디버깅, 테스트, 리플레이 하는 동안 특정 조건에서 항상 같은 결과가 나오도록 통제해야 함
- 멀티 플레이나 저장/불러오기 기능이 있는 게임에서 일관적으로 게임 로직이 작동할 필요가 있음
- 다른 시드를 줘서 무한하게 컨텐츠가 생성되는 걸 방지
- 같은 결과를 재현하여 성능 비교 및 분석에 용이


## 주요 기능
+ in의 파라미터값을 범위로 하여 범위 내의 랜덤한 수를 생성한다.
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
    srand(time(NULL)); // 시드값 설정 (현재 시간 기준)
    
    int random = rand();
    
    printf("%d\n", random); // 실행마다 다른 결과값 출력
    
    return 0;
}
```


- **Swift** - 기본 함수는 seed 지정 불가
```
let random = Int.random(in: 1...100) // 함수 내부적으로 자동으로 seed값 설정

print(random)
```


- **Swift** - GameplayKit의 GKMersenneTwisterRandomSource로 seed값을 설정
```
import GameplayKit

let seed: UInt64 = 12345 // 시드값 설정
let generator = GKMersenneTwisterRandomSource(seed: seed)
let random = generator.nextInt(upperBound: 100)

print(random) //같은 결과값 출력
```

![[스크린샷 2025-07-03 오전 5.20.45.png]]



- **Swift** - GameplayKit의 GKMersenneTwisterRandomSource로 seed값을 설정 후 난수 배열 출력 시 매번 같은 값 출력
```
import GameplayKit

let seed: UInt64 = 1234
let generator = GKMersenneTwisterRandomSource(seed: seed)

let cnt = 10
let upper = 100

let randomNumbers: [Int] = (0..<cnt).map { _ in
    generator.nextInt(upperBound: upper)
}

print(randomNumbers)
```

![[스크린샷 2025-07-03 오전 5.26.45.png]]


## Keywords
+ [[시드(Seed)]]

## References
- [Apple 공식 Swift 문서 - random(in:)](https://developer.apple.com/documentation/swift/int/random(in:)-9mjpw)
- [Apple 공식 Swift 문서 - GameplayKit / GKMersenneTwisterRandomSource](https://developer.apple.com/documentation/gameplaykit/gkmersennetwisterrandomsource)