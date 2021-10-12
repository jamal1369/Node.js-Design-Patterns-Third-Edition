<p dir="rel" align="right">
اشکالی که به تازگی مشاهده کرده اید برای شناسایی در یک برنامه واقعی بسیار پیچیده است. تصور کنید از یک عملکرد مشابه در سرور وب استفاده می کنید جایی که چندین درخواست همزمان می توانید داشته باشید. تصور کنید برخی از ان درخواست ها را بدون هیچ دلیل ظاهری و یا خطایی (خطا ثبت نشده) به حالت تعلیق در اورده اید. این را می توان یک نقص ناخوشایند دانست. 
این جور استفاده از عملکردهای غیرقابل پیش بینی شبیه zALGO می باشد.
</p>

### Using synchronous APIs

<p dir="rel" align="right">
درسی که از مثال ازاد کننده زالگو می توان گرفت این است که برای api ها ماهیت ان را مشخص کنید. -همزمان یا ناهمزمان- 
یک راه حل احتمالی برای تابع inconsistentRead() این است که ان را کاملا همزمان کنید. این امر امکان پذیر است زیرا nodejs مجموعه ای از API های سبک مستقیم همزمان را برای اکثر عملیات اولیه IO دارد. به عنوان مثال می توان از تابع fs.readFileSync() به جای همتای ناهمزمان ان استفاده کرد. کد به صورت زیر خواهد بود:
</p>

```
import { readFileSync } from 'fs'

const cache = new Map()

function consistentReadSync (filename){
 if(cache.has(filename)){
   return cache.get(filename)
 }else{
   const data = readFileSync(filename, 'utf8')
   cache.set(filename, data)
   return data
   }
}
```



# 05-fix-zalgo-with-sync-api

This example demonstrates how to fix Zalgo by making our unpredictable function synchronous.

## Run

To run the example launch:

```bash
node index.js
```
