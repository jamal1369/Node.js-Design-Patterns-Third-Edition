## Mixed exports

<p dir="rtl" align="right">
در ماژول نویسی ESM می توانید named exports و default export رو با هم ترکیب کنید
</p>

```
// logger.js
export default function log (message) {
  console.log(message)
}
export function info (message) {
  log(`info: ${message}`)
}
```

<p dir="rtl" align="right">
در کد بالا تابع log() به عنوان یک default export و تابع info() به صورت named export وجود دارد. توجه کنید که info() به صورت داخلی از log() استفاده کرده است.  نمی توانیم log() رو با default()  جایگزین کنیم و خطای Unexpected token default می خوره
</p>

<p dir="rtl" align="right">
اگر بخواهیم default export و یک یاچند named export رو ایمپورت کنیم ما از فرمت زیر می توانیم استفاده کنیم:
</p>

```
import mylog, {info} from './logger.js
```

<p dir="rtl" align="right">
جزيیات و تفاوت های بین default export و named exports:
</p>

<p dir="rtl" align="right">
- در برخی شرایط، اکسپورت default می تواند اعمال حذف کد مرده (تکان دادن درخت) را دشوار کند. یک ماژول فقط یک خروجی پیش فرض می تواند  داشته باشد که یک ابجکتی از تمامی توابع می باشد. وقتی این ابجکت دیفالت را وارد می کنیم پس کل توابع در قالب یک دیفالت می باشد پس هیچ بخشی رو از درخت نمی توان حذف کرد. 
</p>

<p dir="rtl" align="right">
در صورتی که بخواهید بیش از یک عملکرد رو نشان دهید از روش نام گذاری استفاده کنید. فقط در صورتی که یک عملکرد واحد داشته باشید از اکسپورت دیفالت استفاده کنید. هرچند این قانون الزامی نیست چون بیشتر ماژول های اصلی نودجی اس از نام گذاری شده و دیفالت به صورت ترکیبی استفاده میکنند 
</p> 

# 12-esm-mixed-exports

This sample demonstrates a mixed syntax with named and default import/export

## Run

```bash
node main.js
```
