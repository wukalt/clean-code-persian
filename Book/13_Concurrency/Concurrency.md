# هم‌زمانی

اشیا انتزاعی از پردازش هستند. رشته‌ها انتزاعی از زمان‌بندی هستند.

![image](img-13.1.png)

<div dir="rtl">

نوشتن برنامه‌های همروند **(Concurrent)** تمیز بسیار سخت است — بسیار سخت. نوشتن کدی که در یک نخ **(Thread)** منفرد اجرا شود بسیار آسان‌تر است. همچنین نوشتن کد چندنخی **(Multithreaded)** که در ظاهر خوب به نظر برسد ولی در لایه‌های عمیق‌تر خراب باشد نیز آسان است. چنین کدی تا زمانی که سیستم تحت فشار قرار نگیرد، به درستی کار می‌کند.

در این فصل، نیاز به برنامه‌نویسی هم‌روند و سختی‌های آن را بررسی می‌کنیم. سپس چندین توصیه برای مقابله با این سختی‌ها و نوشتن کد هم‌روند تمیز ارائه می‌دهیم. در پایان نیز به مسائل مربوط به تست کردن کد هم‌روند می‌پردازیم.

هم‌روندی **(Clean Concurrency)** موضوعی پیچیده است که شایسته‌ی یک کتاب کامل می‌باشد. استراتژی ما در این کتاب این است که ابتدا نمای کلی‌ای از آن ارائه دهیم و سپس در فصل ``«Concurrency II»`` در صفحه ۳۱۷ آموزش دقیق‌تری ارائه کنیم.

اگر صرفاً کنجکاو درباره‌ی هم‌روندی هستید، این فصل برای شما کفایت می‌کند. اما اگر نیاز دارید که هم‌روندی را در سطحی عمیق‌تر درک کنید، باید آموزش تکمیلی را نیز مطالعه نمایید.


## چرا هم‌روندی (Concurrency)؟
هم‌روندی یک استراتژی جداسازی (`Decoupling`) است. این کار به ما کمک می‌کند آنچه انجام می‌شود (`what`) را از زمان انجام آن (`when`) جدا کنیم.

در برنامه‌های تک‌نخی (`Single-threaded`)، what و when چنان به هم گره خورده‌اند که حالت کل برنامه اغلب با نگاه به backtrace استک قابل تعیین است. برنامه‌نویسی که چنین سیستمی را دیباگ می‌کند می‌تواند یک یا چند breakpoint تنظیم کند و با توجه به breakهایی که زده می‌شود، وضعیت سیستم را بفهمد.

جداسازی what از when می‌تواند هم توان عملیاتی (throughput) و هم ساختارهای برنامه را به طرز چشمگیری بهبود بخشد. از دیدگاه ساختاری، برنامه شبیه چندین رایانه‌ی کوچک همکاری‌کننده خواهد شد، نه یک حلقه‌ی اصلی بزرگ. این موضوع می‌تواند سیستم را قابل‌فهم‌تر کند و فرصت‌هایی قدرتمند برای جداسازی دغدغه‌ها (`Separation of Concerns`) فراهم آورد.

به عنوان مثال، مدل استاندارد "`Servlet`" در برنامه‌های وب را در نظر بگیرید. این سیستم‌ها زیر چتر یک `container` وب یا `EJB` اجرا می‌شوند که تا حدی مدیریت هم‌روندی را برای شما انجام می‌دهد.

Servlet ها به صورت ناهمزمان (`Asynchronously`) و هر زمان که درخواست‌های وب وارد می‌شوند اجرا می‌شوند. برنامه‌نویس `servlet` لازم نیست تمام درخواست‌های ورودی را به طور مستقیم مدیریت کند. به طور نظری، اجرای هر servlet در دنیای کوچک خودش زندگی می‌کند و از اجرای سایر `servlet` ها جداست.

البته اگر اینقدر ساده بود، این فصل ضرورتی نداشت! در حقیقت، میزان جداسازی که توسط وب‌کانتینرها فراهم می‌شود، بسیار کمتر از حد کامل است. برنامه‌نویسان `servlet` باید کاملاً آگاه و بسیار محتاط باشند تا از درستی برنامه‌های هم‌روند خود اطمینان حاصل کنند.
با این وجود، مزایای ساختاری مدل `servlet` قابل توجه است.

اما ساختار تنها انگیزه‌ی پذیرش هم‌روندی نیست. برخی سیستم‌ها محدودیت‌های پاسخ‌دهی (`Response Time`) و توان عملیاتی (`Throughput`) دارند که نیازمند راهکارهای هم‌روند دست‌ساز هستند.
برای مثال، یک سیستم تک‌نخی برای جمع‌آوری اطلاعات از وب‌سایت‌های مختلف و ترکیب آن‌ها به یک خلاصه‌ی روزانه را در نظر بگیرید. چون این سیستم تک‌نخی است، هر سایت را به ترتیب می‌زند و هر کدام را کامل می‌کند قبل از اینکه به سراغ بعدی برود.
اجرای روزانه باید کمتر از ۲۴ ساعت طول بکشد. اما با اضافه شدن وب‌سایت‌های بیشتر، زمان اجرای کامل بیشتر شده و از ۲۴ ساعت فراتر می‌رود. بخش زیادی از این زمان صرف انتظار برای کامل شدن عملیات I/O در سوکت‌های وب می‌شود.
ما می‌توانیم با استفاده از یک الگوریتم چندنخی که همزمان به چند وب‌سایت مراجعه کند، عملکرد را بهبود بخشیم.

یا سیستم دیگری را تصور کنید که در هر لحظه فقط یک کاربر را پردازش می‌کند و برای هر کاربر تنها به یک ثانیه زمان نیاز دارد. این سیستم برای تعداد کمی کاربر پاسخگوست، اما با افزایش تعداد کاربران، زمان پاسخگویی به شدت بالا می‌رود. هیچ کاربری دوست ندارد پشت صف ۱۵۰ نفره منتظر بماند!
ما می‌توانیم با پردازش هم‌زمان چندین کاربر، زمان پاسخ را بهبود دهیم.

یا سیستمی را در نظر بگیرید که مجموعه داده‌های عظیمی را تفسیر می‌کند ولی فقط پس از پردازش کامل همه‌ی آن‌ها می‌تواند یک راه‌حل کامل ارائه دهد. شاید بتوان هر مجموعه داده را روی یک رایانه‌ی جداگانه پردازش کرد تا پردازش به صورت موازی انجام شود.

## باورهای نادرست (Myths and Misconceptions)

دلایل قانع‌کننده‌ای برای پذیرش هم‌روندی وجود دارد. با این حال، همانطور که گفتیم، هم‌روندی دشوار است. اگر خیلی محتاط نباشید، می‌توانید موقعیت‌های بسیار بدی ایجاد کنید. در ادامه به برخی باورهای غلط رایج اشاره می‌کنیم:

* هم‌روندی همیشه عملکرد را بهبود می‌بخشد.
هم‌روندی می‌تواند گاهی اوقات عملکرد را بهبود دهد، اما فقط زمانی که زمان‌های انتظار زیادی وجود داشته باشد که بتوان بین چند نخ یا چند پردازنده به اشتراک گذاشت. هیچ‌کدام از این شرایط ساده نیستند.

* طراحی هنگام نوشتن برنامه‌های هم‌روند تغییر نمی‌کند.
در واقع، طراحی یک الگوریتم هم‌روند می‌تواند به طرز چشمگیری با طراحی یک سیستم تک‌نخی متفاوت باشد. جداسازی what از when معمولاً تأثیر بزرگی روی ساختار سیستم می‌گذارد.

* در هنگام کار با یک container مانند وب‌کانتینر یا EJB container نیازی به درک مسائل هم‌روندی نیست.
در حقیقت، شما باید دقیقاً بدانید container شما چه کاری انجام می‌دهد و چطور باید در برابر مشکلات به‌روزرسانی هم‌زمان (Concurrent Update) و بن‌بست (Deadlock) که در ادامه این فصل توضیح داده شده، محافظت کنید.

و در نهایت چند نکته‌ی واقع‌بینانه درباره‌ی نوشتن نرم‌افزار هم‌روند:

* هم‌روندی سرباری هم در عملکرد و هم در میزان کد اضافه‌شده ایجاد می‌کند.

* هم‌روندی صحیح حتی برای مسائل ساده نیز پیچیده است.

* اشکالات هم‌روندی معمولاً قابل تکرار نیستند، بنابراین اغلب به عنوان موارد استثنایی (One-off) نادیده گرفته می‌شوند، در حالی که در واقع نقص‌های واقعی هستند.

* هم‌روندی اغلب نیازمند تغییر اساسی در استراتژی طراحی است.

## چالش‌ها (Challenges)
چه چیزی برنامه‌نویسی هم‌روند را تا این اندازه دشوار می‌کند؟ برای درک بهتر، کلاس ساده‌ی زیر را در نظر بگیرید:

</div>

```java
public class X {
    private int lastIdUsed;
    public int getNextId() {
        return ++lastIdUsed;
    }
}
```


<div dir="rtl">

فرض کنیم یک نمونه از کلاس X می‌سازیم، مقدار فیلد `lastIdUsed` را روی ۴۲ تنظیم می‌کنیم، و سپس این نمونه را بین دو نخ (Thread) به اشتراک می‌گذاریم. حال فرض کنید هر دو نخ متد `getNextId()` را فراخوانی می‌کنند؛ در این حالت سه نتیجه‌ی ممکن وجود دارد:

- نخ اول مقدار ۴۳ را دریافت می‌کند، نخ دوم مقدار ۴۴ را دریافت می‌کند، و `lastIdUsed` برابر ۴۴ می‌شود.
- نخ اول مقدار ۴۴ را دریافت می‌کند، نخ دوم مقدار ۴۳ را دریافت می‌کند، و `lastIdUsed` برابر ۴۴ می‌شود.
- نخ اول مقدار ۴۳ را دریافت می‌کند، نخ دوم نیز مقدار ۴۳ را دریافت می‌کند، و `lastIdUsed` برابر ۴۳ می‌شود.

نتیجه‌ی سوم که تعجب‌برانگیز است زمانی اتفاق می‌افتد که دو نخ همزمان بر روی یکدیگر تأثیر می‌گذارند. این اتفاق به این دلیل رخ می‌دهد که مسیرهای متعددی وجود دارند که دو نخ می‌توانند از طریق آن‌ها از همان خط کد جاوا عبور کنند و بعضی از این مسیرها نتایج نادرستی ایجاد می‌کنند. چند مسیر ممکن وجود دارد؟ برای پاسخ واقعی به این سوال باید بدانیم کامپایلر JIT چه کار می‌کند و مدل حافظه‌ی جاوا چه چیزی را عملیات اتمیک (atomic) در نظر می‌گیرد.

پاسخ سریع، با بررسی فقط بایت‌کد تولید شده این است که ۱۲٬۸۷۰ مسیر اجرایی متفاوت برای این دو نخ در متد `getNextId` وجود دارد. اگر نوع `lastIdUsed` از int به `long` تغییر کند، تعداد مسیرهای ممکن به ۲٬۷۰۴٬۱۵۶ می‌رسد. البته بیشتر این مسیرها نتایج صحیحی تولید می‌کنند. مشکل اینجاست که برخی از آن‌ها اینطور نیستند.

## اصول دفاع در برابر مشکلات همزمانی
در ادامه مجموعه‌ای از اصول و تکنیک‌ها برای دفاع از سیستم‌های خود در برابر مشکلات ناشی از کدنویسی همزمان ارائه شده است.

### اصل مسئولیت واحد (SRP)
اصل SRP بیان می‌کند که یک متد/کلاس/کامپوننت باید فقط یک دلیل برای تغییر داشته باشد. طراحی همزمانی به حد کافی پیچیده است که خود به تنهایی یک دلیل برای تغییر باشد و بنابراین باید از بقیه‌ی کد جدا شود. متأسفانه، معمولاً جزئیات پیاده‌سازی همزمانی مستقیماً وارد کد تولیدی می‌شوند.

مواردی که باید مد نظر قرار دهید:

- کدهای مربوط به همزمانی چرخه‌ی توسعه، تغییر و بهینه‌سازی مخصوص به خود دارند.
- این کدها چالش‌های خاص خود را دارند که اغلب سخت‌تر از کدهای غیرهمزمانی است.
- تعداد راه‌های ممکن برای شکست کدهای همزمانی اشتباه نوشته شده، بسیار زیاد است.

**توصیه: کدهای مربوط به همزمانی را از دیگر کدها جدا نگه دارید.**

### قاعده: محدود کردن دامنه‌ی داده‌ها
همانطور که دیدیم، زمانی که دو نخ یک فیلد مشترک را تغییر می‌دهند، می‌توانند باعث بروز رفتار غیرمنتظره شوند. یکی از راهکارها استفاده از کلمه کلیدی synchronized برای محافظت از بخش بحرانی کدی است که از شیء مشترک استفاده می‌کند.

هرچه تعداد این بخش‌های بحرانی بیشتر باشد:

- احتمال فراموش کردن محافظت از یکی از این بخش‌ها بیشتر می‌شود.
- دوباره‌کاری برای محافظت از همه‌ی بخش‌ها افزایش می‌یابد (نقض اصل DRY).
- پیدا کردن منبع خطا سخت‌تر می‌شود.

**توصیه: اصل کپسوله‌سازی داده را رعایت کنید؛ دسترسی به داده‌های مشترک را به شدت محدود کنید.**

### قاعده: استفاده از کپی داده‌ها
راهکار خوب دیگر این است که داده‌ها را اصلاً به اشتراک نگذارید. در برخی موارد می‌توان اشیا را کپی کرد و فقط به صورت فقط‌خواندنی با آن‌ها کار کرد. یا می‌توان نتایج را از چند نخ جمع‌آوری کرد و سپس در یک نخ ادغام نمود.

نگران هزینه‌ی ایجاد اشیای اضافه نباشید؛ اگر این کار باعث حذف قفل‌ها شود، صرفه‌جویی در زمان به مراتب ارزشمندتر خواهد بود.

### توصیه: تا حد امکان از به اشتراک‌گذاری داده‌ها اجتناب کنید.

قاعده: نخ‌ها باید تا حد ممکن مستقل باشند
کدی بنویسید که هر نخ داده‌های مورد نیاز خود را از یک منبع غیرمشترک دریافت کند و فقط از متغیرهای محلی استفاده نماید. این کار باعث می‌شود نخ‌ها گویی تنها نخ سیستم هستند و نیاز به همگام‌سازی نداشته باشند.

مثال: در کلاس‌هایی که از `HttpServlet` ارث‌بری می‌کنند، تمام داده‌ها از طریق پارامترهای `doGet` و `doPost` وارد می‌شوند.

**توصیه: داده‌ها را طوری تقسیم کنید که نخ‌ها بتوانند به صورت مستقل روی بخش‌های مختلف کار کنند.**

### کتابخانه‌های استاندارد را بشناسید
جاوا ۵ امکانات زیادی برای توسعه‌ی همزمانی نسبت به نسخه‌های قبلی فراهم کرده است. نکاتی که باید در نظر داشته باشید:

- از مجموعه‌های داده‌ی نخ-ایمن استفاده کنید.
- از چارچوب Executor برای اجرای وظایف مستقل استفاده کنید.
- از راهکارهای غیرمسدودکننده استفاده کنید.
- برخی کلاس‌های کتابخانه‌ای نخ-ایمن نیستند.

### کالکشن‌های Thread-Safe (ایمن در برابر چندنخی بودن)
در دوران اولیه‌ی جاوا، داگ لیا (Doug Lea) کتاب مهمی با عنوان Concurrent Programming in Java نوشت.
به همراه این کتاب، او چند کالکشن thread-safe توسعه داد که بعدها به بخشی از بسته‌ی `java.util.concurrent` در JDK تبدیل شدند.

کالکشن‌های موجود در این بسته برای استفاده در موقعیت‌های چندنخی ایمن هستند و عملکرد خوبی نیز دارند.
در واقع، پیاده‌سازی `ConcurrentHashMap` در اغلب موارد عملکرد بهتری نسبت به `HashMap` دارد.
این کلاس امکان خواندن و نوشتن هم‌زمان را فراهم می‌کند و دارای متدهایی برای انجام عملیات ترکیبی رایج است که در حالت عادی thread-safe نیستند.

اگر محیط اجرایی شما Java 5 یا بالاتر است، کار خود را با `ConcurrentHashMap` آغاز کنید.

چندین کلاس دیگر نیز برای پشتیبانی از طراحی‌های پیشرفته‌ی هم‌زمانی (concurrency) به این بسته افزوده شده‌اند. در ادامه چند نمونه از آن‌ها آمده است:

| کلاس               | توضیح                                                                                       |
|--------------------|---------------------------------------------------------------------------------------------|
| `ReentrantLock`    | یک قفل که می‌تواند در یک متد گرفته شود و در متدی دیگر آزاد گردد.                          |
| `Semaphore`        | پیاده‌سازی‌ای از سمفور کلاسیک؛ قفلی با شمارنده.                                            |
| `CountDownLatch`   | قفلی که منتظر وقوع چند رویداد می‌ماند تا همه‌ی نخ‌هایی که منتظر آن هستند را آزاد کند. این کار به نخ‌ها فرصت برابر برای شروع تقریباً همزمان می‌دهد. |

**توصیه: کلاس‌هایی که در دسترست هستند را بررسی کن. اگر با زبان Java کار می‌کنی، با بسته‌های `java.util.concurrent`، `java.util.concurrent.atomic` و `java.util.concurrent.locks` آشنا شو.**

## مدل‌های اجرایی خود را بشناسید  
راه‌های مختلفی برای تقسیم رفتار در یک برنامه‌ی هم‌روند (concurrent) وجود دارد.  
برای صحبت درباره‌ی آن‌ها، باید چند تعریف پایه‌ای را بشناسیم.

| مفهوم       | توضیح |
|------------|-------|
| **منابع محدود** | منابعی با اندازه ثابت یا تعداد ثابت که در یک محیط همزمان استفاده می‌شوند، مانند اتصالات پایگاه داده و بافرهای خواندن/نوشتن. |
| **حذف متقابل** | فقط یک رشته می‌تواند در یک زمان به داده‌های مشترک یا منبع مشترک دسترسی داشته باشد. |
| **گرسنگی** | یک رشته یا گروهی از رشته‌ها از پیشرفت برای مدت زمان طولانی یا برای همیشه منع می‌شوند، مانند جلوگیری از اجرای رشته‌های کندتر توسط رشته‌های سریع‌تر. |
| **بن‌بست** | دو یا چند رشته منتظر پایان یکدیگر هستند؛ هر رشته دارای منبعی است که رشته دیگر نیاز دارد، و هیچ‌کدام نمی‌توانند بدون دریافت منبع دیگر به پایان برسند. |
| **قفل زنده** | رشته‌ها در هماهنگی تلاش می‌کنند که کار انجام دهند اما دیگری مانع راه است؛ به دلیل تشدید، رشته‌ها همچنان سعی می‌کنند پیشرفت کنند اما قادر به انجام آن نیستند. |

راه‌های مختلفی برای تقسیم رفتار در یک برنامه‌ی هم‌روند (concurrent) وجود دارد. برای اینکه بتوانیم درباره آن‌ها صحبت کنیم، ابتدا باید چند تعریف پایه را بدانیم.

## مدل تولیدکننده-مصرف‌کننده
در این مدل، یک یا چند نخ (thread) تولیدکننده کاری را انجام می‌دهند و نتیجه آن را داخل یک صف یا بافر قرار می‌دهند. سپس یک یا چند نخ مصرف‌کننده آن کارها را از صف می‌گیرند و ادامه می‌دهند.

این صف یک منبع محدود است؛ یعنی اگر صف پر باشد، تولیدکننده باید منتظر بماند و اگر صف خالی باشد، مصرف‌کننده باید صبر کند. هماهنگی بین تولیدکننده‌ها و مصرف‌کننده‌ها از طریق این صف انجام می‌شود: تولیدکننده‌ها بعد از قرار دادن کار در صف، اطلاع می‌دهند که صف خالی نیست و مصرف‌کننده‌ها بعد از برداشتن کار، اطلاع می‌دهند که صف پر نیست.

## مدل خواننده-نویسنده
وقتی منبعی وجود دارد که اغلب برای خواندن استفاده می‌شود ولی گاهی هم نیاز به نوشتن دارد، باید بین عملکرد و هماهنگی دقت زیادی داشت. اگر به خواندن زیاد اولویت بدهیم، ممکن است نویسنده‌ها مدت زیادی منتظر بمانند. اگر به نوشتن اولویت دهیم، ممکن است عملکرد کلی کاهش یابد. باید بین این دو توازن برقرار شود تا هم عملکرد خوب باشد و هم خطا ایجاد نشود.

## مسئله‌ی فیلسوفان شام‌خور
چند فیلسوف دور یک میز نشسته‌اند و بین هر دو فیلسوف یک چنگال قرار دارد. هر فیلسوف وقتی گرسنه می‌شود، باید هر دو چنگال کناری خود را بردارد تا غذا بخورد. اگر چنگالی در اختیار فیلسوف کناری باشد، باید منتظر بماند.

اگر فیلسوف‌ها را نخ‌ها (threads) و چنگال‌ها را منابع در نظر بگیریم، این وضعیت شبیه به سیستم‌هایی است که منابع را بین چند فرآیند تقسیم می‌کنند. اگر مراقب نباشیم، این نوع طراحی می‌تواند باعث بن‌بست (deadlock)، قفل زنده (livelock) یا کاهش عملکرد شود.

**توصیه: این الگوریتم‌های پایه را یاد بگیرید و راه‌حل‌های آن‌ها را تمرین کنید تا وقتی با مسائل هم‌روندی روبرو شدید، آمادگی داشته باشید.**

## مراقب وابستگی بین متدهای synchronized باشید
در جاوا، استفاده از کلمه‌ی کلیدی synchronized باعث می‌شود فقط یک نخ بتواند وارد یک بخش از کد شود. اما اگر چند متد synchronized روی یک شیء مشترک داشته باشید، ممکن است باگ‌های پیچیده‌ای ایجاد شود.
    
**توصیه: سعی کنید بیش از یک متد روی یک شیء مشترک استفاده نکنید.**
اگر مجبور شدید، یکی از این سه روش را استفاده کنید:

- **قفل‌گذاری در سمت مشتری**: قبل از استفاده از متدها، مشتری قفل را بگیرد و تا پایان کار نگه دارد.
- **قفل‌گذاری در سمت سرور**: یک متد جدید در سرور ایجاد کنید که قفل را بگیرد، همه متدها را صدا بزند و سپس قفل را آزاد کند.
- **سرور واسطه**: یک کلاس واسط ایجاد کنید که قفل‌گذاری را انجام دهد.

## بخش‌های synchronized را کوچک نگه دارید
قفل‌ها باعث کندی سیستم می‌شوند، چون فقط یک نخ در هر لحظه می‌تواند وارد بخش قفل‌شده شود. اگر بخش زیادی از کد را `synchronized` کنید، رقابت بین نخ‌ها بیشتر شده و عملکرد کاهش می‌یابد.

توصیه: بخش‌های قفل‌شده (critical section) را تا حد ممکن کوچک نگه دارید.

## نوشتن کدی که درست خاموش شود، سخت است
نوشتن سیستمی که همیشه روشن بماند با سیستمی که باید به‌درستی خاموش شود فرق دارد. خاموش‌کردن درست سیستم ممکن است باعث بن‌بست شود.

مثلاً تصور کنید یک نخ والد چند نخ فرزند ایجاد می‌کند و منتظر می‌ماند همه آن‌ها تمام شوند. اگر یکی از نخ‌های فرزند در بن‌بست گیر کند، نخ والد هم هیچ‌وقت تمام نمی‌شود.

یا اگر والد به فرزندها بگوید کار را رها کنند و تمام شوند، اما یکی از فرزندها منتظر داده‌ای از دیگری باشد که دیگر تولید نمی‌شود، آن نخ گیر می‌افتد و باعث می‌شود سیستم کامل بسته نشود.

**توصیه: از همان ابتدا به خاموش‌سازی فکر کنید و زودتر آن را پیاده‌سازی کنید. این کار از چیزی که فکر می‌کنید سخت‌تر است.**


## آزمایش کدهای هم‌روند (Threaded)
اثبات درستی کامل یک کد تقریباً غیرممکن است. آزمایش هم تضمینی برای درستی نمی‌دهد، اما آزمایش خوب می‌تواند ریسک را کاهش دهد. این موضوع در مورد برنامه‌های تک‌نخی هم صدق می‌کند، اما به‌محض اینکه دو یا چند نخ به‌طور همزمان روی یک کد یا داده‌ی مشترک کار کنند، پیچیدگی‌ها به‌طور چشم‌گیری افزایش می‌یابد.

**توصیه: تست‌هایی بنویس که امکان آشکار کردن مشکلات را داشته باشند و آن‌ها را مرتباً با پیکربندی‌های مختلف برنامه، سیستم و میزان بار مختلف اجرا کن.**

- اگر حتی یک‌بار تستی شکست خورد، علت را پیدا کن.
- شکست را فقط به این خاطر که در اجرای بعدی تست موفق شده، نادیده نگیر.
- کد چندنخی خود را قابل اتصال (pluggable) طراحی کنید.
- کد چندنخی خود را قابل تنظیم (tunable) طراحی کنید.
- برنامه را با تعداد نخ‌هایی بیشتر از تعداد پردازنده‌ها اجرا کنید.
- برنامه را روی پلتفرم‌های مختلف اجرا کنید.
- کد خود را ابزارگذاری (instrument) کنید تا خطاها را به‌صورت اجباری ایجاد نمایید.

### شکست‌های پراکنده را به‌عنوان نشانه‌هایی از مشکلات چندنخی در نظر بگیرید
کد چندنخی باعث شکست‌هایی می‌شود که "اساساً نباید شکست بخورند."
بیشتر توسعه‌دهندگان (از جمله نویسندگان این متن) درک شهودی درستی از نحوه‌ی تعامل نخ‌ها با سایر بخش‌های کد ندارند.
باگ‌های موجود در کد چندنخی ممکن است تنها یک‌بار در هزار یا حتی یک میلیون اجرای برنامه بروز کنند.
تلاش برای تکرار این شرایط می‌تواند بسیار آزاردهنده باشد. این موضوع معمولاً باعث می‌شود که توسعه‌دهندگان این شکست‌ها را به عواملی مانند پرتوهای کیهانی، اشکالات سخت‌افزاری یا سایر خطاهای غیرقابل تکرار نسبت دهند.
اما بهترین کار این است که فرض کنید خطاهای "یک‌باره" اصلاً وجود ندارند. هرچه این خطاهای نادر بیشتر نادیده گرفته شوند، کد بیشتری ممکن است بر پایه‌ی یک روش معیوب ساخته شود.

**توصیه: شکست‌های سیستمی را به‌عنوان خطاهای یک‌باره نادیده نگیرید.**

### ابتدا از عملکرد صحیح کد غیرچندنخی مطمئن شوید
شاید بدیهی به نظر برسد، اما تأکید بر آن ضرری ندارد.
اطمینان حاصل کنید که کد، خارج از محیط چندنخی نیز به‌درستی کار می‌کند.
در حالت کلی، این به معنای ساختن POJOهایی است که توسط نخ‌ها فراخوانی می‌شوند.
این POJOها از وجود نخ‌ها بی‌اطلاع هستند و بنابراین می‌توان آن‌ها را خارج از محیط چندنخی تست کرد.
هرچه بتوانید بخش‌های بیشتری از سیستم را به این شکل پیاده‌سازی کنید، بهتر خواهد بود.

**توصیه: تلاش نکنید هم‌زمان باگ‌های چندنخی و غیرچندنخی را رفع کنید. ابتدا مطمئن شوید کدتان در خارج از محیط نخ‌ها درست کار می‌کند.**

### کد چندنخی خود را قابل اتصال طراحی کنید
کدی بنویسید که پشتیبانی از هم‌روندی را به‌گونه‌ای فراهم کند که در پیکربندی‌های مختلف قابل اجرا باشد:

اجرای یک نخ، چند نخ، یا تغییر در طول اجرا
تعامل کد چندنخی با اجزایی که هم می‌توانند واقعی باشند و هم تست‌دابل (test double)
اجرای تست‌ها با تست‌دابل‌هایی که سریع، کند یا با سرعت متغیر عمل می‌کنند
پیکربندی تست‌ها برای اجرا در تعداد دفعات مشخص

**توصیه: کدهای مبتنی بر نخ را به‌گونه‌ای طراحی کنید که به‌راحتی در پیکربندی‌های مختلف قابل استفاده باشند**

### تعداد نخ‌ها (threads) باید به‌راحتی قابل تنظیم باشند.
در نظر بگیرید که امکان تغییر آن‌ها در زمان اجرای سیستم فراهم باشد.
همچنین در نظر داشته باشید که سیستم بتواند به‌صورت خودکار و بر اساس میزان توان عملیاتی (throughput) و استفاده از منابع سیستم (system utilization) خودش را تنظیم کند.

### اجرای برنامه با تعداد نخ‌های بیشتر از تعداد پردازنده‌ها
وقتی سیستم بین وظایف مختلف جابه‌جا می‌شود، اتفاقاتی رخ می‌دهد.
برای تشویق به تعویض وظایف (task swapping)، برنامه را با تعداد نخ‌هایی بیشتر از تعداد پردازنده‌ها یا هسته‌ها اجرا کنید. هر چه وظایف بیشتر جابه‌جا شوند، احتمال مواجهه با کدی که بخش بحرانی (critical section) ندارد یا منجر به بن‌بست (deadlock) می‌شود، بیشتر خواهد شد.

### اجرای برنامه روی پلتفرم‌های مختلف
در میانه‌ی سال ۲۰۰۷ ما دوره‌ای درباره‌ی برنامه‌نویسی هم‌روند (concurrent programming) طراحی کردیم. توسعه‌ی این دوره عمدتاً در سیستم‌عامل OS X انجام شد.
اما کلاس با استفاده از Windows XP که در یک ماشین مجازی (VM) اجرا می‌شد، برگزار شد.
تست‌هایی که برای نشان دادن شرایط شکست نوشته شده بودند، در محیط XP به اندازه‌ی اجرای آن‌ها در OS X دچار شکست نمی‌شدند.

در تمام موارد، مشخص بود که کد تحت آزمایش نادرست است. این تجربه فقط این حقیقت را تقویت کرد که سیستم‌عامل‌های مختلف، سیاست‌های نخ‌پردازی متفاوتی دارند که هرکدام روی نحوه‌ی اجرای کد تأثیر می‌گذارند.
کد چندنخی در محیط‌های مختلف رفتاری متفاوت از خود نشان می‌دهد.

**توصیه: کدهای چندنخی خود را از همان ابتدا و به‌صورت مکرر روی تمام پلتفرم‌های هدف اجرا کنید.**

### کد خود را برای تلاش در ایجاد خطا ابزارگذاری کنید
مخفی ماندن نقص‌ها در کدهای هم‌روند (concurrent) یک امر طبیعی است.
آزمایش‌های ساده معمولاً این مشکلات را آشکار نمی‌کنند. در واقع، این نقص‌ها اغلب در پردازش‌های معمول نیز پنهان می‌مانند. ممکن است تنها هر چند ساعت، روز یا هفته یک‌بار ظاهر شوند!

دلیل این‌که باگ‌های مربوط به نخ‌ها (threading bugs) می‌توانند نادر، پراکنده و سخت برای تکرار باشند، این است که تنها تعداد بسیار کمی از مسیرهای ممکن در یک بخش آسیب‌پذیر واقعاً به خطا منتهی می‌شوند. بنابراین احتمال طی شدن یک مسیر خطادار می‌تواند بسیار پایین باشد. این امر تشخیص و اشکال‌زدایی را بسیار دشوار می‌کند.
چگونه می‌توانید شانس خود را برای کشف چنین اتفاقات نادری افزایش دهید؟

شما می‌توانید کد خود را ابزارگذاری (instrument) کنید و ترتیب اجرای آن را با اضافه کردن فراخوانی‌هایی به متدهایی مانند `Object.wait()`، `Object.sleep()`، `Object.yield()` و `Object.priority()` تغییر دهید.
هر یک از این متدها می‌توانند ترتیب اجرا را تحت تأثیر قرار دهند و در نتیجه احتمال کشف یک نقص را افزایش دهند. بهتر است کد معیوب هر چه زودتر و به دفعات بیشتری دچار شکست شود.
برای ابزارگذاری کد دو گزینه وجود دارد:

- کدنویسی دستی (Hand-coded)
- ابزارگذاری خودکار (Automated)

## کدنویسی دستی
شما می‌توانید به صورت دستی در کد خود فراخوانی‌هایی به متدهای wait()، sleep()، yield() و priority() اضافه کنید.
این کار ممکن است هنگام تست یک بخش به‌خصوص دشوار از کد، دقیقاً همان چیزی باشد که نیاز دارید.
در ادامه یک نمونه از انجام این کار آورده شده است:

</div>

```java
public synchronized String nextUrlOrNull() {
    if (hasNext()) {
        String url = urlGenerator.next();
        Thread.yield(); // inserted for testing.
        updateHasNext();
        return url;
    }
    return null;
}
```

<div dir="rtl">

فراخوانی `yield()` که اضافه شده است، مسیرهای اجرایی کد را تغییر می‌دهد و ممکن است باعث شود که کدی که قبلاً مشکلی نداشت، اکنون دچار اشکال شود. اگر این اتفاق بیفتد، علت آن اضافه کردن `yield()` نبوده است؛ بلکه مشکل از قبل در کد وجود داشته و این تغییر فقط باعث نمایان شدن آن شده است.

با این روش مشکلات زیادی وجود دارد:

- باید به صورت دستی مکان‌های مناسب برای اضافه کردن این دستورات را پیدا کنید.
- چگونه می‌توان فهمید کجا باید این فراخوانی‌ها را اضافه کرد و چه نوع فراخوانی‌ای مناسب است؟
- باقی گذاشتن چنین کدی در محیط تولید (Production) باعث کندی غیرضروری برنامه می‌شود.
- این روش شبیه به تیراندازی کورکورانه است؛ ممکن است اشکالات را پیدا کند یا نکند. در واقع، احتمال موفقیت چندان زیاد نیست.

آنچه نیاز داریم، راهی است که بتوانیم این تغییرات را در زمان تست اعمال کنیم، اما در محیط تولید نداشته باشیم. همچنین نیاز داریم بتوانیم به راحتی بین اجراهای مختلف پیکربندی‌ها را تغییر دهیم، که این باعث افزایش احتمال کشف خطاها به طور کلی می‌شود.

بدیهی است که اگر سیستم خود را به بخش‌هایی تقسیم کنیم که:

یک سری POJO داشته باشیم که هیچ دانشی از نخ‌ها (Threading) ندارند،

و کلاس‌هایی که کنترل نخ‌ها را بر عهده دارند،

پیدا کردن مکان‌های مناسب برای ابزارگذاری (Instrumentation) کد آسان‌تر خواهد بود. علاوه بر این، می‌توان تست‌های مختلفی ایجاد کرد که POJOها را تحت الگوهای متفاوتی از فراخوانی‌های `sleep`, `yield` و سایر دستورات مشابه اجرا کند.

## ابزارگذاری خودکار
شما می‌توانید از ابزارهایی مانند یک چارچوب مبتنی بر برنامه‌نویسی جنبه‌گرا (Aspect-Oriented Framework)، یا کتابخانه‌هایی مثل CGLIB یا ASM استفاده کنید تا به صورت برنامه‌نویسی شده (Programmatically) کد خود را ابزارگذاری کنید.
برای مثال، می‌توانید یک کلاسی ایجاد کنید که فقط یک متد داشته باشد.
</div>

```java
public class ThreadJigglePoint {
    public static void jiggle() {}
}
```

<div dir="rtl">
    شما می‌توانید فراخوانی‌هایی به این متد را در بخش‌های مختلفی از کد خود اضافه کنید.
</div>


```java
public synchronized String nextUrlOrNull() {
    if (hasNext()) {
        ThreadJiglePoint.jiggle();
        String url = urlGenerator.next();
        ThreadJiglePoint.jiggle();
        updateHasNext();
        ThreadJiglePoint.jiggle();
        return url;
    }
    return null;
}
```

<div dir="rtl">
    اکنون می‌توانید از یک Aspect ساده استفاده کنید که به صورت تصادفی یکی از گزینه‌های "هیچ کاری انجام ندادن"، "خوابیدن" (sleep) یا "تسلیم کردن" (yield) را انتخاب می‌کند.
یا تصور کنید که کلاس ThreadJigglePoint دارای دو پیاده‌سازی باشد. پیاده‌سازی اول متد jiggle را طوری پیاده‌سازی می‌کند که هیچ کاری انجام نمی‌دهد و در محیط تولید (Production) استفاده می‌شود. پیاده‌سازی دوم یک عدد تصادفی تولید می‌کند تا میان خوابیدن، تسلیم شدن یا عبور بدون انجام عمل خاصی یکی را انتخاب کند. اگر تست‌های خود را هزار بار با این نوع تکان‌های تصادفی اجرا کنید، ممکن است برخی اشکالات را کشف کنید. اگر تست‌ها با موفقیت بگذرند، حداقل می‌توانید ادعا کنید که بررسی‌های لازم را انجام داده‌اید.
هرچند این روش کمی ساده است، اما می‌تواند جایگزینی مناسب در نبود ابزارهای پیشرفته‌تر باشد.

ابزاری به نام ConTest، توسعه‌یافته توسط IBM، وجود دارد که کاری مشابه انجام می‌دهد، اما با پیچیدگی و کارایی بسیار بیشتر.

نکته‌ی اصلی این است که کد را به نحوی تکان دهید که نخ‌ها (Threads) در هر بار اجرا به ترتیب‌های متفاوتی اجرا شوند. ترکیب تست‌های خوب و فرآیند تکان دادن می‌تواند به طور قابل توجهی احتمال یافتن خطاها را افزایش دهد.

توصیه: از استراتژی‌های تکان دادن برای یافتن خطاها استفاده کنید.

## نتیجه‌گیری
کدنویسی همزمان (Concurrent Programming) کاری دشوار است. کدی که پیگیری آن در حالت عادی ساده به نظر می‌رسد، هنگامی که چندین نخ و داده‌های مشترک وارد عمل می‌شوند، می‌تواند به کابوسی پیچیده تبدیل شود.
اگر با نگارش کد همزمان روبرو هستید، باید با سخت‌گیری و دقت زیاد کدی تمیز (Clean Code) بنویسید؛ در غیر این صورت با خطاهای ظریف و نادر مواجه خواهید شد.

قبل از هر چیز، اصل تک مسئولیتی (Single Responsibility Principle) را رعایت کنید. سیستم خود را به POJOهایی تقسیم کنید که کد آگاه از نخ (Thread-Aware) را از کد ناآگاه از نخ (Thread-Ignorant) جدا می‌کنند. مطمئن شوید زمانی که در حال تست کدهای آگاه از نخ هستید، فقط همان بخش‌ها را آزمایش می‌کنید و نه بخش‌های دیگر. این موضوع به این معناست که کد آگاه از نخ باید کوچک و متمرکز باشد.

منابع احتمالی مشکلات همزمانی را بشناسید: نخ‌های متعددی که بر روی داده‌های مشترک یا منابع مشترک کار می‌کنند. موارد مرزی مانند خاموش شدن تمیز (Clean Shutdown) یا پایان یک حلقه می‌توانند به ویژه مشکل‌ساز باشند.

کتابخانه‌ی مورد استفاده‌ی خود را به خوبی بشناسید و با الگوریتم‌های بنیادی آشنا شوید. درک کنید که چگونه ویژگی‌های ارائه شده توسط کتابخانه می‌توانند در حل مسائلی مشابه الگوریتم‌های پایه کمک‌کننده باشند.

یاد بگیرید چگونه بخش‌هایی از کد را که نیاز به قفل شدن دارند، شناسایی کرده و آنها را قفل کنید. مناطقی را که نیازی به قفل شدن ندارند، قفل نکنید. از فراخوانی یک بخش قفل‌شده از درون یک بخش قفل‌شده‌ی دیگر اجتناب کنید. این کار نیازمند درک عمیقی از این است که چه چیزی مشترک است و چه چیزی نیست.
میزان اشیاء مشترک و دامنه‌ی اشتراک‌گذاری را تا حد ممکن محدود نگه دارید. طراحی اشیایی که داده‌ی مشترک دارند را تغییر دهید تا با نیازهای کاربران سازگار شوند، به جای آنکه کاربران مجبور شوند وضعیت‌های مشترک را مدیریت کنند.

مشکلاتی به وجود خواهند آمد. مشکلاتی که در مراحل اولیه ظاهر نمی‌شوند، معمولاً به عنوان رخدادهای نادر تلقی می‌شوند. این رخدادهای به اصطلاح "یک‌باره" غالباً تحت بار زیاد یا در زمان‌های به ظاهر تصادفی اتفاق می‌افتند.
بنابراین، باید بتوانید کدهای مربوط به نخ خود را در پیکربندی‌ها و بر روی پلتفرم‌های مختلف، به صورت مداوم و تکراری اجرا کنید.

قابلیت تست‌پذیری (Testability)، که به طور طبیعی از رعایت سه قانون TDD به دست می‌آید، به معنای نوعی قابلیت اتصال‌پذیری (Plug-ability) است که پشتیبانی لازم برای اجرای کد در طیف گسترده‌ای از پیکربندی‌ها را فراهم می‌کند.

با صرف زمان برای ابزارگذاری (Instrument) کد خود، شانس کشف خطاهای احتمالی را به طور چشمگیری افزایش می‌دهید.
این کار را می‌توانید به صورت دستی یا با استفاده از فناوری‌های خودکار انجام دهید.
از همان ابتدا روی این فرآیند سرمایه‌گذاری کنید. شما می‌خواهید کد مبتنی بر نخ خود را تا حد امکان طولانی‌تر قبل از ورود به محیط تولید اجرا کنید.

اگر رویکردی تمیز و اصولی در پیش بگیرید، شانس شما برای دستیابی به کدی صحیح به طرز چشمگیری افزایش می‌یابد.
</div>

* [فصل قبل](../12_Emergence/Emergence.md)
* [فصل بعد](../14_Successive_Refinement/Successive_Refinement.md)
