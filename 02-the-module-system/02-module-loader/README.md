<p dir='rtl' align='right'># توضیحات فارسی</p>

<p dir='rtl' align='right'>ساخت یک لودر شخصی برای ماژول ها</p>

<p dir='rtl' align='right'>
 برای درک عملکرد commonJS در این مثال یک سیستم مشابه رو می نویسیم
 </p>

 <p dir='rtl' align='right'>
 در این مثال عملکرد require() اصلی نودجی اس را تقلید می کنیم
</p>

```
function loadModule (filename, module, require) {
  const wrappedSrc =
    `(function (module, exports, require) {
      ${fs.readFileSync(filename, 'utf8')}
    })(module, module.exports, require)`
  eval(wrappedSrc)
}
```

<p dir="rtl" align="right">
 لیستی از متغیرها را به ماژول پاس داده ایم با module, exports, و require
</p>

<p dir="rtl" align="right">
  ما از readFileSync استفاده کرده ایم در حالت عادی استفاده از نسخه همزمان توابع سیستم فایل توصیه نمی شود ولی در اینجا منطقی است چون در commonJS بارگذاری ماژول ها عمدا عملیات همزمان است
 </p>
 <p dir="rtl" align="right">
 این باعث می شود اگر ماژول های متعدد وارد می کنیم وابستگی های انها  هم به ترتیب وارد شود
 </p>
 
  <p dir="rtl" align="right">
 دستوراتی مثل eval و دستورات ماژول vm می تواند خطرناک باشد و منجر به تزریق کد به برنامه ما شوند
 </p>
 
 <p dir="rtl" align="right">
 حالا require رو پیاده سازی می کنیم
 </p>
 
 ```
 function require (moduleName) {
  console.log(`Require invoked for module: ${moduleName}`)
  const id = require.resolve(moduleName)                   // (1)
  if (require.cache[id]) {                                 // (2)
    return require.cache[id].exports
  }
  // module metadata
  const module = {                                         // (3)
    exports: {},
    id
  }
  // Update the cache
  require.cache[id] = module                               // (4)
  // load the module
  loadModule(id, module, require)                          // (5)
  // return exported variables
  return module.exports                                    // (6)
}
require.cache = {}
require.resolve = (moduleName) => {
  /* resolve a full module id from the moduleName */
}
```

<p dir='rtl' align='right'>
 مراحل سیستم فایل خانگی ما
  </p>
 <p dir='rtl' align='right'>
 - نام ماژول به عنوان ورودی پذیرفته می شود مسیر کامل ماژول را که id می نامیم را resolve می کنیم
  </p>
  <p dir='rtl' align='right'>
 - اگر ماژول قبلا بارگذاری شده باشد و در حافظه کش باشد بلافاصله ان را برمیگردانیم
  </p>
   <p dir='rtl' align='right'>
 - اگر قبلا ماژول لود نشده باشد محیط را برای اولین لود اماده می کنیم یک ابجکت module می سازیم که حاوی exports است این ابجکت با کد ماژول پر می شود تا API های عمومی ان را export کند
  </p>
    <p dir='rtl' align='right'>
 - پس از بارگذاری اولیه، ابجکت ماژول کش می شود
  </p>
     <p dir='rtl' align='right'>
 - کد ماژول از فایل ان خوانده می شود ما ماژول را با ابجکت ماژول ارائه دادیم و یک ارجاعی به تابع require()  ارائه می دهد ماژول دستورات عمومی خود را با دستکاری یا جایگزینی در شی module.exports برمیگرداند
  </p>
      <p dir='rtl' align='right'>
 - در نهایت module.exports که نمایانگر API های عمومی ماژول است به صدا زننده بازگرداننده می شود
 </p>
 
 <p dir='rtl' align='right'>
 همه چیز داخل یک module خصوصی است مگر اینکه متغیر module.exports اختصاص داده شود محتوای این متغیر در حافظه کش قرار میگرد و بعد از لود شدن توسط require() بازگردانده می شود
 </p>
 
 ## module.exports versus exports
 
 <p dir='rtl' align='right'>
 متغیر exports فقط یک رفرنس از مقدار initial متغیر module.exports می باشد ما دیدیم که این مقدار در واقع یک ابجکت ساده می باشد که قبل از لود ماژول ساخته شده است
 </p>
 
 <p dir='rtl' align='right'>
 این یعنی ما فقط می توانیم ویژگی هایی جدیدی را به ایجکت مورد اشاره توسط متغیر exports وصل کنیم همانطور که در کد زیر می بینید:
 </p>
 
 ```
 exports.hello = () => {
  console.log('Hello')
}
 ```
 
 <p dir='rtl' align='right'>
 تخصیص دوباره متغیر exports هیچ تاثیری ندارد زیرا محتوای module.exports را تغییر نمی دهد بنابراین کد زیر اشتباه است
 </p>
 
 ```
 exports = () => {
  console.log('Hello')
}
 ```
 
 <p dir='rtl' align='right'>
 اگر بخواهیم چیزی غیر از یک ابجکت export کنیم مثل یک تابع یک instance و حتی یک رشته باید دوباره reassign کنیم  module.exports را مثل زیر:
 </p>
 
 ```
 module.exports = () => {
  console.log('Hello')
}
 ```
 
 
 
## Other Description 
<p dir='rtl' align='right'>در نود حی اس require یک تابع گلوبال می باشد. که دارای:</p>

```
require.resolve(fileName)
```

<p dir='rtl' align='right'>که مسیر فایل مورد نظر را برمیگرداند</p>


```nodejs
require.cache
```

<p dir='rtl' align='right'>برای کش کردن</p>

<p dir='rtl' align='right'>
تابع گلوبال require تا قبل از اینکه require شخصی سازی خودمون رو تعریف نکنیم کار میکنه و قابل دسترسی است ولی بعد از اینکه require سفارشی ما تعریف میشه (یک تابع سفارشی)‌ دیگه require اصلی و گلوبال رو نمی شناسه و require شخصی ما رو می شناسه.
 </p>

************************************
<p dir='rtl' align='right'>
برای ساخت و append داده به ابجکت؛
</p>

```nodejs

require.cache = {}

const module = {

 exports: {},
 
 id
 
}

require.cache[id] = module

```


# 02-module-loader

This sample demonstrate how to build a custom module system, which is 
partially compatible with Node.js's CommonJS module system.

## Run

To try it, run the loader and provide it with a module name, as you were requiring
it from inside a module, for example:

```bash
node loader.js ./main
```

The command above will instrument our homemade module loader
to use `main.js` as entry point. From that point on, all the modules
loaded will use our homemade version of `require` instead of the
default one.

File description: 
- `loader.js`:  the homemade module loader
- `main.js` the entry point of the application
- `moduleA.js`, `moduleB.js` two modules to illustrates the functionality of our `require`
