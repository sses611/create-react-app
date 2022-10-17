# JSX (JavaScript XML)

## 특징
 * 자바스크립트에 XML를 확장한 문법이다. 
 * 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 자바스크립 현태의 코드로 변환한다.
 * 공식적인 자바스크립트 문법이 아니라 개발자들이 임의로 만든 문법 혹은 차기 자바스크립트 문법으로 사용될 수 있다.
 * HTML 태그를 변수로 활당, 호출, 리턴할 수 있는 문법
 
## 바벨?
 * 6 to 5라는 의미로 ES6코드를 ES5로 바꾸는 과정을 뜻한다.

## ES, ECMASCRIPT
참고 블로그 : https://hbsowo58.tistory.com/m/407
  * ES6 (2015년) 출시
    *   let, const 키워드 추가 
        * let : 블록 스코프를 가지고 있다.
        * const : 초기화와 동시에 선언을 해야한다.
    *   Arrow function 추가
        * 화살표 함수로 간결한 함수 표현가능, 가독성 및 유지보수성 증가    
    *   Default parameter 추가  
    *   Template literal 추가
    *   Multi-line String
    *   클래스

 
## JSX 문법 규칙
 * 1. 무조건 닫는 태그가 필요하다.
 ```
 <input/>
 ```
 * 2. 여러태그들이 있으면 무조건 감싸는 태그가 필요하다.
 ```
return(
<div>
  <div></div>
  <div></div>
</div>
);
 ```

 * 3. return할 때 ()는 단순가독성을 위한 위한것이다.
 * 4. style 적용시 객체를 만들어 사용해야한다.
```
const style={
	color: #409d00;
    backgroundColor: #000;
}

return(
	<span style={style}>Good</span>
);
```
 * 5. 속성 값 중 -으로 구분 되는 값들은 다 카멜체로 사용해야한다.
 * 6. class 적용대신 className으로 사용해야한다.
 * 7. 주석사용해야한다.
