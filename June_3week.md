## swiperJS
### `😏Discussions`
- Q&A
- polls
- Ideas
- Show and tell
- General

👀 답변을 기다리는 질문들..
[Nuxt with Swiper](https://github.com/nolimits4web/swiper/discussions/5805)
- Nuxt를 사용하면서 Vue 예제를 그대로 따라했으나 Error를 만났다.
```javascript
Loading...
ERROR in ./components/Tutorial.vue?vue&type=script&lang=js& (./node_modules/babel-loader/lib??ref--2-0!./node_modules/vue-loader/lib??vue-loader-options !./components/Tutorial.vue?vue&type=script&lang=js&)
Module not found: Error: Can't resolve 'swiper/css' in 'D:\team\jiangxue.official-website\components'
ERROR in ./components/Tutorial.vue?vue&type=script&lang=js& (./node_modules/babel-loader/lib??ref--2-0!./node_modules/vue-loader/lib??vue-loader-options !./components/Tutorial.vue?vue&type=script&lang=js&)
Module not found: Error: Can't resolve 'swiper/vue' in 'D:\team\jiangxue.official-website\components'
```

[NextJs breakpoints](https://github.com/nolimits4web/swiper/discussions/5776)
- Next에서 breakpoints옵션을 사용했더니 Error를 만났다.
```javascript
Error: Hydration failed because the initial UI does not match what was rendered on the server.
```


## react-query
### `📑Docs update PR`_[React-query Demo update](https://github.com/TanStack/query/pull/3700)
> react-query를 처음 시작할 때 , 전역 오류 헨들러를 추가하면 HMR이 중단됩니다.     
> queryClient를 useState Hook에 래핑하는 것이 합리적인 기본값인 것 같습니다.       
#### ans) 이 예제이서는 QueryClient는 appComponent 외부에서 정의되어 있습니다. 안정적으로 참조하고 있기 때문에, useState에 카피할 필요가 없습니다.   `close`


### `✨typescript PR`_[queryClient.setQueryData can return undefined](https://github.com/TanStack/query/pull/3657/files)
> setQueryData에 undefined 추가
