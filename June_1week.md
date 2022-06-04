## swiperJS
### `🥵이슈`_ [issue5756](https://github.com/nolimits4web/swiper/issues/5756)
>  swiperJS 8.1.6 ~ 8.2.1버전 사용중이고,  window chrome에서 vue 프로젝트 내에서 autoplay를 샤용했더니, 정상 작동했지만 console에서 버그가 났어
   - autoplay Maximum call stack size exceede          
    ![image](https://user-images.githubusercontent.com/63353110/171977674-64bfdc67-7455-44ea-a6a9-479575b8f5cd.png)
   -  [예시](https://stackblitz.com/edit/vitejs-vite-6ep5ry?file=package.json,src%2FApp.vue&terminal=dev)

### `✨PR(fix)`_[fix/autoplay](https://github.com/nolimits4web/swiper/pull/5757)
> fix #5756
> autoplay에서 delay가 0일 때 바로 실행을 하는데(기존 코드)
> firstRun 변수를 만들어서 첫 생성일 때 true로 두고, delay가 0이면서 firstRun이 true일 때만 바로 실행하도록 하면
> 여러번 호출을 방지할 수 있어!
![image](https://user-images.githubusercontent.com/63353110/171977787-a3654bdf-e719-4870-8675-2828fc04dc47.png)


### `✨PR 이후 코드 정리`_maincontributor가 코드 정리한 것
> fix(autoplay): immediate proceed autoplay when `autoplay.delay = 0 `    
> 코드 정리      
> before: run() => proceed => cleartimeOut 후, delay가 0이면 바로 실행        
> aftrer: run() => cleartimeOut => 내부 로직을 timeout안에 넣음.         
![image](https://user-images.githubusercontent.com/63353110/171979093-f7bf5817-e7e6-4731-a210-5a4f4bf5e1d2.png)


## materialUI
### `👀이슈`_ [issue#32144](https://github.com/mui/material-ui/issues/32144)
> componentsProps가 현재는 plainObject를 받고 있어
```javascript
<InputUnstyled
  componentsProps={{
    root: { className: 'custom-root' },
    input: { className: 'custom-input' },
  }}
/>
```
> 이런 방법으로 바꾸면 좋을 것 같아, 
> why? 지금 현재 방법은 조건적으로 무언가를 바꾸고 싶을 때 번거로워. 
```javascript
<InputUnstyled
  componentsProps={{
    root: ({ focused }) => ({ className: focused ? 'custom-focus' : '' }),
    input: ({ error }) => ({ 'data-error': error }),
  }}
/>
```

### `✨PR(add)`_[SwitchUnstyled](https://github.com/mui/material-ui/pull/32993/files)
> Part of #32144.   
> Accept callbacks in componentsProps   
> object뿐 아니라 func도 들어올 수 있도록 코드 정리          
> switchUnstyled에서만 해당 스타일을 쓸 수 있도록 코드 업데이트     
> resolveComponentProps(기존에 있던 util)로 기존 componentProps을 감싸서 사용    
```javascript
/**
 * If `componentProps` is a function, calls it with the provided `ownerState`.
 * Otherwise, just returns `componentProps`.
 */
export default function resolveComponentProps<TProps, TOwnerState>(
  componentProps: TProps | ((ownerState: TOwnerState) => TProps) | undefined,
  ownerState: TOwnerState,
): TProps | undefined {
  if (typeof componentProps === 'function') {
    return (componentProps as (ownerState: TOwnerState) => TProps)(ownerState);
  }

  return componentProps;
}
```
![image](https://user-images.githubusercontent.com/63353110/171981362-cb33f848-c2e0-48b9-8ec9-e620c9cb444a.png)
![image](https://user-images.githubusercontent.com/63353110/171981377-d5682294-a638-4a97-903f-5fc3b69f2031.png)
![image](https://user-images.githubusercontent.com/63353110/171981388-53513383-e1a3-4d7f-9aad-8339fd7b2039.png)
![image](https://user-images.githubusercontent.com/63353110/171981405-e3cf55c1-71cc-4943-a4f0-1940381d5502.png)

