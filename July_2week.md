## rxJS
### `๐ฅตshareReplay with refCount `_[issue6760](https://github.com/ReactiveX/rxjs/issues/6760)             
shared observable์ ์ฌ๋ฌ๋ฒ ๊ตฌ๋ํ๋ฉด ๊ฐ ์ ๋ฌ์ด ์ค์ง๋๋ค.
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

### `๐PR_Prevent setup/reset race condition in share with refCount`_ [#7005](https://github.com/ReactiveX/rxjs/pull/7005)
- share๋ source๋ฅผ ๊ตฌ๋ํ๊ธฐ ์ ์ ์ฌ์ค์  ๊ตฌ๋์ ์ถ๊ฐํ๋ค.    
- ๊ตฌ๋, ๊ตฌ๋์ทจ์๋ฅผ ๋์ํด ํ๋ฉด -> ์ฌ์ค์  ๋ธ๋ก์ด ์คํ ๋  ์ ์๋ค.
- share์ ์คํ ํ  ๋, shareReplay ๊ฐ activate์ธ์ง ํ์ธ   
- ![image](https://user-images.githubusercontent.com/63353110/178141907-81bfb850-9eb1-4d3c-928e-e7df4d82a8b5.png)


### ๐ rxJs - shareReplay๋?
> Share source and replay specified number of emissions on subscription.
- ๊ตฌ๋์๊ฐ ์ฌ๋ฟ์ผ ๋ ์คํํ๊ณ  ์ถ์ง ์์ ๋ถ๋ด์ค๋ฌ์ด ๊ณ์ฐ์ ํฌํจํ๊ณ  ์๋ ๊ฒฝ์ฐ ์ฌ์ฉ
- shareReplay(configOrBufferSize?, windowTime?, scheduler?)
- bufferSize๋? ์บ์๋๊ณ  ์ฌ์๋ ํญ๋ชฉ์ ์ ( https://cyworld.tistory.com/5423 )


##### ์์
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
