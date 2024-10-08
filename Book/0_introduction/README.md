<div dir="rtl">

# مقدمه
<p align="center">
  <img src="https://github.com/hootanht/clean-code-persian/blob/master/0_introduction(completed)/img-0-1.png"/>
</p>

کدامیک از این تصاویر شما را در کدنویسی نشان میدهد؟ کدامیک شما را در تیم یا شرکت نشان میدهد؟ چرا ما در آن اتاق هستیم؟ ایا این فقط یک بررسی ساده کد است یا یک سری مشکلات را مدت کمی بعد از اجرا پیدا خواهیم کرد؟ ایا به خاطر وحشت از خراب شدن کدی که فکر میکردیم درست است ان را دیباگ میکنیم؟ایا مشتری‌ها برنامه ما را ترک میکنند و مدیران ما را تنبیه میکنند؟ چطور مطمئن شویم در لحظات سخت، پشت درب درست قرار گرفته ایم؟ پاسخ این است: مهارت.

یادگیری مهارت از دو بخش تشکیل شده است: دانش و کار. شما باید اصول، الگوها، تمرین‌ها و شیوه‌های اکتشافی که یک هنرمند میداند را یاد بگیرید و آن دانش را با انگشتان، چشم‌ها، دل و روده تان حس کنید و سخت کار کنید و تمرین کنید.

من میتوانم به شما فیزیک راندن یک دوچرخه را یاد بدهم. در واقع، ریاضیات کلاسیک سر راست است. جاذبه، اصطکاک، تکانه زاویه‌ای، مرکز جرم و غیره را میتوان در کمتر از یک صفحه پر از معادلات نشان داد. با توجه به فرمول‌ها میتوانم به شما ثابت کنم که دوچرخه سواری عملی است و میتوانم همه دانشی که برای عملی کردن ان نیاز دارید را یاد بدهم. ولی هنوز در اولین دوچرخه سواری ات زمین خواهی خورد.

کد نویسی تفاوتی ندارد.ما میتوانیم همه اصول کد تمیز را یادداشت کنیم و بعد به شما اطمینان دهیم که با انجام آنها موفق میشوید\(به عبارتی بگذاریم وقتی سوار دوچرخه شدید زمین بخورید\)، اما کدام معلم این کار را میکند؟

نه،این راهی نیست که این کتاب میخواهد برود.

یادگیری نوشتن کد تمیز، کار سختی است. به بیشتر از دانستن اصول و الگوها نیاز دارد.

باید عرق بریزید. باید تمرین کنید و شکست هایتان را ببینید. باید کارهای دیگران و شکست هایشان را ببینید. باید ببینید چطور زمین میخورند و دوباره بلند میشوند.

باید ببینید چطور از شکست هایشان عذاب میکشند و چه قیمتی برای تصمیمات بد شان پرداخته اند.

آماده باشید که حین خواندن این کتاب سخت تمرین کنید. این یک کتاب تفریحی نیست که بتوانید انرا در هواپیما بخوانید و قبل از فرود انرا تمام کنید. این کتاب شما را به کار وادار میکند، پس سخت کار کنید. چه نوع کاری را انجام خواهید داد؟ باید کد بخوانید، مقدار زیادی کد؛ و شما را به چالش میکشد تا در مورد اینکه چه چیز آن کد خوب است و چه بدی‌هایی دارد فکر کنید. از ما خواسته میشود که بخش‌ها \(modules\) را از هم جدا و دوباره به هم وصل کنیم. برای این کار زمان و تلاش صرف خواهد شد ولی ما فکر میکنیم که ارزش آنرا دارد. ما این کتاب را به سه بخش تقسیم کرده ایم.اولین فصل اصول، الگوها و تمرین نوشتن کد تمیز را در بر میگیرد. تعداد کمی کد در این فصل قرار دارد که خواندن آنها چالش برانگیز است. آنها شما را برای خواندن فصل دوم اماده خواهد کرد. اگر پس از خواندن فصل اول کتاب را کنار گذاشتید، موفق باشید!

در بخش دوم کتاب کار سخت‌تر است. این فصل از چند بخش با پیچیدگی افزایشی تشکیل شده است. هر بخش تمرین تمیزکردن کمی کد است؛تبدیل کردن کدی که مشکلاتی دارد به کدی که مشکلات کمتری دارد. جزئیات این فصل سخت است. باید دائم بین کتاب و یادداشت برداری جابه جا شوید. باید کدی که با ان کار میکنیم را آنالیز و درک کنید و برای هر تغییری که در کد ایجاد میکنیم استدلال کنید. کمی زمان بگذارید چون این کار باید چند روز زمان ببرد.

بخش سوم کتاب نتیجه گیری نهایی است. این فصل شامل لیستی از اکتشافات و بوهایی \(منظور چیزهایی که کد را از حالت تمیز بودن خارج میکند که به بوی بد شهرت دارد\) است که هنگام یادگیری جمع‌آوری شده است. همینطور که پیش میرویم و کدها را تمیز میکنیم، برای هر کاری که انجام میدهیم تحت عنوان بو یا اکتشاف نکته برداری میکنیم. ما تلاش میکنیم واکنش خودمان را نسبت به کدی که خوانده ایم و تغییر داده ایم درک کنیم، و سخت تلاش کرده ایم که آنچه احساس کرده ایم، آنچه انجام داده ایم و دلیل ان را ضبط کنیم. نتیجه دانشی پایه است که به درک طرز تفکر ما هنگام نوشتن، خواندن و تمیز کردن کمک میکند.

اگر شما بخش‌های فصل دوم را به دقت نخوانید این دانش پایه ارزش محدودی خواهد داشت. در آن بخش‌های آموزشی هر تغییری که ایجاد کرده ایم را به دقت و با ارجاع به اکتشافات به طور کامل تشریح کرده ایم. این ارجاعات در کروشه مثل این ظاهر میشوند:\[H22\]. این به شما اجازه میدهد متنی که در ان از اکتشافات استفاده شده را ببینید. این نه تنها خودش اکتشاف نیست بلکه بسیار ارزشمند است، این رابطه آن اکتشافات و تصمیمات پراکنده ای که حین تمیز کردن کد در بخش‌های آموزشی گرفته ایم را مشخص می‌کند.

برای کمک بیشتر در رابطه با این روابط ما مرجعی در پایان کتاب قرار داده ایم که شماره صفحه هر ارجاع را نمایش میدهد. میتوانید از آن برای دیدن مکانی که هر اکتشاف ظاهر شده استفاده کنید.

اگر فصل اول و سوم را بخوانید و از بخش‌های اموزشی صرف نظر کنید پس بهتر است یک کتاب تفریحی در باره نوشتن نرم‌افزار خوب بخوانید. اما اگر برای بخش‌های اموزشی در فصل دوم وقت بگذارید، هر قدم کوچک را دنبال کنید، هر تصمیم لحظه ای، اگر خودتان را جای من قرار دهید، و خودتان را به همان مسیری که ما میرویم هدایت کنید، درک بیشتری از اصول و الگوها بدست خواهید اورد. آنها دیگر دانش تفریحی نخواهند بود. آنها به دل و روده، انگشتان و قلبتان راه پیدا کرده اند. آنها به بخشی از خودتان تبدیل خواهند شد همان طور که وقتی دوچرخه سواری یاد میگیرید به بخشی از وجودتان تبدیل میشود.



# تصویر روی جلد کتاب

<p align="center">
  <img width="75%" src=img0-2.png/>
</p>

تصویر روی جلد کتاب، کهکشان کلاه مکزیکی یا M104 است. M1104 در صورت فلکی سنبله واقع شده است و تقریبا ۳۰ میلیون سال نوری از ما فاصله دارد. در هسته آن یک سیاه چاله فوق عظیم قرار دارد که وزن آن حدود یک میلیارد برابر خورشید است.

آیا به شما تصویر انفجار ماه Praxis کلینگان را یادآوری می کند؟

من به طور واضح این صحنه را در پیشتازان فضا ۶ به یاد می آورم که حلقه استوایی، از باقی مانده انفجار دور می شد.

از آن زمان به بعد این صحنه حلقه استوایی یک قسمت مشترک در صحنه های انفجار فیلم های علمی و تخیلی شد. این صحنه حتی در انفجار آلدران در آخرین ویرایش در اولین فیلم جنگ ستارگان اضافه شد.

چه چیزی باعث ایجاد این حلقه در اطراف M104 شد؟ چرا این چنین برآمدگی (بالا رفتگی) مرکزی و چنین هسته ای روشن و کوچک دارد؟ چیزی که من فکر می کنم و به نظر می رسد که سیاه چاله مرکزی سرد شده است و انفجار یک سیاه چاله ۳۰,۰۰۰ سال نوری در وسط کهکشان اتفاق افتاده است. وای به حال هر تمدنی که ممکن است در مسیر این پدیده کیهانی قرار داشته باشد.

این سیاه چاله فوق عظیم کل ستاره ها را برای ناهاراش می بلعد، جرم آنها تبدیل به یک مقدار قابل توجهی از انرژی می شود.

*E = mc2* به اندازه کافی قدرت دارد، اما هنگامی که M یک توده ستاره ای است، توجه داشته باشید! تا زمانی که هیولا سیر شود چند عدد از ستاره ها را بی مهابا درون شکم اش قرار می دهد؟ آیا اندازه فضای خالی مرکزی می تواند یک سرنخ برای ما باشد؟

تصویر M104 بر روی جلد ترکیبی از عکس نور قابل مشاهده از هابل (عکس اول) و تصوير مادون قرمز از تلسکوپ فضایی اسپیتزر (عکس دوم) است.

تصویر مادون قرمز به وضوح به ما ذات حلقه کهکشان را نشان میدهد. در نور مرئی ما تنها به جا حلقه را در به صورت سیاه می بینیم. بالا رفتگی مرکزی، بقیه حلقه را تیره می کند.

اما در مادون قرمز، ذرات داغ در حلقه از طریق باله مرکزی می درخشد. دو تصویر به ما دیدگاهی را نشان می دهد که قبلاً ندیده‌ایم و حاکی از آن است که مدت ها پیش یک فعالیت مانند جهنم رخ داده است.

</div>
