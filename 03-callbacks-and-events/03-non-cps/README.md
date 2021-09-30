<p dir="rtl" align="right">
شرایط متعددی وجود دارد که حضور یک کال بک به ما بگوید یک تابع ناهمزمان است یا اینگه از CPS استفاده میکند. اما این همیشه درست نیست. مثلا، متد map یک شی ارایه را در نظر بگیرید:
</p>

```
const result = [1, 5, 7].map(element => element - 1)
console.log(result) // [0, 4, 6]
```

<p dir="rtl" align="right">
در این مثال، واضح است که کال بک فقط برای تکرار عناصر ارایه استفاده می شود. نه برای نتیجه کار. در واقع، نتیجه به طور همزمان با استفاده از یک سبک مستقیم بازگردانده می شود. هیچ تفاوت نحوی بین فراخوانی غیر CPS و فراخوانی CPS وجود ندارد بنابراین هدف از کال بک باید به وضوح در اسناد API ذکر شود. 
</p>

# 03-non-cps

This example demonstrates how callbacks are not always used to implement continuous-passing style.

## Run

To run the example launch:

```bash
node index
```
