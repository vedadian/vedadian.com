گفته شد که وقتی مشتق نسبت به ماتریس‌ها را مشتق نسبت به وکتورایز شده‌ی آن‌ها در نظر بگیریم (همان عملگر $\text{vec}$) دیگر لازم نیست با تنسورها و کووکتر و کنتراوکتورها سر و کله بزنیم. در عین حال لازم است ضرب کرونکر را از آن دنیا به این دنیا بیاوریم. ضرب کرونکر در محاسبات تنسوری، یک تنسور با ابعادی برابر با مجموع ابعاد تنسورهای ورودی‌اش ایجاد می‌کند. مثلاً ضرب کرونکر دو بردار یک ماتریس است. اما در دنیای ماتریس‌ها ضرب کرونکر صورت ساده‌تری دارد.

اگر دو ماتریس $A$ و $B$ داشته باشیم، ضرب کرونکر آن‌ها بصورت زیر تعریف می‌شود:
$$$
A\otimes B=\left[\begin{array}{cccc}
A_{11}B &A_{12}B &\cdots &A_{1m}B\\
A_{21}B &A_{22}B &\cdots &A_{2m}B\\
\vdots &\ddots &\vdots\\
A_{n1}B &A_{n2}B &\cdots &A_{nm}B
\end{array}\right]
$$$
به عنوان مثال:
$$$
\left[\begin{array}{cc}
3 &4\\
5 &6
\end{array}\right]
\otimes
\left[\begin{array}{cc}
1 &0\\
0 &1
\end{array}\right]
=
\left[\begin{array}{cccc}
3 &0 &4 &0\\
0 &3 &0 &4\\
5 &0 &6 &0\\
0 &5 &0 &6
\end{array}\right]
$$$

این ضرب خواص زیبایی دارد که در زمینه‌ی مشتق‌گیری نسبت به ماتریس، خاصیت زیر یکی از مهمترین‌هاست:
$$$
\text{vec}(AXB)=(B^T\otimes A)\text{vec}(X)
$$$

برای مثالی از کاربرد این رابطه، فرض کنید می‌خواهیم از $AXB$ نسبت به $X$ مشتق بگیریم:
$$$
\frac{\partial AXB}{\partial X}=\frac{\partial \text{vec}(AXB)}{\partial \text{vec}(X)}=\frac{\partial (B^T\otimes A)\text{vec}(X)}{\partial \text{vec}(X)}=(B^T\otimes A)
$$$

## مشتق‌های زنجیره‌ای و مشتق ضرب توابع
در دنیای اسکالرها کلی رابطه برای مشتقات وجود داشت که کار راه‌انداز بودند. این  روابط به شکل‌های مشابهی در دنیای ماتریس‌ها هم برقرارند. یکی از آن رابطه‌ها، رابطه‌ی ضرب اسکالر و جمع بود که در پُست پیشین به آن اشاره کردم.

بد نیست علت برقراری آن رابطه را ببینیم:
$$$
\begin{aligned}
\frac{\partial (\alpha A+\beta B)}{\partial X}_{ij}&=\frac{\partial \text{vec}(\alpha A+\beta B)_i}{\partial \text{vec}(X)_j}=\frac{\partial \alpha\text{vec}(A)_i}{\partial \text{vec}(X)_j}+\frac{\partial \beta\text{vec}(B)_i}{\partial \text{vec}(X)_j}\\
&=\alpha\frac{\partial \text{vec}(A)_i}{\partial \text{vec}(X)_j}+\beta\frac{\partial \text{vec}(B)_i}{\partial \text{vec}(X)_j}\\
&=\alpha\frac{\partial \text{vec}(A)}{\partial \text{vec}(X)}_{ij}+\beta\frac{\partial \text{vec}(B)}{\partial \text{vec}(X)}_{ij}
\end{aligned}
$$$

به همین ترتیب می‌توان مشاهده کرد که مشتق زنجیره‌ای از توابع بصورت زیر قابل محاسبه است:
$$$
\begin{aligned}
\frac{\partial f(g(X))}{\partial X}_{ij}&=\sum_k \frac{\partial \text{vec}(f(g(X)))_i}{\partial \text{vec}(g(X))_k}\frac{\partial \text{vec}(g(X))_k}{\partial \text{vec}(X)_j}\\
&=\left(\frac{\partial f(g(X))}{\partial g(X)}\frac{\partial g(X)}{\partial X} \right)_{ij}
\end{aligned}
$$$
و بصورت عام‌تر:
$$$
\begin{aligned}
\frac{\partial h(f(X),g(X))}{\partial X}_{ij} &=\begin{aligned}
&\sum_k \left(\frac{\partial \text{vec}(h(f(X),g(X)))_i}{\partial \text{vec}(f(X))_k}\frac{\partial \text{vec}(f(X))_k}{\partial \text{vec}(X)_j}+\right.\\
&\left.\frac{\partial \text{vec}(h(f(X),g(X)))_i}{\partial \text{vec}(g(X))_k}\frac{\partial \text{vec}(g(X))_k}{\partial \text{vec}(X)_j}\right)
\end{aligned}\\
&=\left(\frac{\partial h(f(X),g(X))}{\partial f(X)}\frac{\partial f(X)}{\partial X}+\frac{\partial h(f(X),g(X))}{\partial g(X)}\frac{\partial g(X)}{\partial X}\right)_{ij} &
\end{aligned}
$$$
ظاهراً کسانی که قاعده‌ی ضرب ماتریس را بصورت ضرب سطر اولی در ستون دومی تعریف کرده‌اند، بنای درستی را نهاده‌اند! همه چیز با هم جور در می‌آید.

اما در مورد ضرب دو تابع روابط چگونه‌اند؟
$$$
\frac{\partial f(X)g(X)}{\partial X}=?
$$$
اگر از بررسی عنصر به عنصر مشتق خسته شده‌اید، حق دارید؛ من هم خسته شدم. برای همین از استاد کرونکر کمک می‌گیریم تا این یکی را بیابیم. اول تابع $h(X,Y)=XY$ را در نظر می‌گیریم. مشتق این تابع نسبت به $X$ و $Y$ با استفاده از رابطه‌ی کرونکر بصورت زیر قابل محاسبه است:
$$$
\begin{aligned}
&\text{vec}(XYI)=(I\otimes X)\text{vec}(Y)\Rightarrow\frac{\partial XY}{\partial Y}=(I\otimes X)\\
&\text{vec}(IXY)=(Y^T\otimes I)\text{vec}(X)\Rightarrow\frac{\partial XY}{\partial X}=(Y^T\otimes I)
\end{aligned}
$$$
با این حساب مشتق $h(f(X),g(X))=f(X)g(X)$ می‌شود:
$$$
\frac{\partial f(X)g(X)}{\partial X}=(I\otimes f(X))\frac{\partial g(X)}{\partial X}+(g(X)^T\otimes I)\frac{\partial f(X)}{\partial X}
$$$
اینکه ماتریس‌های همانی مربوط به $f$ و $g$ را با یک نماد نوشتم، نباید باعث شود که فرض کنید هر دو ابعاد یکسانی دارند. ماتریس $I$ که در $f(x)$ ضرب کرونکر می‌شود ابعادی برابر $g(X)$ دارد و بالعکس.

نکته‌ی کوچکی هست که صحبتش را بکنیم، پرونده‌ی این پُست را بسته‌ایم. ما در مورد عملگر $\text{vec}$ صحبت کردیم، اما در مورد اثر ترانهاده بر این عملگر چیزی نگفتیم. مثلاً وقتی بخواهیم مشتق $A^T$ را نسبت به $A$ محاسبه کنیم:
$$$
\frac{\partial A^T}{\partial A}=\frac{\partial \text{vec}(A^T)}{\partial \text{vec}(A)}
$$$
مقدار $\text{vec}(A^T)$ چه می‌شود؟ آیا رابطه‌ای با $\text{vec}(A)$ دارد؟ خب معلوم است که سؤال هجو پرسیده‌ام و رابطه دارند اما چه رابطه‌ای؟

برای مثال یک ماتریس ۳ در ۳ را نگاه کنیم:
$$$
\begin{aligned}
&\text{vec}\left(\begin{bmatrix}
1 &2 &3\\
4 &5 &6\\
7 &8 &9
\end{bmatrix}\right)=\begin{bmatrix}
1\\
4\\
7\\
2\\
5\\
8\\
3\\
6\\
9
\end{bmatrix}
&\text{vec}\left(\begin{bmatrix}
1 &2 &3\\
4 &5 &6\\
7 &8 &9
\end{bmatrix}^T\right)=\begin{bmatrix}
1\\
2\\
3\\
4\\
5\\
6\\
7\\
8\\
9
\end{bmatrix}
\end{aligned}
$$$
رابطه‌ی این دو ماتریس را می‌شود اینطور نوشت:
$$$
\begin{bmatrix}
1 &0 &0 &0 &0 &0 &0 &0 &0\\
0 &0 &0 &1 &0 &0 &0 &0 &0\\
0 &0 &0 &0 &0 &0 &1 &0 &0\\
0 &1 &0 &0 &0 &0 &0 &0 &0\\
0 &0 &0 &0 &1 &0 &0 &0 &0\\
0 &0 &0 &0 &0 &0 &0 &1 &0\\
0 &0 &1 &0 &0 &0 &0 &0 &0\\
0 &0 &0 &0 &0 &1 &0 &0 &0\\
0 &0 &0 &0 &0 &0 &0 &0 &1
\end{bmatrix}
\begin{bmatrix}
1\\
2\\
3\\
4\\
5\\
6\\
7\\
8\\
9
\end{bmatrix}
=
\begin{bmatrix}
1\\
4\\
7\\
2\\
5\\
8\\
3\\
6\\
9
\end{bmatrix}
$$$
ماتریس‌های متشکل از یک ۱ در هر سطر، ماتریس‌های بازترتیب(permutation) هستند. کارشان این است که سطرهای بردار (یا ماتریس)ی که در آن ضرب می‌شوند را در ترتیب دیگری قرار می‌دهند. ما در جبر ماتریسی به ماتریس بازترتیبی که $\text{vec}(A)$ را به $\text{vec}(A^T)$ تبدیل کند، ماتریس $T$ می‌گوییم. اما چیزی که باقی مانده بود؛ مشتق ضرب کرونکر. تحوه‌ی محاسبه‌ی این یکی را بیایید بی‌خیال شویم. فقط بدانیم که مشتق ضرب کرونکر نتیجه‌ای مثل زیر دارد:
$$$
\frac{\partial A\otimes B}{\partial A}=(I_n\otimes T_{qm} \otimes I_p)(I_{mn}\otimes \text{vec}(B))
$$$
این دفعه ابعاد ماتریس‌های همانی و $T$ را نوشتم چون اگر ننویسیمشان احتمال قاطی شدن همه چیز با هم بالا میرود و قیمه‌ها درون ماست‌ها میریزد. عبارت بالا با فرض آن است که $A$ ابعادش $m\times n$ و $B$ ابعادش $p\times q$ است.

خب، پرونده کرونکر را هم بستیم، در پست بعدی به توابعی مثل معکوس و دترمینان ماتریس خواهیم پرداخت تا در آخرین پست، چند نمونه مسأله را با این ابزارهایی که صحبتشان شد، حل کنیم.