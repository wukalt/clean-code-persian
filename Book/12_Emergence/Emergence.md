# پاک شدن از طریق طراحی پدیدار‌شونده

![classes image](img-12.1.png)

<div dir="rtl">

چه می‌شود اگر چهار قانون ساده وجود داشته باشد که با دنبال کردن آن‌ها بتوانی هنگام کار، طراحی‌های خوبی ایجاد کنی؟
چه می‌شود اگر با دنبال کردن این قوانین، به درک بهتری از ساختار و طراحی کدت برسی، و اجرای اصولی مثل SRP و DIP برایت آسان‌تر شود؟
چه می‌شود اگر این چهار قانون باعث شوند که طراحی‌های خوب خودشان به‌تدریج پدیدار شوند؟

بسیاری از ما فکر می‌کنیم که چهار قانون طراحی ساده‌ی کنت بک کمک زیادی به ایجاد نرم‌افزارهایی با طراحی خوب می‌کنند.
به گفته‌ی کنت، یک طراحی زمانی "ساده" است که از این قوانین پیروی کند:

- تمام تست‌ها را با موفقیت اجرا کند
- هیچ تکراری نداشته باشد
- منظور برنامه‌نویس را منتقل کند
- تعداد کلاس‌ها و متدها را به حداقل برساند
- این قوانین به ترتیب اهمیت بیان شده‌اند.


## قانون اول طراحی ساده: اجرای تمام تست‌ها

اول از همه، یک طراحی باید سیستمی تولید کند که همان‌طور که انتظار می‌رود عمل کند. ممکن است سیستمی روی کاغذ طراحی خیلی خوبی داشته باشد، اما اگر راه ساده‌ای برای اطمینان از این‌که واقعاً همان‌طور که باید کار می‌کند وجود نداشته باشد، آن طراحی زیر سوال می‌رود.

سیستمی که به‌طور کامل تست شده و همیشه تمام تست‌ها را پاس می‌کند، سیستمی تست‌پذیر است. این جمله واضح است، اما مهم هم هست. سیستم‌هایی که تست‌پذیر نیستند، قابل تأیید هم نیستند. می‌توان گفت سیستمی که نمی‌شود صحت آن را بررسی کرد، نباید به مرحله‌ی استفاده برسد.

خوشبختانه، تست‌پذیر کردن سیستم باعث می‌شود به سمت طراحی‌ای برویم که در آن کلاس‌ها کوچک و تک‌منظوره باشند. چون تست‌ کردن کلاس‌هایی که از قانون SRP پیروی می‌کنند، راحت‌تر است. هرچه بیشتر تست بنویسیم، بیشتر به سمت کدی می‌رویم که راحت‌تر قابل تست است.

بنابراین، مطمئن شدن از اینکه سیستم ما کاملاً تست‌پذیر است، کمک می‌کند طراحی بهتری داشته باشیم.

وابستگی زیاد بین کلاس‌ها (tight coupling) تست نوشتن را سخت می‌کند. پس هرچه بیشتر تست بنویسیم، بیشتر از اصولی مثل DIP و ابزارهایی مثل تزریق وابستگی، رابط‌ها (interfaces) و انتزاع (abstraction) استفاده می‌کنیم تا وابستگی‌ها را کم کنیم. این باعث می‌شود طراحی ما بهتر شود.

نکته‌ی جالب اینجاست که با دنبال کردن یک قانون ساده و واضح که می‌گوید باید تست داشته باشیم و مدام آن‌ها را اجرا کنیم، به هدف‌های اصلی برنامه‌نویسی شی‌گرا یعنی وابستگی کم و انسجام بالا نزدیک‌تر می‌شویم. نوشتن تست باعث طراحی بهتر می‌شود.

## قوانین دوم تا چهارم طراحی ساده: بازسازی (Refactoring)

وقتی تست‌ها را داریم، می‌توانیم راحت‌تر کدها و کلاس‌ها را تمیز نگه داریم. این کار را با بازسازی تدریجی (refactor) کد انجام می‌دهیم. بعد از هر چند خط کدی که اضافه می‌کنیم، مکث می‌کنیم و به طراحی جدید فکر می‌کنیم. آیا طراحی را خراب کردیم؟ اگر این‌طور است، آن را تمیز می‌کنیم و تست‌ها را اجرا می‌کنیم تا مطمئن شویم چیزی خراب نشده.

داشتن تست‌ها باعث می‌شود نترسیم از اینکه با تمیز کردن کد، چیزی خراب شود!

در مرحله‌ی بازسازی می‌توانیم از تمام دانش مربوط به طراحی خوب نرم‌افزار استفاده کنیم:
می‌توانیم انسجام را بیشتر کنیم، وابستگی‌ها را کمتر کنیم، مسئولیت‌ها را جدا کنیم، بخش‌های مختلف سیستم را ماژولار کنیم، توابع و کلاس‌ها را کوچکتر کنیم، اسم‌های بهتری انتخاب کنیم و ...

در همین مرحله است که سه قانون نهایی طراحی ساده را اجرا می‌کنیم:
حذف تکرار، بیان واضح منظور، و کم‌کردن تعداد کلاس‌ها و متدها.

## بدون تکرار (No Duplication)

تکرار، دشمن اصلی یک سیستم با طراحی خوب است. تکرار یعنی کار اضافه، ریسک بیشتر، و پیچیدگی غیرضروری.

تکرار می‌تواند به شکل‌های مختلفی خودش را نشان دهد. خط‌های کدی که دقیقاً شبیه هم هستند، به‌وضوح تکرار محسوب می‌شوند.
خط‌های کدی که فقط شبیه به هم هستند هم اغلب می‌توانند طوری بازنویسی شوند که شبیه‌تر شوند و بعد راحت‌تر Refactor شوند.

تکرار فقط محدود به کدهای شبیه‌به‌هم نیست. مثلاً ممکن است تکرار در پیاده‌سازی (implementation) هم وجود داشته باشد. برای مثال، شاید در یک کلاس مجموعه (collection class) دو متد مختلف داشته باشیم که : 

</div>

```java
int size() {}
boolean isEmpty() {}
```

<div dir="rtl">

ممکن است برای هر متد، پیاده‌سازی جداگانه‌ای داشته باشیم.
مثلاً متد `isEmpty` می‌تواند یک مقدار بولی (`boolean`) را دنبال کند، در حالی که `size` یک شمارنده (`counter`) را دنبال کند.

اما می‌توانیم این تکرار را حذف کنیم، با این روش که `isEmpty` را به تعریف `size` وابسته کنیم. یعنی مثلاً بگوییم:
اگر `size` صفر است، پس مجموعه خالی است.

</div>

```java
boolean isEmpty() {
    return 0 == size();
}
```

<div dir="rtl">

ساختن یک سیستم تمیز نیاز به اراده برای حذف تکرار دارد، حتی اگر فقط در چند خط کد باشد.
برای مثال، به کد زیر توجه کن:

</div>


```java
public void scaleToOneDimension(
    float desiredDimension, float imageDimension) {
    if (Math.abs(desiredDimension - imageDimension) < errorThreshold)
        return;
    float scalingFactor = desiredDimension / imageDimension;
    scalingFactor = (float)(Math.floor(scalingFactor * 100) * 0.01 f);
    RenderedOp newImage = ImageUtilities.getScaledImage(
        image, scalingFactor, scalingFactor);
    image.dispose();
    System.gc();
    image = newImage;
}
public synchronized void rotate(int degrees) {
    RenderedOp newImage = ImageUtilities.getRotatedImage(
        image, degrees);
    image.dispose();
    System.gc();
    image = newImage;
}
```

<div dir="rtl">

برای اینکه این سیستم تمیز بماند، باید مقدار کمی از تکراری که بین متدهای `scaleToOneDimension` و `rotate` وجود دارد را حذف کنیم.

</div>

```java
public void scaleToOneDimension(
    float desiredDimension, float imageDimension) {
    if (Math.abs(desiredDimension - imageDimension) < errorThreshold)
        return;
    float scalingFactor = desiredDimension / imageDimension;
    scalingFactor = (float)(Math.floor(scalingFactor * 100) * 0.01 f);
    replaceImage(ImageUtilities.getScaledImage(
        image, scalingFactor, scalingFactor));
}
public synchronized void rotate(int degrees) {
    replaceImage(ImageUtilities.getRotatedImage(image, degrees));
}
private void replaceImage(RenderedOp newImage) {
    image.dispose();
    System.gc();
    image = newImage;
}
```

<div dir="rtl">

وقتی اشتراک‌های کد را حتی در این سطح خیلی کوچک استخراج می‌کنیم، کم‌کم متوجه نقض قانون `SRP` می‌شویم.
در نتیجه، ممکن است متدی که استخراج کرده‌ایم را به کلاس دیگری منتقل کنیم. این کار باعث می‌شود آن متد بیشتر دیده شود.

ممکن است یکی از اعضای تیم این متد جدید را ببیند و متوجه شود که می‌توان آن را بیشتر انتزاعی کرد و در جای دیگری هم استفاده کرد.
این نوع "استفاده‌ی دوباره در مقیاس کوچک" می‌تواند پیچیدگی سیستم را به‌طور چشم‌گیری کاهش دهد.
یاد گرفتن اینکه چطور می‌توان استفاده‌ی مجدد در مقیاس کوچک را انجام داد، برای رسیدن به استفاده‌ی مجدد در مقیاس بزرگ ضروری است.

الگوی `TEMPLATE METHOD` یکی از روش‌های رایج برای حذف تکرار در سطوح بالاتر است.
برای مثال:

</div>

```java
public class VacationPolicy {
    public void accrueUSDivisionVacation() {
        // code to calculate vacation based on hours worked to date
        // ...
        // code to ensure vacation meets US minimums
        // ...
        // code to apply vaction to payroll record
        // ...
    }
    public void accrueEUDivisionVacation() {
        // code to calculate vacation based on hours worked to date
        // ...
        // code to ensure vacation meets EU minimums
        // ...
        // code to apply vaction to payroll record
        // ...
    }
}
```

<div dir="rtl">

کدهای متدهای `accrueUSDivisionVacation` و `accrueEuropeanDivisionVacation` بیشتر شبیه به هم هستند، با این تفاوت که محاسبه حداقل‌های قانونی در هرکدام متفاوت است.
این قسمت از الگوریتم بسته به نوع کارمند تغییر می‌کند.

می‌توانیم با استفاده از الگوی `TEMPLATE METHOD` تکرار آشکار را حذف کنیم.

</div>

```java
abstract public class VacationPolicy {
    public void accrueVacation() {
        calculateBaseVacationHours();
        alterForLegalMinimums();
        applyToPayroll();
    }
    private void calculateBaseVacationHours() { /* ... */ };
    abstract protected void alterForLegalMinimums();
    private void applyToPayroll() { /* ... */ };
}
public class USVacationPolicy extends VacationPolicy {
    @Override protected void alterForLegalMinimums() {
        // US specific logic
    }
}
public class EUVacationPolicy extends VacationPolicy {
    @Override protected void alterForLegalMinimums() {
        // EU specific logic
    }
}
```

<div dir="rtl">

زیرکلاس ها (subclasses) "حفره" موجود در الگوریتم `accrueVacation` را پر می‌کنند و تنها اطلاعاتی را ارائه می‌دهند که تکرار نمی‌شوند.

## بیان واضح (Expressive)

بیشتر ما تجربه کار با کد پیچیده و درهم‌ریخته را داشته‌ایم. بسیاری از ما خودمان کدهای پیچیده‌ای نوشته‌ایم. نوشتن کدی که خودمان می‌فهمیم راحت است، چون در لحظه نوشتن کد، درک عمیقی از مشکلی که در حال حل آن هستیم داریم. اما کسانی که بعداً کد را نگهداری می‌کنند، این درک عمیق را نخواهند داشت.

بیشترین هزینه در یک پروژه نرم‌افزاری مربوط به نگهداری بلندمدت آن است. برای اینکه پتانسیل بروز خطاها را هنگام اعمال تغییرات کاهش دهیم، حیاتی است که بتوانیم بفهمیم سیستم چه کاری انجام می‌دهد. هرچه سیستم پیچیده‌تر می‌شود، زمان بیشتری برای درک آن نیاز است و شانس بروز سوء‌فهم بیشتر می‌شود. بنابراین، کد باید به‌وضوح منظور نویسنده‌اش را بیان کند. هرچه کد برای نویسنده واضح‌تر باشد، زمان کمتری دیگران باید برای درک آن صرف کنند. این کار باعث کاهش خطاها و کاهش هزینه‌های نگهداری می‌شود.

شما می‌توانید با انتخاب نام‌های مناسب خودتان را بیان کنید. می‌خواهیم وقتی نام یک کلاس یا تابع را می‌شنویم، وقتی که مسئولیت‌های آن را کشف می‌کنیم، شگفت‌زده نشویم.

همچنین می‌توانید با کوچک نگه‌داشتن توابع و کلاس‌های خود، خودتان را بیان کنید. کلاس‌ها و توابع کوچک معمولاً راحت‌تر نام‌گذاری می‌شوند، راحت‌تر نوشته می‌شوند و راحت‌تر قابل درک هستند.

استفاده از اصطلاحات استاندارد هم می‌تواند به بیان شما کمک کند. به عنوان مثال، الگوهای طراحی عمدتاً در مورد ارتباطات و بیان واضح هستند. با استفاده از نام‌های استاندارد الگوها مانند `COMMAND` یا `VISITOR` در نام کلاس‌هایی که این الگوها را پیاده‌سازی می‌کنند، می‌توانید طراحی خود را به طور مختصر برای سایر توسعه‌دهندگان توضیح دهید.

تست‌های واحد خوب نوشته شده نیز بیانی واضح هستند. هدف اصلی تست‌ها این است که به عنوان مستندات از نوع مثال عمل کنند. کسی که تست‌های ما را می‌خواند باید بتواند یک درک سریع از آنچه که یک کلاس انجام می‌دهد پیدا کند.

اما مهم‌ترین روش برای بیان واضح این است که تلاش کنید. متأسفانه، اغلب وقتی کد را به درستی کار می‌کنیم، بدون فکر کافی به خوانایی آن برای شخص بعدی، به سراغ مشکل بعدی می‌رویم. به یاد داشته باشید، ممکن است شخص بعدی که کد را می‌خواند، خود شما باشید.

پس کمی به کار خود افتخار کنید. کمی وقت صرف هر یک از توابع و کلاس‌هایتان کنید. نام‌های بهتر انتخاب کنید، توابع بزرگ را به توابع کوچک‌تر تقسیم کنید و به‌طور کلی مراقب چیزی که ساخته‌اید باشید. مراقبت از کد یک منبع ارزشمند است.

## کلاس‌ها و متدهای حداقلی (Minimal Classes and Methods)

حتی مفاهیم اساسی‌ای مثل حذف تکرار، بیان واضح کد و `SRP` ممکن است به حد افراط برسند. در تلاش برای کوچک نگه‌داشتن کلاس‌ها و متدها، ممکن است کلاس‌ها و متدهای خیلی زیادی ایجاد کنیم. بنابراین این قانون پیشنهاد می‌کند که تعداد توابع و کلاس‌ها را هم در حد معقول نگه داریم.

تعداد زیاد کلاس‌ها و متدها گاهی اوقات نتیجه‌ی دگماتیسم بی‌فایده است. به عنوان مثال، یک استاندارد کدنویسی که بر ایجاد یک رابط برای هر کلاس تأکید دارد. یا توسعه‌دهندگانی که معتقدند فیلدها و رفتارها باید همیشه به کلاس‌های داده و کلاس‌های رفتار جداگانه تقسیم شوند. چنین دگماتیسم‌هایی باید مقاومت شوند و رویکردی واقع‌بینانه‌تر اتخاذ شود.

هدف ما این است که سیستم کلی‌مان را کوچک نگه داریم، در حالی که توابع و کلاس‌ها را کوچک نگه می‌داریم. با این حال، به یاد داشته باشید که این قانون اولویت پایین‌تری نسبت به سه قانون دیگر طراحی ساده دارد. بنابراین، اگرچه مهم است که تعداد کلاس‌ها و توابع کم باشد، اما مهم‌تر این است که تست‌ها داشته باشیم، تکرار را حذف کنیم و منظور خود را به‌وضوح بیان کنیم.

## نتیجه‌گیری

آیا مجموعه‌ای از شیوه‌های ساده وجود دارد که بتواند جایگزین تجربه شود؟ قطعاً نه. از طرف دیگر، شیوه‌هایی که در این فصل و در این کتاب توضیح داده شده‌اند، شکل متبلور شده‌ای از ده‌ها سال تجربه نویسندگان است. پیروی از شیوه طراحی ساده می‌تواند و باعث می‌شود که توسعه‌دهندگان به اصول و الگوهای خوبی پایبند باشند که در غیر این صورت یادگیری آن‌ها سال‌ها طول می‌کشد.

</div>

* [فصل قبل](../11_Systems/Systems.md)
* [فصل بعد](../13_Concurrency/Concurrency.md)