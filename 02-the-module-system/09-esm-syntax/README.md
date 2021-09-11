## ECMAScript Module

<p dir="rtl" align="right">
روش ماژول نویسی ECMAScript ساده تر است، پشتیبانی از وابستگی های چرخه ای و امکان بارگذاری ماژول ها به صورت غیرهمرمان (asynchronously) را دارد.
</p>

<p dir="rtl" align="right">
تفاوت بین ESM و CommonJS در این است که ماژول های ES ایستاتیک هستند. که معنی می دهد ایمپورت ماژول ها در بالای هر ماژول و بیرون از هرگونه جریان کنترلی  انجام می شود. همچنین نام ماژول های وارد شده را نمی توان به طور پویا در زمان اجرا با استفاده از عبارات ایجاد کرد. فقط رشته های ثابت مجاز هستند.
</p>

<p dir="rtl" align="right">
پس کد زیر در ES خطا دارد:
</p>

```
if (condition) {
  import module1 from 'module1'
} else {
  import module2 from 'module2'
}
```

<p dir="rtl" align="right">
در حالی که CommonJS در کد زیر خطا نمی دهد و اجرا میکند:
</p>

```
let module = null
if (condition) {
  module = require('module1')
} else {
  module = require('module2')
}
```

<p dir="rtl" align="right">
این ویژگی ES بعنی واردات استاتیک یکسری خوبی ها دارد که امکان تجزیه و تحلیل استاتیک درخت وابستگی را فراهم میکند که بهینه سازی هایی مانند حذف کد مرده (تکان دادن درخت) و موارد دیگر را ممکن میکند
</p>

<p dir="rtl" align="right">
نودجی اس هر فایل js را پبیش فرض با نحو CommonJS در نظرمیگیرد. اگر از نحو ES در فایل های js استفاده کنید خطا میگیرد. راههای که به نودجی اس بگویید فایل js را براساس ES تحلیل کن:
</p>

<p dir="rtl" align="right">
- دادن فرمت .mjs به فایل ماژول
</p>

<p dir="rtl" align="right">
- به نزدیکترین فایل package.json فیلدی به نام "type" با مقدار "module" اضافه کنید
</p>

<p dir="rtl" align="right">
نکته: از این بخش کتاب تا اخر کتاب کدها براساس ES نوشته می شود با فرمت js. پس حتما تنظیم مربوطه در فایل package.json انجام شود
</p>

<p dir="rtl" align="right">
ماژول های نوشته شده براساس  ESM از  export استفاده می‌کنند (برخلاف  CommonJS که از مدل جمع استفاده می کرد::  exports and module.exports)
</p>

<p dir="rtl" align="right">
در ماژول ES همه چیز خصوصی است مگر چیزی که export شود
</p>

<p dir="rtl" align="right">
مثال از ES:
</p>

```
// logger.js
// exports a function as `log`
export function log (message) {
  console.log(message)
}
// exports a constant as `DEFAULT_LEVEL`
export const DEFAULT_LEVEL = 'info'
// exports an object as `LEVELS`
export const LEVELS = {
  error: 0,
  debug: 1,
  warn: 2,
  data: 3,
  info: 4,
  verbose: 5
}
// exports a class as `Logger`
export class Logger {
  constructor (name) {
    this.name = name
  }
  log (message) {
    console.log(`[${this.name}] ${message}`)
  }
}
```

<p dir="rtl" align="right">
اگر موجودیت ها را از یک ماژول می خواهیم وارد کنیم می توان از import استفاده کرد.  می توان با import یک یا چند موجودیت را وارد کنیم. حتی نام ان را نیز تغییر دهیم.
</p>

```
import * as loggerModule from './logger.js'
console.log(loggerModule)
```

<p dir="rtl" align="right">
ستاره در مثال بالا namespace import نامیده می شود. که تمام عضوهای ماژول را ایمپورت و به متغیر محلی loggerModule اساین می کند.
</p>

<p dir="rtl" align="right">
چیزی که ایمپورت می شود در متغیر محلی:
</p>

```
[Module] {
  DEFAULT_LEVEL: 'info',
  LEVELS: { error: 0, debug: 1, warn: 2, data: 3, info: 4,
    verbose: 5 },
  Logger: [Function: Logger],
  log: [Function: log]
}
```

<p dir="rtl" align="right">
تمامی موجودیت های صادر شده ماژول در فضای نام loggerModule وجود دارد پس به تابع  log()  می توان از طریق loggerModule.log دسترسی داشت
</p>

<p dir="rtl" align="right">
برخلاف CommonJS باید فرمت فایل ها را منشخص کنیم. در CommonJS می توانستیم بنویسیم:  ./logger یا ./logger.js  ولی در ES حتما باید بنویسیم ./logger.js
</p>

<p dir="rtl" align="right">
در ماژول های بزرگ وقتی می خواهیم فقط چند تا تابع مشخص را ایمپورت کنیم (نه همه)
</p>

```
import { log } from './logger.js'
log('Hello World')

یا:

import { log, Logger } from './logger.js'
log('Hello World')
const logger = new Logger('DEFAULT')
logger.log('Hello world')
```

<p dir="rtl" align="right">
وقتی اینطور ایمپورت انجام می دهیم. موجودیت ها به محدوده فعلی وارد می شود و ریسک هم نامی موجودیت واردی وجود دارد. به عنوان مثال کد زیر کار نمی کند:
</p>

```
import { log } from './logger.js'
const log = console.log
```

<p dir="rtl" align="right">
اگر کد قبلی را اجرا کنیم به دلیل هم نام بودن خطای زیر را می دهد:
</p>

```
SyntaxError: Identifier 'log' has already been declared
```

<p dir="rtl" align="right">
می توانید با روش زیر مشکل ریسک هم نامی را رفع کنید
</p>

```
import { log as log2 } from './logger.js'
const log = console.log
log('message from log')
log2('message from log2')
``` 

## Default exports and imports

<p dir="rtl" align="right">
یکی از ویژگی های CommonJS امکان export یک موجودیت تکی بدون نام از طریق انتساب با module.exports بود. این ایده خوبی است چون اصل تک مسئولیتی در هر ماژول را تشویق می کند. در ES هم کاری مشابه از طریق چیزی که default export نامیده می شود می توان انجام د اد. 
</p>

```
// logger.js
export default class Logger {
  constructor (name) {
    this.name = name
  }
  log (message) {
    console.log(`[${this.name}] ${message}`)
  }
}
```

<p dir="rtl" align="right">
در مثال بالا نام Logger نادیده گرفته می شود. و موجودیت صادرشده تحت نام  default ثبت می شود. که این نام به روش خاصی اداره می شود و می توان ان را اینگونه وارد کرد:
</p>

```

// main.js
import MyLogger from './logger.js'
const logger = new MyLogger('info')
logger.log('Hello World')
```

<p dir="rtl" align="right">
تفاوت این روش با named ESM imports در این است که از انجا که default export  بدون نام دز نظر گرفته می شود پس می توانیم ان زا ایمپورت کرده و یک نام محلی دلخواه به ان اختصاص دهیم. نیازی به اکلاد نیست.
</p>

<p dir="rtl" align="right">
روش default export معادل named export با نام default است. با کد زیر این ادعا را می توان ثابت کرد:
</p>
```

// showDefault.js
import * as loggerModule from './logger.js'
console.log(loggerModule)
```

<p dir="rtl" align="right">
خروجی؛
</p>

```
[Module] { default: [Function: Logger] }
```

<p dir="rtl" align="right">
در روش بالا ما نمی توانیم default entity را به صورت صریح وارد کنیم. کد زیر شکست می خورد:
</p>

```
import { default } from './logger.js'
```

<p dir="rtl" align="right">
خطا به این دلیل است چون default نمی تواند به عنوان نام متغیر انتخاب شود. در مثال قبلی loggerModule.default مشکلی ندارد ولی نمی توانیم متغیری با نام default مستقیما در محدوده داشته باشیم
</p>



# 09-esm-syntax

This sample demonstrates the basic syntax of ESM modules

## Run

```bash
node main1.js
node main2.js
node main3.js
node main4.js
node main5.js
```
