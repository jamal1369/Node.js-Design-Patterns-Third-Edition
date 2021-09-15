#### Asynchronous CPS

تابع زیر ناهمزمان است. 

```
function additionAsync (a, b, callback) {
  setTimeout(() => callback(a + b), 100)
}
```

در کد بالا ما از setTimeout() برای شبیه سازی فراخوانی ناهمزمان توسط کال بک استفاده کرده ایم. (روش اجرای کال بک ناهمزمان را شبیه سازی کرده ایم) حال از این تابع استفاده می کنیم و نتایج را می بینیم: 

console.log('before')
additionAsync(1, 2, result => console.log(`Result: ${result}`))
console.log('after')
```

خزوجی این تایع: 

```before
after
Result: 3
```

از انجا که setTimeout() یک عملیات ناهمزمان را فعال میکند منتظر اجرای  کال بک نمی مانند. 

![image](https://user-images.githubusercontent.com/45192069/133479013-217f660c-00f3-4642-8d81-87afb6f47849.png)


# 02-async-cps

This example demonstrates asynchronous continuous passing with callbacks.

## Run

To run the example launch:

```bash
node index
```
