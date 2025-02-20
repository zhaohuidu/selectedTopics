---
title: 抛硬币出现连续正面的概率计算
categories: 算法
keywords: 'coin, probability'
thumbnail: "../thumbnails/coin.jpg"

tags:
  - coin
  - probability
  - Fibonacci
date: 2019-11-3 16:50:20
---

# 摘要
mathe于2008年7月引用[百度知道中一个抛硬币的概率问题]  
抛硬币100次，出现10次以上连续正面的概率是多少？  
我们将它推广一下,抛硬币n次,其中出现过t次连续正面的概率是多少?  
shshsh_0510[发现这个问题](https://bbs.emath.ac.cn/forum.php?mod=redirect&goto=findpost&ptid=667&pid=8254&fromuid=20)
和[广义t阶Fibonacci数列](http://mathworld.wolfram.com/CoinTossing.html) 有关系, 其中不出现t次连续正面的概率为$\frac{F_{n+2}^{(t)}}{2^n}$  
或者说对于本题，就是要计算$1-\frac{F_{102}^{(10)}}{2^{100}}$  
最后我们得出广义t阶Fibonacci数列$F_n^{(t)}=round(\frac{r_t^{-1}(1-r_t)}{2t-(t+1)r_t} r_t^{n})$, 其中出现过t次连续正面的概率是多少
$r_t$是方程$x^{t+1}-2x^t+1=0$的唯一一个大于1的实数根。

# 具体内容
在mathe[转载了内容](https://bbs.emath.ac.cn/thread-667-1-1.html) 而shshsh_0510发现了wolfram网站上给出了问题和冠以Fibonacci数列的关联以后
mathe先[通过矩阵方法验证](https://bbs.emath.ac.cn/forum.php?mod=redirect&goto=findpost&ptid=667&pid=8258&fromuid=20) 了这种关联。  
## 广义Fibonacci数列
我们需要计算硬币抛n次过程中出现t次连续正面的概率。
我们对抛硬币过程中出现的状态进行分类：
其中$s_0$表示还没有出现t次连续正面的情况，而且当前最后一次硬币出现状态为反面或初试状态。
其中$s_1$表示还没有出现t次连续正面的情况，而且当前最后一次硬币出现状态为正面但是最后才连续1次正面
其中$s_2$表示还没有出现t次连续正面的情况，而且当前最后一次硬币出现状态为正面而且最后正好连续2次正面
...
其中$s_{t-1}$表示还没有出现t次连续正面的情况，而且当前最后一次硬币出现状态为正面而且最后正好连续t-1次正面
其中$s_t$表示已经出现t次连续正面的情况。
所以开始抛硬币之前处在状态$s_0$,然后抛硬币过程中，如果当前状态为$s_k$,其中$k\lt t$,那么抛一次硬币有$\frac12$的概率
转移到$s_0$,还有$frac12$的概率转移到$s_{k+1}$;而如果当前状态为$s_t$,那么状态永远不会再改变。
所以我们可以得到状态转移矩阵M为
$\begin{bmatrix}\frac12&\frac12&\frac12&\dots&\frac12&0\\ \frac12&0&0&\dots&0&0\\0&\frac12&0&\dots&0&0\\0&0&\frac12&\dots&0&0\\ \dots&\dots&\dots&\dots&\dots&\dots\\ 0&0&0&\dots&\frac12&1\end{bmatrix}$
其中我们知道对于初试状态，状态只可能为$s_0$,对应概率分布为$b=\begin{bmatrix}1\\ 0\\0\\ \dots\\0\\0\end{bmatrix}$
其中b是一个长度为t+1的列向量，b的第h个分量表示状态在$s_{h-1}$的概率
而经过k步以后，状态分布的概率变化为$M^k b$
而我们需要的值为经过n步以后到达状态$s_t$的概率，也就是$M^n b$最后一个分量，或者我们记
$u=(0,0,0,...,0,1)$
那么所求的结果就是$a_n = u M^n b$
为此我们可以先对矩阵M进行分析，由于矩阵2M更加简单，我们改为分析矩阵2M
而矩阵2M的特征多项式很简单，为
$g(x)=(2-x)(1+x+...+x^{t-1}-x^t)$
$=\frac{(x^{t+1}-2x^t+1)(2-x)}{x-1}$
所以矩阵M的特征多项式为$f(x)=g(2x)$
所以我们知道如果计算结果为$a_n$,
我们知道数列一个特征根为1，而由于n趋向无穷时$a_n\to 1$，
可以假设$a_n=1+u_1 (\frac{x_1}2)^n+u_2 (\frac{x_2}2)^n+...+u_t (\frac{x_t}2)^n$
其中$x_1,x_2,...,x_t$时方程$x^{t+1}-2x^t+1$除去1以后的n个根
我们可以假设$b_n=2^n (1-a_n)$
那么我们可以知道$\frac{x^{t+1}-2x^t+1}{x-1}$是数列$b_n$的特征多项式
也就是说，数列$b_n$满足递推式$b_{n+t+1}=2b_{n+t}-b_n$或者$b_{n+t}=b_{n+t-1}+b_{n+t-2}+...+b_n$,这个应该就是所谓的广义Fibonacci数列

## 数值计算结果
有了上面结论，我们就可以比较轻松数值计算题目中要求的结果了。  

也就是为了对于给定的t,如果我们要计算对应通项公式，可以通过解方程$x^{t+1}-2x^t+1=0$的所有解，然后就可以有通项公式了。  
谁来数值计算一下$F_102^{(10)}$和$F_1002^{(10)}$?
其中$F_0^{(10)}=0,F_1^{(10)}=1,F_2^{(10)}=1,F_3^{(10)}=2,F_4^{(10)}=4,F_5^{(10)}=8,F_6^{(10)}=16,F_7^{(10)}=32,F_8^{(10)}=64,F_9^{(10)}=128,F_10^{(10)}=256,F_11^{(10)}=512,F_12^{(10)}=1023$
而$F_{n+11}^{(10)}=2F_{n+10}^{(10)}-F_n^{(10)}$
而抛100次，存在连续正面10次以上的概率为$1-\frac{F_102^{(10)}}{2^100}$。  

无心人很快给出了数值结果  
$F_102^{(10)} = 1211700015849788251502892752696 $
至于那个分式 = 0.9558627713595783
于是投100次硬币出现连续10次以上的概率为1-0.9558627713595783=0.0441372286404217。  

后面mathe继续用后面分析的公式解进行[数值计算给出了精度更高的结果]  
而对于t=10,使用计算器就可以算出$r_t=1.9990186327101011386634092391292$
所以$b(102)~=1211700015849788251502892752685.8,b(1002)~=6.5849587983847754109895686668823e+300$
而投100次不出现连续10个正面的概率为$\frac{b(102)}{2^100}=0.95586277135957831225932140641244$,
而投1000次不出现连续10个正面的概率为$\frac{b(1002)}{2^1000}=0.61455024758751836408966755676318$,


## 公式解分析
不过mathe显然不满足于对前面的数值解，此后花费了很多精力去分析其公式解。由于已经知道其特征多项式为$\frac{$x^{t+1}-2x^t+1}{x-1}$, 所以其公式解必然和这个多项式的根有关系。  
他先做了[如下的理论推导]  

我们现在看看能否给出t阶Fibonacci数列$b(n)=F_n^{(t)}$的通项公式  
我们采用初试值$b(-t+2)=b(-t+3)=...=b(0)=0,b(1)=1,b(2)=1$  
为了方便起见，我们可以定义$b(-t+1)=1$,在这样定义以后b(1)的值也可以通过递推公式来递推得到了。  
我们定义$h(y)=y^{t+1}-2y+1=k(y)(y-1)$,那么我们知道$\frac{h(\frac 1y)y^{t+1}}{y-1}$是数列的特征方程，所以$h(y)$的所有根是数列特征值的倒数。  
对于复平面上的圆$|z|=d$其中$sqrt(\frac 23)\le d\lt 1$,我们有  
$|z^{t+1}|=d^{t+1}\le 2d-1\le |2z-1|$
所以根据[儒歇定理]，我们知道$h(y)$在圆$|z|\le d$内部解的数目同方程$-2z+1=0$的数目相同。也就是在$|z|\lt 1$内方程$h(z)=0$只有一个解。而如果$|z|=1$满足$h(z)=0$,那么容易得到z=1,而且根的重数为1。  
由此我们得到h(z)有一个根在单位圆内部，t-1个根在单位圆外部，此外还有一个根1。  
假设单位圆内部根为$z_1$,单位圆外部的根为$z_2,z_3,...,z_t$,于是我们可以假设数列$b(n)$的通项公式为  
$u_1z_1^{-n}+u_2z_2^{-n}+...+u_{t}z_t^{-n}$  
其中$u_1,u_2,...,u_t$为待定系数。  
采用类似[随机游走中的概率问题]的方法可以得到其中 $u_s=\frac 1{k'(z_s)}$  
而我们知道  
$k(z)=(z-z_1)(z-z_2)(z-z_3)...(z-z_t)$  
$k'(z_s)=\prod_{h!=s} (z_s-z_h)$  
$h'(z_s)=(z-1)\prod_{h!=s} (z_s-z_h)$  
所以$k'(z-s)=\frac{h'(z_s)}{z_s-1}$  
$u_s=\frac{z_s-1}{h'(z_s)}$  
而$h'(z_s)=(t+1)z_s^t-2=(t+1)(2-z_s^{-1})-2=2t-(t+1)z_s^{-1}$  
所以  
$u_s=\frac{z_s-1}{2t-(t+1)z_s^{-1}}$  

记$r_t=z_1^{-1}$  
而递推式中其他各式绝对值都远远小于，所以我们得到，对于n充分大  
$b(n)=round(\frac{r_t^{-1}(1-r_t)}{2t-(t+1)r_t} r_t^{n})$  
而他怀疑这个表达式对所有的n都成立，不过这是还没有验证，但是如果仅仅数值计算，不需要精确值，由于$|r|$接近2，而其他项都递减，所以用上面公式精度已经非常高了。  

继续对$b(n)~=\frac{r_t^{-1}(1-r_t)}{2t-(t+1)r_t} r_t^{n}$做误差分析可以得出  
除了上面一项以外，实际上还有t-1项$u_s z_s^{-n}, s=2,3,...,t$.  
其中对于$n\ge 1$,有$|u_s z_s^{-n}|\le |u_s*z_s|\le \frac{1+|z_s^{-1}|}{|2t-(t+1)|}\lt \frac2{t-1}$
所以t-1项之和的绝对值小于$\frac{2(t-1)}{t-1}=2$  
也就是说使用这个公式$b(n)$的误差必然小于2. 而则换成计算概率，第n项的计算误差小于$\frac1{2^{n-1}}$，对于比较大的n仅使用第一项计算精度非常高，实际计算过程中可能r采用的精度都没有这么高。  

后面他使用数值计算进行验算发现：  

而对于公式$b(n)=\lflooru_1 r_t^n\rfloor$,计算前面若干项我们可以发现猜想均成立:

[table=450][tr][td=1,1,50][align=center]n[/align][/td][td=1,1,50][align=center]b(n)[/align][/td][td][align=center]$u_1r_t^n$[/align][/td][/tr][tr][td]1[/td][td]1[/td][td]0.50222005921650437539314900083299[/td][/tr][tr][td]2[/td][td]1[/td][td]1.0039472560945626042296717505261[/td][/tr][tr][td]3[/td][td]2[/td][td]2.0069092711912102894563100627129[/td][/tr][tr][td]4[/td][td]4[/td][td]4.0118490272698787619203081600742[/td][/tr][tr][td]5[/td][td]8[/td][td]8.0197609571323822998698197638132[/td][/tr][tr][td]6[/td][td]16[/td][td]16.031651583188626895454838284701[/td][/tr][tr][td]7[/td][td]32[/td][td]32.047570227910457178387829251568[/td][/tr][tr][td]8[/td][td]64[/td][td]64.063690018678506437470207581223[/td][/tr][tr][td]9[/td][td]128[/td][td]128.06451002750246161567818042825[/td][/tr][tr][td]10[/td][td]256[/td][td]256.00334173386700758850659443979[/td][/tr][tr][td]11[/td][td]512[/td][td]511.75545016205159804636690872342[/td][/tr][tr][td]12[/td][td]1023[/td][td]1023.0086802648866917173406684459[/td][/tr][tr][td]13[/td][td]2045[/td][td]2045.0134132736788208304516651412[/td][/tr][tr][td]14[/td][td]4088[/td][td]4088.0199172761664313714470202193[/td][/tr][tr][td]15[/td][td]8172[/td][td]8172.0279855250629839809737322779[/td][/tr][/table]

然后他发现这个问题中特征方程$x^{t+1}-2x^t+1$是[随机游走中的概率问题]中进t退1的特殊情情况,最后可以得出我们使用上面的近似公式那么b(n)的误差为$q(n)-q(n+1)$,其中q(n)是[那个问题中5#]中定义的当A在B后面n步时能够返回原点的概率.所以我们只要能够证明$0<q(n)<\frac12$那么就证明了只使用一项然后四舍五入的方法是正确的。而且也可以解释上面例子中除了第一项以外，后面所有项的误差都是正的情况（因为q(n)应该是单调的）
而且显然对于固定的n,q(n)关于t是递减的，所以如果我们能够对于t=2的情况计算出$q(1)\le\frac12$,那么对于所有这一类题目，都能够直接使用那个近似计算方法，然后四舍五入，而结果就会是精确的。

以此为基础，他又开始对[上面猜测正确性的分析]：  
为了证明猜想公式的正确性，即$b(n)=round(\frac{r^{-1}(r-1)}{(t+1)r-2t}r^n)$  
我们先证明  
$、frac{r^{-1}(r-1)}{(t+1)r-2t}\lt\frac12$  
其中$1\lt r\lt 2$而且$r^{t+1}-2r^t+1=0$,  
首先我们容易得出$(t+1)r-2t>0$  
这个是因为方程$h(x)=x^{t+1}-2x^t+1$中，显然$h(\frac{2t}{t+1})\lt 0,h(2)\gt 0$,而且前面已经证明r是h(x)唯一一个绝对值大于1的解，所以$\frac {2t}{t+1}\lt r\lt 2$  
所以我们需要证明$2(1-r^{-1})\lt(t+1)r-2t$也就是$(2-r)r\lt \frac2{t+1}$  
由于我们知道$r^t(2-r)=1$,所以只要证明$r^{t-1}\gt\frac{t+1}2$也就是$r\gt \sqrt[t-1]{\frac{t+1}2}$（其中$t\ge2$）  
而其中右边容易得出在$t=2$时有最大值1.5,而$r\gt\frac{2t}{t+1}$,所以在$t\ge 4$时都已经有不等式成立。  
而对于t=3时，我们知道$$r_3=1.83\dots\gt 1.5$$,对于t=2,$r_2=1.618\dots\gt 1.5$  
所以我们证明了$\frac{r^{-1}(r-1)}{(t+1)r-2t}\lt\frac 12$  
而更加容易的，我们可以证明$\frac{r-1}{(t+1)r-2t}\gt\frac12$ (这个等价于$r\lt 2$),  
而又有前面的不等式得到$\frac{r-1}{(t+1)r-2t}\lt\frac r2\lt1$  

现在我们定义序列$e(n)=b(n)-\frac{r^{-1}(1-r)}{2t-(t+1)r}r^n$也就是这个误差，其中对于所有的$n\ge-t+2$全部定义。  
而其中$b(n)$在$n\le 0$时定义为$b(-t+2)=b(-t+3)=...=b(0)=0$(这样所有这些项都可以用同一个递推公式推导了)  
显然$e(n)$也满足递推公式$e(n+t)=e(n+t-1)+e(n+t-2)+...+e(n)$,这个递推式对于所有的$n\ge -t+2$都成立。  
而且我们知道$e(0)=b(0)-\frac{r^{-1}(1-r)}{2t-(t+1)r}=-\frac{r^{-1}(1-r)}{2t-(t+1)r}$,所以$-\frac12\lt e(0)\lt 0$  
同样，我们知道$-\frac 12\lt e(-1)\lt 0,-\frac 12\lt e(-2)\lt 0,\dots,-\frac 12\lt e(-t+2)\lt 0$   
而$e(1)=1-\frac{r-1}{(t+1)r-2t}$,所以$0\lt e(1)\lt \frac12$  

我们现在定义一个模型，有一个人在实数轴的整点上行走（开始坐标大于1），他每次分别以均等的概率随机后退t格或前进1格，知道到达坐标位置$[-t+2,1]$之间停止；如果他最后停留在坐标x,那么我们说他最终的得分为$e(x)$,那么请问如果他的起始坐标为x,那么他最终得分的期望值是多少呢？(容易证明他最终会停止下来的概率为1)
非常有意思，对于任何一个位置n,他最终得分的期望值为$e(n)$,由于每个最终得分都在$-\frac12$和$\frac12$之间，我们可以知道所有的$e(n)$也必然只能在$-\frac12$和$\frac12$之间。也就是这种方法可以证明上面的猜想。

这个是因为假设开始在位置n的得分为$f(n)$,我们知道对于任意$n\ge 2$有$f(n)=\frac{f(n+1)+f(n-t)}2$或者说$f(n+1)=2f(n)-f(n-t)$,而且对于$n\le 1$有边界条件$f(n)=e(n)$
根据$f(n)$的递推公式我们知道它对应的特征方程为$x^{t+1}-2x^t+1$,这个方程有一个根的绝对值大于1，还有一个根为1，其余t-1个根绝对值小于1，根据
[随机游走中的概率问题]中的结论，由于数列$f(n)$有界，那么绝对值大于1的根所对应的项的系数必然为0。也就是$f(n)$的通项公式可以由其余t个特征根决定，也就是前t项已经唯一决定了数列$f(n)$,而由于$e(n)$也满足同样递推式，必然得出$f(n)=e(n)$


由此我们可以证明了前面猜想的正确性，即  
$b(n)="round"(\frac{r^{-1}(r-1)}{(t+1)r-2t}r^n)$  
而抛硬币n次出现连续t次正面的概率为  
$p(n)=1-\frac{"round"(\frac{r-1}{(t+1)r-2t}r^{n+1})}{2^n}; r\gt 1 && r^{t+1}-2r^t+1=0$  


[百度知道中一个抛硬币的概率问题]: https://zhidao.baidu.com/question/60176364.html
[如下的理论推导]: https://bbs.emath.ac.cn/forum.php?mod=redirect&goto=findpost&ptid=667&pid=8266&fromuid=20
[儒歇定理]: https://baike.baidu.com/item/%E5%84%92%E6%AD%87%E5%AE%9A%E7%90%86/3899479?fr=aladdin
[随机游走中的概率问题]: https://bbs.emath.ac.cn/viewthread.php?tid=331&page=2&fromuid=20#pid2938
[数值计算给出了精度更高的结果]: https://bbs.emath.ac.cn/forum.php?mod=redirect&goto=findpost&ptid=667&pid=8268&fromuid=20
[那个问题中5#]: https://bbs.emath.ac.cn/viewthread.php?tid=331&page=1&fromuid=20#pid2858
[上面猜测正确性的分析]: https://bbs.emath.ac.cn/forum.php?mod=redirect&goto=findpost&ptid=667&pid=8349&fromuid=20
