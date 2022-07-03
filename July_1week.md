### rxJS
#### ✨ PR _ `[Feature/add some operator](https://github.com/ReactiveX/rxjs/pull/6996)`
- pr이 close된 사례
##### 1) Request
> rxJS에 'some' 기능을 추가했으면 좋겠어!
> ```javascript
> /* example */
> import { of, some } from 'rxjs';
>  of(2, 4, 5, 9)
>  .pipe(some(x => x % 2 !== 0))
>  .subscribe(x => console.log(x)); // true
> ```

##### 2) Ans
> 이건 현재 first와 동일하지 않아? 
> ```javascript
> range(1, 10)
>  .pipe(first(v => v === 5))
>  .subscribe(console.log);
> ```

##### 3) Request
> `Array.prototype.find()`가 있지만 `Array.prototype.some()`이 있는 것 처럼 Obserable emit의 certain value를 확인하고 싶은 경우가 있다.

##### 3) Ans
> 우리 팀은 API를 더 늘리지 않기로 하고 있고, 별도의 npm 패키지로 올리는 것을 권장하고 있어.    
> because, operators를 추가하면 테스트, 문서화 지원 등의 추가 작업이 있고, operator가 어떤 선택이 더 나은 것인지 혼돈이 올 수 있기 때문.              
> example) `multicast`, `publish`, `publishBehavior`, `publishLast`, `publishReplay`는 매우 파워풀한 operator이지만,   
> 유저는 어떤 상황에서 어떤 operator가 유용한지 더 많은 시간을 할애해야 했다. 이는 `share` operator로 업데이트가 이뤄지고 더 간단해졌다.
