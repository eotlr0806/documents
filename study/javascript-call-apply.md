# Javascript Call / Apply



# Why

개발을 하다보면, 객체에 부여된 this 를 재할당해야될 일이 필요한 순간들이 있다.

그렇기에 Call / Apply 함수를 이용해 this 를 재할당한다.



# Call

```
# 사용법
func.call(thisArg[, arg1[, arg2[, ...]]])

# 예시1
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);

# 예시2
var obj = {
  string: 'zero',
  yell: function() {
    alert(this.string);
  }
};
var obj2 = {
  string: 'what?'
};
obj.yell(); 				 // 'zero';
obj.yell.call(obj2); // 'what?'
```

으로, 첫번째 매개변수로 재할당할 this 를 전달하고 그 뒤에 매개수로 옵션값을 전달 해 줄 수 있다.



# Apply

```
# 사용법
const numbers = [5, 6, 2, 3, 7];

# 예시1
const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2
```



Call 의 경우 call 함수에 전달할 인수 리스트를 전달받지만, Apply 함수의 경우 함수에 전달할 인수리스트를 배열에 담아 전달한다.



참조

[MDN Apply](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

[MDN Call](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

[zerocho](https://www.zerocho.com/category/JavaScript/post/57433645a48729787807c3fd)

