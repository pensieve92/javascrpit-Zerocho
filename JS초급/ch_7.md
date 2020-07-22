## 7-1. 가위바위보(이미지 스프라이트)  
이미지 스프라이트 : div에 background를 설정하고, 이미지의 일부를 짤라서 보여줌 
``` css
#imgDiv {
    width: 150px;
    height: 243px;
    background: url(https://en.pimg.jp/023/182/267/1/23182267.jpg) 0 0 ;    /* background:url() left top */
                                                                            /* 바위: 0 0 ,    가위: -142 0,    보: -284 0 */
}
```

```javascript
var left = 0;

setInterval(function (){    // 설정 시간마다 실행
    if(left === 0){
        left = -142;
    }else if(left === -142){
        left = -284;
    }else {
        left = 0;
    }

    document.querySelector('#computer').style.background = 'url(https://img.jpg) ' + left + ' 0';
}, 100); // 0.1초마다 실행
```
## 7-2. 딕셔너리 자료구조  

매번 0, -142, -284처럼 숫자를 사용해서 쓰다보면 헷갈리기 떄문에, 문자로 표현해서 사용위해 딕셔너리 구조를 사용한다.  
```javascript
var 이미지좌표 = '0';
var 가위바위보 = {    // 딕셔너리 자료구조
    바위: '0',
    가위: '-142px',
    보: '-284px'
} // 1대1 매칭 

setInterval(function (){
    if(이미지좌표 === 가위바위보.바위){
        이미지좌표 = 가위바위보.가위;
    }else if(이미지좌표 === 가위바위보.가위){
        이미지좌표 = 가위바위보.보;
    }else {
        이미지좌표 = 가위바위보.바위;
    }

    document.querySelector('#computer').style.background = 'url(https://img.jpg) ' + 이미지좌표 + ' 0';
}, 100); // 0.1초마다 실행

```
## 7-3. Object.entries, find, findIndex  
IE에서 지원안함..  
Object.entries(객체): 객체를 배열로 바꿀수 있음  
find, findIndex 는 2차원 배열에서 많이 사용  
indexOf는 1차원 배열에서 사용  
배열.find는 반복문이지만 원하는 것을 찾으면 (return이 true) 멈춥니다.  

```javascript
    var 가위바위보2 = {
        '0': '바위',
        '-142px': '가위',
        '-284px': '보'
    }

    // 가위바위보2를 하드코딩으로 만들지 않기워한 코딩!! 
    // Object.entries(객체)로 객체를 배열로 바꿀수 있음      
    Object.entries(가위바위보) // 2차원배열로 바꿔줌
    //return [['바위', '0'], ['가위', '-142px'], ['보', '-284px']]
    // 0 : ['바위', '0']
    // 1 : ['가위', '-142px']
    // 2 : ['보', '-284px']

    // 배열.find는 반복문이지만 원하는 것을 찾으면 (return dl true) 멈춥니다.
    // find도 반복문으로 생각하고, 반복문은 중첩은 2개를 넘지 않는 것이 좋다.
    Object.entries(가위바위보).find(function(v) {
        console.log(v);
        return v[0] === '바위'; // return ['바위', '0']
    })
    
    // 원하는 것의 index를 찾아줌
    Object.entries(가위바위보).findIndex(function(v) {
        console.log(v);
        return v[0] === '보'; // return 2
    })

    // 함수로 
    function 컴퓨터의선택(이미지좌표){
        return Object.entries(가위바위보).find(function(v) {
            return v[1] === 이미지좌표; // return ['바위', '0']
        })[0]
    }

```
## 7-4. setTimeout, clearTimeout  
## 7-5. 가위바위보 규칙 찾기  
## 7-6. 변수를 사용해 중복 제거하기  
