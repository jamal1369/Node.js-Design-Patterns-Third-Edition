<p dir='rtl' align='right'># توضیحات فارسی</p>

<p dir='rtl' align='right'>ساخت یک لودر شخصی برای ماژول ها</p>

<p dir='rtl' align='right'>
 برای درک عملکرد commonJS در این مثال یک سیستم مشابه رو می نویسیم
 
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
