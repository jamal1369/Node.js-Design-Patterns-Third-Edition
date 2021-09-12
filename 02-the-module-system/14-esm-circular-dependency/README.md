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

# 13-esm-circular-dependency

This sample demonstrates that ESM can effectively resolve circular dependencies.

## Run

```bash
node main.js
```
