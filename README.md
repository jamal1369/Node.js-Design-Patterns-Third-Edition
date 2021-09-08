# Node.js Design Patterns - Third-Edition

<a href="https://www.nodejsdesignpatterns.com"><img width="240" align="right" src="https://github.com/lmammino/lmammino/blob/master/nodejsdp.jpg?raw=true"></a>

Node.js Design Patterns Third Edition (published by Packt), A book by Mario Casciaro and Luciano Mammino

### [🌎 Official website](https://www.nodejsdesignpatterns.com)

# <p dir="rtl" align="right">توضیحات فارسی مثال ها و تمرینات کتاب دیزاین پترن نودجی اس</p>

## Blocking VS. Non-Blocking

<p dir="rtl" align="right">
 کارهای مثل تعامل با دیسک سخت و شبکه عملیات I/O می گویند
</p>

### Blocking

<p dir="rtl" align="right">
  زمانی که اجرای کدهای جاوااسکریپت به عملیات I/O می رسد باید منتظر بماند تا عملیات تمام شود. دلیل این امر:‌چون حلقه رویداد قادر به اجرای جاوااسکریپت در حین وقوع عملیات Blocking رو ندارد
</p>



<p dir="rtl" align="right">در برنامه نویسی سنتی blocking I/O اجرای نخ تا اتمام عملیات ورودی و خروجی مسدود می شود</p>

```
// blocks the thread until the data is available
data = socket.read()
// data is available
print(data)
```


<p dir="rtl" align="right">راه حل در برنامه نویسی سنتی استفاده از چندین نخ یا پراسس است که مسدود شدن یک نخ بروی انجام دیگر نخ ها تاثیر منفی نخواهد داشت</p>


![نخ ها در برنامه نویسی سنتی](https://user-images.githubusercontent.com/45192069/132132127-7eb646b3-36b0-453b-bfd0-97a4f02806b5.png)

<p dir="rtl" align="right">زبان نودجی اس از روش non-blocking I/O استفاده می کند )ناهمزمان) </p>

```
resources = [socketA, socketB, fileA]
while (!resources.isEmpty()) {
  for (resource of resources) {
    // try to read
    data = resource.read()
    if (data === NO_DATA_AVAILABLE) {
      // there is no data to read at the moment
      continue
    }
    if (data === RESOURCE_CLOSED) {
      // the resource was closed, remove it from the list
      resources.remove(i)
    } else {
      //some data was received, process it
      consumeData(data)
    }
  }
}
```

<p dir="rtl" align="right">در کد بالا سی پی یو زیادی مصرف می شود چون مجبور است بارها بررسی کند تا زمانی که یک منبع اماده شود کد بالا برای مدیریت غیرمسدود شده مناسب نیست یکی از روش های خوب برای غیرمسدود کننده استفاده از رویدادهای همزمان (synchronous event) که رابط اطلاع رسانی رویداد نیز نامیده می شود می باشد</p>


![image](https://user-images.githubusercontent.com/45192069/132132474-b67e4988-07bc-43e6-9c3e-da65e047ecec.png)


<p dir="rtl" align="right">
  ماژول یک برنامه کوچک قابل توسعه و تست می باشد. 
در نودجی است دو سیستم ماژول وجود دارد: یک:  CommonJS دو:  ECMAScript 
می‌توانیم با هر کدام که بخواهیم ماژول بنویسیم. 
  </p>
