<div dir="rtl">

# فصل دو - اسامی با معنی

## مقدمه

![alt text](img-2.1.png "meaningful names")

اسم‌ها همه جای نرم‌افزار وجود دارند. ما متغیر‌ها، تابع‌ها، آرگومان‌ها، کلاس‌ها و پکیج‌هایمان را نام‌گذاری می‌کنیم. ما فایل‌های سورس و دایرکتوری هایی که آنها را شامل میشوند را نام گذاری می‌کنیم. ما حتی فایل‌های jar و war و ear را نامگذاری می‌کنیم. ما نامگذاری می‌کنیم، نامگذاری می‌کنیم و نامگذاری می‌کنیم. از آنجایی که این کار را خیلی زیاد انجام میدهیم، بهتر است که آن را با شیوه درست انجام دهیم. آنچه که در ادامه می‌خوانید، قوانینی ساده برای خلق اسم های خوب هستند.

## استفاده از اسم‌های بیان کننده منظور (Intention-Revealing Names)

 کاملا واضح است که اسم‌ها باید منظور شما را بازتاب دهند. چیزی که ما می‌خواهیم به شما بگوییم این است که ما در این قضیه کاملا جدی هستیم. انتخاب‌کردن اسم‌های خوب زمان‌بر است ولی زمان بیشتری از آنچه می‌گیرد را ذخیره می‌کند. پس به اسم هایتان توجه کنید و هر وقت اسم‌های بهتری یافتید، آنها را عوض کنید. هر کسی که کد شما را بخواند (شامل خودتان) از انجام این کار خوشحال می‌شود. اسم یک متغیر، تابع یا کلاس، باید به تمام سوال‌های بزرگ پاسخ دهد. اسم باید بگوید که چرا وجود دارد، چه می‌کند و چگونه استفاده می‌شود.اگر اسمی به کامنت نیاز داشته باشد،پس منظور خود را نمی‌رساند.
</div>

 ```java
 int d; // elapsed time in days
 ```

<div dir="rtl">
اسم d در کد بالا هیچ منظوری را نمی‌رساند. این اسم احساس گذشت روزها را ایجاد نمی‌کند. ما باید اسمی انتخاب کنیم که مشخص کند چه چیزی در حال اندازه گیری شدن است و واحد و مقیاس آن اندازه گیری چیست.
</div>

```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

<div dir="rtl">
انتخاب اسم‌هایی که منظور ما را القا می‌کند، فهمیدن و تغییر دادن کد را راحت تر می‌کند. می‌توانید بگویید کد زیر چه می‌کند؟
</div>

```java
 public List getThem() {
        List list1 = new ArrayList;
                for (int[] x : theList)
                        If (x[0] == 4)
                                List1.add(x);
         Return list1;
}
```

<div dir="rtl">
چرا گفتن کاری که این کد انجام میدهد سخت است؟ اینجا هیچ کد پیچیده ای وجود ندارد. فاصله ها و تورفتگی ها کاملا معقول هستند. در کل سه متغیر و دو ثابت استفاده شده است. اینجا هیچ کلاس عجیب یا متد پیچیده ای وجود ندارد، فقط یک لیست از آرایه ها وجود دارد. مشکل سادگی کد نیست، صراحت کد است؛ میزان صریح بودن کد در بیان منظور خودش. صریح بودن کد نیازمند این است که ما بتوانیم به سوال های نظیر این ها پاسخ دهیم:

1. چه چیزهایی در theList وجود دارد؟
2. اهمیت عضو صفرم در theList چیست؟  
3. اهمیت مقدار 4 چیست؟
4. چگونه از لیستی که برگشت داده می‌شود استفاده کنم؟

جواب سوال‌ها در مثال قبل قابل تشخیص نیستند، ولی باید مشخص شوند. فرض کنید ما داریم روی یک بازی مین روبی کار می‌کنیم. ما میدانیم که صفحه بازی یک لیست از سلول هاست که با عنوان theList نمایش داده می‌شود. بیایید نام آن را به gameBoard تغییر دهیم. هر سلول در صفحه توسط یک آرایه ساده نشان داده می‌شود. همچنین این را میدانیم که مقدار صفرم، وضعیت سلول است و اینکه وضعیت 4 یعنی "پرچم گذاری شده". بیایید با دادن اسم‌های این کانسپت‌ها کد را به شکل قابل توجهی بهبود دهیم:

</div>

```java
public List getFlaggedCells() {
        List flaggedCells = new ArrayList();
        for (int[] cell : gameBoard)
                If (cell[STATUS_VALUE] == FLAGGED)
                        flaggedCells.add(cell);
        return flaggedCells;
}
```

<div dir="rtl">
دقت کنید که سادگی کد تغییری نکرده است و هنوز هم همان تعداد مقادیر ثابت و متغیر وجود دارد، دقیقا با همان تعداد تورفتگی و بیرون زدگی. ما می‌توانیم پا را فراتر بگذاریم و به جای یک آرایه از اعداد، یک کلاس ساده برای سلول‌ها بنویسیم.این کلاس میتواند یک تابع را شامل شود (که منظور خود را به درستی به نمایش می‌گذارد و آن را isFlagged می‌نامیم) که باعث می‌شود اعداد عجیب از کد حذف شوند:
</div>

```java
public List getFlaggedCells() {
        List flaggedCells = new ArrayList();
        for (Cell cell : gameBoard)
                if (cell.isFlagged())
                        flaggedCells.add(cell);
        return flaggedCells;
}
```

<div dir="rtl">
با این تغییرات ساده، دیگر فهمیدن اینکه چه کاری در حال انجام است سخت نیست. این قدرت انتخاب کردن اسم‌های خوب است.

## خودداری از دادن اطلاعات اشتباه

برنامه نویس‌ها باید از به جا گذاشتن اطلاعات اشتباه که معنی کد را خراب می‌کنند خودداری کنند. ما باید از کلماتی که مفهوم آنها با منظور ما فاصله زیادی دارد دوری کنیم. برای مثال، کلمات hp ،aix و sco نام‌های ضعیفی برای متغیرها هستند زیرا از اسامی یونیکس پلتفرم هستند. حتی اگر شما در حال نوشتن یک Hypotenuse هستید و hp مخفف خوبی برای آن به نظر میرسد، ممکن است اطلاعات ناسازگار در کد به جا بگذارید. هرگز به یک لیست از اکانت‌ها اسم accountList را ندهید زیرا آن واقعا یک لیست است ولی کلمه List به معنی چیزی است که به برنامه نویس ها اختصاص دارد. اگر یک کانتینر اکانت را نگهداری می‌کند، به این معنی نیست که یک List است، این ممکن است به اطلاعات غلط منجر شود. پس accountGroup یا bunchOfAccounts یا فقط accounts اسم‌های بهتری هستند.

از استفاده از اسم‌هایی که تفاوت‌های کوچکی با هم دارند خودداری کنید. چه تضمینی وجود دارد که بعدا دو شی با نام‌های XYZControllerforEfficientHandlingOfStringsin و XYZControllerforEfficientStorageOfStrings با هم اشتباه گرفته نشوند؟ هر دو اسم شکل یکسانی دارند. در انتخاب اسم به تلفظ آن دقت کنید. استفاده از اسم با تلفظ اشتباه نوعی اطلاعات غلط است. به کمک محیط‌های مدرن جاوا ما می‌توانیم از تکمیل شدن خودکار کدها لذت ببریم. ما چند حرف تایپ می‌کنیم و دکمه ای را فشار میدهیم که لیستی از اسامی قابل استفاده برای تکمیل کلمه را به ما نشان میدهد.بسیار مفید است که نامهای بسیار مشابه به ترتیب حروف الفبا مرتب شوند.تفاوت‌ها را آشکار می‌کند.

یک مثال از نام‌هایی که اطلاعات غلط می‌دهند، نام‌هایی هستند که از حروفی استفاده می‌کنند که شبیه حروف دیگر هستند. مثلا حرف I و L، اگر به صورت I و l نوشته شوند،مطمئنا اشتباه گرفته میشوند. همچنین حرف lو1 نیز ممکن است اشتباه گرفته شوند.
</div>

```java
int a = l;
if(0==1)
        a=01;
else
        l = 01;
```

<div dir="rtl">

شاید فکر کنید این اتفاق محال است ؛ اما ما کدهایی را بررسی کرده‌ایم که چنین مواردی در آنها به وفور وجود داشت. در وهله اول نویسنده پیشنهاد می‌کند که فونت متن را تغییر دهید که باعث آشکار شدن بهتر تفاوت‌ها می‌شود، اما این راه حل باید به توسعه دهندگان آینده به صورت شفاهی یا کتبی منتقل شود. بهترین راه حل یک تغییر نام ساده است.

## تفاوت‌های با معنی ایجاد کنید

برنامه نویس‌ها با نوشتن کدهای کامپایلر پسند برای خودشان مشکل ایجاد می‌کنند. برای مثال شما نمی‌توانید از یک اسم در دو متغیر مختلف استفاده کنید،شما مجبورید یکی از اسم‌ها را کمی تغییر دهید. گاهی اوقات اینکار را با تغییر دادن املای کلمات انجام می‌دهید، در اینصورت ممکن است تصحیح خطاهای املایی باعث مشکل در کامپایل نرم افزار شود. افزودن اعداد یا حروف اضافه هرگز کافی نیست. اگر نام‌ها باید متفاوت باشند، پس معنی آنها نیز باید فرق کند.

 اسامی شامل سری اعداد (a1,a2,…,aN) با نام گذاری اصولی در تضاد هستند. این اسم‌ها اطلاعات غلط نمی‌دهند،چون اصلا اطلاعاتی نمی‌دهند و کاملا بی معنی هستند. اینها هیچ سرنخی از منظور شما به دیگران نمی‌دهند. به مثال نگاه کنید:
 </div>

```java
public static void copyChars(char a1[], char a2[] {
        for (int i=0; i&lt;a1.length; i++) {
                A2\[[i] = a1[i];
         }
 }
```

<div dir="rtl">
این تابع خواناتر می‌شود، اگر از اسم های source و destination در ارگومنت ها استفاده کنیم.

حروف اضافه(noise words) موارد بی معنی دیگری هستند. تصور کنید شما کلاسی به اسم Product دارید. اگر شما کلاس‌های دیگری با اسم‌های ProductInfo یا ProductData بسازید، شاید اسم‌های مختلفی انتخاب کرده باشید اما در معنای آنها تفاوتی وجود ندارد. کلمات Info و Data کلمات اضافه هستند، مثل a, an و the. دقت کنید که استفاده از این پیشوندها مشکلی ندارد، مثلا شما می‌توانید از a برای همه متغیرهای محلی و از the برای همه آرگومنت‌های توابعتان استفاده کنید. در حقیقت مشکل از جایی شروع می‌شود که شما یک متغیر را theZork بنامید، فقط به این دلیل که متغیر دیگری به نام Zork دارید. حروف اضافه،زائد هستند. کلمه Variable هیچوقت نباید در اسم یک متغیر نمایان شود. واژه table نباید در اسم یک Table نشان داده شود. چرا فکر میکنید NameString بهتر از Name است؟ آیا ممکن است Name یک عدد اعشاری باشد؟اگر جواب بله است،پس شما کل قوانین را زیر سوال برده اید!

فرض کنیدکلاسی به اسم Customer و کلاسی دیگر به اسم CustomerObject دارید.از اختلاف این دو اسم چه چیزی دستگیرتان می‌شود؟ کدام یک از این دو اسم، بهتر میتواند تاریخچه پرداخت‌های یک مشتری را نشان دهد؟ در مثال‌های زیر استفاده مناسب از حروف اضافه را میبینید.
</div>

```java
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
```

<div dir="rtl">
برنامه نویس به راحتی میفهمد که کدام تابع را نیاز دارد.

دقت کنید که حروف اضافه چه تغییری در اسم متغیر شما می‌دهند. تفاوت متغیر moneyAmount از money غیرقابل تشخیص است. تفاوت customerInfo از customer نیز همینطور.accountData از account و theMessage از message. اسامی قابل تشخیص باید تفاوت خود را به خواننده نشان دهند.

## از اسم‌های قابل تلفظ استفاده کنید

انسان‌ها در استفاده از کلمات ماهرند. قسمت‌های مشخصی از مغز ما به درک مفهوم کلمات اختصاص داده شده اند.مفهوم کلمات ارتباط مستقیمی با تلفظ آنها دارد. مغز ما کلمات را با توجه به تلفظ آنها درک می‌کند، نه نوشتار آنها. بنابراین، بهتر است از اسم‌های قابل تلفظ استفاده کنید. اگر نمی‌توانید یک اسم را تلفظ کنید، نمی‌توانید درباره آن بحث کنید،بدون اینکه مثل یک احمق صدا در بیاورید! "Well,over here on the bee cee arr three cee enn tee we have a pee ess zee kyew int, see?" می‌توانید این متن را بخوانید؟ فهمیدید که این موضوع مهم است، چون برنامه نویسی یک فعالیت اجتماعی است. یک کمپانی در برنامه خود، مفهومی به نام genymdhms دارد (generation date, year, month, day, hour, minute and second ). آنها برای اینکه این اسم را به خاطر بسپارند، قدم زنان میگویند "gen why emm dee aich emm ess". من عادت مسخره ای دارم که در آن هر چیزی را به همان شکلی که نوشته شده میخوانم، پس من شروع کردم به گفتن "gen-yah-mudda-hims.". بعدها این مفهوم توسط جمعی از طراحان و آنالیزورها به همین اسم نامیده شد، و ما هنوز هم به درآوردن این صدای احمقانه ادامه میدهیم. از آنجایی که این برای ما مثل یک شوخی به حساب می‌آمد، برای ما بانمک بود. بانمک باشد یا نباشد، ما داریم اسم گذاری ضعیف را تحمل می‌کنیم. برنامه نویس‌های جدید ما نیازمند این هستند که این مفهوم را برایشان توضیح دهیم و آنها در مورد دلیل در آوردن این صداهای احمقانه به جای استفاده از قوانین صریح و کلمات بامعنای انگلیسی صحبت می‌کنند.مقایسه کنید:
</div>

```java
class DtaRcrd102 {
        private Date genymdhms;
        private Date modymdhms;
        private final String pszqint = “102”;
        /\* _...  \*_/
}
```

 با

```java
class Customer {
        private Date generationTimestamp;
        private Date modificationTimestamp;
        private final String recordId = "102";
        /\* _...  \*_/

}
```

<div dir="rtl">
 مکالمه عاقلانه اکنون امکان پذیر است:

 “Hey, Mikey, take a look at this record! The generation timestamp is set to tommorrow’s date! How can that be?"

## از اسامی قابل جستجو استفاده کنید

اسامی تک حرفی و ثابت‌های عددی این مشکل را دارند که در متن قابل پیداکردن نیستند.احتمالا پیدا کردن MAX\_CLASSES\_PER\_STUDENT در یک متن راحت است، اما مثلا پیدا کردن عدد 7 در یک متن طولانی، سخت و زمان بر است. جستجوها ممکن است اعداد دیگری را نیز برای شما پیدا کنند، مثل قسمت‌های عددی اسم فایل‌ها،ثابت‌های توابع دیگر و در عبارات گوناگونی که از اعداد با اهداف متفاوتی استفاده می‌کنند. وقتی یک عدد طولانی استفاده میکنید، ممکن است شخصی رقم‌هایی را سهوا جابجا کند و همزمان این عدد از جستجوی شما فرار می‌کند. برای مثال، حرف e ضعیف ترین حرف ممکن برای نامگذاری یک متغیر برای برنامه نویسی است که نیازمند جستجو کردن است. چون این حرف پرکاربردترین حرف در زبان انگلیسی است و در هر عبارتی یافت می‌شود و باعث می‌شود حین جستجو هر متنی را که در هر برنامه ای وجود دارد ببینید.

در این راستا در هر کدی، نام‌های طولانی‌تر، بر نام‌های کوتاه‌تر برتری دارند و نام‌های قابل جستجو بر ثابت‌ها. به نظر من نام‌های تک‌حرفی تنها می‌توانند به عنوان متغیرهای محلی در متدهای کوتاه استفاده شوند. **طول یک نام باید با دامنه استفاده آن مطابقت داشته باشد.** اگر ممکن است از یک متغیر در چندین محل از یک کد استفاده کنید، ضروری است که یک اسم جستجو-دوست (search-
friendly)داشته باشید. یکبار دیگر مقایسه کنید.
</div>

```java
 for (int j=0; j<34; j++) {
         S+= (t[j]*4)/5;
 }
```

 با

```java
 int realDaysPerIdealDay = 4;
 const int WORK_DAYS_PER_WEEK = 5;
 int sum = 0;
 for (int j=0; j<NUMBER_OF_TASKS; j++) {
         int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
         int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
         sum += realTaskWeeks;
 }
```

<div dir="rtl">
به sum دقت کنید، این اسم کاملا مناسب نیست، اما حداقل قابل جستجو کردن است. نوشتن کد با این نامگذاری ها شاید کمی طولانی تر باشد، اما این را هم در نظر داشته باشید که پیدا کردن WORK_DAYS_PER_WEEK راحت است زیرا فقط 5 بار از آن استفاده شده.

## از رمزگذاری خودداری کنید

ما به اندازه کافی از رمزنگاری‌ها برای زیاد کردن دردسرمان استفاده می‌کنیم، لطفا آن را از چیزی که هست بیشتر نکنید.نام‌های رمزگذاری شده به سادگی قابل تلفظ نیستند و برای توسعه دهندگان جدید دردسر ایجاد می‌کنند،زیرا هر کارمند جدید باید زبان رمزنگاری شما را بیاموزد که خود باعث فشار روحی زیادی است.

### نمادگذاری مجارستانی

 در روزهای قبل، وقتی روی زبان‌های name-length-challenged کار می‌کردیم، این امر را با پشیمانی و بخاطر ضرورت نقض کردیم. fortran رمزگذاری را اجبار می‌کرد. نسخه‌های اولیه BASIC فقط یک حرف به اضافه یک رقم را مجاز می‌کرد. HungarianNotation (HN) این مرحله را به سطح کاملا جدیدی رساند.

و HN در Windows C API بسیار مهم به حساب می‌آمد،هنگامی که همه چیز یک دسته صحیح یا یک نشانگر طولانی یا یک Voidpointer یا یکی از چندین نوع "رشته" (با کاربردها و ویژگی‌های مختلف) بود. کامپایلر نوع متغیرهای ورودی را بررسی نمی‌کرد، بنابراین برنامه نویسان برای کمک به آن در به خاطر سپردن نوع داده‌ها نیاز به عصا داشتند.

در زبان‌های مدرن سیستم‌های بسیار غنی‌تری داریم و کامپایلرها می‌توانند به خاطر بیاورند که هر داده ای از چه نوع است.گذشته از این موضوع،گرایش به کلاس‌های کوچکتر و عملکردهای کوتاه تر نیز وجود دارد تا افراد معمولا بتوانند مکان اعلام هر متغیری که می‌خواهند را ببینند. برنامه‌نویسان جاوا نیازی به رمزگذاری ندارند.نوع اشیا مشخص است و محیط‌های ویرایش به گونه ای پیشرفت کرده‌اند که یک خطای نوع را قبل از اینکه بتوانید برنامه را کامپایل کنید،تشخیص می‌دهند.

### پیشوندهای متغیر

 شما دیگر نیازی به پیشوند m_ در متغیرهای محلی(member variable) ندارید. کلاس‌ها و توابع شما باید به‌قدری کوچک باشند که نیازی به آنها نباشد و شما باید از یک محیط ویرایش استفاده‌کنید که اعضا را برجسته و یا رنگی و آنها را متمایز کند.
 </div>

```java
 public class Part {
         private String m_dsc; // The textual description
         void setName(String name) {
                 M_dsc = name;
         }
 }
```

```java
 public class Part {
         String description;
         void setDescription(String description) {
                 this.description = description;
         }
}
```

<div dir="rtl">
 علاوه بر این،افراد به سرعت یاد میگیرند که پیشوند ( یا پسوند) را نادیده بگیرند تا بخش بامعنای نام را ببینند. هرچه آن را بیشتر بخوانند، کمتر the را میبینند.در نهایت پیشوندها به صورت تصادفی دیده می شوند و نشانگر کد قدیمی تر هستند.

### واسط‌ها (interface) و پیاده‌سازی‌ها

گاهی اوقات مورد خاصی برای رمزگذاری است. به عنوان مثال، می‌گویند شما در حال ساخت یک ABSTRACT FACTORY برای ایجاد شکل‌ها هستید. این factory یک واسط خواهد بود و توسط یک کلاس concrete (کلاسی که همه متدها را پیاده‌سازی کند) پیاده‌سازی خواهد شد. چه اسمی باید برای انها بگذاریم؟ IShapeFactions و ShapeFective؟ من ترجیح می‌دهم واسط را بدون آراستگی رها کنم. پیشوند i، که در میان میراث (legacy) این روزها معمول است، در بهترین حالت حواس پرتی و در بدترین اطلاعات اضافی است. من نمی خواهم که کاربرانم بدانند که من interface خود را به آنها ارائه می‌کنم. من فقط می‌خواهم آنها بدانند که این یک shape factoryاست. بنابراین اگر من باید یا interface یا implementation را رمزگذاری کنم، implementation را انتخاب می‌کنم. نام ان را ShapeFactoryImp، یا حتی CShapeFective ناخوشایند بگذارید، رمزگذاری واسط‌ها ضروری است.

## از نگاشت ذهنی خودداری کنید

 خوانندگان نباید مجبور باشند که نام انتخابی شما را در ذهنشان به نام دیگری که از قبل می‌شناسند ترجمه کنند. این مشکل معمولاً ناشی از انتخابی است که نه از دامنه اصطلاحات مسئله استفاده کرده است و نه از دامنه واژه‌های راه حل.

 این مشکل مربوط به نام متغیرهای تک حرفی است. مطمئناً یک دامنه نام برای شمارشگر حلقه ای که کوچک باشد و هیچ نام دیگری با آن به تناقض نرسد، i یا j یا k است (هرچند هیچگاه l نامگذاری نمی‌شود). دلیل این امر آن است که این نام‌های تک حرفی از قدیم برای شمارشگر حلقه استفاده شده اند. با این وجود، در بیشتر زمینه‌های دیگر، یک نام تک حرفی انتخاب بدی است. این انتخاب تنها یک جایگزین است که خواننده باید در ذهن خود آن را به مفهومی واقعی نگاشت کند. دلیلی بدتر از اینکه a و b قبلاً استفاده شده بود برای استفاده از نام c نمی‌تواند وجود داشته باشد.

 به طور کلی برنامه نویسان افراد بسیار باهوشی هستند. افراد باهوش گاهی دوست دارند با نشان دادن توانایی‌های ذهنی خود، هوشمندی خود را به معرض نمایش بگذارند. از این گذشته، اگر با اطمینان بتوانید به یاد داشته باشید که r نسخه کوتاه شده از آدرس url با میزبان و شمای حذف شده است، پس مشخصا باید بسیار باهوش باشید.

 یک تفاوت بین یک برنامه نویس هوشمند و یک برنامه نویس حرفه ای در این است که حرفه ای می‌فهمد که شفافیت پادشاه است. حرفه‌ای ها به خوبی از توانایی‌های خود برای نوشتن کدی که دیگران می‌توانند آن را درک کنند استفاده می‌کنند.

## نام کلاس‌ها

 کلاسها و اشیاء باید دارای نام یا عبارت اسمی مانند Customer، WikiPage، Account و AdressParser باشند. از کلماتی مانند Manager، Processor، Data یا Info برای نام کلاس خودداری کنید. **نام کلاس نباید یک فعل باشد.**

## نام متدها

  اسم متدها باید دارای فعل یا عبارت فعلی مانند postPayment، DeletePage یا save باشند. Accessorها، mutator ها و predicate ها را باید با مقدار خودشان نامگذاری کرد و با در نظر گرفتن استانداردهای javabean، با پیشوندهای get، set و is نام گذاری کرد.
 </div>

```java
String name = employee.getName();
customer.setName("mike")
if (paychech.isPosted()) ...
```

<div dir="rtl">

 زمانی که متدهای سازنده (constructor) زیاد شوند، از factory method های static با نام هایی که متغیرها را توصیف کنند استفاده کنید. به عنوان مثال:
 </div>

```java
Complex FulcrumPoint = Complex.FromRealNumber(23.0);
```

<div dir="rtl">

 به طور کلی بهتر است از :
 </div>

```java
Complex FulcrumPoint = new Complex(23.0);
```

<div dir="rtl">

 در نظر بگیرید که با private کردن متدهای سازنده مربوطه، بر استفاده از آنها تاکید کنید.

## بانمک نباشید

 اگر نام‌ها خیلی هوشمندانه باشند، فقط در ذهن افرادی که از نظر شوخ طبعی به نویسنده شبیه هستند، و فقط تا زمانی که این افراد این شوخی را به خاطر می‌آورند، ماندگار می‌شوند. آیا آنها می‌دانند تابعی به نام HolyHandGrenade قرار است چه کاری انجام دهد؟ مطمئنا، بانمک است، اما شاید در این حالت DeleteItems نام بهتری باشد. شفافیت را در برابر سرگرمی انتخاب کنید.

 بامزگی در کد اغلب به شکل محاوره یا زبان عامیانه ظاهر می‌شود. به عنوان مثال، از نام whack به جای kill استفاده نکنید. از شوخی های خاص در یک فرهنگ مانند eatMyShors به جای abort استفاده نکنید.

 آن چیزی را بگویید که منظورتان است. منظورتان همان چیزی است که می‌گویید.

## برای هر مفهوم یک کلمه انتخاب کنید

 یک کلمه را برای یک مفهوم مجزا انتخاب کنید و به آن بچسبید. به عنوان مثال fetch ،retrieve و get به عنوان نام متدهای معادل در کلاسهای مختلف گیج کننده است. چگونه به یاد خواهید آورد که نام کدام متد در کدام کلاس ذکر شده است؟ متأسفانه، شما اغلب باید به خاطر داشته باشید كه كدام شركت، گروه یا فرد كتابخانه یا كلاس را نوشته است تا بتوانید به یاد آورید که كدام اصطلاح استفاده شده است. در غیر این صورت، شما زمان بسیار زیادی را صرف مرور در header ها و نمونه کدهای قبلی می‌کنید.

 محیطهای ویرایش مدرن مانند Eclipse و IntelliJ سرنخ‌های حساس به متن(context-sensitive clues) مانند لیست متدهایی که می‌توانید در یک شی خاص فراخوانی کنید را فراهم می‌کنند. اما توجه داشته باشید که این لیست معمولاً comment هایی را که شما در مورد نام تابع و لیست پارامترهای خود نوشتید به شما نمی‌دهد. خوش شانس باشید، بتواند نام پارامترها را از توصیف توابع به شما ارائه دهد. اسامی توابع باید مستقل واضح باشند، و آنها باید سازگار باشند تا شما بتوانید متد صحیح را بدون جستجوهای اضافی انتخاب کنید.

 به همین ترتیب، داشتن یک Controller و یک manager و یک driver در یک پایگاه کد گیج‌کننده است. تفاوت اساسی بین DeviceManager و ProtocolController چیست؟ چرا هر دو Controller نیستند یا هر دو manager نیستند؟ آیا آن دو واقعاً driver هستند؟ این نام‌ها باعث می‌شود که شما توقع دو شی که از نظر نوع و کلاس هایشان بسیار متفاوت هستند را داشته باشید.

 واژه نامه نامتناقض یک لطف بزرگ به برنامه نویسانی است که باید از کد شما استفاده کنند.

## با ایهام ننویسید

 از استفاده از یک کلمه برای دو منظور مختلف خودداری کنید. استفاده از یک اصطلاح یکسان برای دو ایده متفاوت در اصل یک ایهام است.

 اگر از قانون "یک کلمه برای هر مفهوم" پیروی کنید، می‌بینید که بسیاری از کلاس‌ها مثلا یک متد add دارند. تا زمانی که لیست پارامترها و مقادیر برگشتی در متدهای مختلف add معنایی معادل داشته باشند، همه چیز خوب است.

 اما ممکن است شخص تصمیم بگیرد از کلمه add وقتی که منظورش افزودن نیست استفاده کند و تنها بخواهد سازگاری ایجاد کند. بیایید بگوییم که ما کلاسهای زیادی داریم که در آنها با اضافه کردن یا ملحق کردن دو مقدار موجود، مقدار جدیدی ایجاد می‌شود. حال فرض کنیم ما در حال نوشتن یک کلاس هستیم که متدی در آن است که یک پارامتر را درون یک مجموعه می‌گذارد. آیا باید این متد را add بنامیم؟ ممکن است به این دلیل که متدهای add دیگری داریم، سازگار به نظر برسد، اما در این حالت، مفاهیم متفاوت هستند، بنابراین باید به جای آن از اسمی مانند insert یا append استفاده کنیم. نامیدن متد جدید به add ایهام ایجاد می‌کند.

 هدف ما، به عنوان نویسنده، این است که کدهای خود را تا جایی که می‌توانیم قابل درک کنیم. ما می‌خواهیم کد ما به جای اینکه یک مطالعه عمیق و مفهومی باشد، سریع خوانده شود. ما می‌خواهیم از مدل رایج کتاب‌های کاغذی استفاده کنیم که به موجب آن نویسنده وظیفه دارد منظور خود را شفاف سازد و نه مدل دانشگاهی كه در آن محققان وظیفه دارند مفاهیم و معانی را از مقاله‌ها استخراج کنند.

## از دامنه واژگان مربوط به راه حل استفاده کنید

 به یاد داشته باشید افرادی که کد شما را می‌خوانند، برنامه نویس هستند. بنابراین از اصطلاحات علوم کامپیوتر، نام الگوریتم ها، نام الگوها، اصطلاحات ریاضی و موارد دیگر استفاده کنید. عاقلانه نیست که هر نام را از دامنه صورت مسئله انتخاب کنیم زیرا ما نمی‌خواهیم همکاران ما مجبور به فرار از مشتری ای باشند که چون هر مفهوم را با نام دیگری میشناسد، معنی هر کلمه را می پرسد.

 نام AccountVisitor برای یک برنامه نویس که با الگوی VISITOR آشنا است بسیار پر معنی است. چه برنامه نویسی نمی داند JobQueue چیست؟ موارد فنی بسیار زیادی وجود دارد که برنامه نویسان باید انجام دهند. انتخاب نام‌های فنی برای آن موارد معمولاً مناسب ترین روش است.

## از دامنه واژگان صورت مسئله استفاده کنید

 وقتی "اصطلاح برنامه نویسی" برای کاری که انجام می‌دهید وجود ندارد، از دامنه صورت مسئله برای انتخاب نام استفاده کنید. حداقل برنامه نویسی که کد شما را نگهداری می‌کند می تواند از یک متخصص دامنه سوال کند که منظور چیست.

 جدا کردن دامنه راه حل و صورت مسئله بخشی از کار یک برنامه نویس و طراح خوب است. کدی که ارتباط بیشتری با مفاهیم مربوط به صورت مسئله دارد باید دارای نامهایی باشد که از دامنه صورت مسئله گرفته شده است.

## ساختارهای با معنی اضافه کنید

تنها چند نام وجود دارند که به خودی خود معنی دارند و اکثر نام‌ها به خودی خود بی معنی اند. درعوض، باید با قرار دادن آنها در کلاسها، توابع یا مکانهای نامگذاری شده، خوانندتان را در جریان موضوع قرار دهید. وقتی همه موارد دیگر شکست بخورد، ممکن است استفاده از پیشوند برای نام به عنوان آخرین راه حل اساسی مورد استفاده قرار گیرد.
تصور کنید که متغیرهایی با اسامی firstName،lastName ،street ،houseNumber ،city ،state و zipcode دارید. آنها به طور واضح در کنار هم یک آدرس را تشکیل می‌دهند. اما اگر متغیر state را به تنهایی در یک تابع دیدید چه؟ آیا به طور خودکار استنباط می‌کنید که این متغیر بخشی از یک آدرس است؟
می‌توانید پیشوندهایی را به متن اضافه کنید: addrFirstName ،addrLastName ،addrState و غیره. حداقل خوانندگان خواهند فهمید که این متغیرها بخشی از یک ساختار بزرگتر هستند. البته یک راه حل بهتر ایجاد کلاس با نام Address است. سپس، حتی کامپایلر هم می‌داند که متغیرها متعلق به یک ساختار بزرگتر هستند.
متد موجود در Listing 2-1 را در نظر بگیرید. آیا متغیرها به یک ساختار معنی دار تر نیاز دارند؟ نام تابع تنها بخشی از ساختار، و الگوریتم بقیه را ارائه میدهد. پس از خواندن این تابع، می‌بینید که سه متغیر، number، verb و pluralModifier بخشی از پیام "guess statistics" هستند. متأسفانه، مفهوم باید حدس زده شود. وقتی برای اولین بار به متد نگاه می‌کنید، معنی متغیرها مبهم است.

</div>
Listing 2-1
Variables with unclear context.

```java
private void printGuessStatistics(char candidate, int count)
{
    String number;
    String verb;
    String pluralModifier;
    if (count == 0){
        number = "no";
        verb = "are";
        pluralModifier = "s";
    } else if (count == 1) {
        number = "1";
        verb = "is";
        pluralModifier = "";
    } else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    String guessMessage = String.format( "There %s %s %s%s", verb, number, candidate, pluralModifier );
    print(guessMessage);
}
```

<div dir="rtl">
این تابع کمی طولانی است و از متغیرها در کل آن استفاده می شود. برای تقسیم تابع به قطعات کوچکتر باید یک کلاس GuessStatisticsMessage ایجاد کنیم و سه فیلد متغیر این کلاس را بسازیم. بدین ترتیب یک متن واضح را برای سه متغیر فراهم می کنیم. آنها به طور قطع بخشی از GuessStatisticsMessage هستند. بهبود زمینه همچنین به الگوریتم اجازه می دهد تا با شکستن شدن به توابع کوچکتر، بسیار تمیزتر شود.
</div>

Listing 2-2
Variables have a context.

```java
public class GuessStatisticsMessage{
    private String number;
    private String verb;
    private String pluralModifier;
    public String make(char candidate, int count){
        createPluralDependentMessageParts(count);
        return String.format( "There %s %s %s%s", verb, number, candidate, pluralModifier );
    }
    private void createPluralDependentMessageParts(int count{
        if (count == 0){
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }
    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are"; pluralModifier = "s";
    }
    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }
    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}
```

<div dir="rtl">
 
## متن(context) بیخود اضافه نکنید.
در یک برنامه فرضی به نام “Gas Station Deluxe”، این که هر کلاس با پیشوند GSD شروع شود ایده بدی است. صادقانه بگویم، شما با این کار در برابر ابزارهای خود می ایستید. G را تایپ می کنید و کلید تکمیل را فشار می دهید و با یک لیست بلند بالا از تمام کلاس های سیستم مواجه می شوید. این عاقلانه است؟ چرا راه کمک را برای IDE دشوار میکنید؟
به همین ترتیب، بیایید بگوییم که شما یک کلاس به نام MailingAddress را در ماژول حسابداری GSD نوشته اید و نام آن را GSDAccountAddress گذاشتید. بعداً، برای تماس با مشتری خود به یک آدرس پستی نیاز دارید. آیا از GSDAccountAddress استفاده می کنید؟ آیا این نام مناسب به نظر می رسد؟ 10 تا از 17 کاراکتر زائد یا بی ربط هستند.
معمولا تا زمانی که نامهای کوتاهتر واضح باشند، از نامهای طولانی تر بهتر هستند. هیچ متنی بیشتر از آن چیزی که لازم است، به یک نام اضافه نکنید.
نام های accountAddress و customerAddress نام های خوبی برای نمونه های کلاس Address هستند اما برای نام کلاس ها مناسب هستند. Address یک نام خوب برای یک کلاس است. اگر نیاز به تفکیک بین آدرس های MAC، آدرس پورت ها و آدرس های وب داشته باشم، ممکن است PostalAddress، MAC و URI را در نظر بگیرم. نامهای به دست آمده دقیق تر هستند، و این، هدف همه نامگذاری ها است.
</div>

<div dir="rtl">
 
## سخن پایانی

سخت‌ترین کار در انتخاب نام‌های خوب این است که به مهارت‌های توصیفی خوب و پیشینه فرهنگی مشترک نیاز دارد. این مسئله یک مسئله فنی، تجاری یا مدیریتی نیست بلکه یک موضوع آموزشی است. در نتیجه بسیاری از افراد یاد نمی‌گیرند که این کار را به درستی انجام دهند.
مردم همچنین از ترس اینکه برخی از توسعه دهندگان دیگر مورد تخریب قرار گیرند، از تغییر نام چیزها می‌ترسند. ما این ترس را نداریم و وقتی اسم ها به چیز بهتری تغییر می‌کنند واقعاً سپاسگزاریم. بیشتر مواقع ما واقعا نام کلاس‌ها و متدها را به خاطر نمی‌آوریم. ما از ابزارهای مدرن برای کلنجار رفتن با جزئیاتی از این دست استفاده می‌کنیم تا بتوانیم تمرکز خود را بر این مسئله بگذاریم که آیا کد باید مانند پاراگراف‌ها و جملات خوانده شود، یا شبیه به جدول‌ها و ساختار داده‌ها (یک جمله همیشه بهترین راه برای نمایش داده‌ها نیست). احتمالاً وقتی چیزها را تغییر نام دهید، دقیقاً مانند هر بهبود دیگری در کد، افراد را شگفت زده خواهید کرد. اجازه ندهید که این مسئله شما را در مسیرتان متوقف کند.
برخی از این قوانین را دنبال کنید و ببینید آیا خوانایی کد خود را بهبود می‌بخشید یا خیر. اگر کد شخص دیگری را نگهداری می‌کنید، برای حل این مشکلات از ابزارهای refactoring استفاده کنید. در کوتاه مدت نتیجه را می بینید و درازمدت به افزایش بهبودها ادامه می‌دهد.
</div>

* [فصل قبل](../01_Clean_Code/Clean_Code.md)
* [فصل بعد](../03_Functions/3_Functions.md)
