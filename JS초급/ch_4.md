## 4-1. 비동기 & 숫자야구 순서도  
```javascript
폼.addEventListener('submit', function 콜백함수() { // 엔터 쳤을 때

})
```
## 4-2. 배열 push, pop, shift, unshift  
배열에서 해당 하는 것은 사라짐  
push: 마지막에 추가   
pop: 마지막 것 뽑기  
unshift: 처음에 추가  
shift: 처음 것 뽑기  
return [].length  

## 4-3. 배열 splice  
splice(위치, 개수): 위치로 부터 개수만큼 배열에서 뽑기 
return []  
배열에서 해당 하는 것은 사라짐  

## 4-4. split & join  
```javascript
폼.addEventListener('submit', function 비동기(이벤트) { // 엔터 쳤을 때, 언제 실행될지 모른다.  
    이벤트.preventDefault();  

}) 
```
submit시 새로고침 작동 (기본동작) >> 이벤트.preventDefault();  
  
문자.split(구분자) -> 배열  
배열.join(구분자) -> 문자  
## 4-5. indexOf & 숫자야구 구현  
## 4-6. 리팩토링 & 개념 복습  
```javascript
var 틀린횟수 = 0;
폼.addEventListener('submit', function 비동기(이벤트) { // 엔터 쳤을 때, 언제 실행될지 모른다.  
    이벤트.preventDefault();

    틀린횟수++; // 이벤트 리스너에서 반목문을 대체할수 있다.
    if(틀린횟수 > 10) {
        
    }
}) 
```

