## 6-1. 로또추첨기 Array & fill  
```javascript
var 후보군  = Array(45);    // 길이만 있고, 빈배열 empty 상태 >> 반복문 불가

//IE에서는 fill안됨...
var 필 =  후보군.fill(); // undefined 상태가 됨, 

필.forEach(function (요소, 인덱스) {
    필[인덱스] = 인덱스 + 1; // 억지로 넣는 방법    
    // 요소 = 인덱스 + 1;  
    // 필[인덱스] 대신 요소를 사용하면 안되는 이유
    // 참조 관계가 있습니다. 요소는 복사된 값이라서 원래 값인 필[인덱스]와 별개가 됩니다.
    // 객체를 다른 변수에 대입하지 않는 한 대부분의 경우는 복사라고 보시면 됩니다. 대부분의 경우 객체면 참조가 되고 원시값이면 복사가 됩니다.
});
``` 

 
## 6-2. 배열 map 메서드  
```javascript
[undefined, undefined, undefined, undefined,undefined,]
[1,2,3,4,5]
// mapping, 맵 1:1로 맵핑하려면 map()를 사용
// 새로운 배열을 받아온다.
var 맵 = 필.map(function (요소, 인덱스) {
    return 인덱스 + 1;   
});

```

## 6-3. 배열 slice & sort  
## 6-4. JS로 HTML태그 선택하기
## 6-5. JS로 CSS 조작하기  
## 6-6. 로또추첨기 마무리 & querySelector  
