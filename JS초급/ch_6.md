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
// map을 forEach로 ?? : return이 있는 경우에는 forEach로 하면 매우 복잡해집니다.
var 맵 = 필.map(function (요소, 인덱스) {
    return 인덱스 + 1;   
});

```
for문은 몇번 반복문을 돌지 알때  
while은 몇번 반복문은 돌지 모를때, 기준값이 바뀔때  
```javascript
for(var i = 0; i< 후보군.length; i += 1){} // X 후보군에서 하나씩 뽑다보면 후보군의 length가 줄어든다.
while(후보군.length > 0 ){                // O
    var 이동값 = 후보군.splice(Math.floor(Math.random() * 후보군.length),1)[0];
    셔플.push(이동값);
}   
```


## 6-3. 배열 slice & sort  
slice(시작, 끝)  끝은 미포함
[].sort() 정렬
```javascript

var 당첨숫자들 = 셔플
    .slice(0,6) // 0 ~ 5  
    .sort(function(p,c){return p - c}) // 오름차순
    //.sort(function(p,c){return c - p}) // 내림차순


// [2,5,6,7,3,1]
// p: 현재 숫자, c: 다음숫자
// 뺀 결과가 0보다 크면 순서를 바꾼다
//      p - c 
// 1. 2 - 5 < 0 
// 2. 5 - 6 < 0
```
## 6-4. JS로 HTML태그 선택하기
```javascript
for(){
    setTimeout(function 비동기콜백함수(){

    },1000) // 1초
}
```
클로저문제 -- 자바스클립트 중급  
클로저가 머냐?... 
for문 안에 비동기 함수가 들어있는 경우 클로저 문제가 생김   

## 6-5. JS로 CSS 조작하기  
```javascript
공.style.display = 'inline-block';
공.style.border = '1px solid black';
공.style.borderRadius = '10px';
공.style.width='20px';
공.style.height='20px';
공.style.textAlign = 'center';
공.style.marginRight = '10px';
```

다른 부분은 매개변수로 받고, 겹치는 부분은 함수 내부로 묶는다.  

## 6-6. 로또추첨기 마무리 & querySelector  
```javascript
공.id =  '공아이디' + 숫자       // 가능
공.class = '공아이디' + 숫자     // 불가능
공.className = '공아이디' + 숫자 // 가능
```

querySelector('#결과창');
querySelector('.보너스');
css 형식으로 사용할 수 있다.
querySelector추천

html element요소, nodeList >> 중급, 실무에선 거의.. 안씀  


