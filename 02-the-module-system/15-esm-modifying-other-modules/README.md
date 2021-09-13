
<p dir="rtl" align="right">
اطلاح دیگر ماژول ها
</p>

<p dir="rtl" align="right">
موجودیت هایی که از طریق ماژول نویسی ES ایمپورت می شوند پیوندهای زنده فقط خواندنی هستند. بنابراین انها را از یک ماژول خارجی نمی توانیم reassign کنیم. 
</p>

<p dir="rtl" align="right">
در ES ما نمی توانیم ماژول های default و named رو از یک ماژول دیگر تغییر دهیم. اما اگر این binding یک ابجکت باشد ما می توانیم ابجکت رو با reassign کردن برخی از ویژگی هاش تغییر دهیم.
</p>

<p dir="rtl" align="right">
می خواهیم ماژولی بنویسیم که رفتار ماژول اصلی fs را تغییر دهد تا مانع از دسترسی ماژول به سیستم فایل شود و به جای ان داده های مسخره ای را برگرداند. 
</p>

```
// mock-read-file.js
import fs from 'fs'                                        // (1)
const originalReadFile = fs.readFile                       // (2)
let mockedResponse = null
function mockedReadFile (path, cb) {                       // (3)
  setImmediate(() => {
    cb(null, mockedResponse)
  })
}
export function mockEnable (respondWith) {                 // (4)
  mockedResponse = respondWith
  fs.readFile = mockedReadFile
}
export function mockDisable () {                           // (5)
  fs.readFile = originalReadFile
}
```

<p dir="rtl" align="right">
۱. اولین کار ایمپورت ماژول fs است. اکسپورت دیفالت ماژول fs یک ابجکت است که شامل مجموعه ای از توابع است که به ما امکان تعامل با سیستم فایل را می دهد..
</p>

<p dir="rtl" align="right">
۲. ما می خواهیم تابع readFile()  رو با پیاده سازی ساختگی جایگزین کنیم. ما پیاده سازی اصلی رو ذخیره میکنیم. همچنین مقدار mockedResponse را تعریف میکنیم که بعدا از ان استفاده کنیم
</p>

<p dir="rtl" align="right">
۳. تابع mockedReadFile()  نسخه ای که ما قرار اسیت به جای تابع اصلی ایجاد کنیم. این تابع کال بک را با مقدار فعلی mockedResponse صدا میزند. (این یک پیاده سازی ساده از تابع اصلی است. تابع اصلی قبل از ارگومان کال بک یک ارگومان اختیاری دارد که می تواند کدگذاری های مختلف را هندل کند)
</p>

<p dir="rtl" align="right">
۴. تابع mockEnable()  را برای فعال کردن تابع مسخره خودمون استفاده میکنیم. پیاده سازی اصلی با تابع مسخره ما عوض میشه. پیاده سازی مسخره ما، مقداری که از طریق ارگومان respondWith ارسال شده را برمیگرداند.
</p>

<p dir="rtl" align="right">
۵. در نهایت تابع mockDisable()  اکسپورت شده را استفاده کردیم تا تابع اصلی را فعال کنیم.
</p>

<p dir="rtl" align="right">
یک مثال ساده از استفاده از ماژول: 
</p>

```
// main.js
import fs from 'fs'                                          // (1)
import { mockEnable, mockDisable } from './mock-read-file.js'
mockEnable(Buffer.from('Hello World'))                       // (2)
fs.readFile('fake-path', (err, data) => {                    // (3)
  if (err) {
    console.error(err)
    process.exit(1)
  }
  console.log(data.toString()) // 'Hello World'
})
mockDisable()
```

<p dir="rtl" align="right">
بررسی انچه در این مثال اتفاق افتاده است:
</p>

<p dir="rtl" align="right">
۱. ماژول پیش فرض fs را ایمپورت می کنیم. ما ایمپورت کردیم default export  ماژول اصلی را دقیقا شبیه انجه در فایل mock-read-file.js انجام داده بودیم
</p>

<p dir="rtl" align="right">
۲. در اینجا ما تابع تقلبی را فعال میکنیم و برای هر فایل خوانده شده می خواهیم وجود رشته hello world در فایل را شبیه سازی کنیم
</p>

<p dir="rtl" align="right">
۳. در نهایت یک فایل را با یک مسیر الکی میخوانیم. که hello world توسط تابع ساختگی ما چاپ می شود و پس از فراخوانی تابع، با استفاده از mockDisable() دوباره نسخه اصلی تابع را فعال میکنیم.
</p>

<p dir="rtl" align="right">
 این روش کار می کند ولی شکننده است.ممکن است در جاهایی کار نکند. در سمت mock-read-file.js ما می توانیم دو ایمپورت زیر برای ماژول fs امتحان کنیم:
 </p>

```
import * as fs from 'fs' // then use fs.readFile
or
import { readFile } from 'fs'
```

<p dir="rtl" align="right">
هر دو ایمپورت معتبری هستند چون ماژول fs تمام توابع خود را named exports اکپسورت میکند (به غیراز default export که یک ابجکت با مجموعه ای از توابع به عنوان ویژگی می باشد)
</p>

<p dir="rtl" align="right">
مشکلات خاصی در دو دستور قبلی import وجود دارد:
</p>

<p dir="rtl" align="right">
- ما یک read only live binding در تابع readFile()  میگیریم. و از این رو، ما نمی توانیم ان را از ماژول خارجی تغییر دهیم. اگر اینکار را انجام دهیم خطایی در موقع reassign تایع readFile() میگیریم
</p>

<p dir="rtl" align="right">
- مشکل دیگر در سمت مصرف کننده با main.js ماست. جایی که ما از این دو سبک ایمپورت می توانیم استفاده کنیم. پس اگر کسی از این دو سبک استفاده کند دیگر کد سفارشی شده ما را نمی تواند استفاده کند و خطا می خورد.
</p>

<p dir="rtl" align="right">
دلیل استفاده نکردن از دو روش import بالا در این مثال، این است که کد مسخره ما فقط یک کپی تابع readFile() که در داخل ابجکت اکسپورت شده از default exportقرار دارد را تغییر می دهد. (نه تایع موجود که با named export در بالای ماژول اکسپورت شده است)
</p>

<p dir="rtl" align="right">
این مثال نشان می دهد که چگونه وصله میمون می تواند در ماژول ES می تواند پیچیده تر و غیرقابل اعتمادتر باشد. به هیمن دلیل فریمورک های تست مثل Jest ویژگی هایی ارائه می دهند تا بتوانیم با اطمینان بیشتری این کار را انجام دهیم (jest-mock)
</p>

<p dir="rtl" align="right">
رویکرد دیگری که برای mock modules می توان استفاده کرد تکیه بر hook های موجود در ماژول های هسته نود جی اس می باشد که module نامیده میشود (nodejsdp.link/module-doc) یک کتابخانه از این ماژول mocku نام دارد. (با بررسی کدش ببین چیکار میکنه)
</p>

<p dir="rtl" align="right">
همچنین می توانیم از تابع syncBuiltinESMExports() از module package استفاده کنیم. وقتی این تابع فراخوانی می شود مقادیر ویژگی ها در ایجکت default export دوباره دز معادل named exports  ترسیم میشود. که هرگونه تغییرات خارجی در عملکرد ماژول را حتی در named exports را نیز ممکن می سازد. 
</p>

```
import fs, { readFileSync } from 'fs'
import { syncBuiltinESMExports } from 'module'
fs.readFileSync = () => Buffer.from('Hello, ESM')
syncBuiltinESMExports()
console.log(fs.readFileSync === readFileSync) // true
```

<p dir="rtl" align="right">
ما از این روش می توانیم برای انعطاف پذیر کردن فایل سیستم کوچک مسخره (mock) خودمون بوسیله صدا زدن syncBuiltinESMExports()  بهره ببریم. 
</p>

<p dir="rtl" align="right">
توجه کنید که syncBuiltinESMExports() فقط برای ماژول های درونی نودجی اس مثل fs در این مثال کار میکند (ماژول هسته)
</p>



# 15-esm-modifying-other-modules

This sample demonstrates how an ESM module can be use to alter another module.

## Run

```bash
node main.js
```

## Extras

- `mock-read-file-wrong.js` has a wrong implementation of the mocking mechanism
- `main-wrong.js` has a wrong implementation of the main file where we end up importing the live bindings and not the actual mock function.
- `example-sync.js` shows how use the `syncBuiltinESMExports` functionality
- `mock-read-file-sync.js` fixes all the problems above by using `syncBuiltinESMExports`
