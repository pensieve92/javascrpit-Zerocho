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
## 8-8. 스코프 체인  
## 8-9. 렉시컬 스코프  
## 8-10. 클로저  
## 8-11. 클로저 문제 해결법  
## 8-12. 주변 칸 한 번에 열기(재귀)  
## 8-13. 재귀 코드 효율 개선하기  
## 8-14. 에러 잡아내기  
## 8-15. 데이터 딕셔너리로 정리  
## 8-16. 남은 버그들 해결하기  

