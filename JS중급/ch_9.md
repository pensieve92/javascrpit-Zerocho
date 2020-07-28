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
## 9-3. 호출 스택(call stack)  
## 9-4. 타이머 제거  
## 9-5. 재귀, 비동기와 호출 스택

