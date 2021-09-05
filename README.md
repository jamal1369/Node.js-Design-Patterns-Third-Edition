# Node.js Design Patterns - Third-Edition

<a href="https://www.nodejsdesignpatterns.com"><img width="240" align="right" src="https://github.com/lmammino/lmammino/blob/master/nodejsdp.jpg?raw=true"></a>

Node.js Design Patterns Third Edition (published by Packt), A book by Mario Casciaro and Luciano Mammino

### [🌎 Official website](https://www.nodejsdesignpatterns.com)

# <p dir="rtl" align="right">توضیحات فارسی مثال ها و تمرینات کتاب دیزاین پترن نودجی اس</p>

<p dir="rtl" align="right">در برنامه نویسی سنتی blocking I/O اجرای نخ تا اتمام عملیات ورودی و خروجی مسدود می شود</p>

```
// blocks the thread until the data is available
data = socket.read()
// data is available
print(data)
```


<p dir="rtl" align="right">راه حل در برنامه نویسی سنتی استفاده از چندین نخ یا پراسس است که مسدود شدن یک نخ بروی انجام دیگر نخ ها تاثیر منفی نخواهد داشت</p>

