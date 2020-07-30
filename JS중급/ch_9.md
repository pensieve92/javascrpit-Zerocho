## 9-1. 반응속도 테스트  
```javascript
var 스크린 = document.querySelector('#screen');

스크린.addEventListener('click', function(){
    if(스크린.classList.contains('waiting')){
        스크린.classList.remove('waiting');
        스크린.classList.add('ready');
        스크린.textContent = '초록색이 되면 클릭하세요';
    }else if(스크린.classList.contains('reday')){
        스크린.classList.remove('ready');
        스크린.classList.add('now');
        스크린.textContent = '클릭하세요!';

    }else if(스크린.classList.contains('now')){
        스크린.classList.remove('now');
        스크린.classList.add('waiting');
        스크린.textContent = '클릭해서 시작하세요';

    }
})
```

```css
#screen {
    user-select : none; /* 클릭시 선택안됨 */
}
```
## 9-2. 시간 체크와 예약어  
```javascript
var 시작시간 = new Date();
var 끝시간 = new Date();
console.log((끝시간 - 시작시간) / 1000);
```
```javascript
console.time('시간');
console.timeEnd('시간');
```
performance.now(); - 정밀한 시간
```javascript
var 시작시간 = performance.now();
var 끝시간 = performance.now();
console.log((끝시간 - 시작시간) / 1000);

```
## 9-3. 호출 스택(call stack)  
Last In First Out : LIFO 후입선출 구조  
```javascript
function d() {
    console.log('d');
}

function e() {
    console.log('e');
}

function a() {
    function b() {
        function c() {
            console.log('c');
        }
        c();
        console.log('b');
    }
    b();
    console.log('a');
}

d();
e();
a();
```
## 9-4. 타이머 제거  

```javascript
var 타임아웃;
타임아웃 = setTimeout(function(){

})
clearTimeout(타임아웃);
```
## 9-5. 재귀, 비동기와 호출 스택

