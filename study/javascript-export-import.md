# Common JS vs ES 6 on import/export



# Why

CommonJS 는 Javascript 에 대한 워킹그룹인데, 여기서 모듈에 대한 명세서를 지정하였고, 해당 명세서에서 따라 Node.js 도 사용중(1.0)

모듈에 대한 명세가 필요한 이유로는, Javascript 의 특성때문이지 않을까 싶음.
서버사이드 Javascript의 경우 scope가 파일로 나눠지지만, Front 의 경우는 스코프를 나눠주지 않는 이상 모듈간 충돌이 발생할 수 있음.

또한 특정 전체 파일에 대한 import가 아닌 해당 Js 파일에서 모듈로 공개하고 싶은 변수나 함수만을 선택하여 export / import 할 필요가 있기에 이러한 모듈방식이 출현하지 않았나 싶음.



# Common JS Module

- 스코프(Scope): 모든 모듈은 자신만의 독립적인 실행 영역이 있어야 한다.
- 정의(Definition): 모듈 정의는 exports 객체를 이용한다.
- 사용(Usage): 모듈 사용은 require 함수를 이용한다.

```
fileA.js

var a = 3;  
b=4;  
exports.sum = function(c, d) {  
return a + b + c + d;  
}

fileB.js
var a = 5;  
b = 6;  
var moduleA = require("fileA");  
moduleA.sum(a,b); // 3+4+5+6 = 18  

서버에서의 동작으로라고 생각했기에, fileA.js 와 fileB.js의 변수에 대한 충돌이 발생하지 않음.
```

필요한 모듈을 모두 내려받을 때까지 아무것도 할 수 없게 되는 것이다. 





# ES6 

- import , from , export, default 등 방식을 사용해 가독성이 좋다.
- 비동기 방식으로 작동
- 모듈에서 실제로 쓰이는 부분만 불러오기 때문에 성능과 메모리 부분에서 유리



ES6에서는 `import` 키워드의 짝꿍인 `export` 키워드를 사용해서 명시적으로 선언해줌.

이 때 내보내는 변수나 함수의 이름이 그대로 불러낼 때 사용하게 되는 이름이 되기 때문에 이를 Named Exports라고함.

```
file : currency-functions.js

const exchangeRate = 0.91;

// 안 내보냄
function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

// 내보내기 1
export function canadianToUs(canadian) {
  return roundTwoDecimals(canadian * exchangeRate);
}

// 내보내기 2
const usToCanadian = function (us) {
  return roundTwoDecimals(us / exchangeRate);
};
export { usToCanadian };


file : test-currency-functions.js
// Destructuring
import { canadianToUs } from "./currency-functions";	// 특정 모듈

console.log("50 Canadian dollars equals this amount of US dollars:");
console.log(canadianToUs(50));

// Alias
import * as currency from "./currency-functions";			// 전역

console.log("30 US dollars equals this amount of Canadian dollars:");
console.log(currency.usToCanadian(30));
```



단일 객체로 내보낼 때, `export default` 키워드를 사용해서 명시적으로 선언가능. 

하나의 모듈에서 하나의 객체만 내보내기 때문에 이를 Default Export라고 함.

하나의 객체(Default Export)를 불러올 때는 간단하게 `import` 키워드를 사용해서 아무 이름이나 원하는 이름을 주고 해당 객체를 통해 속성에 접근하면 됨.



```
file : currency-object.js

const exchangeRate = 0.91;

// 안 내보냄
function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

// 내보내기
export default {
  canadianToUs(canadian) {
    return roundTwoDecimals(canadian * exchangeRate);
  },

  usToCanadian: function (us) {
    return roundTwoDecimals(us / exchangeRate);
  },
};

file : test-currenct-object.js

import currency from "./currency-object";

console.log("50 Canadian dollars equals this amount of US dollars:");
console.log(currency.canadianToUs(50));

console.log("30 US dollars equals this amount of Canadian dollars:");
console.log(currency.usToCanadian(30));
```







출처

[js-module-import](https://www.daleseo.com/js-module-import/)

[d2.naver](https://d2.naver.com/helloworld/12864)