## debounce示意图

![[Untitled.png]]

## debounce函数示例代码

```jsx
function debounce(func, delay = 1000) {
  let timer = null;
  return function (...args) {
    if (timer) clearTimeout(timer);
    timer = setTimeout(() => {
      func.apply(this, args);
      timer = null;
    }, delay);
  };
}

function debounce2(func, { delay = 1000, immediate = false }) {
  let timer = null;
  if (immediate) {
    return function (...args) {
      let res = null;
      if (timer == null) res = func.apply(this, args);
      timer = setTimeout(() => {
        timer = null;
      }, delay);
      return res;
    };
  } else return debounce(func, delay);
}

function debounce3(func, delay = 1000, callback = null, immediate = true) {
  let timer = null;
  if (immediate) {
    return function (...args) {
      let res = null;
      if (timer == null) res = func.apply(this, args);
      timer = setTimeout(() => {
        callback instanceof Function && callback.call();
        timer = null;
      }, delay);
      return res;
    };
  } else return debounce(func, delay);
}

export { debounce, debounce2, debounce3 };
```