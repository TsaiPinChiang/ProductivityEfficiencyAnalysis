---
tags: PEA
---

# PEA 期末報告 Tutorial Report

## 主題介紹： 環境 PEA

我們的主題是環境 PEA，這個主題的重點是在主產物 (good output) 與副產物 (bad output) 之間做出選擇。換句話說，我們希望在對副產物的產出做出各種限制的同時，盡可能去的產出更多的主產物，並去衡量每一個 DMU 的效率。

## 背景與動機
### 動機

人類目前以前所未有的速度在製造污染，因此，在確保經濟發展的狀況之下同時限制污染排放有事極為重要的。為了達成上述目的，各國開始使用 Cap-and-Trade System 來達到限制二氧化碳排放的手段，其中 Allocation of emission permits (AEP) 為立定相關法規提供了良好的基礎，同時也提供該如何分配碳權給各 DMU 的參考。

### 背景
我們這組以全台灣各縣市的焚化廠為分析對象，希望它們能在不製造太多污染物的狀況之下，盡可能的多生產電。我們將比較利用不同市場假設下的模型所算出來的最適碳權分配量的結果，而此研究將會確保各焚化廠產出最適切的污染量，同時維持效率，以增加社會福祉。


## 分析流程
![](https://i.imgur.com/2L4OnhQ.jpg)


## Data 介紹


| 參數 | 內容 |
| --- | --- |
| Desirable output($y$) | 實際發電量(kwhr) |
| Bad output($b$) | 二氧化碳當量(ton) |
| Fixed input($X$) | 設計處理量、設計發電量(ton) |
| Variable input($x$)| 勞動成本、其他成本、焚化處理量 |

* 資料來源主要來自環保署焚化廠營運管理資訊系統，我們搜集了來自於台灣 24 座發電廠 2020 的上述表格資料
* 因為搜集不到焚化廠的二氧化碳排放量，我們參考下述文章，並將焚化處理量乘以0.75(垃圾焚化後二氧化碳排放列約為0.75噸/噸垃圾)得二氧化碳之當量
https://www.sinotech.org.tw/journal/pdfview.aspx?n=65&s=35

## 模型介紹：
| $$x_1$$| 勞動成本|
| --- | --- |
| $$x_2$$| 其他成本|
| $$x_3$$| 焚化處理量|
| $$x_4$$| 設計處理量|
| $$x_5$$| 設計發電量(ton)|

### 1. trading system

我們的第一個模型是參考[paper]的一個交易模型。這個模型使用 by-production 的方式，將每一個焚化廠產出 good output 及產出 bad output 的兩個部分。產出 good output 的部分，會取所有的 input 及 good output 來計算生產可能函數。bad output 的部分則是取與 bad output 相關的 input ($x_3$) 及 bad output 來計算生產可能函數。

- 首先，先使用 NT(no trading) 模型，使用兩組變數 $\lambda$ 及 $w$ 分別向兩個生產函數的前緣靠近，並讓兩個生產函數中有重複出現的的 input 使用總量一樣。

$$
\begin{aligned}
Y^{NTt}_{j_{0}} & = Max_{\lambda_{j},\omega_{j},\theta}  \theta y_{j_0} \\
s.t.
& \quad x_{1j_{0}}\geq \sum_{j=1}^nx_{1j}\lambda_{j} \\
& \quad x_{2j_{0}}\geq \sum_{j=1}^nx_{2j}\lambda_{j} \\
& \quad x_{3j_{0}}\geq \sum_{j=1}^nx_{3j}\lambda_{j} \\
& \quad x_{4j_{0}}\geq \sum_{j=1}^nx_{4j}\lambda_{j} \\
& \quad x_{5j_{0}}\geq \sum_{j=1}^nx_{5j}\lambda_{j} \\
& \theta y_{j_{0}} \leq \sum^n_{j=1}y_{j}\lambda_{j} \\
& \sum^n_{j=1}\lambda_{j}=1\\
& x_{3j_{0}} \leq \sum_{j=1}^n x_{3j}\omega_{j}, \\
& u_{j_{0}} \geq \sum_{j=1}^n u_{j}\omega_{j} \\
& \sum_{j=1}^n\omega_{j}=1 \\
& \sum_{j=1}^n x_{3j}\lambda_{j} =\sum_{j=1}^n x_{3j}\omega_{j} \\
& \lambda_{j},\omega_{j}\geq 0,\quad j=1,2,...,n
\end{aligned}
$$

- 再來，使用 CIT(cross-industrial trading) 模型，在 NT 的基礎上，再加上一個調整係數 $\delta$ ，讓同一個時期內(年)的焚化廠之間可以交易碳排權，並限制交易後的總排放量不能比原本的總排放量多

$$ 
\begin{aligned}
Max_{\lambda_{jk}^t,\omega_{jk}^t,\theta_{k}^t,\delta_{k}^t} & \sum_{k=1}^n\theta_{k}^t y_{j}^t \\
s.t. & \quad DMU_{i}:
 \quad x_{1i}^t \geq \sum_{j=1}^n x_{1j}^t\lambda_{ji}^t,
 \quad x_{2i}^t \geq \sum_{j=1}^n x_{2j}^t\lambda_{ji}^t,
 \quad x_{3i}^t \geq \sum_{j=1}^n x_{3j}^t\lambda_{ji}^t,
 \quad x_{4i}^t \geq \sum_{j=1}^n x_{4j}^t\lambda_{ji}^t,
 \quad x_{5i}^t \geq \sum_{j=1}^n x_{5j}^t\lambda_{ji}^t \quad \forall i \in {1,2,...,n} \\
& \theta_{i}^t y_{i}^t \leq\sum^n_{j=1}y_{j}^t\lambda_{ji}^t \\
& \sum^n_{j=1}\lambda_{ji}^t=1\\
& x_{3i}^t \leq \sum_{j=1}^n x_{3j}^t\omega_{ji}^t, \\
& \delta_{i}^t u_{i}^t \geq \sum_{j=1}^n u_{j}^t\omega_{ji}^t \\
& \sum^n_{j=1}\omega_{ji}^t=1\\
& \sum^n_{j=1}x_{3j}^t\lambda_{ji}^t=\sum_{j=1}^nx_{3j}^t\omega_{ji}^t\\
& \mbox{Aggregate bad output:}\\
& \sum_{k=1}^n\delta_{k}^t u_{k}^t \leq \sum_{k=1}^n u_{k}^t \\
& \lambda_{jk}^t,\omega_{jk}^t
& \geq0 \quad j=1,2,...n;\quad k =1,2,...n
\end{aligned}
$$

- 最後，使用 CIIT(cross-industrial and intertemporal trading)，與 CIT 一樣加入一個調整係數 $\delta$，但讓不同時期(年)的焚化廠之間也可以交易碳排權，並限制交易後的總排放量不能比原本的總排放量多


$$
\begin{aligned}
Max_{\lambda_{jk}^t,\omega_{jk}^t,\theta_{k}^t,\delta_{k}^t} & \sum_{t=1}^T\sum_{k=1}^n\theta_{k}^t y_{j}^t \\
s.t. & \quad DMU_{1}:
\quad x_{11}^t \geq \sum_{j=1}^n x_{1j}^t\lambda_{j1}^t,
\quad x_{21}^t \geq \sum_{j=1}^n x_{2j}^t\lambda_{j1}^t,
\quad x_{31}^t \geq \sum_{j=1}^n x_{3j}^t\lambda_{j1}^t,
\quad x_{41}^t \geq \sum_{j=1}^n x_{4j}^t\lambda_{j1}^t,
\quad x_{51}^t \geq \sum_{j=1}^n x_{5j}^t\lambda_{j1}^t \quad t=1,2,....,T\\
& \theta_{1}^t y_{1}^t \leq\sum^n_{j=1}y_{j}^t\lambda_{j1}^t,\quad t=1,2,....,T \\
& \sum^n_{j=1}\lambda_{j1}^t=1,\quad t=1,2,....,T\\
& x_{31}^t \leq \sum_{j=1}^n x_{3j}^t\omega_{j1}^t,\quad t=1,2,....,T \\
& \delta_{1}^t u_{1}^t \geq \sum_{j=1}^n u_{j}^t\omega_{j1}^t ,\quad t=1,2,....,T\\
& \sum^n_{j=1}\omega_{j1}^t=1,\quad t=1,2,....,T\\
& \sum^n_{j=1}x_{3j}^t\lambda_{j1}^t=\sum_{j=1}^nx_{3j}^t\omega_{j1}^t,\quad t=1,2,....,T\\
& .......\\
\end{aligned}

$$
\begin{aligned}
& DMU_{n}:
\quad x_{1n}^t \geq \sum_{j=1}^n x_{1j}^t\lambda_{jn}^t,
\quad x_{2n}^t \geq \sum_{j=1}^n x_{2j}^t\lambda_{jn}^t,
\quad x_{3n}^t \geq \sum_{j=1}^n x_{3j}^t\lambda_{jn}^t,
\quad x_{4n}^t \geq \sum_{j=1}^n x_{4j}^t\lambda_{jn}^t,
\quad x_{5n}^t \geq \sum_{j=1}^n x_{5j}^t\lambda_{jn}^t, \quad t=1,2,....,T\\
& \theta_{n}^t y_{n}^t \leq\sum^n_{j=1}y_{j}^t\lambda_{jn}^t,\quad t=1,2,....,T \\
& \sum^n_{j=1}\lambda_{jn}^t=1,\quad t=1,2,....,T\\
& x_{3n}^t \leq \sum_{j=1}^n x_{3j}^t\omega_{jn}^t,\quad t=1,2,....,T \\
& \delta_{n}^t u_{n}^t \geq \sum_{j=1}^n u_{j}^t\omega_{jn}^t ,\quad t=1,2,....,T\\
& \sum^n_{j=1}\omega_{jn}^t=1,\quad t=1,2,....,T\\
& \sum^n_{j=1}x_{3j}^t\lambda_{jn}^t=\sum_{j=1}^nx_{3j}^t\omega_{jn}^t,\quad t=1,2,....,T\\
& \text{Aggregate bad output:} \\
& \sum_{t=1}^T\sum_{k=1}^n\delta_{k}^t u_{k}^t \leq \sum_{t=1}^T\sum_{k=1}^n u_{k}^t\\
& \geq0 \quad j=1,2,...n;\quad k =1,2,...n,\quad t=1,2,....,T
\end{aligned}
$$

使用上述三種模型並比較三者之間的差異來驗證交易是否可以在不增加 bad output 的情況下製造更多的 good output。

### 2. Centralized Model 
#### Lazona Model

##### Phase I

$$
\begin{aligned}
Max &\quad\frac{1}{|J|} \sum_{j} \gamma_{j} \\
s.t. &\quad \sum_{k} \lambda_{k r} X_{i k} \leqslant {x}_{i r} \quad \forall i \forall r, \\
&\sum_{k} \lambda_{k r} Y_{j k} \geqslant {y}_{jr} \quad \forall j \forall r \\
&\sum_{k} \lambda_{k r} B_{s k}={b}_{s k} \quad \forall s \forall r \\
&{x}_{i r} \leqslant X_{i r} \quad \forall i \in I, \forall r \in K, \\
&{y}_{j r} \geqslant Y_{j r} \quad \forall j \in J, \forall r \in K, \\
&\sum_{k} {y}_{j k} \geqslant \gamma_{j} \sum_{k} Y_{j k} \quad \forall j, \\
&\sum_{k} {b}_{s k} \leqslant \sum_{k} B_{s k} \quad \forall s, \\
&\lambda_{k r}, {x}_{i r}, {y}_{j r}, {b}_{s r} \geqslant 0 \quad \forall i \forall j \forall k \forall s \forall r
\end{aligned}
$$


##### Phase II

$$
\begin{aligned}
Min &\quad \frac{1}{|S|} \sum_{s} \theta_{s}\\
s.t. &\sum_{k} \lambda_{k r} X_{i k} \leqslant {x}_{ir}\quad \forall i \in I ,\forall r \in K,\\ 
&\sum_{k} \lambda_{k r} Y_{j k} \geqslant {y}_{j r} \quad \forall j\in J ,\forall r \in K \\
&\sum_{k} \lambda_{k r} B_{s k}={b}_{s k} \quad \forall s \in S,\forall r \in K \\
&{x}_{i r} \leqslant X_{i r} \quad \forall i \forall r,\\
&\sum_{k} {b}_{s k}=\theta_{s} \sum_{k} B_{s k} \quad \forall s,\\
&{y}_{j r} \geqslant Y_{j r} \quad \forall j \forall r,\\
&\sum_{k} {y}_{j k} \geqslant  \sum_{k} y_{j k}^{*} \quad \forall j,\\
&\lambda_{j r}, {x}_{i r}, {y}_{k r}, {b}_{s r} \geqslant 0 \quad \forall i \forall j \forall k \forall s \forall r,\\
\end{aligned}
$$



##### Phase III
$$
\begin{aligned}
Min &\quad \frac{1}{\left|I_{v}\right|} \cdot \sum_{i \in I_V} \sum_{r} \beta_{i r}\\
s.t. &\sum_{k} \lambda_{k r} X_{i k} \leqslant {x}_{i r} \quad \forall i  \in I, \forall r \in K,\\
&\sum_{k} \lambda_{k r} Y_{j k} \geqslant y_{k r} \quad \forall j \in J, \forall r \in K,\\
&\sum_{k} \lambda_{k r} b_{s k}=b_{s r} \quad \forall s \in S, \forall r \in K,\\
&{x}_{i r}=\beta_{i r} X_{i r} \quad \forall i, \forall r \in K,\\
&\sum_{r} {b}_{s r} \leqslant \theta_{s}^{*} \sum_{r} B_{s r} \quad \forall s \in S,\\
&y_{j r} \geqslant Y_{j r} \quad \forall j, \forall r \in K,\\
&\sum_{r} {y}_{j r} \geqslant \sum_{r} y_{j r}^{*} \quad \forall j,\\
&\lambda_{j r}, {x}_{i r}, {y}_{k r}, {b}_{s r} \geqslant 0 \quad \forall i \forall j, \forall k, \forall s, \forall r,\\
&0 \leqslant \beta_{i r} \leqslant 1 \quad \forall i \forall r
\end{aligned}
$$

#### Feng
$$
\begin{aligned}
Max & \sum_{r=1}^{K} \sum_{k=1}^{K} \lambda_{k r} Y_{k} \\
s.t. & \sum_{k=1}^{K} \lambda_{k r} X_{i k} \leq w_{r} X_{i r}, \quad r \in K, i\in I \\
&\sum_{k=1}^{K} \lambda_{k r} B_{k}=B_{k}-b_{k}, \quad r \in K\\
&\sum_{k=1}^{K} b_{k}=B\\
&\sum_{k=1}^{K} \lambda_{k r}=w_{r}\\
&b_{k} \in\left[b_{k}^{-}, b_{k}^{+}\right], \quad r \in K\\
&\lambda_{k r} \geq 0, \quad r \in K ; k \in K\\
& \epsilon \leq w_{r} \leq 1, \quad r \in K
\end{aligned}
$$

### 3. Decentralized Model

#### Price Function
$$
\text { Price }=P^{Y_{0}}-\tau \hat{Y}=P^{Y_{0}}-\tau\left(\sum y_{k}+\bar{Y}\right)
$$


#### Nash CRS Model
$$
\begin{aligned}
&\operatorname{Max}\left(P^{Y_{0}}-\tau \hat{Y}\right) y_{r}-\sum_{i \in I_{v}} P_{i}^{X} x_{i r}-\varepsilon\left(B_{r}-\Delta b_{r}\right) \\
&\text { s.t } \sum_{k \in K} \lambda_{k r} X_{i k} \leq X_{i r}, \forall i \in I_{f} ; \\
&\sum_{k \in K} \lambda_{k r} X_{i k} \leq x_{i r}, \forall i \in I_{v} ; \\
&\sum_{k \in K} \lambda_{k r} Y_{k} \geq y_{r} ; \\
&\sum_{k \in K} \lambda_{k r} B_{k}=B_{r}-\Delta b_{r} ; \\
&\sum_{r \in K} y_{r} \geq D \\
&\sum_{r \in K} \Delta b_{r}=C ; \\
&\Delta b_{r} \text { unrestricted and } \Delta b_{r} \in\left[\Delta B_{r}^{-}, \Delta B_{r}^{+}\right] ; \\
&x_{i r}, y_{r}, \lambda_{k r} \geq 0, \forall k \in K, \quad i \in I_{v}
\end{aligned}
$$

#### to compute $\varepsilon$
$$
\begin{aligned}
&0 \leq x_{i r} \perp-P_{i}^{X}+\varphi 2_{i r} \leq 0, \quad \forall i \in I_{v}, r \in K\\
&0 \leq y_{r} \perp P^{Y_{0}}-\tau \hat{Y}-\tau y_{r}-\varphi 3_{r}+\varphi 5 \leq 0, \quad \forall r \in K\\
&\varepsilon-\varphi 4_{r}+\varphi 6+\varphi 7_{r}-\varphi 8_{r}=0\left(\Delta b_{r}\right.unrestricted), \quad \forall r \in K\\
&0 \leq \lambda_{k r} \perp-\sum_{i \in I_{f}} \varphi 1_{i r} X_{i k}-\sum_{i \in I_{v}} \varphi 2_{i r} X_{i k}+\varphi 3_{r} Y_{k}-\varphi 4_{r} B_{k} \leq 0,\\
&\forall k, r \in K\\
&0 \leq \varphi 1_{i r} \perp \sum_{k \in K} \lambda_{k r} X_{i k}-X_{i r} \leq 0, \quad \forall i \in I_{f}, r \in K\\
&0 \leq \varphi 2_{i r} \perp \sum_{k \in K} \lambda_{k r} X_{i k}-x_{i r} \leq 0, \quad \forall i \in I_{v}, r \in K\\
&0 \leq \varphi 3_{r} \perp y_{r}-\sum_{k \in K} \lambda_{k r} Y_{k} \leq 0, \quad \forall r \in K\\
&\sum_{k \in K} \lambda_{k r} B_{k}-B_{r}+\Delta b_{r}=0\left(\varphi 4_{r}\right.unrestricted ), \quad \forall r \in K\\
&0 \leq \varphi 5 \perp D-\sum_{r \in K} y_{r} \leq 0\\
&C-\sum_{r \in K} \Delta b_{r}=0 ( \varphi 6 unrestricted),\\
&0 \leq \varphi 7_{r} \perp \Delta B_{r}^{-}-\Delta b_{r} \leq 0, \quad \forall r \in K\\
&0 \leq \varphi 8_{r} \perp \Delta b_{r}-\Delta B_{r}^{+} \leq 0, \quad \forall r \in K\\
\end{aligned}
$$



## 結果

### 1. trading system

我們實作後發現三個模型的 good output 是一樣多的，且無論我們如何變動原資料中的 bad output，這個數值都不會變。經過多次 debug 及討論後，我們發現了這個模型出現的問題：

- 在 NT 模型中，產生 good output 及產生 bad output 的兩個部分都會用到會產生 bad output 的 input。同時，為了讓兩個部分之間有掛勾，原作者加入了讓這些 input 的值在兩個部分中相同的限制，這使得我們同時擁有了以下三個限制式
    
    1. $$x_{3j_{0}}\geq \sum_{j=1}^nx_{3j}\lambda_{j}$$
    2. $$x_{3j_{0}} \leq \sum_{j=1}^n x_{3j}\omega_{j}$$
    3. $$\sum_{j=1}^n x_{3j}\lambda_{j}=\sum_{j=1}^n x_{3j}\omega_{j}$$
    
    這使得我們可以得出
    $$\sum_{j=1}^nx_{3j}\lambda_{j} = x_{3j_{0}} = \sum_{j=1}^n x_{3j}\omega_{j}$$
    並且替換掉原模型中的限制式，讓模型變成(以 NT 為例)：
    $$
        Y^{NTt}_{j_{0}}= \max_{\lambda_{j},\omega_{j},\theta}\theta y_{j_0} \\
        \mbox{s.t.}
        \quad x_{1j_{0}}\geq \sum_{j=1}^nx_{1j}\lambda_{j} ,
        \quad x_{2j_{0}}\geq \sum_{j=1}^nx_{2j}\lambda_{j} ,
        \quad x_{3j_{0}} = \sum_{j=1}^nx_{3j}\lambda_{j},
        \quad x_{4j_{0}}\geq \sum_{j=1}^nx_{4j}\lambda_{j},
        \quad x_{5j_{0}}\geq \sum_{j=1}^nx_{5j}\lambda_{j}\\
        \theta y_{j_{0}} \leq \sum^n_{j=1}y_{j}\lambda_{j} \\
        \sum^n_{j=1}\lambda_{j}=1\\
        x_{3j_{0}} = \sum_{j=1}^n x_{3j}\omega_{j}, \\
        u_{j_{0}} \geq \sum_{j=1}^n u_{j}\omega_{j} \\
        \sum_{j=1}^n\omega_{j}=1 \\
        \lambda_{j},\omega_{j}\geq 0,\quad j=1,2,...,n
    $$
    在這個情況下，我們就可以很清楚地發現，這個模型的兩個部分已經完全分離了。而在目標式的作用下，我們會先找到一組最好的 $\lambda$ 使目標函數最大化。by-production 的部分只要找到一組符合限制式的 $w$ 即可，完全不會影響到最後的結果，也解釋了為甚麼我們大幅度的變動資料中的 bad output 後仍得出一樣的結果。
    
- 在 CIT 模型中 ( CIIT 同理 )，加入了 $\delta$ 作為交易的係數，但實際上這並不會影響目標式，且 by-production 完全不會影響到 good output 的產出，也因此沒有交易所帶來的額外 good output，也解釋了為甚麼三個模型的產出會一樣。


### 2. decentralized and centralized model

#### <center>排碳分配量</center>
![](https://i.imgur.com/YrscgZS.png)

* Feng 下的排碳量全部都是正的，代表所有的 DMUs 都需要減少排碳量來達到 maximal output level
* Nash CRS Model 有較大的排碳量標準差

#### <center>Efficiency</center>
![](https://i.imgur.com/wZSPJiw.png)
![](https://i.imgur.com/7Mqv05I.png=50)




#### <center>FFurther analysis on Nash CRS AEP model </center> 

* Take a deeper look into the new inputs, outputs, and bad outputs after Nash CRS AEP.
* We get to compare the policies for DMUs with different environmental efficiency.


![](https://i.imgur.com/nx61niC.png)
<center>這些 DMU 本來就已經達到最佳獲利，故不用調整生產狀況</center>

![](https://i.imgur.com/2bvdv3V.png)
<center>分配到負的碳減量，而且可以多排碳，意味這些焚化廠較環保，所以三種變動投入都要增加，可以多燃燒碳，增加產出。</center>

![](https://i.imgur.com/FMHxogC.png)
<center>分配到負的碳減量，代表可以多排碳，而且要增加垃圾焚化量，可能因為這些焚化廠較環保，所以把一些其他焚化廠的垃圾拿來這邊燒。而發電量些微減少，是因為整體發電量只需滿足最低需求。</center>

![](https://i.imgur.com/TizZeoo.png)
<center>分配到較多減碳量的焚化廠，因為較不環保，所以要減少發電量，同時減少垃圾焚化量或其他投入。</center>

#### <center> Conclusion </center>
* 在不同的市場模型假設之下，碳權的分配可以完全不同
* Nash CRS model 分配給不同焚化廠的碳權差異較大，它讓較乾淨的焚化廠排更多的碳.
* Decentralized 的模型整體上有較低的平均效率值（efficiency score)。可能原因為，我們在使用 Färe's model 計算效率值時，我們用的是原始的 inputs 和 outputs，而不是新的 bad ouputs. ( Nash CRS model 分配的碳權有較大標準差 )


