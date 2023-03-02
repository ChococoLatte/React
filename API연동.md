## 1. 비동기 작업

특정 로직의 실행이 끝날 때까지 기다려주지 않고 나머지 코드를 먼저 처리하는 것
- 비동기로 처리하게 되면 웹 애플리케이션이 멈추지 않기 때문에 동시에 여러가지 요청을 처리할 수 있고 기다리는 과정에서 다른 함수를 호출할 수 있다. 

## 2. 콜백(Callback) 함수

### 2-(1). 콜백(Callback)이란

- 다른 함수가 실행을 끝낸 뒤 실행되는 함수
- 코드를 통해 명시적으로 호출하는 함수가 아니라, 함수를 등록해 놓은 후 어떤 이벤트가 발생하거나 특정 시점에 도달했을 때 시스템에서 시스템에서 호출하는 함수
- 파라미터로 함수를 전달받아, 함수의 내부에서 실행된다.

### 2-(2). 콜백함수 사용 시 주의할 점

- 콜백지옥(Callback Hell)
콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로깊어지는 현상

* 비동기적인 작업을 수행하기 위해 콜백함수를 익명함수로 전달하는 과정에서 생기는 콜백 지옥을 Promise,async/await,Generator 등을 사용해 방지할 수 있다.

## 3. Promise

처리에 성공했을 때 실행할 콜백 함수와 성공하지 않았을 때 실행할 콜백함수를 미리 약속하는 것
- 프로미스는 자바스크립트 비동기 처리에 사용되는 객체
- 자바스크립트에서 비동기 처리를 위해 사용한 Callback 함수의 에러/예외 처리의 어려움, 중첩으로 인한

복잡도 증가라는 단점을 해결하기 위해 프로미스 객체를 ES6에서 언어직 차원으로 지원

**※ 프로미스의 상태(states) : 프로미스의 처리 과정**

**1). Pending(대기): 비동기 처리 로직이 아직 완료되지 않은 상태**
**2). Fulfilled(이행): 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태**
**3). Rejected(실패): 비동기 처리가 실패하거나 오류가 발생한 상태**

- Pending(대기)
new Promise() 메서드를 호출하면 대기 (Pending) 상태가 된다.

 ``` javascript
new Promise();
 ```

new Promise() 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 resolve,reject이다.
 ``` javascript
new Promise(function(resolve,reject){

});
```

- Fulfilled(이행)
콜백 함수의 인자 resolve를 실행하면 이행(Fulfilled) 상태가 된다.

```javascript
new Promise(function(resolve,reject){
   resolve();
});
```

그리고 이행 상태가 되면 then()을 이용하여 처리 결과 값을 받을 수 있다.

```javascript
function getData(){
  return new Promise(function(resolve,reject){
      var data=100;
      resolve(data);
   });
}

getData().then(function(resolvedData){
  console.log(resolvedData);
});
``` 

- Rejected(실패)

콜백 함수 인자로 resolve와 reject를 사용할 수 있다. 여기서 reject를 호출하면 실패(Rejected) 상태가 된다.

```javascript
new Promise(function(resolve,reject){
 reject();
});
 ```

실패 상태가 되면 실패 처리의 결과 값을 catch()로 받을 수 있다.

``` javascript
function getData(){
  return new Promise(function(resolve,reject){
    reject(new Error("Request is failed"));
  });
}

getData().then().catch(function(err){
  console.log(err);
});
```

**<Promise 함수를 이용한 비동기 처리 예제>**

``` javascript
function getData(){
  return new Promise(function(resolve,reject){
    $.get('url',function(response){
      if(response){
        resolve(response);
      }
      reject(new Error("Request is Failed"));
    });
  });
}

getData().then(function(data){
  console.log(data);
}).catch(function(err){
  console.error(err);
});
```

## 4. async/await

async/await는 Promise를 더욱 쉽게 사용할 수 있도록 해주는 ES2017(ES8) 문법이다.

``` javascript
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function process() {
  console.log('안녕하세요!');
  await sleep(1000); // 1초쉬고
  console.log('반갑습니다!');
}

process();
```
async/ await 문법을 사용할 때에는, 함수를 선언할 때 함수의 앞부분에 async 키워드를 붙이고, Promise의 앞부분에 await를 넣어주면 해당 프로미스가 끝날 때까지 기다렸다가 다음 작업을 수행할 수 있다.

 
