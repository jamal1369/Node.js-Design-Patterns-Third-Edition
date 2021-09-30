<p dir="rtl" align="right">
همزمان یا ناهمزمان؟؟
</p>

<p dir="rtl" align="right">
دیدیم که ترتیب اجرای دستورات بسته به ماهیت یک تابع - همزمان یا ناهمزمان - به طور اساسی تغییر می کند. این امر پیامدهای شدیدی در جریان کل برنامه دارد. هم از نظر صحت و هم از نظر کارایی. در زیر تجزیه و تحلیل این دو پارادایم و مشکلات ان امده است. از انچه باید اجتناب شود ایجاد ناسازگاری و سردرگمی، در مورد ماهیت API است. زیرا این امر می تواند منجر به مجموعه ای از مشکلات شود که تشخیص ان بسیار مشکل است. برای پیش بردن تجزیه و تحلیل خود، مثالی از یک تابع همزمان ناسازگار را در نظر می گیریم. 
</p>

<p dir="rtl" align="right">
- عملکرد غیرقابل پیش بینی
</p>


<p dir="rtl" align="right">
یکی از خطرناک ترین موقعیت ها داشتن یک API است که در شرایط خاص همزمان و در شرایط دیگر ناهمزمان رفتار میکند. یک مثال از این سناریو:
</p>

```
import { readFile } from 'fs'
const cache = new Map()
function inconsistentRead (filename, cb) {
  if (cache.has(filename)) {
    // invoked synchronously
    cb(cache.get(filename))
  } else {
    // asynchronous function
    readFile(filename, 'utf8', (err, data) => {
      cache.set(filename, data)
      cb(data)
    })
  }
}
```

<p dir="rtl" align="right">
تابع قبلی از cache map استفاده می کند تا نتایج خواندن از فایل های مختلف را ذخیره کند. این فقط یک مثال است (مدیریت خطا ندارد و منطق ذخیره سازی نیز بهینه نیست در فصل ۱۱ نحوه مدیریت صحیح ذخیره سازی ناهمزمان را می اموزید)
</p>


<p dir="rtl" align="right">
عملکرد تابع بالا خطرناک است زیرا تا زمانی که فایل برای اولین بار خوانده نشود و حافظه پنهان تنظیم نشود به صورت غیرهمزمان رفتار میکند. اما هنگامی که محتوای فایل در حافظه کش قرار گرفت برای همه درخواست های بعدی همزمان است. 
</p>


<p dir="rtl" align="right">
- رها کردن Zalgo
</p>


<p dir="rtl" align="right">
بیایید صحبت کنیم که چکونه استفاده از یک تابع غیرقابل پیش بینی، مانند تابعی که در بالا تعریف کردیم می تواند به راحتی یک برنامه را خراب کند. کد زیر را در نظر بگیرید:
</p>

```
function createFileReader (filename) {
  const listeners = []
  inconsistentRead(filename, value => {
    listeners.forEach(listener => listener(value))
  })
  return {
    onDataReady: listener => listeners.push(listener)
  }
}
```


<p dir="rtl" align="right">
هنگامی که تابع قبلی فراخوانی می شود، یک شی جدید ایجاد میکند که به عنوان یک اعلان عمل میکند و به ما امکان می دهد چندین شنونده را برای عملیات خواندن فایل تنظیم کنیم. پس از اتمام عملیات خواندن و در دسترس بودن داده ها، همه شنوندگان بلافاصله فراخوانی می شوند. تابع قبلی از تابع inconsistentRead() ما برای پیاده سازی این قابلیت استفاده میکند. بیایید ببینیم چگونه از تابع createFileReader() استفاده کنیم::
</p>

```
const reader1 = createFileReader('data.txt')
reader1.onDataReady(data => {
  console.log(`First call data: ${data}`)
  // ...sometime later we try to read again from
  // the same file
  const reader2 = createFileReader('data.txt')
  reader2.onDataReady(data => {
    console.log(`Second call data: ${data}`)
  })
})
```


<p dir="rtl" align="right">
کد قبلی مورد زیر را چاپ میکند:
</p>

> First call data: some data


<p dir="rtl" align="right">
همانطور که می بینید کال بک ریدر دوم هرگز فراخوانی نمی شود. بیا ببینیم چرا؟؟
</p>


<p dir="rtl" align="right">
- در هنگام ایجاد ریدر یک، تابع inconsistentRead() ما به صورت ناهمزمان رفتار میکند زیرا هیچ نتیجه ذخیره شده ای در دسترس نیست. این بدان معناست که هر شنونده onDataReady بعدا در چرخه دیگری از حلقه رویداد فراخوانی می شود. بنابراین ما تمام وقت را داریم تاشنونده خود را ثبت کنیم.
</p>


<p dir="rtl" align="right">
- سپس ریدر دو، در سیکلی از حلقه رویداد ساخته می شود که در ان کش برای فایل های درخواستی از قبل وجود دارد. در این حالت فراخواین داخلی inconsistentRead() همزمان خواهد بود. بنابراین کال بک ان بلافاصله فراخوانی می شود. به این معنی که همه شنوندگان خواننده دو نیز به طور همزمان فراخوانی می شوند. با این حال، ما در حال ثبت شنونده پس از ایجاد خواننده دو هستیم، بنابراین هرگز مورد استناد قرار نمیگیرد.
</p>


 




# 04-an-unpredictable-function

This example demonstrates how a function can behave synchronously under certain circumstances and asynchronously under other.

## Run

To run the example launch:

```bash
node index.js
```
