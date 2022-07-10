## rxJS
### `🥵shareReplay with refCount `_[issue6760](https://github.com/ReactiveX/rxjs/issues/6760)             
shared observable을 여러번 구독하면 값 전달이 중지된다.
```javascript
import { BehaviorSubject, merge, of, Subject } from 'rxjs';
import {
  shareReplay,
  take,
  takeUntil,
  materialize,
  filter,
} from 'rxjs/operators';

const shared$ = new BehaviorSubject('7.5.1').pipe(
  shareReplay({ refCount: true, bufferSize: 1 })
);

const isCompleted$ = shared$.pipe(
  materialize(),
  filter((n) => n.kind === 'C'),
  take(1)
);

const work = new Subject().pipe(takeUntil(isCompleted$));

merge(work, shared$)
  .pipe(take(1))
  .subscribe((value) => console.log('first: ', value));

shared$.subscribe((value) => console.log('second:', value));
```

### `📑PR_Prevent setup/reset race condition in share with refCount`_ [#7005](https://github.com/ReactiveX/rxjs/pull/7005)
- share는 source를 구독하기 전에 재설정 구독을 추가한다.    
- 구독, 구독취소를 동시해 하면 -> 재설정 블록이 실행 될 수 있다.
- share을 실행 할 때, shareReplay 가 activate인지 확인   
- ![image](https://user-images.githubusercontent.com/63353110/178141907-81bfb850-9eb1-4d3c-928e-e7df4d82a8b5.png)


### 👀 rxJs - shareReplay란?
> Share source and replay specified number of emissions on subscription.
- 구독자가 여럿일 때 실행하고 싶지 않은 부담스러운 계산을 포함하고 있는 경우 사용
- shareReplay(configOrBufferSize?, windowTime?, scheduler?)
- bufferSize란? 캐시되고 재생된 항목의 수 ( https://cyworld.tistory.com/5423 )


##### 예시
```javascript
import { interval, take, shareReplay } from 'rxjs';
 
const shared$ = interval(2000).pipe(
  take(6),
  shareReplay(3)
);
 
shared$.subscribe(x => console.log('sub A: ', x));
shared$.subscribe(y => console.log('sub B: ', y));
 
setTimeout(() => {
  shared$.subscribe(y => console.log('sub C: ', y));
}, 11000);
 
// Logs:
// (after ~2000 ms)
// sub A: 0
// sub B: 0
// (after ~4000 ms)
// sub A: 1
// sub B: 1
// (after ~6000 ms)
// sub A: 2
// sub B: 2
// (after ~8000 ms)
// sub A: 3
// sub B: 3
// (after ~10000 ms)
// sub A: 4
// sub B: 4
// (after ~11000 ms, sub C gets the last 3 values)
// sub C: 2
// sub C: 3
// sub C: 4
// (after ~12000 ms)
// sub A: 5
// sub B: 5
// sub C: 5
```
