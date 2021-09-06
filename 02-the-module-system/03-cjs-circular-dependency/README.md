
## 

<p dir="rtl" align="right">
وابستگی دایره ای
</p> 

<p dir="rtl" align="right">
میخواهیم بدانیم CommonJS هنگام برخورد با وابستگی های دایره ای چکونه رفتار میکند. سناریوی زیر را داریم:
</p>

![image](https://user-images.githubusercontent.com/45192069/132248459-34f19159-7294-4fd6-b180-c5680ca7d116.png)


<p dir="rtl" align="right">
ماژولی به نام main.js نیاز به a.js و b.js دارد. 
</p>

<p dir="rtl" align="right">
خود a.js به b.js نیاز دارد
</p>

<p dir="rtl" align="right">
همچنین b.js به a.js نیاز دارد.
</p>

<p dir="rtl" align="right">
اگر کد را اجرا کنیم خروجی زیر را می بینیم:
</p>

```
a -> {
  "b": {
    "a": {
      "loaded": false
    },
    "loaded": true
  },
  "loaded": true
}
b -> {
  "a": {
    "loaded": false
  },
  "loaded": true
}
```

<p dir="rtl" align="right">
قسمت های مختلف برنامه ما نسبت به ترتیب بارگذاری وابستگیها، دید متفاوتی از انچه واقعا a  و b هستند خواهد داشت. در حالی که هردو ماژول به محض require از ماژول main کامل راه اندازی می شوند ولی ماژول a هنگام بارگیری از b ناقص خواهد بود. به طورخاص، وضعیت ان همان وضعیتی است که به لحظه require برای b داشته ایم. مسیر ماژول ها تااجرا:
</p>

![image](https://user-images.githubusercontent.com/45192069/132248475-dadedbe5-756c-4002-b6a5-68f521c77dbf.png)


<p dir="rtl" align="right">
۱. پردازش در main شروع می شود که بلافاصله به a نیاز دارد
</p>

<p dir="rtl" align="right">
۲. اولین کاری که a انجام میدهد این است که loaded را false میکند
</p>

<p dir="rtl" align="right">
۳. در این هنگام ماژول a به b نیاز دارد
</p>

<p dir="rtl" align="right">
۴. مانند a، اولین کار ماژول b این است که مقدار loaded را  false کند.
</p>

<p dir="rtl" align="right">
۵. در حال حاضر، b  به a (چرخه) نیاز دارد
</p>

<p dir="rtl" align="right">
۶. از انجایی که a قبلا پیموده شده است. ارزش فعلی ان بلافاصله در محدوده ماژول b کپی می شود
</p>

<p dir="rtl" align="right">
۷. ماژول b سرانجام مقدار بارگذاری شده را به true تغییر میدهد
</p>

<p dir="rtl" align="right">
۸. اکنون که b به طور کامل اجرا شده است. کنترل به a بازمیگردد که اکنون یک کپی از وضعیت فعلی ماژول b را در محدوده خود دارد
</p>

<p dir="rtl" align="right">
۹. اخرین مرحله از ماژول a این است که مقدار loaded زا بروی true تنظیم کند
</p>

<p dir="rtl" align="right">
۱۰. ماژول a هم اکنون کامل اجرا شده است. و کنترل به mainباز میگردد. که در حال حاضر یک کپی از وضعیت فعلی ماژول a در محدوده داخلی خود دارد
</p>

<p dir="rtl" align="right">

۱۱. ماژول main به b نیاز دارد که بلافاصله از حافظه کش بارگیری می شود
</p>

<p dir="rtl" align="right">

۱۲. وضعیت فعلی ماژول b در محدوده ماژول main کپی می شود جایی که در نهایت می توانیم تصویر کامل وضعیت هر ماژول را مشاهده کنیم. 
</p>

<p dir="rtl" align="right">

مشکل اینجاست که ماژول b دارای نمای جزیی از ماژول a است. و این نمای جزیی زمانی که b در main وارد میشود (require)، منتشر میبشود.  اگر ترتیب مورد نیاز دو ماژول را در main عوض کنید می بینید اینبار ماژول a است که نسخه ناقص b را دارد. 
</p>
<p dir="rtl" align="right">
روش ماژول نویسی ESM می تواند با وابستگی های دایره ای موثرتر برخورد کند. در commonJSفقط باید مراقب بود
</p>


# 03-cjs-circular-dependency

This sample demonstrates what happens when there are cycles in module
dependencies using CommonJS.

## Run

```bash
node main
```
