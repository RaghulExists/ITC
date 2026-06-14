# Module 3 — Information Channels · Question Bank Solutions

> Complete, step-by-step solutions to all 20 questions (Unit 1, 2 & 3), faithful to the class notes (ITC, Module 3 — Dr. Madhavi M, ECE, GAT).
> Each question includes a worked solution, a short **Steps explained** note, and **LaTeX (TikZ)** code for every diagram.
> A full **formula sheet** is given at the end.

> **Handy constants used throughout:**
> $\log_2 x = 3.3219\,\log_{10} x$.  Quick values: $\log_2 2=1$, $\log_2 3=1.585$, $\log_2 4=2$, $\log_2 5=2.322$, $\log_2 \tfrac43=0.415$, $\log_2 \tfrac83=1.415$, $\log_2 10=3.322$.
> **Decibel → ratio:** $\dfrac{S}{N}\Big|_{\text{ratio}} = 10^{(\text{dB}/10)}$ → 10 dB = 10, 20 dB = 100, 30 dB = 1000.

---

## Table of Contents

**Unit 1 — Entropies & Mutual Information**
- [Q1 — Types of special channels (theory)](#q1--types-of-special-channels-theory)
- [Q2 — Mutual information for continuous channel (theory)](#q2--mutual-information-for-continuous-channel-theory)
- [Q3 — Entropies of a 5×4 JPM](#q3--entropies-of-a-54-jpm)
- [Q4 — All entropies of a 4×4 JPM](#q4--all-entropies-of-a-44-jpm)
- [Q5 — Mutual information of the 4×4 JPM](#q5--mutual-information-of-the-44-jpm)
- [Q6 — Rate of transmission through a channel](#q6--rate-of-transmission-through-a-channel)

**Unit 2 — Channel Capacity (BSC, BEC, Muroga)**
- [Q7 — BSC diagram & capacity](#q7--bsc-diagram--capacity)
- [Q8 — BEC diagram & capacity](#q8--bec-diagram--capacity)
- [Q9 — Capacity of a DMC (4×4)](#q9--capacity-of-a-dmc-44)
- [Q10 — Non-symmetric binary channel](#q10--non-symmetric-binary-channel)
- [Q11 — Muroga's method (theory)](#q11--murogas-method-theory)
- [Q12 — Muroga's method (3×3)](#q12--murogas-method-33)
- [Q13 — BSC capacity by Muroga + verify](#q13--bsc-capacity-by-muroga--verify)

**Unit 3 — Shannon-Hartley (Continuous Channels)**
- [Q14 — Telephone channel capacity](#q14--telephone-channel-capacity)
- [Q15 — TV picture: minimum bandwidth](#q15--tv-picture-minimum-bandwidth)
- [Q16 — Alphanumeric data channel](#q16--alphanumeric-data-channel)
- [Q17 — Analog signal information rate](#q17--analog-signal-information-rate)
- [Q18 — Shannon-Hartley law & C∞ proof](#q18--shannon-hartley-law--c-proof)
- [Q19 — AWGN channel capacity](#q19--awgn-channel-capacity)
- [Q20 — CRT terminal channel](#q20--crt-terminal-channel)

- [📋 Master Formula Sheet](#-master-formula-sheet)
- [Quick Answer Table](#quick-answer-table)

---

# UNIT 1

## Q1 — Types of special channels (theory)

The important special discrete channels are:

1. **Lossless channel** — each output uniquely identifies the input; $H(X/Y)=0$, so $I(X;Y)=H(X)$ and capacity $C=\log_2 n$ (n = no. of inputs). No information is lost.

2. **Deterministic channel** — each input produces exactly one output (each row of the channel matrix has a single `1`); $H(Y/X)=0$, so $C=\log_2 m$.

3. **Noiseless channel** — both lossless and deterministic; the channel matrix is an identity-like square matrix. $H(X/Y)=H(Y/X)=0$, $C=\log_2 n$.

4. **Symmetric (uniform) channel** — every row of the channel matrix contains the same set of entries (in some order), and so does every column. Then $H(Y/X)=h$ is constant, and $C=\log_2 m - h$.

5. **Binary Symmetric Channel (BSC)** — a symmetric channel with 2 inputs and 2 outputs; its channel matrix has rows $(p,\ q)$ and $(q,\ p)$. Capacity $C = 1-h$, where $h = p\log_2\tfrac1p + q\log_2\tfrac1q$.

6. **Binary Erasure Channel (BEC)** — a symbol is either received correctly or "erased" (output `Y`); no wrong decisions are made. Capacity $C = (1-P)=\bar P$.

7. **Cascaded channel** — two channels in series; overall matrix = product of the individual channel matrices. Capacity of the cascade ≤ capacity of each stage.

> **Steps explained:** Classify by what the channel matrix looks like — single 1 per column (lossless), single 1 per row (deterministic), identity (noiseless), repeated-row structure (symmetric/BSC), erasure column (BEC). Each special structure gives a simple closed form for $C$.

---

## Q2 — Mutual information for continuous channel (theory)

For a **continuous channel**, mutual information uses **differential entropy** $h(\cdot)$ in place of discrete entropy:

$$I(X;Y) = h(X) - h(X|Y) = h(Y) - h(Y|X)$$

where

$$h(X) = -\int_{-\infty}^{\infty} f_X(x)\log_2 f_X(x)\,dx, \qquad h(X|Y) = -\iint f_{XY}(x,y)\log_2 f_{X|Y}(x|y)\,dx\,dy$$

and $f(\cdot)$ are probability **density** functions.

**Properties (same as the discrete case):**

1. **Symmetry:** $I(X;Y) = I(Y;X)$
2. **Non-negativity:** $I(X;Y) \ge 0$
3. **Output form:** $I(X;Y) = h(Y) - h(Y|X)$
4. **Bound:** $I(X;Y) \le \min\{h(X),\,h(Y)\}$ *(interpretation only — differential entropies can be negative)*
5. **Joint form:** $I(X;Y) = h(X) + h(Y) - h(X,Y)$

> **Steps explained:** Replace probabilities by densities and sums by integrals — the *definitions* of entropy become differential entropies, but every algebraic property of $I(X;Y)$ carries over unchanged.

---

## Q3 — Entropies of a 5×4 JPM

Given joint probability matrix:

$$
P(X,Y)=
\begin{bmatrix}
0.25 & 0 & 0 & 0 \\
0.10 & 0.30 & 0 & 0 \\
0 & 0.05 & 0.10 & 0 \\
0 & 0 & 0.05 & 0.10 \\
0 & 0 & 0.05 & 0
\end{bmatrix}
$$

**Step 1 — Marginals.** Row sums → $P(X)$, column sums → $P(Y)$:

$$P(X) = [\,0.25,\ 0.40,\ 0.15,\ 0.15,\ 0.05\,] \qquad P(Y) = [\,0.35,\ 0.35,\ 0.20,\ 0.10\,]$$

**Step 2 — Input entropy $H(X)$:**

$$H(X) = -\big[0.25\log_2 0.25 + 0.40\log_2 0.40 + 2(0.15\log_2 0.15) + 0.05\log_2 0.05\big]$$
$$= 0.5 + 0.5288 + 0.8211 + 0.2161 = \mathbf{2.066\ bits/symbol}$$

**Step 3 — Output entropy $H(Y)$:**

$$H(Y) = -\big[2(0.35\log_2 0.35) + 0.20\log_2 0.20 + 0.10\log_2 0.10\big] = 1.0602 + 0.4644 + 0.3322 = \mathbf{1.857\ bits/symbol}$$

**Step 4 — Joint entropy $H(X,Y)$** (non-zero cells: one 0.25, one 0.30, three 0.10, three 0.05):

$$H(X,Y) = -\big[0.25\log_2 0.25 + 0.30\log_2 0.30 + 3(0.10\log_2 0.10) + 3(0.05\log_2 0.05)\big]$$
$$= 0.5 + 0.5211 + 0.9966 + 0.6483 = \mathbf{2.666\ bits/symbol}$$

**Step 5 — Conditional entropies:**

$$H(X/Y) = H(X,Y) - H(Y) = 2.666 - 1.857 = \mathbf{0.809\ bits/symbol}$$
$$H(Y/X) = H(X,Y) - H(X) = 2.666 - 2.066 = \mathbf{0.600\ bits/symbol}$$

**Mutual information:** $I(X;Y) = H(X) - H(X/Y) = 2.066 - 0.809 = \mathbf{1.257\ bits/symbol}$

> **Steps explained:** Row-sum and column-sum the JPM to get the marginals, then apply $H=-\sum P\log_2 P$ to each of $P(X)$, $P(Y)$, and the joint cells. The two conditional entropies come instantly from $H(X,Y)-H(Y)$ and $H(X,Y)-H(X)$ — no need to build the conditional matrices.

---

## Q4 — All entropies of a 4×4 JPM

*(Same as Problem 2 in the notes — identical matrix.)*

$$
P(X,Y)=
\begin{bmatrix}
0.05 & 0 & 0.20 & 0.05 \\
0 & 0.10 & 0.10 & 0 \\
0 & 0 & 0.20 & 0.10 \\
0.05 & 0.05 & 0 & 0.10
\end{bmatrix}
$$

**Marginals:** $P(X) = [0.30,\ 0.20,\ 0.30,\ 0.20]$, $P(Y) = [0.10,\ 0.15,\ 0.50,\ 0.25]$.

**$H(X)$:**
$$H(X) = 2(0.3\log_2\tfrac{1}{0.3}) + 2(0.2\log_2\tfrac{1}{0.2}) = \mathbf{1.9709\ bits/symbol}$$

**$H(Y)$:**
$$H(Y) = 0.1\log_2 10 + 0.15\log_2\tfrac{1}{0.15} + 0.5\log_2 2 + 0.25\log_2 4 = \mathbf{1.742\ bits/symbol}$$

**$H(X,Y)$** (cells: four 0.05, two 0.20, four 0.10):
$$H(X,Y) = 4(0.05\log_2 20) + 2(0.20\log_2 5) + 4(0.10\log_2 10) = \mathbf{3.121\ bits/symbol}$$

**Conditional entropies:**
$$H(Y/X) = H(X,Y) - H(X) = 3.121 - 1.9709 = \mathbf{1.150\ bits/symbol}$$
$$H(X/Y) = H(X,Y) - H(Y) = 3.121 - 1.742 = \mathbf{1.379\ bits/symbol}$$

> **Steps explained:** Same procedure as Q3. The conditional matrices $P(Y/X)$ (divide each row by $P(X_i)$) and $P(X/Y)$ (divide each column by $P(Y_j)$) can be built to verify, but the subtraction identities give the answers directly.

---

## Q5 — Mutual information of the 4×4 JPM

*(Uses the same matrix and results as Q4.)*

$$I(X;Y) = H(X) + H(Y) - H(X,Y) = 1.9709 + 1.742 - 3.121 = \mathbf{0.592\ bits/symbol}$$

**Cross-checks:**
$$I(X;Y) = H(X) - H(X/Y) = 1.9709 - 1.379 = 0.592 \quad\checkmark$$
$$I(X;Y) = H(Y) - H(Y/X) = 1.742 - 1.150 = 0.592 \quad\checkmark$$

> **Steps explained:** Once the four entropies from Q4 are known, mutual information is a one-line subtraction. All three equivalent formulas agree — a good way to confirm no arithmetic slipped.

---

## Q6 — Rate of transmission through a channel

*(Same style as Mutual-Information Problem 2 in the notes — here $r_s = 10000$.)*

**Channel:** $x_1\!\to\!y_1=\tfrac34,\ x_1\!\to\!y_2=\tfrac14;\ x_2\!\to\!y_2=\tfrac12,\ x_2\!\to\!y_3=\tfrac12$, with $P(X_1)=P(X_2)=\tfrac12$.

Multiply each row of the channel matrix by its input probability $P(X_i)=\tfrac12$ to obtain the JPM:

$$
P(Y/X)=
\begin{bmatrix}
3/4 & 1/4 & 0 \\
0 & 1/2 & 1/2
\end{bmatrix}
\qquad
P(X,Y)=
\begin{bmatrix}
3/8 & 1/8 & 0 \\
0 & 1/4 & 1/4
\end{bmatrix}
$$

**Output probabilities:** $P(Y) = \big[\tfrac38,\ \tfrac38,\ \tfrac14\big]$

**Output entropy:**
$$H(Y) = 2\Big(\tfrac38\log_2\tfrac83\Big) + \tfrac14\log_2 4 = 1.0613 + 0.5 = 1.5612\ \text{bits/symbol}$$

**Noise entropy:**
$$H(Y/X) = \tfrac38\log_2\tfrac43 + \tfrac18\log_2 4 + \tfrac14\log_2 2 + \tfrac14\log_2 2 = 0.1556 + 0.25 + 0.25 + 0.25 = 0.9056\ \text{bits/symbol}$$

**Mutual information:**
$$I(X;Y) = H(Y) - H(Y/X) = 1.5612 - 0.9056 = 0.6556\ \text{bits/symbol}$$

**Rate of transmission:**
$$R_t = I(X;Y)\cdot r_s = 0.6556 \times 10000 = \boxed{6556\ \text{bits/sec}}$$

**LaTeX (TikZ) — channel diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=1.8cm and 4cm,
   every node/.style={font=\small}]
  \node (x1) {$x_1$};
  \node (x2) [below=of x1] {$x_2$};
  \node (y1) [right=of x1] {$y_1$};
  \node (y2) [below=of y1] {$y_2$};
  \node (y3) [below=of y2] {$y_3$};
  \draw[->] (x1) -- node[above]{$3/4$} (y1);
  \draw[->] (x1) -- node[pos=.35,above]{$1/4$} (y2);
  \draw[->] (x2) -- node[pos=.35,below]{$1/2$} (y2);
  \draw[->] (x2) -- node[below]{$1/2$} (y3);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Multiply each channel-matrix row by its $P(X_i)$ to get the JPM; column-sum for $P(Y)$. Then $H(Y)$ from the output marginals and $H(Y/X)$ from the JPM weighted by $\log_2\tfrac{1}{P(Y_j/X_i)}$. Mutual information × symbol rate = transmission rate.

---

# UNIT 2

## Q7 — BSC diagram & capacity

A **Binary Symmetric Channel** has 2 inputs, 2 outputs, error probability $P$ and correct probability $q=1-P$:

$$
P(Y/X)=
\begin{bmatrix}
P & q \\
q & P
\end{bmatrix}
\qquad P(X_1)=P(X_2)=\tfrac12
$$

**Derivation.** Because both rows contain the same two entries (just swapped), the conditional entropy is the same constant $h$ for every input:

$$H(Y/X) = h = P\log_2\tfrac1P + q\log_2\tfrac1q$$

Then

$$C = \max\{I(X;Y)\} = \max\{H(Y)\} - h$$

$H(Y)$ is maximum when both outputs are equiprobable, i.e. $H(Y)_{\max}=\log_2 m = \log_2 2 = 1$. Hence

$$\boxed{C = 1 - h = 1 - \Big[P\log_2\tfrac1P + q\log_2\tfrac1q\Big] \ \text{bits/symbol}}$$

and $C\cdot r_s$ bits/sec.

**LaTeX (TikZ) — BSC diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=2.2cm and 4cm,
   every node/.style={font=\small}]
  \node (x1) {$x_1$};
  \node (x2) [below=of x1] {$x_2$};
  \node (y1) [right=of x1] {$y_1$};
  \node (y2) [below=of y1] {$y_2$};
  \draw[->] (x1) -- node[above]{$P$ (correct)} (y1);
  \draw[->] (x1) -- node[pos=.3,above,sloped]{$1-P$} (y2);
  \draw[->] (x2) -- node[pos=.3,below,sloped]{$1-P$} (y1);
  \draw[->] (x2) -- node[below]{$P$ (correct)} (y2);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** The symmetric row structure makes $H(Y/X)$ a fixed constant $h$ independent of the input distribution. Capacity is then just the maximum output entropy ($\log_2 2 = 1$) minus that constant.

---

## Q8 — BEC diagram & capacity

In a **Binary Erasure Channel**, a transmitted bit is either received correctly (prob $\bar P = 1-P$) or **erased** to symbol `Y` (prob $P$). No bit is ever flipped, so no wrong decision is made. With $P(X_1)=W$, $P(X_2)=\bar W$:

$$
P(Y/X)=
\begin{bmatrix}
\bar P & P & 0 \\
0 & P & \bar P
\end{bmatrix}
$$

**Derivation.** Building the JPM and then $P(X/Y)$ gives

$$H(X/Y) = P\Big[W\log_2\tfrac1W + \bar W\log_2\tfrac1{\bar W}\Big] = P\,H(X)$$

So

$$I(X;Y) = H(X) - H(X/Y) = H(X) - P\,H(X) = (1-P)\,H(X) = \bar P\,H(X)$$

Maximising over the input ($H(X)_{\max}=\log_2 2 = 1$):

$$\boxed{C = \bar P = (1-P)\ \text{bits/symbol}, \qquad C = \bar P\, r_s \ \text{bits/sec}}$$

**LaTeX (TikZ) — BEC diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=2.4cm and 4.5cm,
   every node/.style={font=\small}]
  \node (x1) {$x_1\ (0)$};
  \node (x2) [below=of x1] {$x_2\ (1)$};
  \node (y1) [right=of x1] {$y_1\ (0)$};
  \node (ye) [below=of y1] {$Y$ (erasure)};
  \node (y2) [below=of ye] {$y_2\ (1)$};
  \draw[->] (x1) -- node[above]{$\bar P=1-P$} (y1);
  \draw[->] (x1) -- node[pos=.4,above,sloped]{$P$} (ye);
  \draw[->] (x2) -- node[pos=.4,below,sloped]{$P$} (ye);
  \draw[->] (x2) -- node[below]{$\bar P=1-P$} (y2);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Erasures lose information proportional to $P$, giving $H(X/Y)=P\,H(X)$. The surviving fraction $(1-P)$ of $H(X)$ is the mutual information; maximising $H(X)$ at 1 bit yields $C=\bar P$.

---

## Q9 — Capacity of a DMC (4×4)

**Channel:** inputs $\{0,1,2,3\}$, outputs $\{0,1,2,3\}$ with $q=1-p$:

$$
P(Y/X)=
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & p & q & 0 \\
0 & q & p & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

**Key observation — the channel splits into two independent sub-channels with disjoint inputs and outputs:**

- **Sub-channel A** (inputs 0, 3 → outputs 0, 3): perfectly **noiseless**, capacity $C_A = \log_2 2 = 1$ bit.
- **Sub-channel B** (inputs 1, 2 → outputs 1, 2): a **BSC** with crossover $q$, capacity $C_B = 1 - H(p)$, where $H(p) = p\log_2\tfrac1p + q\log_2\tfrac1q$.

For a channel that is the **sum of disjoint sub-channels**, the overall capacity satisfies $2^{C} = 2^{C_A} + 2^{C_B}$:

$$\boxed{C = \log_2\!\Big(2^{C_A} + 2^{C_B}\Big) = \log_2\!\Big(2 + 2^{\,1-H(p)}\Big)\ \text{bits/symbol}}$$

**Checks (limiting cases):**
- If $p=q=\tfrac12$ (sub-channel B useless, $H=1$, $C_B=0$): $C=\log_2(2+1)=\log_2 3 = 1.585$ bits.
- If $p=1$ (B noiseless, $H=0$, $C_B=1$): $C=\log_2(2+2)=\log_2 4 = 2$ bits (all 4 inputs distinguishable).

**LaTeX (TikZ) — channel diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=1.4cm and 4.5cm,
   every node/.style={font=\small}]
  \node (x0) {$0$};
  \node (x1) [below=of x0] {$1$};
  \node (x2) [below=of x1] {$2$};
  \node (x3) [below=of x2] {$3$};
  \node (y0) [right=of x0] {$0$};
  \node (y1) [right=of x1] {$1$};
  \node (y2) [right=of x2] {$2$};
  \node (y3) [right=of x3] {$3$};
  \draw[->] (x0) -- node[above]{$1$} (y0);
  \draw[->] (x1) -- node[above]{$p$} (y1);
  \draw[->] (x1) -- node[pos=.3,above,sloped]{$q$} (y2);
  \draw[->] (x2) -- node[pos=.3,below,sloped]{$q$} (y1);
  \draw[->] (x2) -- node[below]{$p$} (y2);
  \draw[->] (x3) -- node[below]{$1$} (y3);
  \node[font=\scriptsize] at ($(x1)!0.5!(y2)+(0,-0.9)$) {$q=1-p$};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Notice that inputs 0,3 and inputs 1,2 use **completely separate** outputs — the channel is two non-interacting channels glued together. Capacity of such a "sum" channel is $\log_2(2^{C_A}+2^{C_B})$. Plug in the noiseless capacity (1) and the BSC capacity ($1-H(p)$).

---

## Q10 — Non-symmetric binary channel

**Channel:** $\alpha = P(y_1|x_1)$, $1-\alpha = P(y_2|x_1)$, $1-\beta = P(y_1|x_2)$, $\beta = P(y_2|x_2)$.

### (i) Given $P(X{=}0)=\tfrac14$, $P(X{=}1)=\tfrac34$, $\alpha=0.75$, $\beta=0.9$

Multiply each row of the channel matrix by its input probability to obtain the JPM:

$$
P(Y/X)=
\begin{bmatrix}
0.75 & 0.25 \\
0.10 & 0.90
\end{bmatrix}
\qquad
P(X,Y)=
\begin{bmatrix}
0.1875 & 0.0625 \\
0.075 & 0.675
\end{bmatrix}
$$

**Output marginals:** $P(Y) = [0.2625,\ 0.7375]$.

**$H(X)$:**
$$H(X) = 0.25\log_2 4 + 0.75\log_2\tfrac{1}{0.75} = 0.5 + 0.3113 = \mathbf{0.8113\ bits/symbol}$$

**$H(Y)$:**
$$H(Y) = 0.2625\log_2\tfrac{1}{0.2625} + 0.7375\log_2\tfrac{1}{0.7375} = 0.5066 + 0.3240 = \mathbf{0.8306\ bits/symbol}$$

**$H(X,Y)$** (cells 0.1875, 0.0625, 0.075, 0.675):
$$H(X,Y) = 0.4528 + 0.25 + 0.2803 + 0.3828 = \mathbf{1.3658\ bits/symbol}$$

**Conditional entropies:**
$$H(Y/X) = H(X,Y) - H(X) = 1.3658 - 0.8113 = \mathbf{0.5545\ bits/symbol}$$
$$H(X/Y) = H(X,Y) - H(Y) = 1.3658 - 0.8306 = \mathbf{0.5352\ bits/symbol}$$

### (ii) Capacity of the BSC when $\alpha = \beta = 0.75$

Now correct prob $=0.75$, error $=0.25$:

$$h = 0.75\log_2\tfrac{1}{0.75} + 0.25\log_2\tfrac{1}{0.25} = 0.3113 + 0.5 = 0.8113$$
$$\boxed{C = 1 - h = 1 - 0.8113 = 0.1887\ \text{bits/symbol}}$$

**LaTeX (TikZ) — non-symmetric binary channel:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=2.2cm and 4cm,
   every node/.style={font=\small}]
  \node (x1) {$x_1\ (0)$};
  \node (x2) [below=of x1] {$x_2\ (1)$};
  \node (y1) [right=of x1] {$(0)\ y_1$};
  \node (y2) [below=of y1] {$(1)\ y_2$};
  \draw[->] (x1) -- node[above]{$\alpha$} (y1);
  \draw[->] (x1) -- node[pos=.3,above,sloped]{$1-\alpha$} (y2);
  \draw[->] (x2) -- node[pos=.3,below,sloped]{$1-\beta$} (y1);
  \draw[->] (x2) -- node[below]{$\beta$} (y2);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Part (i) is a standard JPM problem — build the JPM (rows × input probs), then the marginals and the three entropies, with conditionals from the subtraction identities. Part (ii) collapses to a symmetric channel, so the BSC formula $C=1-h$ applies.

---

## Q11 — Muroga's method (theory)

**Muroga's method** estimates the capacity of a discrete channel whose channel matrix is **square and non-singular** ($n$ inputs, $n$ outputs), without searching over input distributions. (Throughout, $\log \equiv \log_2$.)

**Step 1 — Set up the matrix equation.** Multiply the channel matrix $P(Y/X)$ by the unknown column vector $\mathbf{Q}=[Q_1,\dots,Q_n]^{T}$, and set it equal to a column vector whose $i$-th entry is the **row-sum** $\sum_j P_{ij}\log_2 P_{ij}$ of that same row:

![Muroga matrix equation - general form](https://latex.codecogs.com/png.image?%5Cdpi%7B120%7D%5Cbg%7Bwhite%7D%5Cbegin%7Bbmatrix%7D%20P%5F%7B11%7D%20%26%20P%5F%7B12%7D%20%26%20%5Ccdots%20%26%20P%5F%7B1n%7D%20%5C%5C%20P%5F%7B21%7D%20%26%20P%5F%7B22%7D%20%26%20%5Ccdots%20%26%20P%5F%7B2n%7D%20%5C%5C%20%5Cvdots%20%26%20%5Cvdots%20%26%20%5Cddots%20%26%20%5Cvdots%20%5C%5C%20P%5F%7Bn1%7D%20%26%20P%5F%7Bn2%7D%20%26%20%5Ccdots%20%26%20P%5F%7Bnn%7D%20%5Cend%7Bbmatrix%7D%5Cbegin%7Bbmatrix%7D%20Q%5F1%20%5C%5C%20Q%5F2%20%5C%5C%20%5Cvdots%20%5C%5C%20Q%5Fn%20%5Cend%7Bbmatrix%7D%3D%5Cbegin%7Bbmatrix%7D%20%5Csum%5Fj%20P%5F%7B1j%7D%5Clog%5F2%20P%5F%7B1j%7D%20%5C%5C%20%5Csum%5Fj%20P%5F%7B2j%7D%5Clog%5F2%20P%5F%7B2j%7D%20%5C%5C%20%5Cvdots%20%5C%5C%20%5Csum%5Fj%20P%5F%7Bnj%7D%5Clog%5F2%20P%5F%7Bnj%7D%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for the equation above</summary>

```latex
\begin{bmatrix} P_{11} & P_{12} & \cdots & P_{1n} \\ P_{21} & P_{22} & \cdots & P_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ P_{n1} & P_{n2} & \cdots & P_{nn} \end{bmatrix}
\begin{bmatrix} Q_1 \\ Q_2 \\ \vdots \\ Q_n \end{bmatrix}
=
\begin{bmatrix} \sum_j P_{1j}\log_2 P_{1j} \\ \sum_j P_{2j}\log_2 P_{2j} \\ \vdots \\ \sum_j P_{nj}\log_2 P_{nj} \end{bmatrix}
```

</details>

The left matrix is the **channel matrix** $P(Y/X)$; the middle column is the unknown vector $\mathbf{Q}$; the right column is the **row-sums** vector. Here $P_{ij} = P(y_j\,|\,x_i)$ is the entry in **row $i$, column $j$** of the channel matrix.

**Step 2 — Solve the $n$ simultaneous equations** for $Q_1,\dots,Q_n$.

**Step 3 — Capacity:**

$$C = \log_2\!\Big[\,2^{Q_1}+2^{Q_2}+\cdots+2^{Q_n}\,\Big]\ \text{bits/symbol}$$

> **Steps explained:** Each row of the matrix equation reads "(row of channel matrix) · $\mathbf{Q}$ = (that row's $\sum P\log P$)". The right-hand vector is just the **negative row-entropy** of each row. Solve the linear system, then take $C=\log_2\sum 2^{Q_j}$. Works only for a **square, invertible** channel matrix.

---

## Q12 — Muroga's method (3×3)

$$
P(Y/X)=
\begin{bmatrix}
0.8 & 0.2 & 0 \\
0.1 & 0.8 & 0.1 \\
0 & 0.2 & 0.8
\end{bmatrix}
$$

**LaTeX (TikZ) — channel diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=1.8cm and 4.5cm,
   every node/.style={font=\small}]
  \node (x1) {$x_1$};
  \node (x2) [below=of x1] {$x_2$};
  \node (x3) [below=of x2] {$x_3$};
  \node (y1) [right=of x1] {$y_1$};
  \node (y2) [right=of x2] {$y_2$};
  \node (y3) [right=of x3] {$y_3$};
  \draw[->] (x1) -- node[above]{$0.8$} (y1);
  \draw[->] (x1) -- node[pos=.3,above]{$0.2$} (y2);
  \draw[->] (x2) -- node[pos=.3,above,sloped]{$0.1$} (y1);
  \draw[->] (x2) -- node[above]{$0.8$} (y2);
  \draw[->] (x2) -- node[pos=.3,below,sloped]{$0.1$} (y3);
  \draw[->] (x3) -- node[pos=.3,below,sloped]{$0.2$} (y2);
  \draw[->] (x3) -- node[below]{$0.8$} (y3);
\end{tikzpicture}
\end{document}
```

**Step 1 — Write the matrix equation** $P(Y/X)\,\mathbf{Q} = \mathbf{r}$, where each RHS entry is that row's $\sum_j P_{ij}\log_2 P_{ij}$:

![Muroga matrix equation - Q12](https://latex.codecogs.com/png.image?%5Cdpi%7B120%7D%5Cbg%7Bwhite%7D%5Cbegin%7Bbmatrix%7D%200%2E8%20%26%200%2E2%20%26%200%20%5C%5C%200%2E1%20%26%200%2E8%20%26%200%2E1%20%5C%5C%200%20%26%200%2E2%20%26%200%2E8%20%5Cend%7Bbmatrix%7D%5Cbegin%7Bbmatrix%7D%20Q%5F1%20%5C%5C%20Q%5F2%20%5C%5C%20Q%5F3%20%5Cend%7Bbmatrix%7D%3D%5Cbegin%7Bbmatrix%7D%200%2E8%5Clog%5F2%200%2E8%20%2B%200%2E2%5Clog%5F2%200%2E2%20%5C%5C%200%2E1%5Clog%5F2%200%2E1%20%2B%200%2E8%5Clog%5F2%200%2E8%20%2B%200%2E1%5Clog%5F2%200%2E1%20%5C%5C%200%2E2%5Clog%5F2%200%2E2%20%2B%200%2E8%5Clog%5F2%200%2E8%20%5Cend%7Bbmatrix%7D%3D%5Cbegin%7Bbmatrix%7D%20%2D0%2E7219%20%5C%5C%20%2D0%2E9219%20%5C%5C%20%2D0%2E7219%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for the equation above</summary>

```latex
\begin{bmatrix} 0.8 & 0.2 & 0 \\ 0.1 & 0.8 & 0.1 \\ 0 & 0.2 & 0.8 \end{bmatrix}
\begin{bmatrix} Q_1 \\ Q_2 \\ Q_3 \end{bmatrix}
=
\begin{bmatrix} 0.8\log_2 0.8 + 0.2\log_2 0.2 \\ 0.1\log_2 0.1 + 0.8\log_2 0.8 + 0.1\log_2 0.1 \\ 0.2\log_2 0.2 + 0.8\log_2 0.8 \end{bmatrix}
=
\begin{bmatrix} -0.7219 \\ -0.9219 \\ -0.7219 \end{bmatrix}
```

</details>

**Step 2 — Expand into 3 simultaneous equations** (each row of the matrix product):

$$0.8\,Q_1 + 0.2\,Q_2 + 0\,Q_3 = -0.7219 \qquad (1)$$

$$0.1\,Q_1 + 0.8\,Q_2 + 0.1\,Q_3 = -0.9219 \qquad (2)$$

$$0\,Q_1 + 0.2\,Q_2 + 0.8\,Q_3 = -0.7219 \qquad (3)$$

Equations (1) and (3) are mirror images $\Rightarrow$ by symmetry $Q_1 = Q_3$. Solving:

$$\boxed{Q_1 = Q_3 = -0.6553, \qquad Q_2 = -0.9886}$$

**Step 3 — Capacity:**

$$C = \log_2\!\big[2^{-0.6553} + 2^{-0.9886} + 2^{-0.6553}\big] = \log_2[0.6349 + 0.5040 + 0.6349] = \log_2(1.7738)$$
$$\boxed{C = 0.827\ \text{bits/symbol}}$$

> **Steps explained:** Compute each row's $\sum P\log_2 P$ for the RHS, solve the 3×3 system (the symmetry $Q_1=Q_3$ halves the work), then $C=\log_2\sum 2^{Q_j}$.

---

## Q13 — BSC capacity by Muroga + verify

$$
P(Y/X)=
\begin{bmatrix}
3/4 & 1/4 \\
1/4 & 3/4
\end{bmatrix}
$$

**LaTeX (TikZ) — BSC channel diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=2.2cm and 4cm,
   every node/.style={font=\small}]
  \node (x1) {$x_1$};
  \node (x2) [below=of x1] {$x_2$};
  \node (y1) [right=of x1] {$y_1$};
  \node (y2) [below=of y1] {$y_2$};
  \draw[->] (x1) -- node[above]{$3/4$} (y1);
  \draw[->] (x1) -- node[pos=.3,above,sloped]{$1/4$} (y2);
  \draw[->] (x2) -- node[pos=.3,below,sloped]{$1/4$} (y1);
  \draw[->] (x2) -- node[below]{$3/4$} (y2);
\end{tikzpicture}
\end{document}
```

**Step 1 — Write the matrix equation** $P(Y/X)\,\mathbf{Q} = \mathbf{r}$:

![Muroga matrix equation - Q13 BSC](https://latex.codecogs.com/png.image?%5Cdpi%7B120%7D%5Cbg%7Bwhite%7D%5Cbegin%7Bbmatrix%7D%203%2F4%20%26%201%2F4%20%5C%5C%201%2F4%20%26%203%2F4%20%5Cend%7Bbmatrix%7D%5Cbegin%7Bbmatrix%7D%20Q%5F1%20%5C%5C%20Q%5F2%20%5Cend%7Bbmatrix%7D%3D%5Cbegin%7Bbmatrix%7D%20%5Cfrac34%5Clog%5F2%5Cfrac34%20%2B%20%5Cfrac14%5Clog%5F2%5Cfrac14%20%5C%5C%20%5Cfrac14%5Clog%5F2%5Cfrac14%20%2B%20%5Cfrac34%5Clog%5F2%5Cfrac34%20%5Cend%7Bbmatrix%7D%3D%5Cbegin%7Bbmatrix%7D%20%2D0%2E8113%20%5C%5C%20%2D0%2E8113%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for the equation above</summary>

```latex
\begin{bmatrix} 3/4 & 1/4 \\ 1/4 & 3/4 \end{bmatrix}
\begin{bmatrix} Q_1 \\ Q_2 \end{bmatrix}
=
\begin{bmatrix} \frac34\log_2\frac34 + \frac14\log_2\frac14 \\ \frac14\log_2\frac14 + \frac34\log_2\frac34 \end{bmatrix}
=
\begin{bmatrix} -0.8113 \\ -0.8113 \end{bmatrix}
```

</details>

**Step 2 — Expand into 2 simultaneous equations:**

$$0.75\,Q_1 + 0.25\,Q_2 = -0.8113 \qquad (1)$$

$$0.25\,Q_1 + 0.75\,Q_2 = -0.8113 \qquad (2)$$

By symmetry $Q_1 = Q_2$. Adding (1) and (2): $Q_1+Q_2 = -1.6226 \Rightarrow Q_1 = Q_2 = -0.8113$.

**Step 3 — Capacity:**
$$C = \log_2\big[2^{-0.8113} + 2^{-0.8113}\big] = \log_2\big[2\cdot 2^{-0.8113}\big] = 1 - 0.8113 = \boxed{0.1887\ \text{bits/symbol}}$$

**Verification** using the BSC formula $C = 1-h$:
$$h = 0.75\log_2\tfrac43 + 0.25\log_2 4 = 0.3113 + 0.5 = 0.8113 \;\Rightarrow\; C = 1 - 0.8113 = 0.1887 \quad\checkmark$$

> **Steps explained:** Muroga gives $Q_1=Q_2=-h$, so $C=\log_2(2\cdot2^{-h})=1-h$ — exactly the standard BSC capacity. The two methods match.

---

# UNIT 3 — Shannon-Hartley (Continuous Channels)

> **Master formula (all of Unit 3):** $C = B\log_2\!\big(1 + \tfrac{S}{N}\big)$ bits/sec, with $\tfrac{S}{N}$ as a **ratio** (convert from dB first).

## Q14 — Telephone channel capacity

$B = 3.4$ kHz $= 3400$ Hz.

### (i) Capacity at SNR = 30 dB

$\dfrac{S}{N} = 10^{30/10} = 1000$.

$$C = B\log_2\!\Big(1+\tfrac{S}{N}\Big) = 3400\log_2(1001) = 3400 \times 9.967 = \boxed{33{,}887\ \text{bits/sec} \approx 33.9\ \text{kbps}}$$

### (ii) Minimum SNR to support 4800 bits/sec

$$4800 = 3400\log_2\!\Big(1+\tfrac{S}{N}\Big) \;\Rightarrow\; \log_2\!\Big(1+\tfrac{S}{N}\Big) = \tfrac{4800}{3400} = 1.4118$$
$$1+\tfrac{S}{N} = 2^{1.4118} = 2.66 \;\Rightarrow\; \tfrac{S}{N} = 1.66 \;\;(\approx \mathbf{2.2\ dB})$$

> **Steps explained:** Convert dB→ratio, then apply Shannon-Hartley. For (ii), rearrange the same formula to solve for $S/N$, then convert back to dB with $10\log_{10}(\cdot)$.

---

## Q15 — TV picture: minimum bandwidth

$3\times10^5$ elements/frame, 10 equiprobable levels, 30 frames/sec, SNR = 30 dB.

**Information per element:** $\log_2 10 = 3.3219$ bits.

**Information rate:**
$$R = 3\times10^5 \times 3.3219 \times 30 = \mathbf{2.99\times10^{7}\ bits/sec}$$

**Minimum bandwidth** (need $C \ge R$, with $S/N = 1000$):
$$B = \frac{R}{\log_2(1+S/N)} = \frac{2.99\times10^{7}}{\log_2(1001)} = \frac{2.99\times10^{7}}{9.967} = \boxed{3.0\times10^{6}\ \text{Hz} = 3\ \text{MHz}}$$

> **Steps explained:** Total source rate = (bits/element) × (elements/frame) × (frames/sec). Set channel capacity equal to this rate and solve Shannon-Hartley for $B$.

---

## Q16 — Alphanumeric data channel

$B = 3.4$ kHz, SNR = 20 dB $\Rightarrow S/N = 100$, 128 equiprobable symbols.

**Channel capacity:**
$$C = 3400\log_2(101) = 3400 \times 6.658 = \boxed{22{,}638\ \text{bits/sec} \approx 22.64\ \text{kbps}}$$

**Information per symbol:** $\log_2 128 = 7$ bits.

**Maximum symbol rate (error-free):**
$$r_s = \frac{C}{\log_2 128} = \frac{22{,}638}{7} = \boxed{3234\ \text{symbols/sec}}$$

> **Steps explained:** Capacity from Shannon-Hartley. Each symbol carries $\log_2 128 = 7$ bits, so the largest symbol rate the channel supports without error is $C/7$.

---

## Q17 — Analog signal information rate

$B = 4$ kHz, sampled at **2.5× Nyquist rate**, 256 equiprobable levels.

**Sampling rate:** Nyquist rate $= 2B = 8$ kHz, so $r_s = 2.5\times8000 = 20{,}000$ samples/sec.

**Information per sample:** $\log_2 256 = 8$ bits.

**Information rate:**
$$R = 20{,}000 \times 8 = \boxed{160{,}000\ \text{bits/sec} = 160\ \text{kbps}}$$

**Can it pass a 50 kHz, 20 dB Gaussian channel?** $S/N = 100$:
$$C = 50{,}000\log_2(101) = 50{,}000\times6.658 = 332{,}910\ \text{bits/sec}$$
Since $C = 332.9\ \text{kbps} > R = 160\ \text{kbps}$ → **Yes, error-free transmission is possible.**

**Bandwidth needed if $S/N = 10$ dB $=10$:**
$$B = \frac{R}{\log_2(1+10)} = \frac{160{,}000}{\log_2 11} = \frac{160{,}000}{3.459} = \boxed{46.25\ \text{kHz}}$$

> **Steps explained:** Source rate = (samples/sec) × (bits/sample). Compare with the channel's capacity to test feasibility. For the last part, set $C=R$ and solve Shannon-Hartley for $B$ at the new SNR.

---

## Q18 — Shannon-Hartley law & C∞ proof

**Shannon-Hartley law:** The capacity of a band-limited AWGN channel of bandwidth $B$ with signal power $S$ and noise power $N$ is

$$\boxed{C = B\log_2\!\Big(1 + \frac{S}{N}\Big)\ \text{bits/sec}}$$

**Proof that $C_\infty = 1.44\,\dfrac{S}{\eta} = 1.44\,B\,\dfrac{S}{N}$ as $B\to\infty$:**

Noise power $N = \eta B$ ($\eta$ = noise PSD). Substitute:

$$C = B\log_2\!\Big(1 + \frac{S}{\eta B}\Big)$$

Let $x = \dfrac{S}{\eta B}$, so $B = \dfrac{S}{\eta x}$:

$$C = \frac{S}{\eta x}\log_2(1+x) = \frac{S}{\eta}\log_2\big[(1+x)^{1/x}\big]$$

As $B\to\infty$, $x\to 0$, and $\displaystyle\lim_{x\to0}(1+x)^{1/x} = e$:

$$C_\infty = \frac{S}{\eta}\log_2 e = 1.44\,\frac{S}{\eta}$$

Since $\dfrac{S}{N} = \dfrac{S}{\eta B}$ gives $\dfrac{S}{\eta} = B\,\dfrac{S}{N}$:

$$\boxed{C_\infty = 1.44\,B\,\frac{S}{N}\ \text{bits/sec}}$$

> **Steps explained:** Write $N=\eta B$, substitute, and rearrange into the form $\frac{S}{\eta}\log_2(1+x)^{1/x}$. The limit $(1+x)^{1/x}\to e$ as bandwidth grows gives the famous $1.44\,S/\eta$ ceiling — capacity does **not** grow without bound as $B\to\infty$.

---

## Q19 — AWGN channel capacity

$B = 4$ kHz, noise PSD $\tfrac{\eta}{2} = 10^{-14}$ W/Hz $\Rightarrow \eta = 2\times10^{-14}$ W/Hz, signal power $S = 0.1$ mW $= 10^{-4}$ W.

**Noise power:**
$$N = \eta B = 2\times10^{-14} \times 4000 = 8\times10^{-11}\ \text{W}$$

**Signal-to-noise ratio:**
$$\frac{S}{N} = \frac{10^{-4}}{8\times10^{-11}} = 1.25\times10^{6}$$

**Capacity:**
$$C = B\log_2\!\Big(1+\tfrac{S}{N}\Big) = 4000\log_2(1.25\times10^{6}) = 4000 \times 20.25 = \boxed{81{,}014\ \text{bits/sec} \approx 81\ \text{kbps}}$$

> **Steps explained:** Get noise power $N=\eta B$ (use the **one-sided** $\eta=2\times10^{-14}$), form $S/N$, and apply Shannon-Hartley. The "+1" inside the log is negligible here since $S/N\gg1$.

---

## Q20 — CRT terminal channel

$B = 3$ kHz, SNR = 10 dB $\Rightarrow S/N = 10$, 128 equiprobable characters.

**(i) Average information per character:**
$$H = \log_2 128 = \boxed{7\ \text{bits/character}}$$

**(ii) Channel capacity:**
$$C = B\log_2\!\Big(1+\tfrac{S}{N}\Big) = 3000\log_2(11) = 3000\times3.459 = \boxed{10{,}378\ \text{bits/sec}}$$

**(iii) Maximum error-free data rate (characters/sec):**
$$r_s = \frac{C}{H} = \frac{10{,}378}{7} = \boxed{1483\ \text{characters/sec}}$$

> **Steps explained:** Equiprobable 128 symbols ⇒ 7 bits each. Capacity from Shannon-Hartley, then divide by 7 bits/character to get the maximum symbol (character) rate the channel can carry without error.

---

# 📋 Master Formula Sheet

### 1. Input / Output entropy
$$H(X) = \sum_i P(x_i)\log_2\frac{1}{P(x_i)}, \qquad H(Y) = \sum_j P(y_j)\log_2\frac{1}{P(y_j)}$$
- $P(x_i)$ = row sums of JPM; $P(y_j)$ = column sums of JPM.
- **Used in:** Q3, Q4, Q6, Q10.

### 2. Joint entropy
$$H(X,Y) = \sum_i\sum_j P(x_i,y_j)\log_2\frac{1}{P(x_i,y_j)}$$
- **Used in:** Q3, Q4, Q10.

### 3. Conditional (noise) entropies
$$H(Y/X) = H(X,Y) - H(X), \qquad H(X/Y) = H(X,Y) - H(Y)$$
- Direct form: $H(Y/X)=\sum\sum P(x_i,y_j)\log_2\frac{1}{P(y_j/x_i)}$.
- **Used in:** Q3, Q4, Q6, Q7, Q10.

### 4. Joint-probability matrix (JPM)
$$P(x_i,y_j) = P(y_j/x_i)\,P(x_i)$$
- Multiply each channel-matrix row by its input probability.
- **Used in:** Q4, Q6, Q10, Q13.

### 5. Mutual information
$$I(X;Y) = H(X) - H(X/Y) = H(Y) - H(Y/X) = H(X) + H(Y) - H(X,Y)$$
- **Used in:** Q3, Q5, Q6, Q10.

### 6. Rate of transmission
$$R_t = I(X;Y)\cdot r_s \ \text{bits/sec}$$
- $r_s$ = symbol rate (symbols/sec).
- **Used in:** Q6.

### 7. Channel capacity (general)
$$C = \max\{I(X;Y)\} = \max\{H(Y)\} - H(Y/X)$$
- **Used in:** Q7, Q8, Q9, Q10, Q12, Q13.

### 8. Symmetric channel capacity
$$C = \log_2 m - h, \qquad h = \sum_j P_j\log_2\frac{1}{P_j}\ \text{(any row of channel matrix)}$$
- $m$ = no. of outputs; $h$ = constant row entropy.
- **Used in:** Q7, Q9.

### 9. BSC capacity
$$C = 1 - h, \qquad h = P\log_2\frac1P + q\log_2\frac1q, \quad q = 1-P$$
- **Used in:** Q7, Q10(ii), Q13.

### 10. BEC capacity
$$C = (1-P) = \bar P \ \text{bits/symbol}, \qquad C = \bar P\, r_s \ \text{bits/sec}$$
- $P$ = erasure probability.
- **Used in:** Q8.

### 11. Sum of disjoint sub-channels
$$2^{C} = 2^{C_A} + 2^{C_B} \;\Rightarrow\; C = \log_2\!\big(2^{C_A}+2^{C_B}\big)$$
- Valid when sub-channels have disjoint input **and** output alphabets.
- **Used in:** Q9.

### 12. Muroga's method
$$[\,P(Y/X)\,]\,\mathbf{Q} = \mathbf{r}, \quad r_i = \sum_j P_{ij}\log_2 P_{ij}; \qquad C = \log_2\!\Big[\sum_j 2^{Q_j}\Big]$$
- **Used in:** Q11, Q12, Q13.

### 13. Shannon-Hartley law
$$C = B\log_2\!\Big(1 + \frac{S}{N}\Big)\ \text{bits/sec}$$
- $B$ = bandwidth (Hz); $S/N$ = power ratio.
- **Used in:** Q14, Q15, Q16, Q17, Q18, Q19, Q20.

### 14. Decibel → ratio
$$\frac{S}{N}\Big|_{\text{ratio}} = 10^{(\text{dB}/10)}$$
- **Used in:** Q14, Q15, Q16, Q17, Q19, Q20.

### 15. Infinite-bandwidth capacity
$$C_\infty = 1.44\,\frac{S}{\eta} = 1.44\,B\,\frac{S}{N}, \qquad N = \eta B$$
- $\eta$ = noise PSD.
- **Used in:** Q18, Q19 (noise-power relation).

### 16. Source information rate
$$R = (\text{bits per symbol/sample}) \times (\text{symbols or samples per sec})$$
- Sampling: Nyquist rate $=2B$; bits/level $=\log_2 L$.
- **Used in:** Q15, Q16, Q17, Q20.

---

# Quick Answer Table

| Q  | Key answers |
|----|-------------|
| 3  | $H(X)=2.066$, $H(Y)=1.857$, $H(X,Y)=2.666$, $H(X/Y)=0.809$, $H(Y/X)=0.600$, $I=1.257$ |
| 4  | $H(X)=1.9709$, $H(Y)=1.742$, $H(X,Y)=3.121$, $H(Y/X)=1.150$, $H(X/Y)=1.379$ |
| 5  | $I(X;Y)=0.592$ bits/symbol |
| 6  | $H(Y)=1.5612$, $H(Y/X)=0.9056$, $I=0.6556$, $R_t=6556$ bits/sec |
| 7  | $C = 1 - h$, $h = P\log_2\tfrac1P + q\log_2\tfrac1q$ |
| 8  | $C = (1-P) = \bar P$ |
| 9  | $C = \log_2\!\big(2 + 2^{\,1-H(p)}\big)$ bits/symbol |
| 10 | (i) $H(X)=0.8113$, $H(Y)=0.8306$, $H(X,Y)=1.3658$, $H(Y/X)=0.5545$, $H(X/Y)=0.5352$; (ii) $C=0.1887$ |
| 11 | Solve $[P(Y/X)]Q=r$, then $C=\log_2\sum 2^{Q_j}$ |
| 12 | $Q_1=Q_3=-0.6553$, $Q_2=-0.9886$, $C=0.827$ bits/symbol |
| 13 | $Q_1=Q_2=-0.8113$, $C=0.1887$ (matches $1-h$) |
| 14 | (i) $C=33.9$ kbps; (ii) $S/N=1.66 \approx 2.2$ dB |
| 15 | $R=2.99\times10^7$ bits/sec; $B=3$ MHz |
| 16 | $C=22.64$ kbps; $r_s=3234$ symbols/sec |
| 17 | $R=160$ kbps; passes (C=332.9 kbps); $B=46.25$ kHz at 10 dB |
| 18 | $C=B\log_2(1+S/N)$; $C_\infty = 1.44\,S/\eta$ |
| 19 | $S/N=1.25\times10^6$; $C\approx81$ kbps |
| 20 | (i) 7 bits/char; (ii) $C=10{,}378$ bits/sec; (iii) 1483 chars/sec |

---

### Exam tips

- **Entropy problems (Q3–Q6, Q10):** always start by extracting $P(X)$ (row sums) and $P(Y)$ (column sums) from the JPM. The two conditional entropies are then just $H(X,Y)-H(Y)$ and $H(X,Y)-H(X)$ — no need to build conditional matrices.
- **Capacity problems:** identify the channel type first. Symmetric → $C=\log_2 m - h$; BSC → $1-h$; BEC → $\bar P$; square non-singular → Muroga.
- **Muroga:** the RHS is the row entropy $\sum P\log_2 P$ (negative). Exploit symmetry ($Q_i$ equal for identical rows) to solve fast.
- **Unit 3:** *always convert dB to a ratio first* ($10^{dB/10}$), then plug into $C=B\log_2(1+S/N)$. For "max symbol rate", divide $C$ by bits-per-symbol.
- The TikZ blocks are complete standalone documents — paste into Overleaf and compile directly.

---

*Module 3 — Information Channels. Notes by Dr. Madhavi M, ECE, GAT. Math renders on GitHub via native LaTeX ($\dots$ / $$\dots$$) support.*
