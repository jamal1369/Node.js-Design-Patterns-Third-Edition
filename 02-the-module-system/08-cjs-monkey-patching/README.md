# Modifying other modules or the global scope

<p dir="rtl" align="right">
یک ماژول حتی می تواند چیزی صادر نکند. یک ماژول می تواند محدوده جهانی و هرابچکت موجود در ان، از جمله سایر ماژول ها در حافظه کش را تغییر دهد. (اینها روش های بد هستند) برای برخی شرایط مفید است (برای تست) یک ماژول دیگر ماژول ها را در محدوده جهانی می تواند تغییر دهد به این وصله میمون می گویند. تغییر ابجکت موجود در زمان اجرا، تغییر یا توسعه رفتار یا اعمال یک اصلاح موقت باشد. 
</p>

<p dir="rtl" align="right">
در زیر می بینید چطور می توان یک تایع به یک ماژول دیگر اضافه کرد:
</p>

```
// file patcher.js
// ./logger is another module
require('./logger').customMessage = function () {
  console.log('This is a new functionality')
}
```

<p dir="rtl" align="right">
استفاده:
</p>

```
// file main.js
require('./patcher')
const logger = require('./logger')
logger.customMessage()
```

<p dir="rtl" align="right">
در استفاده از این روش احتیاط کنید. اگر دو ماژول همزمان بخواهند یک ماژول را تغییر دهند کدام برنده می شود؟؟
</p>

<p dir="rtl" align="right">
یکی از ماژول هایی که از این روش استفاده می کند nock (nodejsdp.link/nock) می باشد. ماژولی که کمک میکند پاسخ های HTTP را دستکار یکنی. این ماژول به ماژول HTTP نود جی اس وصل شده و رفتار ان را تغییر میدهد. 
</p>




# 08-cjs-monkey-patching

This sample demonstrates how to monkey patch an existing module with CommonJS

## Run

```bash
node main
```
