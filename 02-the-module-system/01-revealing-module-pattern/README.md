<p dir="rtl" align="right">
  در این مثال تابع self-invoking نوشته ایم یعنی خودش را فراخوانی میکند
  </p>
  
  ```
  const myModule = (() => {
  const privateFoo = () => {}
  const privateBar = []
  const exported = {
    publicFoo: () => {},
    publicBar: () => {}
  }
  return exported
})() // once the parenthesis here are parsed, the function
     // will be invoked
console.log(myModule)
console.log(myModule.privateFoo, myModule.privateBar)
```

<p dir="rtl" align="right">
  متغیر myModule تنها چیزی است که از بیرون در دسترس است و در ان با return مشخص کرده ایم چه داده هایی را برگرداند
  </p>
  
  <p dir="rtl" align="right">
  داده های خروجی لاگ
  </p>
  
  ```
  { publicFoo: [Function: publicFoo],
  publicBar: [Function: publicBar] }
undefined undefined
```
<p dir="rtl" align="right">
  در این مثال از سیستم ماژول نویسی CommonJS استفاده شده است
  </p>
  
<p dir="rtl" align="right">
  سیستم ماژول نویسی CommonJS
  
  - Require => 
  اجازه می دهد یک ماژول را از سیستم فایل محلی وارد کنید
  
  - exports and module.exports => 
  برای خارج کردن قابلیت های عمومی از ماژول فعلی مورد استفاده قرار میگیرند
  
  
# 01-revealing-module-pattern

This example demonstrates the revealing module pattern.

## Run

To run the example launch:

```bash
node index.js
```

```nodejs
exports.loaded = false
const b = require('./b')

exports.test = "a"

module.exports = {
  b,
  loaded: true // overrides the previous export
}

```
