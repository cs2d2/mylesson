---
date: 2026-11-05
created: 2026-11-05
---

# 可控标准型的变换矩阵：$q_1$ 法与 $Q_cW$ 法

创建日期：2026-11-05

## 0. 一句话概括

系统能控 ⇒ 能控性矩阵 $Q_c$ 可逆 ⇒ 取 **$q_1=e_n^{\mathrm T}Q_c^{-1}$（即 $Q_c^{-1}$ 的最后一行）**，再用 $q_1,q_1A,\dots,q_1A^{n-1}$ 拼出 $P$，就得到能控标准型；这件事等价于**用 Toeplitz 矩阵把 $Q_c$ 修正成 $P^{-1}$**：$P^{-1}=Q_cW$。两种写法算出来的 $P$ 是同一个。

## 1. 前提：能控性 $\iff$ $Q_c$ 可逆

考虑单输入系统

$$\dot x=Ax+bu,\qquad y=c^{\mathrm T}x$$

（$A\in\mathbb R^{n\times n}$，$b\in\mathbb R^n$。）能控性矩阵

$$Q_c=\begin{bmatrix}b&Ab&A^2b&\cdots&A^{n-1}b\end{bmatrix}\in\mathbb R^{n\times n}$$

**系统完全能控 $\iff \operatorname{rank}Q_c=n \iff Q_c^{-1}$ 存在**。

后面所有的构造都建立在"$Q_c$ 可逆"之上——这就是为什么这一页只讨论**能控**标准型。

特征多项式记作

$$\Delta(s)=\det(sI-A)=s^{n}+a_{n-1}s^{n-1}+\cdots+a_1s+a_0$$

> 注意系数的编号：**常数项是 $a_0$，最高次（$s^{n-1}$）的系数是 $a_{n-1}$**。后面矩阵里出现的顺序会反过来，这一点最容易搞混。

## 2. 目标：能控标准型长什么样

我们要找一个可逆矩阵 $P$，做状态变换

$$\boxed{\;x=P^{-1}\bar x\quad\Longleftrightarrow\quad \bar x=Px\;}$$

也就是说：**$P$ 负责把原坐标送到新坐标（$\bar x=Px$），$P^{-1}$ 负责把新坐标送回原坐标**。

使得新坐标下的系统变成

$$\dot{\bar x}=\bar A\bar x+\bar bu,\qquad y=\bar c^{\mathrm T}\bar x$$

$$\bar A=\begin{bmatrix}0&1&0&\cdots&0\\ \vdots&&\ddots&\ddots&\vdots\\ 0&&&0&1\\ -a_0&-a_1&\cdots&-a_{n-2}&-a_{n-1}\end{bmatrix},\qquad \bar b=\begin{bmatrix}0\\ \vdots\\ 0\\ 1\end{bmatrix}=e_n$$

其中 $\bar b=e_n$（$e_n$ 是第 $n$ 个单位向量）是能控标准型最醒目的标志：**输入只进入最后一个状态**。

把 $x=P^{-1}\bar x$ 代入 $\dot x=Ax+bu$、$y=c^{\mathrm T}x$，就得到相似变换的公式

$$\bar A=PAP^{-1},\qquad \bar b=Pb,\qquad \bar c^{\mathrm T}=c^{\mathrm T}P^{-1}$$

（推导：$P^{-1}\dot{\bar x}=AP^{-1}\bar x+bu$，两边左乘 $P$ 得 $\dot{\bar x}=PAP^{-1}\bar x+Pbu$。）

> **记号提醒（很重要）**：本页统一用 $x=P^{-1}\bar x$（即 $\bar x=Px$）。不少教材反过来写成 $\bar x=P^{-1}x$（即 $x=P\bar x$）——那里的 $P$ 正好等于本页的 $P^{-1}$，$\bar A$ 也会写成 $P^{-1}AP$。看别人的推导时，先确认方向再套公式。

## 3. 第一种构造：$q_1$ 法

### 3.1 取 $q_1$：$Q_c^{-1}$ 的最后一行

$$\boxed{\;q_1=e_n^{\mathrm T}Q_c^{-1}=\begin{bmatrix}0&0&\cdots&0&1\end{bmatrix}Q_c^{-1}\;}$$

也就是说，**$q_1$ 就是 $Q_c^{-1}$ 的最后一行**（是一个 $1\times n$ 行向量）。有的书写成 $P_1$，与本页的 $q_1$ 是同一个东西。

### 3.2 用它拼出 $P$

把 $q_1$ 不断右乘 $A$，按行堆成一个 $n\times n$ 矩阵：

$$\boxed{\;P=\begin{bmatrix}q_1\\ q_1A\\ q_1A^2\\ \vdots\\ q_1A^{n-1}\end{bmatrix}\;}$$

于是 $\bar x=Px$，回到原坐标用 $P^{-1}$，变换就是 $x=P^{-1}\bar x$。

### 3.3 为什么它正好给出能控标准型

**关键引理**：因为 $Q_c^{-1}Q_c=I$，所以

$$Q_c^{-1}A^{k}b=e_{k+1}\qquad(k=0,1,\dots,n-1)$$

（$Q_c$ 的第 $k+1$ 列就是 $A^kb$，乘上 $Q_c^{-1}$ 正好挑出第 $k+1$ 个单位向量。）

**验证 $\bar b$**：

$$\bar b=Pb=\begin{bmatrix}q_1b\\ q_1Ab\\ \vdots\\ q_1A^{n-1}b\end{bmatrix}=\begin{bmatrix}e_n^{\mathrm T}e_1\\ e_n^{\mathrm T}e_2\\ \vdots\\ e_n^{\mathrm T}e_n\end{bmatrix}=\begin{bmatrix}0\\0\\ \vdots\\ 1\end{bmatrix}=e_n\;\checkmark$$

**验证 $\bar A$ 的前 $n-1$ 行**：$\bar A=PAP^{-1}$ 的第 $i$ 行 $=(q_1A^{i-1})AP^{-1}=q_1A^{i}P^{-1}$；而 $q_1A^{i}$ 正是 $P$ 的第 $i+1$ 行，乘上 $P^{-1}$ 得 $e_{i+1}^{\mathrm T}$——所以 $\bar A$ 的**主对角线上方**（超对角）全是 1：

$$\bar A=PAP^{-1}=\begin{bmatrix}0&1&&\\&\ddots&\ddots&\\&&0&1\\ *&*&\cdots&*\end{bmatrix}$$

**验证最后一行**：最后一行是 $(q_1A^{n-1})AP^{-1}=q_1A^{n}P^{-1}$。由 Cayley–Hamilton 定理 $A^n+a_{n-1}A^{n-1}+\cdots+a_0I=0$，两边左乘 $q_1$：

$$q_1A^n=-a_{n-1}q_1A^{n-1}-\cdots-a_1q_1A-a_0q_1$$

再右乘 $P^{-1}$；因为 $q_1A^{k}P^{-1}=e_{k+1}^{\mathrm T}$，就得到 $-a_0,-a_1,\dots,-a_{n-1}$ 那一行 ✓

## 4. 第二种构造：$T_{c1}=Q_cW$（Toeplitz 法）

### 4.1 $W$ 长什么样

取一个**反对角 Toeplitz 矩阵** $W$，它的元素由一条简单规则给出：

$$W_{ij}=a_{i+j-1}\qquad\big(\text{约定 }a_n:=1,\ \ a_k:=0\ \ (k>n)\big)$$

也就是说：**主反对角线（$i+j=n+1$）上全是 1；从这条反对角线往左上走，依次是 $a_{n-1},a_{n-2},\dots,a_1$，左上角就是 $a_1$。**

$n=2$、$n=3$ 时看得最清楚（注意 $a_1$ 在左上角，不是 $a_{n-1}$）：

$$W_{2}=\begin{bmatrix}a_1&1\\ 1&0\end{bmatrix},\qquad W_{3}=\begin{bmatrix}a_1&a_2&1\\ a_2&1&0\\ 1&0&0\end{bmatrix}$$

然后令

$$\boxed{\;T_{c1}=Q_c\,W\;}$$

同样令 $x=T_{c1}\bar x$（即 $\bar x=Px$），得到的就是同一个能控标准型——因为 $T_{c1}$ 正是本页的 $P^{-1}$。

**为什么"乘一个 $W$ 就够了"**：如果不乘 $W$，直接用 $Q_c^{-1}$ 去变换，得到的是"次对角为 1、系数挤在最后一列"的伴随形式；乘上 $W$ 相当于把坐标轴重新组合一次（$W$ 是单位反对角阵的推广），正好把系数从最后一列"挪"到最后一行 ✓

### 4.2 两种构造给出同一个 $P^{-1}$

可以证明（下面例子会验证）

$$\boxed{\;P^{-1}=Q_c\,W=\Big(\begin{bmatrix}q_1\\ q_1A\\ \vdots\\ q_1A^{n-1}\end{bmatrix}\Big)^{-1}\;}$$

因此：

- **$q_1$ 法**是"先求 $Q_c$ 的逆、再按行堆出 $P$"，逻辑直观、便于手算；
- **$Q_cW$ 法**是"只做矩阵乘法"，不需要求逆，更适合编程与符号计算。

两者殊途同归。

## 5. 一个完整的三阶例子

取一个**本来不是**标准型的系统（三个特征值是 $-1,-2,-3$）：

$$A=\begin{bmatrix}0&1&0\\-6&-5&0\\-6&-5&-1\end{bmatrix},\qquad b=\begin{bmatrix}0\\1\\1\end{bmatrix},\qquad c^{\mathrm T}=\begin{bmatrix}1&1&1\end{bmatrix}$$

### 5.1 特征多项式与系数

$$\Delta(s)=\det(sI-A)=s^3+6s^2+11s+6=(s+1)(s+2)(s+3)$$

对照 $\Delta(s)=s^3+a_2s^2+a_1s+a_0$，得

$$a_2=6,\qquad a_1=11,\qquad a_0=6$$

### 5.2 能控性矩阵与它的逆

$$Ab=\begin{bmatrix}1\\-5\\-6\end{bmatrix},\qquad A^2b=\begin{bmatrix}-5\\19\\25\end{bmatrix}$$

$$Q_c=\begin{bmatrix}b&Ab&A^2b\end{bmatrix}=\begin{bmatrix}0&1&-5\\1&-5&19\\1&-6&25\end{bmatrix},\qquad \det Q_c=-1\neq0\;\checkmark$$

$$Q_c^{-1}=\begin{bmatrix}11&-5&6\\6&-5&5\\1&-1&1\end{bmatrix}$$

### 5.3 用 $q_1$ 法

取 $Q_c^{-1}$ 的最后一行：

$$q_1=\begin{bmatrix}0&0&1\end{bmatrix}Q_c^{-1}=\begin{bmatrix}1&-1&1\end{bmatrix}$$

$$q_1A=\begin{bmatrix}0&1&-1\end{bmatrix},\qquad q_1A^2=\begin{bmatrix}0&0&1\end{bmatrix}$$

$$P=\begin{bmatrix}q_1\\ q_1A\\ q_1A^2\end{bmatrix}=\begin{bmatrix}1&-1&1\\0&1&-1\\0&0&1\end{bmatrix}\qquad\Longrightarrow\qquad P^{-1}=\begin{bmatrix}1&1&0\\0&1&1\\0&0&1\end{bmatrix}$$

### 5.4 用 $Q_cW$ 法核对

$$W_3=\begin{bmatrix}a_1&a_2&1\\ a_2&1&0\\ 1&0&0\end{bmatrix}=\begin{bmatrix}11&6&1\\6&1&0\\1&0&0\end{bmatrix}$$

$$T_{c1}=Q_cW_3=\begin{bmatrix}0&1&-5\\1&-5&19\\1&-6&25\end{bmatrix}\begin{bmatrix}11&6&1\\6&1&0\\1&0&0\end{bmatrix}=\begin{bmatrix}1&1&0\\0&1&1\\0&0&1\end{bmatrix}=P^{-1}\;\checkmark$$

**和 $P^{-1}$ 完全一样** ✓ 这就验证了 4.2 节的结论。

### 5.5 变换后的三个矩阵

$$\bar b=Pb=\begin{bmatrix}0\\0\\1\end{bmatrix}=e_3\;\checkmark$$

$$\bar A=PAP^{-1}=\begin{bmatrix}0&1&0\\0&0&1\\-6&-11&-6\end{bmatrix}\;\checkmark$$

（对照 $\begin{bmatrix}0&1&0\\0&0&1\\-a_0&-a_1&-a_2\end{bmatrix}=\begin{bmatrix}0&1&0\\0&0&1\\-6&-11&-6\end{bmatrix}$ ✓）

$$\bar c^{\mathrm T}=c^{\mathrm T}P^{-1}=\begin{bmatrix}1&1&1\end{bmatrix}\begin{bmatrix}1&1&0\\0&1&1\\0&0&1\end{bmatrix}=\begin{bmatrix}1&2&2\end{bmatrix}$$

### 5.6 三个检查

- **特征多项式不变**：$\det(sI-\bar A)=s^3+6s^2+11s+6$，与原系统一致 ✓（相似变换不改变特征值）；
- **输入只进最后一个状态**：$\bar b=e_3$ ✓；
- **传递函数不变**：记 $\bar c^{\mathrm T}=[\bar c_0,\bar c_1,\bar c_2]=[1,2,2]$，对能控标准型有
  $$G(s)=\frac{\bar c_2s^2+\bar c_1s+\bar c_0}{\Delta(s)}=\frac{2s^2+2s+1}{s^3+6s^2+11s+6}$$
  与原系统直接算出的结果一致 ✓（这正是 7.1 节"相似变换不改变传递函数"的实例）

## 6. 三点注意事项

| 注意点 | 说明 |
| --- | --- |
| **前提是能控** | $Q_c$ 不可逆时 $q_1$ 根本取不出来；这时系统不能化成能控标准型（能控标准型的维数只等于能控子空间的维数） |
| **系数顺序别搞反** | 特征多项式写作 $s^n+a_{n-1}s^{n-1}+\cdots+a_1s+a_0$，而矩阵最后一行是从 $-a_0$ 排到 $-a_{n-1}$——**首尾是反的** |
| **$P$ 与 $P^{-1}$ 的方向** | 本页统一是 $x=P^{-1}\bar x$、$\bar x=Px$；不少教材反过来写 $\bar x=P^{-1}x$，那里的 $P$ 等于本页的 $P^{-1}$ |

> 一句话：**能控 ⇒ $Q_c$ 可逆 ⇒ $q_1$ 取 $Q_c^{-1}$ 的最后一行、按 $q_1A^k$ 堆出 $P$，或者直接用 $P^{-1}=Q_cW$（$W$ 是主反对角线全为 1、往左上依次是 $a_{n-1},\dots,a_1$ 的 Toeplitz 矩阵）；两条路得到的是同一个变换矩阵，结果都是"超对角为 1、最后一行放系数、输入只进最后一个状态"的能控标准型。**

## 7. 传递函数的相似不变性，与能控性结构分解

### 7.1 换了坐标，传递函数不变

做相似变换后

$$\bar A=PAP^{-1},\qquad \bar b=Pb,\qquad \bar c^{\mathrm T}=c^{\mathrm T}P^{-1}$$

要证明

$$G(s)=c^{\mathrm T}(sI-A)^{-1}b=\bar c^{\mathrm T}(sI-\bar A)^{-1}\bar b$$

**关键恒等式**：对可逆的 $P$，

$$(PMP^{-1})^{-1}=PM^{-1}P^{-1}$$

（两边相乘就回到单位阵：$PMP^{-1}\cdot PM^{-1}P^{-1}=PMM^{-1}P^{-1}=I$。）把它用到 $M=sI-A$：

$$(sI-\bar A)^{-1}=\big(sI-PAP^{-1}\big)^{-1}=\big(P(sI-A)P^{-1}\big)^{-1}=P(sI-A)^{-1}P^{-1}$$

于是

$$\bar c^{\mathrm T}(sI-\bar A)^{-1}\bar b=c^{\mathrm T}P^{-1}\cdot P(sI-A)^{-1}P^{-1}\cdot Pb=c^{\mathrm T}(sI-A)^{-1}b=G(s)\quad\checkmark$$

**一句话**：中间那两个 $P$ 和 $P^{-1}$ 正好首尾相消。所以"传递函数"刻画的是**输入输出关系**，它不随状态坐标的选取而改变——这正是它能作为系统"外部描述"的原因。

### 7.2 能控性结构分解：块三角形式

如果系统**不是**完全能控（$\operatorname{rank}Q_c=r<n$），我们依然可以做一个变换，把状态分成"能控的 $r$ 维"和"不能控的 $n-r$ 维"两块，得到

$$\boxed{\;\bar A=\begin{bmatrix}\bar A_{11}&\bar A_{12}\\ \mathbf{0}&\bar A_{22}\end{bmatrix},\qquad \bar b=\begin{bmatrix}\bar b_1\\ \mathbf{0}\end{bmatrix},\qquad \bar c^{\mathrm T}=\begin{bmatrix}\bar c_1^{\mathrm T}&\bar c_2^{\mathrm T}\end{bmatrix}\;}$$

（多输入多输出时把 $\bar b$、$\bar c^{\mathrm T}$ 换成矩阵 $\bar B$、$\bar C$，块结构完全一样。）

**怎么构造**：从 $Q_c$ 里挑出 $r$ 个线性无关的列（例如 $b,Ab,\dots,A^{r-1}b$ 若已独立），再补上 $n-r$ 个向量凑成一组基。注意按本页的记号，**这组基拼成的是 $P^{-1}$**（它的每一列就是一个基向量）：

$$P^{-1}=\begin{bmatrix}b&Ab&\cdots&A^{r-1}b&v_{r+1}&\cdots&v_n\end{bmatrix}$$

**为什么左下角是零块**：

- $\bar b=Pb=e_1$，所以 $\bar b$ 的下半部分全是 0（输入进不了不能控的部分）；
- $AP^{-1}$ 的前 $r$ 列是 $Ab,\dots,A^rb,Av_{r+1},\dots$，而 $A^kb$（$k\le r$）都在能控子空间里，可以写成 $P^{-1}$ 前 $r$ 列的线性组合，因此**这 $r$ 列在"下半部分"的分量全为 0** —— 这就是 $\bar A$ 左下角的 $\mathbf 0$。

物理含义：**不能控的那部分状态，既不受 $u$ 驱动，也不会通过 $u$ 影响别的状态**，它只是自己按 $\dot{\bar x}_2=\bar A_{22}\bar x_2$ 自由演化。

### 7.3 为什么不可控部分在传递函数里"消失"

因为 $\bar A$ 是**块上三角**的，所以 $sI-\bar A$ 也是块上三角：

$$sI-\bar A=\begin{bmatrix}sI-\bar A_{11}&-\bar A_{12}\\ \mathbf 0&sI-\bar A_{22}\end{bmatrix}$$

块上三角矩阵的逆仍然是块上三角：

$$(sI-\bar A)^{-1}=\begin{bmatrix}(sI-\bar A_{11})^{-1}&*\\ \mathbf 0&(sI-\bar A_{22})^{-1}\end{bmatrix}$$

再乘上 $\bar b=\begin{bmatrix}\bar b_1\\ \mathbf 0\end{bmatrix}$，最后那一列 $\mathbf 0$ 会把右上角那块"吃"掉：

$$\bar c^{\mathrm T}(sI-\bar A)^{-1}\bar b=\begin{bmatrix}\bar c_1^{\mathrm T}&\bar c_2^{\mathrm T}\end{bmatrix}\begin{bmatrix}(sI-\bar A_{11})^{-1}&*\\ \mathbf 0&(sI-\bar A_{22})^{-1}\end{bmatrix}\begin{bmatrix}\bar b_1\\ \mathbf 0\end{bmatrix}=\bar c_1^{\mathrm T}(sI-\bar A_{11})^{-1}\bar b_1$$

$$\boxed{\;G(s)=\bar c_1^{\mathrm T}\big(sI-\bar A_{11}\big)^{-1}\bar b_1\;}$$

**结论**：传递函数里**只有能控子系统 $(\bar A_{11},\bar b_1,\bar c_1)$**，不可控部分 $\bar A_{22}$ 连影子都没有。

从零极点的角度看：$\bar A_{22}$ 的特征值原本是系统的极点，但它们在 $\bar c^{\mathrm T}(sI-\bar A)^{-1}\bar b$ 的计算中被**零极点相消**掉了——这就是"**传递函数看不见内部不可控/不可观的状态**"这句话的代数根据。

### 7.4 一个例子：3 个状态，2 阶传递函数

$$A=\begin{bmatrix}0&1&0\\0&0&0\\0&0&2\end{bmatrix},\qquad b=\begin{bmatrix}0\\1\\0\end{bmatrix},\qquad c^{\mathrm T}=\begin{bmatrix}1&0&0\end{bmatrix}$$

**能控性检查**：

$$Ab=\begin{bmatrix}1\\0\\0\end{bmatrix},\qquad A^2b=\begin{bmatrix}0\\0\\0\end{bmatrix}\qquad\Longrightarrow\qquad Q_c=\begin{bmatrix}0&1&0\\1&0&0\\0&0&0\end{bmatrix},\quad \operatorname{rank}Q_c=2<n=3$$

所以 $r=2$，有一个不可控状态（对应 $A$ 的第三个分量，它既不被 $b$ 驱动，也不出现在 $c^{\mathrm T}$ 里）。

系统其实已经是块三角形式了：

$$\bar A_{11}=\begin{bmatrix}0&1\\0&0\end{bmatrix},\quad \bar A_{22}=[2],\quad \bar b_1=\begin{bmatrix}0\\1\end{bmatrix},\quad \bar b_2=0,\quad \bar c_1^{\mathrm T}=\begin{bmatrix}1&0\end{bmatrix},\quad \bar c_2^{\mathrm T}=0$$

**算原系统的传递函数**：

$$(sI-A)^{-1}=\begin{bmatrix}\dfrac1s&\dfrac{1}{s^2}&0\\[4pt] 0&\dfrac1s&0\\[4pt] 0&0&\dfrac{1}{s-2}\end{bmatrix}\qquad\Longrightarrow\qquad G(s)=\begin{bmatrix}1&0&0\end{bmatrix}(sI-A)^{-1}\begin{bmatrix}0\\1\\0\end{bmatrix}=\frac{1}{s^2}$$

**算能控子系统的传递函数**：

$$(sI-\bar A_{11})^{-1}=\begin{bmatrix}\dfrac1s&\dfrac{1}{s^2}\\[4pt] 0&\dfrac1s\end{bmatrix}\qquad\Longrightarrow\qquad \bar c_1^{\mathrm T}(sI-\bar A_{11})^{-1}\bar b_1=\begin{bmatrix}\dfrac1s&\dfrac1{s^2}\end{bmatrix}\begin{bmatrix}0\\1\end{bmatrix}=\frac{1}{s^2}$$

两者**完全一致** ✓，而 $\bar A_{22}=[2]$ 给出的极点 $s=2$ 在传递函数里**彻底消失了**——系统有 3 个状态，传递函数却只有 2 阶。

### 7.5 这件事的意义：最小实现

| 概念 | 含义 |
| --- | --- |
| 传递函数的阶数 | 只等于**既能控又能观**的那部分子系统的维数 |
| 不可控部分 | 不影响 $G(s)$，它的极点被相消 |
| 不可观部分 | 同理，也不影响 $G(s)$ |
| **最小实现** | 维数等于传递函数阶数、既完全能控又完全能观的实现 |

所以：

- 传递函数只是**外部描述**，同一个 $G(s)$ 可以对应无穷多个状态空间实现（维数可以更大，但多出来的都是不可控/不可观的部分）；
- 反过来，如果系统本来就完全能控，那么 $\bar A_{22}$ 这一块根本不存在（$r=n$），此时能控标准型的阶数就等于传递函数的阶数；
- "**状态空间模型保留内部信息，传递函数只保留输入输出信息**"这个区别，在 7.3 节的块矩阵里看得最清楚。

> 一句话：**相似变换不改变传递函数（$P$ 与 $P^{-1}$ 首尾相消）；若系统不完全能控，可以化成 $\bar A=\begin{bmatrix}\bar A_{11}&\bar A_{12}\\0&\bar A_{22}\end{bmatrix}$、$\bar b=\begin{bmatrix}\bar b_1\\0\end{bmatrix}$ 的块三角形式，此时 $G(s)=\bar c_1^{\mathrm T}(sI-\bar A_{11})^{-1}\bar b_1$ —— 不可控那块的极点被零极点相消，从输入输出上彻底看不见。**
