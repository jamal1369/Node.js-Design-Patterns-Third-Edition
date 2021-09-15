#### Asynchronous CPS

<P dir="rtl" align="right">
تابع زیر ناهمزمان است. 
</p>

```
function additionAsync (a, b, callback) {
  setTimeout(() => callback(a + b), 100)
}
```


  <P dir="rtl" align="right">
در کد بالا ما از setTimeout() برای شبیه سازی فراخوانی ناهمزمان توسط کال بک استفاده کرده ایم. (روش اجرای کال بک ناهمزمان را شبیه سازی کرده ایم) حال از این تابع استفاده می کنیم و نتایج را می بینیم: 
  </p>
  
console.log('before')
additionAsync(1, 2, result => console.log(`Result: ${result}`))
console.log('after')
```
  
<P dir="rtl" align="right">
خزوجی این تایع: 
  </p>
  
```before
after
Result: 3
```
  
<P dir="rtl" align="right">
از انجا که setTimeout() یک عملیات ناهمزمان را فعال میکند منتظر اجرای  کال بک نمی مانند. 
  </p>
  
![image](https://user-images.githubusercontent.com/45192069/133479013-217f660c-00f3-4642-8d81-87afb6f47849.png)

  <P dir="rtl" align="right">
پس از اتمام عملیات ناهمزمان، اجرا از سرگرفته می شود. به لطف closures حفظ CONTEXT کالر تابع ناهمزمان بی اهمیت است حتی اگر کال بک از نقطه متفاوتی از زمان و مکان صدا زده شود. 
  </p>
  
 <P dir="rtl" align="right">
خلاصه: یک تابع همزمان مسدود می شود تا عملیات خود را کامل کند. یک تابع ناهمزمان بلافاصله برمیگردد و نتیجه ان در چرخه بعدی حلقه رویداد به یک کنترل کننده (مثلا کال بک) منتقل می شود 
   </p>
  
# 02-async-cps

This example demonstrates asynchronous continuous passing with callbacks.

## Run

To run the example launch:

```bash
node index
```
