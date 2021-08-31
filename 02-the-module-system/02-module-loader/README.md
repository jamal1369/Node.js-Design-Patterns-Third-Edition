# توضیحات فارسی

در نود حی اس require یک تابع گلوبال می باشد. که دارای:

```
require.resolve(fileName)
```

که مسیر فایل مورد نظر را برمیگرداند

```nodejs
require.cache
```
کش کردن

تابع گلوبال require تا قبل از اینکه require شخصی سازی خودمون رو تعریف نکنیم کار میکنه و قابل دسترسی است ولی بعد از اینکه require سفارشی ما تعریف میشه (یک تابع سفارشی)‌ دیگه require اصلی و گلوبال رو نمی شناسه و require شخصی ما رو می شناسه. 

************************************
برای ساخت و append داده به ابجکت؛

‍‍‍```node
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
