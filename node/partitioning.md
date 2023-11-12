# partitioning
- event loop는 메인라인 이후 각 phase(타이머, 폴, 체크)의 콜백(=microtask)을 직접 실행한다(이벤트루프가 별도의 스레드가 아닌 코드 실행되는 바로 그 스레드에서 동작함)
  - timer: setTimer, setInterval 의 콜백을 time 도달하면 실행
  - poll: os에 i/o 가 완료되었는지 확인해서 완료되면 해당 i/o작업의 콜백실행
  - check: setImmediate의 콜백을 실행
    - process.nextTick() 의 콜백은 메인라인의 마이크로태스크로 해당 tick 에 바로 실행, setImmediate는 한바퀴 지나야 실행
- 따라서 콜백이 다른 단계의 콜백 실행을 블락할 수도 있다
- 이는 partitioning 기법으로 해결가능하다
  - 재귀호출을 사용하여 일부 로직이 다음 tick에 실행되도록 처리하는 것
  - 더 복잡한 처리가 있는 경우 아예 다른 스레드에 위임할 수도 있다 (ChildProcess ..)

### 이벤트루프
![image](https://github.com/hotpineapple/TIL-Today-I-Learned-/assets/77835382/e07dfb40-4191-42b3-8eac-e0c9fd3b756d)


### 코드
- 다음은 공백으로 구분된 정수로 이루어진 큰 파일을 읽고 그중 최대값을 100ms 마다 나누어 계산하는 코드이다.
```javascript
(function fn() {
    let cnt=0;
    const fs = require('fs'); 
    let max = 0;
    const timeout = setInterval(()=>{
        if(cnt===50) clearInterval(timeout);
        console.log(`max=${max}`);
        cnt++;
    }, 100);
    let result = [];
    const unit = 1000;
    function help(from, to) {
        if(from >= result.length) return;
        for(let i = from; i < Math.min(result.length-1, to); i++){
            max = Math.max(max, parseInt(result[i]));
        }

        setImmediate(help.bind(null, to, to + unit));
    }
    fs.readFile('./input.txt', 'utf-8', (err, data) => {
        if(err) console.error(err);

        else {
            result = data.split(' ');
            help(0, unit);
        }
    });
})();
```

### 출처
- [node.js](https://nodejs.org/en/docs/guides/dont-block-the-event-loop#partitioning)
- [ibm developer](https://developer.ibm.com/tutorials/learn-nodejs-the-event-loop/)
