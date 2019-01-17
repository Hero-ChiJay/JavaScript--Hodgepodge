# JavaScript--大杂烩
##### 说明（个人收录的一些文章跟自己的一些个人见解

### md 语法
<p align="left"><a href="https://github.com/younghz/Markdown" target="_blank" rel="noopener noreferrer"><img width="100" src="https://github.com/Hero-ChiJay/JavaScript--Hodgepodge/blob/master/images/md.png" alt="md logo"></a></p>

### 优化（if， switch）嵌套写法

```javascript
const actions = () => {
  const functionA = () => {/*Do sth*/}
  const functionB = () => {/*Do sth*/}
  const functionC = () => {/*Send log*/}
  return new Map([
    [/^guest_[1-4]$/,functionA],
    [/^guest_5$/,functionB],
    [/^guest_.*$/,functionC],
    //...
  ])
}

const onButtonClick = (identity,status) => {
  let action = [...actions()].filter(([key,value]) => (key.test(`${identity}_${status}`)))
  action.forEach(([key,value]) => value.call(this))
}
```

<p align="left">原文链接：<a href="https://juejin.im/post/5bdfef86e51d453bf8051bf8" target="_blank" rel="noopener noreferrer">JavaScript 复杂判断的更优雅写法</a></p>

### async function错误处理

利用async function做请求一些异步数据时，利用`try...catch`捕获错误，一般情况下错误先行，我们可以根据api接口返回的code进行判断；
错误先行， 先判断code是否符合，符合的进行数据操作， 不符合的直接抛出异常， 然后在catch统一处理

```javascript
async function Handle() {
  try {
    let result = await $http.getData(argument)
    if (result.code !== 0) throw Error(result.errorMsg) /*Error First*/
    /*Do sth*/
  } catch(error) {
    /*Unified processing error*/
    let error  = error.message || '默认错误信息'
    /*Do sth*/
  } finally {
    /*Do sth*/
  }
}
```
<p align="left">错误处理链接：<a href="http://javascript.ruanyifeng.com/grammar/error.html" target="_blank" rel="noopener noreferrer">错误处理机制</a></p>
<p align="left">了解async函数链接：<a href="http://es6.ruanyifeng.com/#docs/async" target="_blank" rel="noopener noreferrer">async 函数</a></p>

### 那些年我们的JavaScript注释都写规范了吗

<p align="left">腾讯参考文档：<a href="https://cloud.tencent.com/developer/section/1135849" target="_blank" rel="noopener noreferrer">valid-jsdoct</a></p>

<p align="left">JSDoc参考文档：<a href="http://usejsdoc.org" target="_blank" rel="noopener noreferrer">@use JSDoc</a></p>

### 在VUE中http请求参数加密

引入jsencrypt库
```javascript
npm i jsencrypt OR
yarn add jsencrypt
```
<p align="left">在线生成publicKey：<a href="http://travistidwell.com/jsencrypt/demo/" target="_blank" rel="noopener noreferrer">生成publicKey地址</a></p>
在vue的mainJS中混入,将生成的的秘钥放置在服务端上, 以下是演示放置出来
```javascript
import JsEncrypt from 'jsencrypt' //加密
Vue.mixin({//混入加密
  methods: {
    RSAencrypt(params){
      //实例化jsEncrypt对象
      const jse = new JsEncrypt()
      //设置公钥
      jse.setPublicKey(keys.publicKey)
      return jse.encrypt(params)
    },
    RSAdecrypt(params){ //解密
      const jse = new JsEncrypt()
      //设置私钥
      jse.setPrivateKey(keys.privateKey)
      return jse.decrypt(params)
    }
  }
})
```
全局混入使用
```javascript
/**
 * @params {Object} params 待加密的参数
 * @returns {String} secretParams 加密后的参数
 */
let secretParams = this.RSAencrypt(params)
```
局部混入，命名为a.js
```javascript
import JsEncrypt from 'jsencrypt' //加密
export default {
  methods: {
    RSAencrypt(params){
      //实例化jsEncrypt对象
      const jse = new JsEncrypt()
      //设置公钥
      jse.setPublicKey(keys.publicKey)
      return jse.encrypt(params)
    },
    RSAdecrypt(params){ //解密
      const jse = new JsEncrypt()
      //设置私钥
      jse.setPrivateKey(keys.privateKey)
      return jse.decrypt(params)
    }
  }
}
```
局部使用
```vue
<template>
</template>
<script>
import a from 'a.js'
export default {
  mixins: [a]
  method: {
    async login() {
    /**
     * @params {Object} params 待加密的参数
     * @returns {String} secretParams 加密后的参数
     */
    let secretParams = this.RSAencrypt(params)
    let rep = await $http.login(secretParams)
    }
  }
}
</script>
<style lang="scss" scoped>
</style>
```

