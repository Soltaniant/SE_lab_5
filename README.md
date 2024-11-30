## گام اول: تحلیل برنامه و یافتن نقاط نابهینگی

بر اساس نتیجه گزارشات profiling ابزار yourkit میزان زیادی از cpu در متود temp صرف شده است و همچنین از نظر استفاده از memory، متغیر محلی `Arraylist a` و به تبعش اشیا `Integer` ساخته شده برای محتویات آن بیش‌ترین میزان مصرف از حافظه را داشته‌اند. این میزان مصرف به حدی بود که سیستم کامپیوتری بنده در اجرای کامل آن دچار مشکل شد و خطای OutOfMemory خورد.
![before_fix1](https://github.com/user-attachments/assets/06204736-4924-44c3-bcd9-1ba79054ccd1)
![before_fix2](https://github.com/user-attachments/assets/94275055-528b-4090-9c09-034124ff0902)
![before_fix3](https://github.com/user-attachments/assets/a8e72411-46a9-4182-8f45-77af1a261674)

## گام دوم: اصلاح نابهینگی

دلایل نابهنیگی بسیار مشخص است و به طرق و میزان‌های مختلف می‌توان آن را بهبود داد:

1. میزان capacity آرایه ساخته شده با توجه به کد فعلی می‌توانست تعیین شود
2. باتوجه به اینکه `Arraylist` یک لیست از Object ها را در خود نگه‌داری می‌کند، لازم است عملیات `Boxing` روی تمام اعداد انجام شود. (یعنی از `int` به `Integer` تبدیل شوند)
3. می‌توانستیم برای این منظور از آرایه ساده‌ای از primitive های `int` استفاده کنیم.

در نهایت کد داده شده به نحوی تغییر داده شد تا این نقاط نابهینگی به طور کلی حذف شود.

نسخه جدید متود `temp`:

```java
 public static void temp() {
  int size = 10000 * 20000; // Total size needed
  int[] a = new int[size];

  int index = 0;
  for (int i = 0; i < 10000; i++) {
    for (int j = 0; j < 20000; j++) {
      a[index++] = i + j;
    }
  }
}
```

## گام سوم: پروفایلینگ بعد از اصلاح کد

همانطور که در تصویر مشخص است، کد به سرعت اجرا شده و به پایان رسیده است و در این میان، میزان مصرف منابع اندک و به نسبت خالت قبلی بسیار کمتر بوده است.
![Screenshot from 2024-11-29 16-43-45](https://github.com/user-attachments/assets/c404df69-6b46-49a9-be6c-efe19efe3471)
![Screenshot from 2024-11-29 16-43-54](https://github.com/user-attachments/assets/6274227c-3b09-4475-9dc2-700da3684cfc)

## گام چهارم: نوشتن برنامه‌ای جدید

در این گام مطابق خواسته سوال یک تابعی طراحی شده است که سبب مصرف بیش از حد منابع بشود. تابع `createRandomString` به صورت زیر به دنبال تولید متنی تصادفی به طول ۱ میلیون کاراکتر است:
‍‍‍```java
private static String createRandomString() {
String a = "";
for (int i = 0; i < 1_000_000; i++) {
a += String.valueOf(randomCharacter());
}
return a;
}

## گام پنجم: نتیجه پروفایلینگ برنامه

همانطور که انتظار می‌رود، به دلیل استفاده نامناسب از String که یک داده‌ساختار immutable یا به اصطلاح تغییرناپذیر است، میزان مصرف منابع این متود بالا خواهد بود.

![image1](\images\image1.png)
![image2](\images\image2.png)

## گام ششم: اصلاح متود `createRandomString`

با توجه به نکته‌ای که درباره داده‌ساختار String بیان کردیم، بهتر است از StringBuilder برای مواردی که تغییرات در آن زیاد رخ می‌دهند (مثل اینجا) استفاده کنیم. در نتیجه کد ما به صورت زیر در خواهد آمد:

```java
private static String createRandomString() {
        StringBuilder a = new StringBuilder();
        for (int i = 0; i < 1_000_000; i++) {
            a.append(randomCharacter());
        }
        return a.toString();
    }
```

## گام هفتم: پروفایلینگ برنامه جدید

همانطور که انتظار می‌رفت، با تغییر ایجاد شده، مدت زمان اجرا و منابع مصرفی برنامه به شدت کاهش یافت.

![image3](\images\image3.png)
![image4](\images\image4.png)
