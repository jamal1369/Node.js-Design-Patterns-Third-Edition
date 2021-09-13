<p dir="rtl" align="right">
تفاوت ها و قابلیت های همکاری CommonJS و ESM:
</p>

<p dir="rtl" align="right">
تفاوت ها: نیاز به صراحت در تعیین فر مت فایل ها در ایمپورت در ESm و ... 
</p>

## ESM runs in strict mode

<p dir="rtl" align="right">
اجرای ES در حال سخت است. پس نیازی به نوشتن دستور "use strict"  در ابتدای هر فایل نیست. این مد رو نمی توانید غیرفعال کنید. بنابراین ما نمی توانیم از متغیرهای اعلان نشده یا دستور with یا ویژگی هایی دیگری استفاده کنیم. این خوب است چون اجرای سخت ایمن تر است. 
</p>

<p dir="rtl" align="right">
تفاوت های حالت سخت و غیرسخت:
</p>

> https://nodejsdp.link/strict-mode

## Missing references in ESM
 
<p dir="rtl" align="right">
در ES برخی مراجع مهم commonJs تعریف نشده است. شامل require و exports و module.exports و __filename و __direname می شود. اگر تلاش کنیم از هر کدام از اینها در ES  استفاده کنیم ReferenceError برخورد می کنیم.
</p>

```
console.log(exports) // ReferenceError: exports is not defined
console.log(module) // ReferenceError: module is not defined
console.log(__filename) // ReferenceError: __filename is not defined
console.log(__dirname) // ReferenceError: __dirname is not defined
```
 
<p dir="rtl" align="right">
موارد __filename و __dirname مسیر مطلق به فایل ماژول فعلی یا به دایرکتوری پدر را برمیگرداند. در ES این امکان وجود دارد تا به ادرس فایل جاری بوسیله ابجکت خاص import.meta دسترسی داشته باشیم. مورد import.meta.url  به file:///path/to/current_module.js اشاره دارد. این مقدار را می توان به جای __filename و __dirname استفاده کرد. 
</p>

```
import { fileURLToPath } from 'url'
import { dirname } from 'path'
const __filename = fileURLToPath(import.meta.url)
const __dirname = dirname(__filename)
```
 
<p dir="rtl" align="right">
همچنین می توان تابع require()  را به صورت زیر باز افرینی کرد:
</p>

``` 
import { createRequire } from 'module'
const require = createRequire(import.meta.url)
```
 
<p dir="rtl" align="right">
حالامی توانیم از require برای ایمپورت توابع ماژول های CommonJS در محیط ES استفاده کنیم. 
<p dir="rtl" align="right">
دیگر تفاوت در رفتار this  می باشد که در ES این کلمه در محدوده جهانی undefined می باشد در حالی که در CommonJS کلمه this یک ارجاعی به exports است. 
</p>

```
// this.js - ESM
console.log(this) // undefined
// this.cjs – CommonJS
console.log(this === exports) // true
```

## Interoperability
 
<p dir="rtl" align="right">
در قسمت قبل دیدید چگونه ماژول های commonjs را در محیط ES می توان با استفاده از تابع module.createRequire وارد کرد. همچنین امکان ایمپورت ماژول های commonJS از ES بوسیله استفاده از import استاندارد وجود دارد. این فقط در default exports کار میکند: 
</p>

```
import packageMain from 'commonjs-package' // Works
import { method } from 'commonjs-package'  // Errors
```
 
<p dir="rtl" align="right">
متاسفانه امکان ایمپورت ماژول های ES از ماژول های CommonJS وجود ندارد. همچنین ESM فایل های جیسون را نمی تواند مستقیما به عنوان ماژول ایمپورت کند. این ویژگی اغلب در commonJS مورد استفاده قرار می گیرد. دستور زیر ایمپورت نمی شود:
</p>

```
import data from './data.json'
```
 
<p dir="rtl" align="right">
این یک typeError (پسوند فایل ناشناخته است json) ایجاد می کند. برای غلبه بر این محدودیت می توانیم دوباره از ابزار module.createRequire استفاده کنیم:
</p>

```
import { createRequire } from 'module'
const require = createRequire(import.meta.url)
const data = require('./data.json')
console.log(data)
```



# 16-esm-cjs-interop

This sample demonstrates some ESM and CommonJS differences and ways to make the two systems work together.

This folder contains multiple examples:

 - `node filename.js` shows how to re-implement `__filename` and `__dirname` with ESM
 - `node import-json.js` shows how to import JSON modules with ESM
 - `node require.js` shows how to import CommonJS modules in ESM using `module.createRequire`
 - `node this.js` shows that `this` is `undefined` in ESM
