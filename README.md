# Compass_Colibration
matlab code for algorithm of calibration for a compass sensor 

1	‌مقدمه
سنسورهای اندازه‌گیری میدان مغناطیسی، عموما دارای سه درجه آزادی در اندازه‌گیری هستند. به عبارتی، اندازه میدان مغناطیسی را در سه راستای عمود برهم (دستگاه مختصات کارتزین) اندازه‌گیری و گزارش می‌کنند. مجذور مربعات مقادیر اندازه‌گیری شده با این سنسورها در حالت آرمانی، برابر با اندازه میدان مغناطیسی زمین در موقعیت سنسور است. به این معنی که در صورت دوران سنسور در جهت‌های مختلف، تمام مقادیر اندازه‌گیری شده در سه محور، در بازه‌ی یکسانی حول مبدا مختصات خواهند بود.
اما در واقعیت ممکن است داده‌های اندازه‌گیری شده، ویژگی عنوان شده را نداشته‌باشند. برای مثال ممکن است داده‌های اندازه‌گیری شده در سه راستا به صورت متقارن و همگن نباشند. همچنین ممکن است مرکز دستگاه مختصات و مرکز داده‌های اندازه‌گیری شده بر یکدیگر منطبق نبوده و به اصطلاح داده‌ها بایاس داشته باشند. 
در این گزارش سعی می‌شود فرایندی برای کالیبره کردن سنسورهای میدان مغناطیسی معرفی شود.   
2	داده‌برداری و پالایش داده‌ها
برای انجام کالیبراسیون سنسور، ابتدا باید تعداد مناسبی داده‌برداری انجام شود. به صورتی که فضای کاری سنسور تا حد قابل قبولی پوشش داده شود. برای این منظور، به هنگام داده‌برداری، سنسور را در زوایای مختلف دوران می‌دهیم. به گونه‌ای که برای مثال، محور x سنسور در راستای تمام جهت‌های فضای سه‌بعدی قرار گرفته‌باشد. سپس پیش‌پردازش‌های مورد نیاز بر روی داده‌ها انجام می‌شود و ماتریس تبدیل به گونه‌ای محاسبه می‌شود که داده‌های اندازه‌گیری شده بر کره‌ای به مرکز مبدا مختصات و با شعاعی برابر با اندازه میدان مغناطیسی محل کالیبراسیون منطبق شود. در ادامه به بررسی هریک از مراحل می‌پردازیم.

2‏.‏1	داده‌برداری از سنسور  
درصورتی که اندازه‌گیری به صورت صحیح انجام شده‌باشد، با ترسیم مقادیر اندازه‌گیری‌شده در دستگاه مختصات سه‌بعدی، باید حدودا تصویر یک بیضی‌گون به صورت زیر نمایش داده‌شود. مرکز این بیضی‌گون ممکن‌است منطبق بر مبدا دستگاه مختصات نباشد. همچنین اندازه‌ی سه قطر اصلی این بیضی‌گون می‌توانند با‌یکدیگر متفاوت باشند.

![image](https://github.com/SDNT8810/Compass_Colibration/assets/110291520/040bfdc2-edad-4047-8cbb-3a5b4bab8b90)

شكل ‏2‏.‏‌1  داده خام سنسور مغناطیسی

2‏.‏2	داده ایده‌ال
در صورت استفاده از سنسور آرمانی، و همچنین در صورت دوران سنسور به صورت آرمانی به گونه‌ای که تمام جهت‌های ممکن را پوشش دهد، داده نمونه‌برداری شده به صورت یک کره به مرکز مبدا مختصات و با شعاعی برابر با اندازه میدان مغناطیسی محل کالیبراسیون، به صورت زیر اندازه‌گیری می‌گردد. 

 ![image](https://github.com/SDNT8810/Compass_Colibration/assets/110291520/8be14ec1-269d-440e-b695-6eb28a93b1ef)

شكل ‏2‏.‏‌2  داده‌های فرضی از یه سنسور آرمانی

لازم به ذکر است که اندازه میدان مغناطیسی زمین در هر موقعیت جغرافیایی و همچنین در زمان‌های مختلف، متفاوت است. برای بدست‌آوردن اندازه میدان مغناطیسی، می‌توان از سایت‌های مختلفی استفاده کرد. اما در این گزارش با استفاده از تابع wrldmagm نرم‌افزار Matlab به صورت زیر، اندازه میدان برحسب میلی تسلا محاسبه شده‌است.
[XYZ, H, D, I, F] = wrldmagm(hight, Lat, Lon, decyear(2022,11,21),'2020');
2‏.‏3	محاسبه ماتریس تبدیل
برای محاسبه ماتریس انتقال مناسب به گونه‌ای که داده‌های سنسور را بر داده‌های سنسور آرمانی نگاشت کند، از تابع magcal متلب استفاده می‌کنیم. این تابع داده‌های اندازه‌گیری شده با سنسور را به یک کره‌ی حول مبدا مختصات، تصویر می‌کند. شعاع این کره ممکن است با اندازه میدان مغناطیسی برابر نباشد. لذا با ضرب اندازه میدان مغناطیسی زمین که با استفاده از تابع wrldmg محاسبه شده‌بود در کره‌ی بدست‌آمده از تابع magcal، و تقسیم آن بر شعاع کره‌ی بدست آمده از تابع magcal، داده‌های اندازه‌گیری‌شده با سنسور، بر کره‌ی سنسور آرمانی نگاشت می‌شود. نگاشت به صورت زیر انجام می‌شود.
Calibrated_Data = (Sensor_Data – b) × A × R_Ideal / R_Sensor
که در این رابطه b بردار انتقال بیضی‌گون به مبدا مختصات و A ماتریس تبدیل بیضی‌گون به کره آرمانی است. R_Sensor از تابع magcal بدست می‌آید و R_Ideal برابر با اندازه میدان مغناطیسی است که با استفاده از تابع wrdmg محاسبه شده.
مراحل فوق برای داده‌های شكل ‏2‏.‏‌2 طی شده و نتیجه به صورت زیر است.

 ![image](https://github.com/SDNT8810/Compass_Colibration/assets/110291520/96a0a6a6-9da9-44df-9d44-8bc755a92bf8)

شكل ‏2‏.‏‌3  داده‌های کالیبره‌نشده، کلیبره‌شده و سنسور آرمانی

 
3	بررسی صحت داده‌ها 
برای حصول اطمینان از صحت فرایند اندازه‌گیری و پالایش داده‌ها، بررسی چند شرط لازم است. یکی از این شروط، پوشش کل فضای کاری سنسور برروی سطح کره یا بیضی‌گون است. به عبارتی باید سنسور در تمامی جهت‌ها دوران داده‌شود که داده‌های اندازه‌گیری‌شده، سطح کره یا بیضی‌گون را به صورت کامل جارو کرده‌باشد. درغیراین‌صورت، ممکن‌است بیضی‌گونی که بر داده‌های اندازه‌گیری محاط می‌شود، با واقعیت هم‌خوانی نداشته‌باشد. شرط مهم دیگر، از ثابت‌بودن اندازه میدان مغناطیسی زمین در مکان و زمان مشخص، برداشت می‌شود. به عبارتی به دلیل ثابت بودن میدان مغناطیسی زمین در موقعیت مکانی و زمانی خاص، با دوران سنسور آرمانی نباید اندازه میدان که به وسیله سنسور اندازه‌گیری می‌شود تغییر کند. به بیان دیگر، در حالت آرمانی همه‌ی نقاط اندازه‌گیری شده باید برروی سطح یک کره با شعاعی مشخص قرار گیرند. خروج نقاط اندازه‌گیری‌شده از سطح کره، نشان‌دهنده‌ی اختلالات مغناطیسی اطراف سنسور است. در ادامه به روش بررسی برآورده‌شدن هریک از این شروط می‌پردازیم.
3‏.‏1	بررسی شرط پوشش سطح
برای بررسی این موضوع، می‌توان از هیستوگرام دوبعدی بر روی سطح کره‌ی نگاشته‌شده استفاده کرد. ابتدا داده‌های اندازه‌گیری‌شده با سنسور را از فضای کارتزین به دستگاه مختصات کروی تبدیل می‌کنیم. این کار با استفاده از تابع cart2sph در برنامه Matlab انجام می‌شود. با استفاده از تابع histcount2 با ورودی‌های azimuth و elevation داده‌های بیان‌شده در دستگاه کروی، به‌صورتی‌که در تصویر زیر مشاهده‌می‌شود، تعداد داده‌های اندازه‌گیری‌شده در بخش‌های مختلف سطح کره مشخص می‌شود. لازم است که درصد قابل توجهی از خانه‌های هیستوگرام دارای داده باشند.

 ![image](https://github.com/SDNT8810/Compass_Colibration/assets/110291520/4cc4e6a9-1233-43a2-ad60-1ee62e2f6a5f)

شكل ‏3‏.‏‌1  هیستوگرام درجه دو زوایای azimuth و elevation 

3‏.‏2	بررسی شرط هم‌شعاعی
همان‌طور که پیش‌تر عنوان‌شد، همه‌ی نقاط اندازه‌گیری‌شده در یک سنسور آرمانی، بر روی سطح یک کره با شعاعی برابر با اندازه‌ی میدان مغناطیسی زمین در مکان و زمان اجرای اندازه‌گیری قرار می‌گیرند. انتظار می‌رود ابر نقاط اندازه‌گیری‌شده با سنسور، بعد از اجرای فرایند کالیبراسیون، برروی سطح همین کره‌ی مرجع قرار بگیرد. دلیل این انتظار، فرض ثابت بودن اندازه‌ی میدان مغناطیسی طی فرایند اندازه‌گیری است. برای بررسی این موضوع، فاصله نقاط با مبدا مختصات مورد توجه قرار می‌گیرد.
طبیعتا همه‌ی نقاط به صورت کامل برروی سطح کره قرار ندارند. این امر می‌تواند بر اثر عدم تعامد محورهای سنسور، وجود نویز در داده‌ها یا اختلالات مغناطیسی محیط نظیر نزدیکی اجسام تاثیرگذار بر میدان مغناطیسی در مجاورت سنسور باشد. در صورتی که توزیع نقاط اندازه‌گیری‌شده در راستای شعاع در دستگاه مختصات کروی، حول مقدار شعاع کره‌ی مرجع متراکم و در شعاع‌های دیگر بسیار کمتر توزیع شده‌باشد، داده‌برداری قابل اعتماد بوده و شرط هم‌شعاعی برآورده می‌شود. 
در این گزارش از تابع histcount برای بررسی توزیع نقاط در راستای شعاع استفاده‌شده‌است. 


 ![image](https://github.com/SDNT8810/Compass_Colibration/assets/110291520/9a9d86cb-1094-40cf-80fd-af9c3a675dde)

شكل ‏3‏.‏‌2  هیستوگرام داده‌ها در راستای شعاع 
  