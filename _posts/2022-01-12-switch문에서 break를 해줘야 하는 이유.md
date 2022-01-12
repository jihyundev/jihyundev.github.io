---

switch문은 C언어 등에서 사용하는 제어문 중의 하나로, 분기 명령에 속한다. 내가 자주 사용하는 javascript에서도 switch 문법을 지원한다. 같은 변수를 비교하는 경우, 나는 if - else if 로 중첩된 조건문보다 switch문을 더 선호하는 편이다. 비교 상황을 직관적으로 잘 설명해주기도 하고, 무엇보다도 indent 구조가 더 깔끔해보이기 때문이다. 그런데 switch문을 쓰면서 항상 궁금했던 것이 있다. 왜 case마다 break를 굳이 사용해서 다른 case로 넘어가는 것을 막아줘야 할까? if문과 어떻게 다르게 동작하길래 break를 해주지 않으면 다음 case로 넘어가서 나머지 코드블럭이 전부 실행되는 것일까?

Javascript 뿐만 아니라 switch 문법을 가지고 있는 많은 언어들에서 위와 같은 현상이 나타나는데, 이걸 fall-through라고 한다. C언어에서부터 시작된 switch는 [분기 테이블(branch table)](https://en.wikipedia.org/wiki/Branch_table) 구조를 통해 구현되었다.

```jsx
switchfunc(inputNumber) {
    switch(inputNumber) {
        case 1:
            // Do Something
            break;
        case 2:
            // Do Something
            break;
        case 3:
            // Do Something
            break;
        default:
            // Do Something
            break;
	}
}
```

위와 같은 switch문이 있을 때, case에 있는 상수 1, 2, 3 등은 바이너리의 분기 테이블에 미리 기록되고, 테이블에서 값에 해당하는 주소를 참조하여 빠르게 다음 코드를 처리할 수 있다고 한다. 컴파일러 최적화를 쉽게 할 수 있어서, 특히 조건이 많을수록 if문을 통한 제어보다 실행속도가 빠르다. 이 switch문이 특이한 점은, 제어식이 평가될 때 제어는 case 표현식의 값에 해당되는 조건부호로 점프하게 된다는 것이다. 조건의 마지막 구문이 실행되면, 제어는 그 다음 조건의 첫 번째 구문으로 넘어가는데, 이 fall through 현상 때문에 문제가 생긴다. 여기서 이 조건의 조건부호는 무시된다. 따라서 break문 또는 기타 도약문이 없을 경우 제어는 계속해서 다음 조건으로 넘어가게 되는 것이다.

switch 제어문에 fall through 기능이 들어가 있기 때문에, 우리는 이를 이용할 수도 있다. 다음 코드블럭과 같이 여러 가지 케이스를 OR 조건으로 판별하여 구문을 실행시키고 싶은 경우, 또는 해당 제어 case 이하의 나머지 구문을 모두 실행시키고 싶은 경우 의도적으로 fall through를 사용할 수 있겠다. 이때 break를 빠뜨린 것과 혼동하지 않기 위해 /_ intentional fallthrough _/ 라는 주석을 적어두는 것이 관례라고 한다.

```jsx
switchfunc(month) {
    switch(month) {
        case 'January':
        case 'February':
            console.log('겨울');
	        /* intentional fallthrough */
        case 'March':
        case 'April':
        case 'May':
	        console.log('봄');
	        break;
        case 'June':
        case 'July':
        case 'August':
	        console.log('여름');
	        break;
	}
}

```

#### 참고한 문서

[https://ko.wikipedia.org/wiki/분기\_테이블](https://ko.wikipedia.org/wiki/%EB%B6%84%EA%B8%B0_%ED%85%8C%EC%9D%B4%EB%B8%94)

[https://ko.wikipedia.org/wiki/Switch\_문](https://ko.wikipedia.org/wiki/Switch_%EB%AC%B8)

[https://wikidocs.net/26932](https://wikidocs.net/26932)
