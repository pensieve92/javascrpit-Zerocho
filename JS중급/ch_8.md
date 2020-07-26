## 8-1. 지뢰찾기 기본화면  
## 8-2. 지뢰 심기  
화면과 데이터 따로 관리, 화면과 데이터의 싱크를 맞춰야한다. 

```javascript
// 지뢰 테이블 만들기
var dataset = []; // 데이터
var tbody = document.querySelector('#table tbody');
for(var i = 0; i < ver ; i++){
    var arr = [];
    dataset.push(arr);                      // 데이터
    var tr = document.createElement('tr');  // 화면
    
    for(var j = 0; j< hor; j++){
        arr.push(1);                            // 데이터
        var td = document.createElement('td');  // 화면
        tr.appendChild(td);
    }
    tbody.appendChild(tr);
}

// 지뢰 심기
for (var k = 0; k< 셔플.length; k++ ){
    var 가로 = Math.floor(셔플[k]/10);
    var 세로 = 셔플[k] % 10;
    tbody.children[세로].children[가로].textContent = 'X';
    dataset[가로][세로] = 'X';
}
```
### 2차원 배열 만들기 - 다르지만 같은 결과
이차원 배열에 값 넣기 (1) - row를 먼저 넣고, row의 값 채우기
```javascript
var dataset = [];
for(var i = 0; i < 3 ; i++){
    var arr = [];
    dataset.push(arr);
    for(var j = 0; j< 3; j++){
        arr.push(1);
    }
}
console.log(dataset);
```
이차원 배열에 값 넣기 (2) - 값을 먼저 채우고, row 넣기
```javascript
var dataset = [];
for(var i = 0; i < 3 ; i++){
    var arr = [];
    for(var j = 0; j< 3; j++){
        arr.push(1);
    }
    dataset.push(arr);
}
console.log(dataset);
```
## 8-3. 우클릭으로 깃발 꼽기  
코드 실행순서 : 동기 >> 비동기 
```javascript
documemt.querSelector('#exec').addEventListener('click',function(){ ... }) ; // 내부(비동기)에서 td 생성 

// 스코프 문제
// 동기가 먼저 실행되므로, td가 만들어 지지 않은상태이다.
// 따라서 td가 없다.
td.addEventListener('contextmenu', function(e){ 
    e.preventDefalut();
    var 부모tr = e.currentTarget.parentNode;
    var 부모tbody = e.currentTarget.parentNode.parentNode;
    var 칸 = 부모tr.children.indexOf(td);
    var 줄 = 부모tbody.children.indexOf(tr);

})
```

비동기 안에서 td에 이벤트 등록


```javascript
documemt.querSelector('#exec').addEventListener('click',function(){ 
    ...
    
    for(var i = 0; i < ver ; i++){
        var arr = [];
        dataset.push(arr);                      // 데이터
        var tr = document.createElement('tr');  // 화면    
        for(var j = 0; j< hor; j++){
            arr.push(1);                            // 데이터
            var td = document.createElement('td');  // 화면
            td.addEventListener('contextmenu', function(e){ // contextmenu 이벤트 등록
                e.preventDefalut();
                // e.target vs e.currentTarget
                // e.currentTarget을 쓰는게 안헷갈림
                var 부모tr = e.currentTarget.parentNode;
                var 부모tbody = e.currentTarget.parentNode.parentNode;
                // indexOf(td) 안됨 : children이 배열이 아니고 유사배열!!
                var 칸 = 부모tr.children.indexOf(td);         
                var 줄 = 부모tbody.children.indexOf(tr);
            })
            tr.appendChild(td);
        }
        tbody.appendChild(tr);
    }

    ...
    
}) ;
```
```javascript

// 스코프 문제로 밖으로 이동
var tbody = document.quertselector('#table tbody');
var dataset = [];

documemt.querSelector('#exec').addEventListener('click',function(){ 
    ...
    
    for(var i = 0; i < ver ; i++){
        var arr = [];
        dataset.push(arr);                      // 데이터
        var tr = document.createElement('tr');  // 화면    
        for(var j = 0; j< hor; j++){
            arr.push(1);                            // 데이터
            var td = document.createElement('td');  // 화면
            td.addEventListener('contextmenu', function(e){ // contextmenu 이벤트 등록
                e.preventDefalut();
                // e.target vs e.currentTarget
                // e.currentTarget을 쓰는게 안헷갈림
                var 부모tr = e.currentTarget.parentNode;
                var 부모tbody = e.currentTarget.parentNode.parentNode;

                // indexOf(td) 안됨 : children이 배열이 아니고 유사배열!!
                // var 칸 = 부모tr.children.indexOf(td);         
                // var 줄 = 부모tbody.children.indexOf(tr);

                // Array.prototype.indexOf.call()배열이 아닌 얘들(유사배열)한테도 indexOf를 사용할 수 있음
                // var 칸 = Array.prototype.indexOf.call(부모tr.children, td);  // 클로저 문제
                var 칸 = Array.prototype.indexOf.call(부모tr.children, e.currentTarget);  // 클로저 문제 회피
                var 줄 = Array.prototype.indexOf.call(부모tbody.children, 부모tr);
                console.log(부모tr, 부모tbody, e.currentTarget, 칸, 줄);

                // 화면, 데이터 일치
                // 데이터와 화면을 일치시키기위해
                // jquery로 일치시키기 어려움 >> react, vue, anglur를 사용
                e.currentTarget.textContent = '!';  // 화면
                dataset[줄][칸] = '!';              // 데이터
            })
            tr.appendChild(td);
        }
        tbody.appendChild(tr);
    }

    ...
    
}) ;
```

## 8-4. e.target vs e.currentTarget  
```javascript
tbody.addEventListener('contextmenu', function(e){
    // 이벤트리스너를 단 대상과 실제 이벤트가 발생하는 대상이 다를 수있다.
    // 이벤트 버블링, 캡처링과연관

    console.log('커런트타겟', e.currentTarget); // tbody : eventListener가 달린 요소
    console.log('타겟', e.target);              // td   : 실제 이벤트가 발생한 요소
})
```
## 8-5. 물음표 기능과 중간 정리  
```javascript
documemt.querSelector('#exec').addEventListener('click',function(){ 
    // 내부 초기화
    tbody.innerHTML = '';
})
```

## 8-6. 주변 지뢰 개수 세기  
```javascript
documemt.querSelector('#exec').addEventListener('click',function(){ 
    ...
    
    var 주변 = [        
            dataset[줄-1][칸-1], dataset[줄-1][칸], dataset[줄-1][칸+1],
            dataset[줄][칸-1],                      dataset[줄][칸+1],
            dataset[줄+1][칸-1], dataset[줄+1][칸] ,dataset[줄+1][칸+1]
    ];

    if
    e.currentTarget.textContext = 주변.filter(function(v){
        return v === 'X';
    }).length;
  
    ...
})
```


```javascript
documemt.querSelector('#exec').addEventListener('click',function(){ 
    ...
    
    var 주변 = [                   
        dataset[줄][칸-1], dataset[줄][칸+1],            
    ];
    // concat은 배열과 배열을 합쳐서 "새로운" 배열을 만들어준다.
    // 배열[-1]은 항상 undefined입니다. 배열[-1][0]은 undefined의 [0]을 접근하는 것이라 에러가 납니다.
    if(dataset[줄-1]){
        주변 = 주변.concat([ dataset[줄-1][칸-1], dataset[줄-1][칸], dataset[줄-1][칸+1] ])     
    }else if(ataset[줄+1]){
        주변 = 주변.concat([ dataset[줄+1][칸-1], dataset[줄+1][칸] ,dataset[줄+1][칸+1] ])     
    }
    e.currentTarget.textContext = 주변.filter(function(v){
        return v === 'X';
    }).length;
  
    ...
})
```
## 8-7. 스코프  

펑션(함수) 스코프 : 함수안에 정의된 변수는 함수 밖으로 나올 수 없다.  

```javascript
// 변수명이 같을 경우
var x = 'global';
function ex(){  
    var x = 'local';    // 함수 밖으로 빠져나갈 수 없다
    x = 'change';       
}
ex();       // 
alert(x);   // global
```
## 8-8. 스코프 체인  
함수 내에 없으면, 그 위의 함수, 위 함수도 없으면 전체범위헤서 변수를 찾는다.  
스코프 간의 상하관계를 스코프 체인이라 한다.  
```javascript
var name = 'zero';
function outer(){  
    console.log('외부', name);      // 1. zero
    function innner() {
        var enemy = 'nero';
        console.log('내부', name);  // 3. zero
    }
    inner();                        // 2. inner()
}
outer();       
console.log(enemy);                 // not defined 
``` 

## 8-9. 렉시컬 스코프  
렉시컬 스코프 == static 스코프 == 정적 스코프 : 코드가 적힌 순간 스코프가 정해짐  
스코프를 통해서 비밀 변수를 설정 (즉시실행)

```javascript
var name = 'zero';
function log() {
    console.log(name);
}   

function wrapper() {
    name = 'nero';
    log();
}
wrapper();      // nero  
```

```javascript
var name = 'zero';
function log() {
    console.log(name);  // 아무런 영향을 받지 않는다
}   

function wrapper() {
    var name = 'nero';
    log();  
}
wrapper();     // zero
```
## 8-10. 클로저  
클로저 :  함수와 함수가 선언된 어휘적(렉시컬) 환경의 조합 (MDM)  
클로저를 이루다 : 변수를 공유하다?  

클로저 관계
```javascript
// 전역범위와 log()가 클로저 관계이다.
var name = 'zero';
function log() {
    console.log(name);
}   

function wrapper() {
    var name = 'nero';
    log();  
}
wrapper();
```

반복문과 비동기 함수
```javascript
// 반복문과 비동기 함수가 만날때 클로저 문제가 자주발생한다.

                                // 예상 시나리오
for( var i = 0; i< 100; i++){   // 3. 전역 범위에서 i를 찾는다. for문의 i
    setTimeout(function(){      // 2. 익명함수 내부에서 i가 있나 ?? NO
        console.log(i);         // 1. i가 머지?                     
    }, i *1000)
}
                                // 결과 100 만 찍힘
```

반복문 결과  
비동기 함수가 for문의 i의 마지막값을 사용한다.  
for문은 callstack에서 이미 다 돌고 i가 증가된 상태로 setTimeout이 호출되서 그런거임  
변수가 렉시컬 스코프 안에 없으면 스코프체인 , 비동기 함수 안에 내용은 실행되는 순간에 결정된다.  
```javascript
// javascript 해석  
// setTimeout 비동기 함수 내부의 i는 i이고, 딜레이 시간 변수에 i는 반복문에 의해 1씩 증가된 값이다.
// 비동기 함수가 실행되기 전까지는 그 내용을 그대로 가지고 있다.
// 함수 안의 변수는 실행될때 값이 결정됨
setTimeout(function(){      
    console.log(i);         
}, 0 * 1000)

setTimeout(function(){      
    console.log(i);         
}, 1 * 1000)

...

setTimeout(function(){      
    console.log(i);         
}, 98 * 1000)

setTimeout(function(){      
    console.log(i);         
}, 99 * 1000)

```
setTimeout(function(), 0)
```javascript
// 0초 이지만 i= 100이다.
setTimeout(function(){      
    console.log(i);  //100
}, 0 * 1000)         // o초
```

## 8-11. 클로저 문제 해결법  
클로저 문제 해결 - 즉시실행함수  
```javascript                            
for( var i = 0; i< 100; i++){   
    setTimeout(function(){      
        console.log(i);         
    }, i * 1000)
}

// 클로저 문제 해결
for( var i = 0; i< 100; i++){   
    function 클로저(j){
        setTimeout(function(){      
            console.log(J);         
        }, j * 1000)
    }
    클로저(i);    
}

// 클로저 문제 해결 - 즉시 실행함수
for( var i = 0; i< 100; i++){   
    (function 클로저(j){
        setTimeout(function(){      
            console.log(J);         
        }, j * 1000)
    })(i)
    
}
```

## 8-12. 주변 칸 한 번에 열기(재귀)  
재귀함수 : 반복문을 함수로 표현
```javascript
function 재귀함수(숫자) {
    console.log(숫자);
    if(숫자 < 5){
        재귀함수(숫자 + 1);    
    }    
}

재귀함수(1); // 무한반복(stackoverFlow) >> if문으로 제어 
```

```javascript

...

//  undefined, 0, null을 제거하는 코드
// 주변칸.filter(function(v){ return !!v; }) 
주변칸.filter(function(v){ return !!v; }).forEach(function (옆칸){
    옆칸.click();
})
// 재귀함수를 사용하면 실행하는데 시간이 오래걸릴 수 있다.

...

```
## 8-13. 재귀 코드 효율 개선하기  
열린 칸들이 반복적으로 열림 문제
```javascript

... 

주변칸.filter(function(v){ return !!v; }).forEach(function (옆칸){
    // 조건추가
    if(dataset[옆칸줄][옆칸칸] !== 1){
        옆칸.click();
    }
})

...

```
재귀함수는 사람이 이해하기 쉽고, 컴퓨터가 이해하기 어려움
반복문은 사람이 이해하기 어렵고, 컴퓨터가 이해하기 쉬움

## 8-14. 에러 잡아내기  
```javascript
// || 주변지뢰개수가 거짓값이면 뒤에 꺼('')를 대신써라
// 거짓인값 : '', 0, NaN, null, undefined, false
e.currentTarget.textContent = 주변지뢰개수 || '';
```
중단플래그 : 코드의 흐름을 제어
```javascript
td.addEventListener('click', function(e){
    if(중단플래그){
        return; // 함수의 실행을 중간에 끊을 수 있다.
    }
})
```

## 8-15. 데이터 딕셔너리로 정리 
```javascript
var 코드표 = {
    옆칸 : -1,
    물음표: -2,
    깃발: -3,
    깃발지뢰: -4,
    물음표지뢰: -5,
    지뢰: 1,
    보통칸: 0
}
```

## 8-16. 남은 버그들 해결하기  
