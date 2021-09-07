
# named exports: 

<p dir="rtl align="right">
ابتدایی ترین روش برای افشای یک API عمومی استفاده از named exports است به این ترتیب ابجکت خروجی یک ظرف یا فضای نامی است که شامل مجموعه ای از توابع است:
</p>

<p dir="rtl align="right">
 کد زیر ماژولی را نشان می دهد که این الگو را پیاده سازی کرده است:
 </p>

``` 
 // file logger.js
exports.info = (message) => {
  console.log(`info: ${message}`)
}
exports.verbose = (message) => {
  console.log(`verbose: ${message}`)
}
```

<p dir="rtl align="right">
حالا توابع اکسپورت شده به عنوان خواص ماژول بارگذاری شده در دسترس هستند. مثل زیر:
</p>

```
// file main.js
const logger = require('./logger')
logger.info('This is an informational message')
logger.verbose('This is a verbose message')
```

<p dir="rtl align="right">
اکثر ماژول های هسته ی نودجی اس از این الگو استفاده می کنند. با این حال cOMMONjs  مشخصا فقط اجازه استفاده از متغیر exports را می دهد. از این رو الگوی named exports تنها مدلی است که واقعا با CommonJs سازگار است. استفاده از module.exports یک برنامه افزودنی است که توسط نودجی اس برای پشتیبانی از طیف وسیع تری از الگوهای تعریف ماژول تعریف شده است
</p>

## Exporting a function:

<p dir="rtl align="right">
یکی از محبوب ترین الگوهای تعریف ماژول که شامل اختصاص مجدد کل متغیر module.exports به یک تابع می شود. نقطه قوت اصلی این الگو این است که امکان می دهد فقط یک عملکرد واحد را در معرض دید قرار دهید. پس در این حالت فقط یک نقطه ورود مشخص برای ماژول داریم که درک رو راخت تر می کند. همچنین اصل سطح دید کوچکتر رو رعایت میکند. نام دیگر این روش تعریف ماژول substack pattern است. 
</p>

```
// file logger.js
module.exports = (message) => {
  console.log(`info: ${message}`)
}
```

<p dir="rtl align="right">
توسعه احتمالی دیگر این الگو استفاده از تابع صادر شده به عنوان یک فضای نام برای دیگر API های عمومی می باشد. این ترکیب خیلی قدرتمندی است چون هنوز یک نقطه ورودی به ماژول را به ما می دهد در کنارش هم این امکان را می دهد تا سایر ویژگی هایی که دارای موارد استفاده ثانویه یا پیشرفته هستند در معرض دید قرار دهیم. در زیر می بینید چگونه ماژولی که قبلا تعریف شده با استفاده از تابع صادرشده به عنوان یک فضای نام گسترش دهیم:
</p>

```
module.exports.verbose = (message) => {
  console.log(`verbose: ${message}`)
}
```

<p dir="rtl align="right">
استفاده:
</p>

```
// file main.js
const logger = require('./logger')
logger('This is an informational message')
logger.verbose('This is a verbose message')
```

<p dir="rtl align="right">
قانون SRP یا single-responsibility principle: هر ماژول باید یک مسئولیت در یک تابع واحد داشته باشد و این مسئولیت باید کاملا توسط ماژول محصور شده باشد
</p>

## Exporting a class:

<p dir="rtl align="right">
این روش شبیه روش قبلی است ولی اجازه می دهد از یک کلاس نمونه سازی کنیم . نمونه را گسترش و کلاس جدیدی بسازیم و نسبت به روش قبلی قدرت بیشتری دارد:
</p>

```
class Logger {
  constructor (name) {
    this.name = name
  }
  log (message) {
    console.log(`[${this.name}] ${message}`)
  }
  info (message) {
    this.log(`info: ${message}`)
  }
  verbose (message) {
    this.log(`verbose: ${message}`)
  }
}
module.exports = Logger
```

<p dir="rtl align="right">
استفاده:
</p>

```
// file main.js
const Logger = require('./logger')
const dbLogger = new Logger('DB')
dbLogger.info('This is an informational message')
const accessLogger = new Logger('ACCESS')
accessLogger.verbose('This is a verbose message')
```

## Exporting an instance:

<p dir="rtl align="right">
ما می توانیم از مکانیزم کش require() استفاده کنیم تا به اسانی یک stateful instances از یک سازنده یا یک فکتوری بسازیم که بتواند در ماژول های مختلف به اشتراک گداشته شود. 
</p>

```
// file logger.js
class Logger {
  constructor (name) {
    this.count = 0
    this.name = name
  }
  log (message) {
    this.count++
    console.log('[' + this.name + '] ' + message)
  }
}
module.exports = new Logger('DEFAULT')
```

<p dir="rtl align="right">
استفاده:
</p>

```
// main.js
const logger = require('./logger')
logger.log('This is an informational message')
```

<p dir="rtl align="right">
چون ماژول کش شده است. هرماژولی که به ماژول loggerنیاز دارد در و.اقع همیشه یک نمونه از شی را بازیابی میکند. بنابراین وضعیت ماژول به اشتراک گذاشته می شود. این الگو بسیار شبیه ی الگوی singleton است. با این حال، این روش منحصربه فرد بودن در کل اپ را تضمین نمیکند همانطور که در singleton همینطور بود. قبلا دیدیم که ممکن است یک ماژول چندین بار در درخت وابستگی نصب شود این منجر به موارد متعددی از یک ماژول یکسان می شود که همه در یک محیط اجرا می شوند. 
</p>

<p dir="rtl align="right">
یکی از نکات جالب این الگو این است که مانع از ایجاد  نمونه های جدید نمی شود حتی اگر به صراحت کلاس را صادر کنیم. در واقع می توانیم برای ساخت نمونه جدید از همان نوع، به property های سازنده تکیه کنیم. 
</p>

```
const customLogger = new logger.constructor('CUSTOM')
customLogger.log('This is an informational message')
```

<p dir="rtl align="right">
همانطور که می بینید با استفاده از logger.constructor() می توانیم اشیا جدید Logger را بسازیم. از این روش با احتیاط یا اصلا استفاده نکند اگر نویسنده کلاس تصمیم گرفته کلاس به طور صریح صادر نمی شود احتمالا میخواهد کلاس خصوصی باشد. 
</p>


 

                         
# 04-cjs-named-exports

This sample demonstrates how to use named exports with CommonJS

## Run

```bash
node main
```
