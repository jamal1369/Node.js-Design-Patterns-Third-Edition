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
 
 
# 10-esm-default

This sample demonstrates the default import/export syntax of ESM modules

## Run

```bash
node main.js
node showDefault.js
```
