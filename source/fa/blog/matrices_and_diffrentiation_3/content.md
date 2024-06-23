عمده‌ی کارکرد مشتق‌گیری، یافتن نقاط اکسترموم است؛ اما نحوه‌ی استفاده از مشتق برای رسیدن به اکسترموم می‌تواند یا از طریق روش‌های iterative بهینه‌سازی باشد یا حل فرمال معادلات. برای اینکه از هر کدام از این موارد یک مثال دیده باشیم، اول مقدار ماتریس correlation را از روی تعدادی نمونه از یک توزیع گوسی تخمین می‌زنیم و بعدش گرادیان یک MLP را نسبت به وزن‌هایش محاسبه می‌کنیم.

## تخمین ماتریس correlation
فرمول توزیع گوسی چند متغیره چیزی است که در زیر آمده:
$$$
p(x|\mu,\Sigma)=\text{det}(2\pi\Sigma)^{-\frac{1}{2}}\text{exp}\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)
$$$
خب حالا اگر نمونه‌های $x_1$ تا $x_n$ از این توزیع داشته باشیم، چه مقادیری از $\mu$ و $\Sigma$ احتمال رخداد این نمونه‌ها را بیشینه می‌کنند؟

با فرض اینکه نمونه‌ها از هم مستقل باشند، احتمال رخداد همه‌شان می‌شود:
$$$
\begin{aligned}
&p(x_1,x_2,\ldots x_n|\mu,\Sigma)=\prod p(x_i|\mu,\Sigma)\\
&\text{log}\left(p(x_1,x_2,\ldots x_n|\mu,\Sigma)\right)=\sum \text{log}\left(p(x_i|\mu,\Sigma)\right)
\end{aligned}
$$$

وقتی می‌شود با لگاریتم احتمال همان کاری را کرد که با احتمال می‌شود کرد، چرا سراغش نرویم. مخصوصاً برای توزیع گوسی که تابع نمایی نقش اساسی در آن دارد. خب، با جایگذاری رابطه‌ی توزیع احتمال در فرمول لگاریتم‌ها داریم:
$$$
\text{log}\left(p(x_1,x_2,\ldots x_n|\mu,\Sigma)\right)=\sum \left[\text{log}\left(\text{det}(2\pi\Sigma)^{-\frac{1}{2}}\right)-\frac{1}{2}(x_i-\mu)^T\Sigma^{-1}(x_i-\mu)\right]
$$$
باز هم ساده‌ترش کنیم می‌شود:
$$$
\begin{aligned}
y&=\text{log}\left(p(x_1,x_2,\ldots x_n|\mu,\Sigma)\right)\\
&=-\frac{n}{2}\text{log}\left(2^m\pi^m\right)-\frac{n}{2}\text{log}\left(\text{det}(\Sigma)\right)-\frac{1}{2}\sum (x_i-\mu)^T\Sigma^{-1}(x_i-\mu)
\end{aligned}
$$$

برای این ساده‌سازی، من اول عبارت دترمینان را که ربطی به اندیس جمع نداشت از جمع خارج کردم که طبیعتاً ضرب در تعداد دفعات جمع خوردن می‌شود (یعنی $n$). بعدش $-\frac{1}{2}$ را از لگاریتم خارج کردم که بصورت ضرب ظاهر شود. در مرحله‌ی بعد، $\text{det}(2\pi\Sigma)$ را با استفاده از رابطه‌ی $\text{det}(kA)=k^n\text{det}(A)$ ساده کردم. در نهایت هم ضریب $-\frac{1}{2}$ را از درون جمع به بیرون انتقال دادم که به دلیل خاصیت پخشی ضرب امکان‌پذیر است (توجه کرده‌اید که $n$ تعداد نمونه‌ها و $m$ تعداد عناصر هر نمونه است؟)

حالا کافیست دو تا مشتق بگیریم و برابر صفر قرار دهیم. یکی نسبت به $\mu$(یک بردار) و دیگری نسبت به $\Sigma$ (یک ماتریس). اول با بردار شروع می‌کنیم:
$$$
\begin{aligned}
&\frac{\partial y}{\partial \mu}=-\frac{1}{2}(\Sigma^{-1}+\Sigma^{-T})\left((\sum x_i)-n\mu\right)=0\\
&\Rightarrow \mu=\frac{1}{n}\sum x_i
\end{aligned}
$$$
به همین سرعت محاسبه شد! می‌دانید چرا به $\Sigma^{-1}$ و همزادش $\Sigma^{-T}$ محل ندادم؟ چون طبق فرمول، این ماتریس‌ها معکوس‌پذیرند و فقط در یک حالت حاصل ضرب آن‌ها در یک بردار برابر صفر می‌شود و آن وقتی است که خود آن بردار صفر باشد؛ یعنی $(\sum x_i)-n\mu=0$.
اما مشتق دوم که جذاب‌تر هم هست:
$$$
\begin{aligned}
&\frac{\partial y}{\partial \Sigma}=\frac{\partial y}{\partial \Sigma^{-1}}\frac{\partial \Sigma^{-1}}{\partial \Sigma}\\
&\frac{\partial y}{\partial \Sigma^{-1}}=-\frac{n}{2}\text{vec}^T(\Sigma^T)-\frac{1}{2}\sum \left((x_i-\mu)^T\otimes(x_i-\mu)^T\right)\\
&\frac{\partial \Sigma^{-1}}{\partial \Sigma}=-(\Sigma^{-T}\otimes \Sigma^{-1})\\
&\Downarrow\\
&\frac{\partial y}{\partial \Sigma}=\begin{aligned}&\frac{n}{2}\text{vec}^T(\Sigma^T)(\Sigma^{-T}\otimes \Sigma^{-1})\\&+\frac{1}{2}\sum \left((x_i-\mu)^T\otimes(x_i-\mu)^T\right)(\Sigma^{-T}\otimes \Sigma^{-1})\end{aligned}
\end{aligned}
$$$
از طول و درازای فرمول بالا نترسید، در دلش چیزی نیست. کافیست از رابطه‌ی زیر برای ساده‌سازی استفاده کنیم:
$$$
\text{vec}(AXB)=(B^T\otimes A)\text{vec}(X)
$$$
و یا معادلش:
$$$
\text{vec}^T(AXB)=\text{vec}^T(X)(B\otimes A^T)
$$$
با جایگذاری به نتیجه‌ی زیر می‌رسیم:
$$$
\frac{\partial y}{\partial \Sigma}=\frac{n}{2}\text{vec}^T(\Sigma^{-T})+\frac{1}{2}\left[\sum \left((x_i-\mu)^T\otimes(x_i-\mu)^T\right)\right](\Sigma^{-T}\otimes \Sigma^{-1})=0
$$$
یا به عبارتی:
$$$
\text{vec}^T(\Sigma^{-T})(\Sigma^{-T}\otimes \Sigma^{-1})^{-1}=\frac{1}{n}\sum \left((x_i-\mu)^T\otimes(x_i-\mu)^T\right)=0
$$$
باز هم همان رابطه‌ی $\text{vec}(AXB)$. در پست اول که گفتم، این رابطه در مشتق‌گیری خیلی کاربرد دارد:
$$$
\text{vec}^T(\Sigma^{-T})(\Sigma^{-T}\otimes \Sigma^{-1})^{-1}=\text{vec}^T(\Sigma^{-T})(\Sigma^T\otimes \Sigma)=\text{vec}^T(\Sigma^T\Sigma^{-T}\Sigma^T)=\text{vec}^T(\Sigma^T)
$$$
اورکا، اورکا:
$$$
\text{vec}^T(\Sigma^T)=\frac{1}{n}\sum \left((x_i-\mu)^T\otimes(x_i-\mu)^T\right)
$$$
باز هم $\text{vec}(AXB)$! اگر $X$ یک عدد اسکالر، $A$ یک بردار سطری ($n\times 1$) و $B$ یک بردار ستونی باشد، چه رابطه‌ای حاصل می‌شود؟
$$$
\begin{aligned}
&\text{vec}(\alpha ab)=\text{vec}(a\alpha b)=(b^T\otimes a)\text{vec}(\alpha)=\alpha(b^T\otimes a)\\
&\text{vec}^T(\alpha ab)=\text{vec}^T(a\alpha b)=\text{vec}^T(\alpha)(b\otimes a^T)=\alpha(b\otimes a^T)
\end{aligned}
$$$
زیبا نیست؟ با این حساب:
$$$
\begin{aligned}
&(x_i-\mu)^T\otimes(x_i-\mu)^T=\text{vec}^T((x_i-\mu)(x_i-\mu)^T)\\
&\text{vec}^T(\Sigma^T)=\frac{1}{n}\sum\text{vec}^T((x_i-\mu)(x_i-\mu)^T)\\
&\text{vec}^T(\Sigma^T)=\text{vec}^T\left(\frac{1}{n}\sum \left((x_i-\mu)(x_i-\mu)^T\right)\right)\\
&\Rightarrow \Sigma=\Sigma^T=\frac{1}{n}\sum \left((x_i-\mu)(x_i-\mu)^T\right)
\end{aligned}
$$$
در اینجا باید بپرسید که چرا $\Sigma=\Sigma^T$؟ جوابش این است که وقتی $\Sigma^T=\frac{1}{n}\sum \left((x_i-\mu)(x_i-\mu)^T\right)$ باشد، ترانهاده‌اش برابر خودش می‌شود.

## مشتق نسبت به وزن‌های MLP
لایه‌های یک شبکه‌ی MLP را می‌توان بصورت زیر نشان داد:
$$$
y_{n+1}=\phi\left(W_ny_n+b_n\right)
$$$
بسته به این که MLP مورد نظر ما چند لایه باشد، وقتی می‌خواهیم آموزشش بدهیم، خروجی لایه‌ی $m$م را با نتایج دلخواه مقایسه می‌کنیم و خطا را محاسبه می‌کنیم. فرض کنیم که معیار خطای کمترین متوسط مربعات را در نظر داشته باشیم:
$$$
j(W_0,b_0,W_1,b_1,\ldots W_{m-1},b_{m-1})=\frac{1}{2}\|y_m-y\|^2=\frac{1}{2}(y_m-y)^T(y_m-y)
$$$
از ماشین‌آلاتی که برای مشتق‌گیری ساختیم برای محاسبه‌ی $\frac{\partial j}{\partial W_i}$ استفاده می‌کنیم. محاسبه‌ی مشتق نسبت به بایاس‌ها هم کاملاً مشابه است:
$$$
\frac{\partial j}{\partial W_i}=\frac{\partial j}{\partial y_m}\left(\prod_{m-1}^i \frac{\partial y_{j+1}}{\partial y_j}\right)\frac{\partial y_i}{\partial W_i}
$$$
حالا کافیست هر جزء را محاسبه کنیم.
$$$
\begin{aligned}
&\frac{\partial j}{\partial y_m}=(y_m-y)^T\\
&\frac{\partial y_{j+1}}{\partial y_j}=\text{diag}\left(\text{vec}\left(\phi'(W_jy_j+b_j)\right)\right)W_j\\
&\frac{\partial y_i}{\partial W_i}=(y_i^T\otimes I)
\end{aligned}
$$$
یافتن اینکه این‌ها را چطور با استفاده از مطالبی که در سه پُست گذشته ارائه کردم محاسبه کرده‌ام به خود شما واگذار می‌کنم که بیابید (این خودش تمرین خوبی است.) اما مباحث از اینجا به بعد شیرین‌تر می‌شود. برای اینکه بتوانم ادامه بدهم باید یک رابطه‌ی جدید را معرفی کنم.
$$$
a\odot b=\text{diag}(a)b \Rightarrow (a\odot b)^T=b^T\text{diag}(a)
$$$
علامت $\odot$ نمایشگر ضرب هادامارد یا عنصربه‌عنصر است. از طرفی وقتی $W_jy_j+b_j$ یک بردار است، $\phi'$ هم رویش اعمال شود، باز هم خروجی یک بردار است، پس می‌شود کل مشتق بالا را اینگونه نوشت:
$$$
\frac{\partial j}{\partial W_i}=(y_m-y)^T\left(\prod \text{diag}\left(\phi'(W_jy_j+b_j)\right)W_j\right)(y_i^T\otimes I)
$$$
بد نیست ببینیم، آن $\prod$ در وسط در واقع چه بلایی سر مقدار خطا ($y_m-y$) می‌آورد. از خود خطا شروع می‌کنیم:
$$$
e_j^T=(y_m-y)^T\text{diag}\left(\phi'(W_jy_j+b_j)\right)W_j=\left((y_m-y)\odot \phi'(W_jy_j+b_j)\right)^TW_j
$$$
انگار هر عنصر از خطا، با مقدار مشتق تابع $\phi$ به ازای همان عنصر وزن‌دهی می‌شود و بعدش بردار به دست آمده از سمت چپ در ماتریس وزن لایه‌ی قبلی ضرب می‌شود. این همان پس انتشار خطاست. هر عنصری از خطا، با وزن مربوطه، به یکی از گره‌های لایه‌ی زیرین مرتبط می‌شود.
همین عملیات را می‌شود ادامه داد:
$$$
e_{j-1}^T=e_j^T\text{diag}\left(\phi'(W_{j-1}y_{j-1}+b_{j-1})\right)W_{j-1}=\left(e_j\odot \phi'(W_jy_j+b_j)\right)^TW_{j-1}
$$$
باز هم مقدار خطای لایه‌ی بالا با مشتق $\phi$ در لایه‌ی زیرین وزن‌دهی شده و به لایه‌ی پایین‌تر منتشر می‌شود. اگر با همین فرمان پیش برویم به خطای لایه‌ی مورد نظر یعنی $e_i$ می‌رسیم:
$$$
\frac{\partial j}{\partial W_i}=e_i^T(y_i\otimes I)=\text{vec}^T(e_iy_i^T)
$$$
تمرین خوبی است که ببینید چرا رابطه‌ی بالا برقرار است. بررسی اینکه $\text{vec}(e_i)$ چه فرمی دارد در این مورد نقطه‌ی شروع است. بگذریم.

در نهایت این مقدار مشتق چه کاربردی دارد؟ از آن در بهینه‌سازی و رفتن به سمت مقدار آموزش دیده‌ی وزن‌ها استفاده می‌کنیم. در روش‌های بهینه‌سازی مبتنی بر گرادیان، همیشه مقدار گرادیان خطا نسبت به پارامترها با نسبتی منفی جمع می‌شود. این اساس همه‌ی روش‌های بهینه‌سازی مبتنی بر گرادیان است. اما در اینجا گرادیان نسبت به $W_i$ ابعادی مشابه با $W_i$ ندارد؛ چگونه باید این دو را جمع بزنیم؟

اگر از ابتدا سراغ تنسورها رفته بودیم، این مشکل پیش نمی‌آمد. مقدار مشتق هم ابعادی برابر با خود $W_i$ می‌داشت (صد البته چون $j$ یک عدد اسکالر است.) حالا هم اتفاق خاصی نیفتاده. یادتان هست که برای تعریف اینگونه مشتق‌گیری که در این چهار پُست با هم مرورش کردیم، از ابتدا قرار گذاشتیم که هر ماتریس را با یک زوج مرتب $(\text{vec}(A), (m,n))$ نمایش بدهیم. الآن هم که مشتق را داریم، ابعاد ماتریس‌های $W_i$ را هم که داریم. کافیست مشتق محاسبه شده را با ابعاد ماتریس اولیه باز نمایی کنیم. تعداد عناصر مشتق طبق تعریف یکی است.

نکته‌ی جالب ماجرا اینجاست که تا الآن که این پست را می‌نویسم، بدون استثنا، باز مرتب‌سازی مشتق برای من برابر با نگاه کردن به درون عملگر $\text{vec}$ بوده است. همین مثال MLP را نگاه کنید:
$$$
\frac{\partial j}{\partial W_i}=e_i^T(y_i\otimes I)=\text{vec}^T(e_iy_i^T)
$$$
مقدار $e_iy_i^T$ ابعادی دقیقاً برابر ماتریس اولیه دارد!

## نتایج اخلاقی
در نهایت این نتایج را به عنوان خلاصه تقدیم می‌کنم:

۱-برای گرفتن مشتق نسبت به ماتریس نیازی به محاسبات تنسوری نیست
۲-مشتق‌گرفتن برای ورزش ذهنی خوب است
۳-اصولاً اکثر اوقات نیاز نیست خودتان مشتق نسبت به ماتریس بگیرید!