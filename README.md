# JavaScript--大杂烩

### mk 语法
<p align="left"><a href="https://github.com/younghz/Markdown" target="_blank" rel="noopener noreferrer"><img width="100" src="https://github.com/Hero-ChiJay/JavaScript--Hodgepodge/blob/master/images/md.png" alt="md logo"></a></p>

### 优化（if， switch）嵌套写法

```javascript
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  const functionC = ()=>{/*send log*/}
  return new Map([
    [/^guest_[1-4]$/,functionA],
    [/^guest_5$/,functionB],
    [/^guest_.*$/,functionC],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.test(`${identity}_${status}`)))
  action.forEach(([key,value])=>value.call(this))
}
```

<p align="left">原文链接：<a href="https://juejin.im/post/5bdfef86e51d453bf8051bf8" target="_blank" rel="noopener noreferrer">JavaScript 复杂判断的更优雅写法</a></p>