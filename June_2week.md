## swiperJS
### `🥵이슈`_ [issue5782](https://github.com/nolimits4web/swiper/issues/5782)
>  swiperJS 8.2.2 사용중, slideLabelMessage null일 때, 슬라이더 충돌 오류가 있는 것 같다.
   -  [예시](https://codesandbox.io/s/swiper-navigation-forked-3uguc8?file=/index.html)
   ```javascript
   a11y.js:217 Uncaught TypeError: Cannot read properties of null (reading 'replace')
    at HTMLLIElement.eval (a11y.js:217:57)
    at eval (dom7.esm.js:774:14)
    at Dom7.forEach (<anonymous>)
    at Dom7.each (dom7.esm.js:773:8)
    at initSlides (a11y.js:214:19)
    at Swiper.eval (a11y.js:312:5)
    at eval (events-emitter.js:119:24)
    at Array.forEach (<anonymous>)
    at eval (events-emitter.js:118:37)
    at Array.forEach (<anonymous>)
   ```
   
> ### Accessibility(A11y)    
> slideLabelMessage: 스크린 리더기에게 현재 slide Element의 라벨을 알린다.     
> string 값으로 {{index}} / {{ slidesLength}}를 계산한 값
- [swiperJS Docs](https://swiperjs.com/swiper-api)

### `✨PR(fix)`_[slideLabelMessage:null](https://github.com/nolimits4web/swiper/pull/5783)
> fix #5782         
> slideLabelMessage은 {{index}} / {{slidesLength }} (기존 코드)       
> if문을 추가해서 param.slideLabelMessage가 있을 때만 ariaLabelMessage를 할당하도록 (반영된 pr) 추가했어          
> slideLabelMessage의 default 값이 null이어야 한다고 생각해.      
> 스크린 리더기를 위해 text를 덮어쓰는건 실용적이지 않아. 이번 PR에는 이 내용을 넣지는 않았지만..   


## quill
### `✨PR(enhance)`_[Improve Tab handling in code block](https://github.com/quilljs/quill/pull/3593)
> code Editor에서 영역을 잡아서 Tab을 누르는 경우가 더 흔해    
> 영역을 잡아서(collapsed selection) Tab을 누르면 CodeBlock Tab으로 작동해야하고,          
> Shift를 누르고, Tab을 누르면 Tab이 줄어들어야 하고,           
> 나머지는 지금처럼 작동해야해      

#### before     
```javascript
/* Keyboard Tab Handler */
handler(range) {
      const CodeBlock = this.quill.scroll.query('code-block');
      const lines =
        range.length === 0
          ? this.quill.getLines(range.index, 1)
          : this.quill.getLines(range);
      let { index, length } = range;
      lines.forEach((line, i) => {
        if (indent) {
          line.insertAt(0, CodeBlock.TAB);
          if (i === 0) {
            index += CodeBlock.TAB.length;
          } else {
            length += CodeBlock.TAB.length;
          }
        } else if (line.domNode.textContent.startsWith(CodeBlock.TAB)) {
          line.deleteAt(0, CodeBlock.TAB.length);
          if (i === 0) {
            index -= CodeBlock.TAB.length;
          } else {
            length -= CodeBlock.TAB.length;
          }
        }
      });
      this.quill.update(Quill.sources.USER);
      this.quill.setSelection(index, length, Quill.sources.SILENT);
    },
```


#### after
```javascript
 handler(range, { event }) {
      const CodeBlock = this.quill.scroll.query('code-block');
      if (range.length === 0 && !event.shiftKey) {
        this.quill.insertText(range.index, CodeBlock.TAB, Quill.sources.USER);
        this.quill.setSelection(
          range.index + CodeBlock.TAB.length,
          Quill.sources.SILENT,
        );
        return;
      }

      const lines =
        range.length === 0
          ? this.quill.getLines(range.index, 1)
          : this.quill.getLines(range);
      let { index, length } = range;
      lines.forEach((line, i) => {
        if (indent) {
          line.insertAt(0, CodeBlock.TAB);
          if (i === 0) {
            index += CodeBlock.TAB.length;
          } else {
            length += CodeBlock.TAB.length;
          }
        } else if (line.domNode.textContent.startsWith(CodeBlock.TAB)) {
          line.deleteAt(0, CodeBlock.TAB.length);
          if (i === 0) {
            index -= CodeBlock.TAB.length;
          } else {
            length -= CodeBlock.TAB.length;
          }
        }
      });
      this.quill.update(Quill.sources.USER);
      this.quill.setSelection(index, length, Quill.sources.SILENT);
    },
```


##### 변경사항
만약 range의 length가 0이고, shift키를 누른게 아니라면 선택한 전체 영역을 모두 insert 시킨다.    






