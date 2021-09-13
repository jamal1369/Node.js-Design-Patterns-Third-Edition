### Circular dependency resolution

<p dir="rtl" align="right">
مثال وابستکی های حلقه ای که با commonJS نوشتیم رو اینجا دوباره بررسی میکنیم:
</p>

```
// a.js
import * as bModule from './b.js'
export let loaded = false
export const b = bModule
loaded = true
// b.js
import * as aModule from './a.js'
export let loaded = false
export const a = aModule
loaded = true
```
<p dir="rtl" align="right">
ایمپورت این دو ماژول:
</p>

```
// main.js
import * as a from './a.js'
import * as b from './b.js'
console.log('a ->', a)
console.log('b ->', b)
```

<p dir="rtl" align="right">
دقت کنید در اینجا ما از JSON.stringify استفاده نکردیم چون خطای TypeError: Converting circular structure to JSON مس دهد. دلیلش: چون در ES این یک وابستگی دایره ای واقعی بین a.js و b.js است
</p>

<p dir="rtl" align="right">
خروجی:
</p>

```
a -> <ref *1> [Module] {
  b: [Module] { a: [Circular *1], loaded: true },
  loaded: true
}
b -> <ref *1> [Module] {
  a: [Module] { b: [Circular *1], loaded: true },
  loaded: true
}
```

<p dir="rtl" align="right">
در ES ماژول های a.js و b.js یک تصویر کامل از هم دارند. برخلاف commonJS که اطلاعات جزیی از هم داشتند. و می بینیم که همه loaded روی درست تنظیم شده است. همچنین b  درون a یک مرجع واقعی به همان نمونه b موجود در محدوده ی فعلی است. (و برعکس) به همین دلیل از JSON.stringify() برای سریال سازی این ماژول ها نیم توان استفاده کرد. اگر ترتیب واردات برای a.js و b.js را تغییر دهیم در خروجی هیچ تغییری نمیکند. (برخلاف commonjs)
</p>

<p dir="rtl" align="right">
فاز یک: تجزیه
</p>

  <p dir="rtl" align="right">
کد از نقطه ورود (main.js) بررسی می شود. مفسر فقط به دنبال دستورات import برای یافتن همه ماژول های لازم و بارگذاری سورس کد از فایل های ماژول است. نمودار وابستگی به طور عمیق بررسی می شود و هر ماژول فقط یکبار بازدید می شود. پس نمایی از وابستگی ها شبیه درخت ایجاد میکند. 
</p>

![image](https://user-images.githubusercontent.com/45192069/133125766-cba97fa9-d056-4413-a7c4-e46da46a80d1.png)

 <p dir="rtl" align="right">
مراحل مختلف فاز اول:
 </p>
 
 
 <p dir="rtl" align="right">
 ۱. از main.js اولین import ما را به a.js می برد
 </p>
 
 <p dir="rtl" align="right">
 ۲. در a.js ایمپورتی را مشاهده که به b.js اشاره دارد.
 </p>
 
 <p dir="rtl" align="right">
 ۳. در b.js ما همچنین ایمپورت a.js را داریم. اما چون a.js قبل بازدید شده است دوباره بررسی نمی شود
 </p>
 
 <p dir="rtl" align="right">
 ۴. در این مرحله اکتشاف به عقب برمیگردد. b.js ایمپورت دیگری ندارد بنابراین به a.js بارمیگردیم. که اینم ایمپورت دیگری ندارد پس به main.js برمی گردیم. که در اینجا ایمپورت دیگری را مشاهده که به b.js اشاره دارد ولی چون قبلا این ماژول بررسی شده نادیده گرفته می شود. 
  </p>
  
  
# 13-esm-circular-dependency

This sample demonstrates that ESM can effectively resolve circular dependencies.

## Run

```bash
node main.js
```
