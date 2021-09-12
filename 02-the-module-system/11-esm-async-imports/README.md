## Module identifiers

<p dir="rtl" align="right">
شناسه های ماژول انواع مقادیری هستند که می توان در import ماژول برای تعیین محل ماژول استفاده کرد
</p>

<p dir="rtl" align="right">
- تعیین کننده های نسبی: مثل ./logger.js یا ../logger.js انها برای اشاره به یک مسیر نسبت به محل فایل استفاده می شوند
</p>

<p dir="rtl" align="right">
- تعیین کننده مطلق: مثل: file:///opt/nodejs/config.js  انها به طور مستقیم و صریح به یک مسیرکامل اشاره دارند. تنها راه برای دادن مسیر به صورت مطلق در ESM  همین است استفاده از یک / یا دوتا // در قبل مسیر به هیچ وجه کار نمیکند. (با commonJS متفاوت است)
</p>

<p dir="rtl" align="right">
- استفاده از نام ماژول مثل fastify یا http برای مسیردادن: که نشان دهنده ی ماژول های موجود در پوشه node_modules هستند و عموما از طریق npm نصب می شوند یا که به عنوان ماژول های اصلی نودجی اس در دسترس هستند.
</p>

<p dir="rtl" align="right">
- ایمپورت عمیق مثل fastify/lib/logger.js که به یک مسیر در node_modules اشاره دارد. (در این مورد به بسته fastify)
</p>

<p dir="rtl" align="right">
در محیط های مرورگر امکان وارد کردن مستقیم ماژول با تعیین URL ماژول، مثلا https://unpkg.com/lodash  وجود دارد که در NodeJS پشتیبانی نمی شود (در ری اکت و ... میشه استفاده کرد)
</p>

## Async imports

<p dir="rtl" align="right">
ورادات ماژول به صورت ایستاتیک انجام می شود. بنابراین دو محدودیت داریم:
</p>

<p dir="rtl" align="right">
- یک شناسه ماژول (روش های ایمپورت) نمی تواند در زمان اجرا ساخته شود
</p>

<p dir="rtl" align="right">
- واردات ماژول در سطح بالای هر فایل اعلام می شود و نمی توان انها را در دستورات کنترل جریان تو در تو قرار دارد.
</p>

<p dir="rtl" align="right">
مواردی وجود دارد که این محدودیت ها برای ما سخت می شود مثلا یک ماژول ترجمه کننده را براساس زبان کاربر فعلی لود کنیم یا  یک ماژولی که به سیستم عامل کاربر بستگی دارد را وارد کنیم. یا ماژولی پرحجم را فقط زمانی وارد کنیم که کاربر به ان نیاز دارد.
</p>

<p dir="rtl" align="right">
برای غلبه بر این محدودیت ها ESM واردات ASYNC یا واردات پویا را ارائه داده است.
</p>

<p dir="rtl" align="right">
واردات async رو در زمان اجرا با import() می توان انجام داد. 
</p>

<p dir="rtl" align="right">
عملگر import()  معادل یک تابع است که یک شناسه ماژول را به عنوان ارگومان دریافت کرده و یک پرامیس که شامل ابجکتی از ماژول هست را برمیگرداند.
</p>

<p dir="rtl" align="right">
مثال: برنامه خط فرمان درست کنیم که hello world رو به زبان های مختلف چاپ کند پس برای هر زیان یک فایل با ترجمه همه رشته ها رو داریم. ساخت چند ماژول نمونه برای زبان های مختلف:
</p>

```
// strings-el.js
export const HELLO = 'Γεια σου κόσμε'
// strings-en.js
export const HELLO = 'Hello World'
// strings-es.js
export const HELLO = 'Hola mundo'
// strings-it.js
export const HELLO = 'Ciao mondo'
// strings-pl.js
export const HELLO = 'Witaj świecie'
```

<p dir="rtl" align="right">
اسکریپت اصلی ما که براساس زبان کاربر یکی از ماژول ها رو لود می کند:
</p>

```
// main.js
const SUPPORTED_LANGUAGES = ['el', 'en', 'es', 'it', 'pl']   // (1)
const selectedLanguage = process.argv[2]                     // (2)
if (!SUPPORTED_LANGUAGES.includes(selectedLanguage)) {       // (3)
  console.error('The specified language is not supported')
  process.exit(1)
}
const translationModule = `./strings-${selectedLanguage}.js` // (4)
import(translationModule)                                    // (5)
  .then((strings) => {                                       // (6)
    console.log(strings.HELLO)
  })
  ```
  
  <p dir="rtl" align="right">
  ۱. لیستی از زبان های پشتیبانی شده تعریف می کنیم
  </p>
  
  <p dir="rtl" align="right">
  ۲. زبان کاربر رو از اولین ارگومان خط فرمان می خوانیم
  </p>
  
  <p dir="rtl" align="right">
  ۳. در نهایت، اگر زبان کاربر پشتیبانی نمی شد ان را هندل میکنیم
  </p>
  
  <p dir="rtl" align="right">
  ۴. به طور پویا، نام ماژولی که میخواهیم براساس زبان وارد کنیم را ایجاد میکنیم. توجه: نام ماژول باید یک مسیر نسبی برای فایل ماژول باشد برای همین از ./ استفاده می کنیم
  </p>
  
  <p dir="rtl" align="right">
  ۵. از import() برای وارد کردن پویا استفاده می کنیم
  </p>
  
  <p dir="rtl" align="right">
  ۶. واردات پویا به صورت ناهمزمان اتفاق می افتد بنابراین می توان از then برای کال بک استفاده کرد. بعد از لود کامل ماژول، کال بک اجرا می شود
  </p>
  
  <p dir="rtl" align="right">
  اجرای اسکریپت:
  </p>
  
  ```
  node main.js it
  ```
  
# 11-esm-async-imports

This sample demonstrates how to load modules dynamically with ESM

## Run

```bash
node main.js el
node main.js en
node main.js es
node main.js it
node main.js pl
node main.js pt # <- this one will fail
```
