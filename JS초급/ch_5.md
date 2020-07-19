## 5-1. 틱택토 순서도 & 화면  
## 5-2. 이차원 배열  
들여쓰기 최소화
```javascript
for (){
    for (){
        칸.addEventListener('click', function 비동기콜백(이벤트){

       });
    }
}
```
함수를 바깥으로 빼서 복잡성을 줄인다.  
```javascript
var 비동기콜백 = function(이벤트){};
...
for (){
    for (){
        칸.addEventListener('click', 비동기콜백);
    }
}
```

## 5-3. e.target과 parentNode 
배열에 html 요소객체를 담는다.  
클릭할때 html 부모가 줄, 자식이 칸  
몇번째인지 indexOf로 알아낸다.  
```javascript
var 줄들 = []; // 일차원배열
var 칸들 = []; // 이차원배열
// 줄들, 칸들에 push 생략...

var 턴 = 'X';

var 비동기콜백 = function(이벤트) {
    console.log(이벤트.target);// 칸
    console.log(이벤트.target.parentNode);// 줄
    console.log(이벤트.target.parentNode.parentNode); //테이블

    var 몇줄 = 줄들.indexOf(이벤트.target.parentNode);
    console.log('몇줄', 몇줄);
    var 몇칸 = 칸들[몇줄].indexOf(이벤트.target);
    console.log('몇칸', 몇칸);

    // .value = input태그    (jQuery: .val())
    // .textContent = 나머지 (jQuery: .text()) >> text-align: center;
    if(칸들[몇줄][몇칸].textContent !== ''){
        console.log('빈칸아님');
    } else {
        console.log('빈칸');
        칸들[몇줄][몇칸].textContent = 턴;
        if( 턴 === 'X') {
            턴 = 'O';
        }else {
            턴 = 'X';
        }
    }
}

```
## 5-4. 틱택토 구현  
```javascript
var 비동기콜백 = function(이벤트) {
    ...
    console.log('빈칸아님');
    /// 세칸 다 채워졌나??
    var 다참 = false;
    // 가로줄 검사
    if(
        칸들[몇줄][0].textContent === 턴 &&
        칸들[몇줄][1].textContent === 턴 &&
        칸들[몇줄][2].textContent === 턴 
    ){
        다참 = true;
    }
    // 세로줄 검사
    if(
        칸들[0][몇칸].textContent === 턴 &&
        칸들[1][몇칸].textContent === 턴 &&
        칸들[2]][몇칸].textContent === 턴 
    ){
        다참 = true;
    }
    // 대각선 검사
    if(몇줄 - 몇칸 === 0){  // 대각선 검사가 필요한 경우
        if(
            칸들[0][0].textContent === 턴 &&
            칸들[1][1].textContent === 턴 &&
            칸들[2]][2].textContent === 턴 
            ){
            다참 = true;
        }
    }
    // 대각선 검사
    if(Math.abs(몇줄 - 몇칸 ) === 2  ){  // 대각선 검사가 필요한 경우
        if(
            칸들[0][2].textContent === 턴 &&
            칸들[1][1].textContent === 턴 &&
            칸들[2]][0].textContent === 턴 
            ){
            다참 = true;
        }
    }    
    // 다 찼으면
    if(다참) {
        console.log(턴 + '승리');
    }
}
```
## 5-5. forEach와 중첩 반복문  
```javascript
[1,2,3,4,5].forEach(function(요소){
    console.log(요소);
})

칸들.forEach(function (줄){
    줄.forEach(function (칸){
        칸.textContent = '';
    });
});
```

중첩 반복문 횟수를 줄이는게 실력  



