# Module 5 — Convolution Codes · Question Bank Solutions

> Complete, exam-ready solutions to all 20 questions (Unit 1: encoding methods, Unit 2: state/tree/trellis and Viterbi), faithful to the class notes (Ref: Simon Haykin).
> Each question is restated, marked **(Same method as Qx)** when it reuses a method, solved in ordered steps, and followed by a short **Steps explained**.
> Every diagram has compilable **LaTeX (TikZ)**. Generator matrices are shown as images (GitHub-safe). A full **Formulas** sheet is at the end.

> **Conventions used throughout.** `+` inside code equations means **mod-2 (Ex-OR)**: `0+0=0, 1+0=1, 0+1=1, 1+1=0`. For an `(n, k, m)` code: `n` = outputs (mod-2 adders), `k` = input bits per shift, `m` = number of register stages. Message length `L`; codeword length per output stream `= L + m`; total codeword bits `= n(L + m)`. Constraint length `K = m + 1`. Code rate `= k/n`.

---

## Table of Contents

**Unit 1 — Encoding**
- [Q1 — Time-domain (shifting / convolution) method](#q1--time-domain-shifting--convolution-method)
- [Q2 — Time-domain matrix method](#q2--time-domain-matrix-method)
- [Q3 — Transform-domain method](#q3--transform-domain-method)
- [Q4 — (3,1,2) encoder diagram and G](#q4--312-encoder-diagram-and-g)
- [Q5 — (3,1,3) encoder, output for 10110](#q5--313-encoder-output-for-10110)
- [Q6 — (3,1,2) codeword for 11101 (time + transform)](#q6--312-codeword-for-11101-time--transform)
- [Q7 — (3,2,1) encoder, codeword (matrix + transform)](#q7--321-encoder-codeword-matrix--transform)
- [Q8 — (2,1,2) codeword for 10101 (matrix + transform)](#q8--212-codeword-for-10101-matrix--transform)
- [Q9 — (2,1,4) parameters and output for 10111](#q9--214-parameters-and-output-for-10111)
- [Q10 — (2,1,3) encoder, G and codeword for 10111](#q10--213-encoder-g-and-codeword-for-10111)

**Unit 2 — State, Tree, Trellis, Viterbi**
- [Q11 — Define state table, code tree, trellis, Viterbi decoder](#q11--define-state-table-code-tree-trellis-viterbi-decoder)
- [Q12 — (2,1,2) state table, state diagram, code tree](#q12--212-state-table-state-diagram-code-tree)
- [Q13 — (2,1,1) state table, state diagram, code tree for 10111](#q13--211-state-table-state-diagram-code-tree-for-10111)
- [Q14 — (2,1,2) encoder: tables and codeword for 10011](#q14--212-encoder-tables-and-codeword-for-10011)
- [Q15 — (3,1,2) state table, state diagram, code tree](#q15--312-state-table-state-diagram-code-tree)
- [Q16 — (2,1,2) state table and trellis for 10111](#q16--212-state-table-and-trellis-for-10111)
- [Q17 — (2,1,2) state table and state diagram](#q17--212-state-table-and-state-diagram)
- [Q18 — Steps of the Viterbi algorithm](#q18--steps-of-the-viterbi-algorithm)
- [Q19 — Viterbi decoding of R = 1100000111](#q19--viterbi-decoding-of-r--1100000111)
- [Q20 — BCH codes and Golay codes](#q20--bch-codes-and-golay-codes)

- [📋 Formulas](#-formulas)

---

# UNIT 1 — ENCODING

## Q1 — Time-domain (shifting / convolution) method

**Restate:** Explain the encoding of convolutional codes using the time-domain (shifting / discrete-convolution) approach.

**Idea.** Each output stream $C^j$ is the **discrete convolution** (mod-2) of the message $d$ with the generator (impulse-response) sequence $g^j$ of that adder:

$$C_l^{\,j}=\sum_{i=0}^{m} d_{l-i}\,g_{i+1}^{\,j}\pmod 2,\qquad l=1,2,\dots,L+m .$$

**Procedure**

1. Read the generator sequence $g^j=g_1^j g_2^j\dots g_{m+1}^j$ of each adder directly from the encoder (a tap = a `1`).
2. For each output index $l$ from $1$ to $L+m$, slide the generator over the message and XOR the overlapping products (treat any $d$ with index $\le 0$ or $> L$ as $0$).
3. Repeat for every adder $j=1,\dots,n$.
4. **Multiplex** the streams bit-by-bit: $C = C_1^1C_1^2\cdots C_1^n,\;C_2^1C_2^2\cdots,\;\dots$

**Worked illustration** (the standard $(2,1,2)$ example, $g^1=111,\ g^2=101$, $D=10111$):

$$C^1 = 1\,1\,0\,0\,1\,0\,1,\qquad C^2 = 1\,0\,0\,1\,0\,1\,1$$
$$C = 11\;10\;00\;01\;10\;01\;11 .$$

> **Steps explained:** The encoder is a linear filter, so each output is "message convolved with the tap pattern." Slide-multiply-XOR for every time slot, then interleave the output streams in transmission order.

---

## Q2 — Time-domain matrix method

**Restate:** Explain convolutional encoding using the time-domain **matrix** method.

**Idea.** Encoding is $C = D\,G$, where $G$ is built by interleaving the generator sequences and shifting them down one message position per row.

**Procedure**

1. Interleave the $n$ generators into one "first row block": at tap position $i$ write the $n$ bits $g_i^1 g_i^2\dots g_i^n$.
2. **Row 1** = this interleaved pattern followed by zeros to fill $n(L+m)$ columns.
3. **Each next row** = previous row shifted right by $n$ positions (i.e. by one message-bit step).
4. There are $L$ rows (one per message bit). Then $C = D\,G$ over GF(2).

**Worked illustration** ($(2,1,2)$, $g^1=111,\ g^2=101$, interleave $\to 11,10,11$):

![matrix](https://latex.codecogs.com/png.image?%5Cdpi%7B130%7D%5Cbg%7Bwhite%7DG%3D%5Cbegin%7Bbmatrix%7D%2011%2610%2611%2600%2600%2600%2600%5C%5C00%2611%2610%2611%2600%2600%2600%5C%5C00%2600%2611%2610%2611%2600%2600%5C%5C00%2600%2600%2611%2610%2611%2600%5C%5C00%2600%2600%2600%2611%2610%2611%20%5Cend%7Bbmatrix%7D)

With $D=10111$, $C=DG = 11\;10\;00\;01\;10\;01\;11$.

> **Steps explained:** The shifting structure of the encoder becomes a banded "staircase" matrix. Multiplying the message row vector by $G$ (mod 2) produces the same output as the convolution method, in one matrix step.

---

## Q3 — Transform-domain method

**Restate:** Explain convolutional encoding using the **transform-domain** (polynomial) approach.

**Idea.** Represent message and generators as polynomials in $x$; each output is a polynomial product:

$$D(x)=\sum d_{i+1}x^{i},\qquad g^j(x)=\sum g_{i+1}^j x^{i},\qquad C^j(x)=D(x)\,g^j(x)\pmod 2 .$$

**Procedure**

1. Convert $D$ and each $g^j$ to polynomials (bit at position $i$ ↔ $x^{i}$, starting at $x^0$).
2. Multiply $C^j(x)=D(x)g^j(x)$; reduce coefficients mod 2 (so $x^a+x^a=0$).
3. Read each $C^j(x)$ back as a bit string of length $L+m$.
4. Multiplex the streams bit-by-bit.

**Worked illustration** ($(2,1,2)$, $D=10111\Rightarrow D(x)=1+x^2+x^3+x^4$, $g^1(x)=1+x+x^2$, $g^2(x)=1+x^2$):

$$C^1(x)=1+x+x^4+x^6\Rightarrow 1100101,\qquad C^2(x)=1+x^3+x^5+x^6\Rightarrow 1001011$$
$$C=11\;10\;00\;01\;10\;01\;11 .$$

> **Steps explained:** Convolution in time = multiplication of polynomials. Turn everything into polynomials, multiply, reduce mod 2, then interleave — the fastest hand method once you are comfortable with polynomial products.

---

## Q4 — (3,1,2) encoder diagram and G

**Restate:** A $(3,1,2)$ code has $g^{(1)}=110$, $g^{(2)}=100$, $g^{(3)}=111$. Draw the encoder and find the generator matrix $G$.

$n=3,\ k=1,\ m=2$ (2 register stages, 3 adders). Tap reading: $g^1=110\Rightarrow$ adder-1 from input and FF1; $g^2=100\Rightarrow$ adder-2 from input only; $g^3=111\Rightarrow$ adder-3 from input, FF1, FF2.

**Step 1 — Interleave generators** (position $i$: $g_i^1g_i^2g_i^3$): $\,111,\ 101,\ 001$.

**Step 2 — Generator matrix** ($L=5$ rows, $n(L+m)=3(7)=21$ columns; each row shifts right by 3):

![matrix](https://latex.codecogs.com/png.image?%5Cdpi%7B130%7D%5Cbg%7Bwhite%7DG%3D%5Cbegin%7Bbmatrix%7D%20111%26101%26001%26000%26000%26000%26000%5C%5C000%26111%26101%26001%26000%26000%26000%5C%5C000%26000%26111%26101%26001%26000%26000%5C%5C000%26000%26000%26111%26101%26001%26000%5C%5C000%26000%26000%26000%26111%26101%26001%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for G</summary>

```
$$G=\begin{bmatrix} 111&101&001&000&000&000&000 \\ 000&111&101&001&000&000&000 \\ 000&000&111&101&001&000&000 \\ 000&000&000&111&101&001&000 \\ 000&000&000&000&111&101&001 \end{bmatrix}$$
```

</details>

**LaTeX (TikZ) — (3,1,2) encoder, $g^1=110,\ g^2=100,\ g^3=111$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=10mm,minimum height=9mm},
  xor/.style={draw,circle,inner sep=1.5pt}]
  \node (in) {$D$};
  \node[ff,right=9mm of in] (f1) {FF1};
  \node[ff,right=9mm of f1] (f2) {FF2};
  \draw[->] (in) -- (f1); \draw[->] (f1) -- (f2);
  % tap points
  \coordinate (tin) at ($(in)!0.5!(f1)$);
  \coordinate (t1)  at ($(f1.east)+(4.5mm,0)$);
  \coordinate (t2)  at ($(f2.east)+(6mm,0)$);
  % adder 1: input + FF1  (g1=110)
  \node[xor,below=14mm of f1] (a1) {$+$};
  \draw[->] (tin) |- (a1); \draw[->] (f1.south) -- (a1);
  \draw[->] (a1) -- ++(0,-8mm) node[below]{$C^{(1)}$};
  % adder 2: input only (g2=100)
  \node[xor,below=26mm of in] (a2) {$+$};
  \draw[->] ($(in.south)$) |- (a2);
  \draw[->] (a2) -- ++(0,-8mm) node[below]{$C^{(2)}$};
  % adder 3: input + FF1 + FF2 (g3=111)
  \node[xor,below=14mm of f2] (a3) {$+$};
  \draw[->] (t2) |- (a3); \draw[->] (f2.south) -- (a3);
  \draw[->] ($(in.south)+(2mm,0)$) |- (a3);
  \draw[->] (a3) -- ++(0,-8mm) node[below]{$C^{(3)}$};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Read each generator as a tap pattern to draw the three adders. Interleave the three generators into the first row of $G$, then shift right by $n=3$ each subsequent row for the $L=5$ message bits.

---

## Q5 — (3,1,3) encoder, output for 10110

*(Encoding method same as Q1; here $n=3,\ m=3$.)*

**Restate:** A $(3,1,3)$ code has $g^{(1)}=110$, $g^{(2)}=100$, $g^{(3)}=111$. With registers initially 0 and one bit shifted at a time, find the output for input $10110$.

> **Note on the data.** The generators are written with 3 bits but the code is labelled $m=3$ (so the constraint length is $K=m+1=4$). Padding each generator to length $m+1=4$ with a trailing zero — $g^1=1100,\ g^2=1000,\ g^3=1110$ — the convolution gives the result below. (If instead $m=2$ is intended, the streams are one bit shorter; the bit values are identical up to the trailing flush.)

$n=3,\ k=1$, $L=5$. Using $C_l^j=\sum_i d_{l-i}g_{i+1}^j$ for $j=1,2,3$ with $D=10110$:

$$C^1 = 1\,1\,0\,0\,1\,1\,0,\qquad C^2 = 1\,0\,1\,1\,0\,0\,0,\qquad C^3 = 1\,1\,1\,0\,0\,1\,0$$

Multiplexing:

$$\boxed{C = 111\;101\;011\;010\;100\;101\;000 }$$

> **Steps explained:** Same slide-multiply-XOR as Q1 but with three adders. Compute each stream, then interleave triplets $C_l^1C_l^2C_l^3$ for $l=1\dots L+m$.

---

## Q6 — (3,1,2) codeword for 11101 (time + transform)

*(Time method as Q1; transform method as Q3.)*

**Restate:** A $(3,1,2)$ code has $g^{(1)}=110$, $g^{(2)}=101$, $g^{(3)}=111$. Find the codeword for $d=11101$ by time-domain and transform-domain approaches.

$n=3,\ m=2,\ L=5$, codeword length $=3(5+2)=21$ bits.

**(A) Time domain** ($C_l^j=\sum_i d_{l-i}g_{i+1}^j$):

$$C^1 = 1\,0\,0\,1\,1\,1\,0\quad(g^1=110)$$
$$C^2 = 1\,1\,0\,1\,0\,0\,1\quad(g^2=101)$$
$$C^3 = 1\,0\,1\,0\,0\,1\,1\quad(g^3=111)$$

Multiplexing $C_l^1C_l^2C_l^3$:

$$\boxed{C = 111\;010\;001\;110\;100\;101\;011 }$$

**(B) Transform domain** ($D(x)=1+x+x^2+x^4$):

$$C^1(x)=D(x)(1+x)=1+x^3+x^4+x^5\Rightarrow 1001110$$
$$C^2(x)=D(x)(1+x^2)=1+x+x^3+x^6\Rightarrow 1101001$$
$$C^3(x)=D(x)(1+x+x^2)=1+x^2+x^5+x^6\Rightarrow 1010011$$

Multiplexing gives the same $C = 111\;010\;001\;110\;100\;101\;011$.

> **Steps explained:** Both methods agree. Time domain slides the taps; transform domain multiplies polynomials. Read the three streams, interleave, done.

---

## Q7 — (3,2,1) encoder, codeword (matrix + transform)

**Restate:** A $(3,2,1)$ encoder has $d^{(1)}=101$, $d^{(2)}=110$ with generators $g_1^{(1)}=11,\ g_1^{(2)}=10,\ g_1^{(3)}=11$ (from input 1) and $g_2^{(1)}=01,\ g_2^{(2)}=11,\ g_2^{(3)}=00$ (from input 2). Draw the encoder and find the codeword by matrix and transform methods.

$n=3,\ k=2,\ m=1$, $L=3$ (bits per input stream).

**(A) Matrix method.** Rows $=L+L=6$ (3 per input), columns $=n(L+m)=3(4)=12$. The interleaved message is $D=11\,01\,10$ (pair up $d^1_i d^2_i$).

![matrix](https://latex.codecogs.com/png.image?%5Cdpi%7B130%7D%5Cbg%7Bwhite%7DG%3D%5Cbegin%7Bbmatrix%7D%20111%26101%26000%26000%5C%5C010%26110%26000%26000%5C%5C000%26111%26101%26000%5C%5C000%26010%26110%26000%5C%5C000%26000%26111%26101%5C%5C000%26000%26010%26110%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for G</summary>

```
$$G=\begin{bmatrix} 111&101&000&000 \\ 010&110&000&000 \\ 000&111&101&000 \\ 000&010&110&000 \\ 000&000&111&101 \\ 000&000&010&110 \end{bmatrix}$$
```

</details>

$$C = D\,G = \boxed{101\;001\;001\;101 }$$

**(B) Transform domain.** With $d^1(x)=1+x^2$, $d^2(x)=1+x$ and the generator matrix

![matrix](https://latex.codecogs.com/png.image?%5Cdpi%7B130%7D%5Cbg%7Bwhite%7DG%28x%29%3D%5Cbegin%7Bbmatrix%7D%201%2Bx%20%26%201%20%26%201%2Bx%5C%5Cx%20%26%201%2Bx%20%26%200%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for G(x)</summary>

```
$$G(x)=\begin{bmatrix} 1+x & 1 & 1+x \\ x & 1+x & 0 \end{bmatrix}$$
```

</details>

$$C(x)=[\,d^1(x)\ d^2(x)\,]\,G(x)=[\,1+x^3,\ 0,\ 1+x+x^2+x^3\,]$$

so the three streams are $1001,\ 0000,\ 1111$, and interleaving $C^1_lC^2_lC^3_l$ gives

$$\boxed{C = 101\;001\;001\;101 }$$

**LaTeX (TikZ) — (3,2,1) encoder:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm},
  xor/.style={draw,circle,inner sep=1.5pt}]
  \node (d1) {$d^{(1)}$};
  \node[ff,right=9mm of d1] (f1) {FF1};
  \node[below=12mm of d1] (d2) {$d^{(2)}$};
  \node[ff,right=9mm of d2] (f2) {FF2};
  \draw[->] (d1) -- (f1); \draw[->] (d2) -- (f2);
  \node[xor,right=18mm of f1] (a1) {$+$};   % C1 = d1 + FF1
  \draw[->] ($(d1)!0.5!(f1)$) |- (a1); \draw[->] (f1) -- (a1);
  \draw[->] (a1) -- ++(10mm,0) node[right]{$C^{(1)}$};
  \node[xor,below=8mm of a1] (a2) {$+$};     % C2 = d1 + d2 + FF1
  \draw[->] (f1.south) -- (a2);
  \draw[->] ($(d2)!0.5!(f2)$) -| (a2);
  \draw[->] (a2) -- ++(10mm,0) node[right]{$C^{(2)}$};
  \node[xor,below=16mm of a1] (a3) {$+$};    % C3 = d1 (+ FF? g=11 means d1+FF1? here 1+x)
  \draw[->] ($(d1)!0.5!(f1)+(0,-1mm)$) |- (a3); \draw[->] (f1.south east) -- (a3);
  \draw[->] (a3) -- ++(10mm,0) node[right]{$C^{(3)}$};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Two input streams ⇒ two registers and an interleaved message $D=11\,01\,10$. Build the staircase $G$ (shift right by $n=3$ each message step) and multiply; or use the $k\times n$ polynomial matrix $G(x)$ and multiply the message-polynomial row vector. Both give $101\,001\,001\,101$.

---

## Q8 — (2,1,2) codeword for 10101 (matrix + transform)

*(Matrix as Q2, transform as Q3.)*

**Restate:** A $(2,1,2)$ encoder has $g^{(1)}=101$, $g^{(2)}=111$. Find the code for message $10101$ by matrix and transform methods.

$n=2,\ m=2,\ L=5$, codeword $=2(7)=14$ bits. Interleave generators: $g^1=101,g^2=111\Rightarrow 11,01,11$.

**(A) Matrix method.**

![matrix](https://latex.codecogs.com/png.image?%5Cdpi%7B130%7D%5Cbg%7Bwhite%7DG%3D%5Cbegin%7Bbmatrix%7D%2011%2601%2611%2600%2600%2600%2600%5C%5C00%2611%2601%2611%2600%2600%2600%5C%5C00%2600%2611%2601%2611%2600%2600%5C%5C00%2600%2600%2611%2601%2611%2600%5C%5C00%2600%2600%2600%2611%2601%2611%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for G</summary>

```
$$G=\begin{bmatrix} 11&01&11&00&00&00&00 \\ 00&11&01&11&00&00&00 \\ 00&00&11&01&11&00&00 \\ 00&00&00&11&01&11&00 \\ 00&00&00&00&11&01&11 \end{bmatrix}$$
```

</details>

$$C = D\,G = \boxed{11\;01\;00\;01\;00\;01\;11 }$$

**(B) Transform domain.** $D(x)=1+x^2+x^4$, $g^1(x)=1+x^2$, $g^2(x)=1+x+x^2$:

$$C^1(x)=D(x)(1+x^2)=1+x^6\Rightarrow 1000001$$
$$C^2(x)=D(x)(1+x+x^2)=1+x+x^3+x^5+x^6\Rightarrow 1101011$$

Multiplexing $\Rightarrow C = 11\;01\;00\;01\;00\;01\;11$ (matches).

> **Steps explained:** Build the banded $G$ from the interleaved taps and multiply, or multiply the two generator polynomials by $D(x)$. Interleaving the two 7-bit streams gives the 14-bit codeword.

---

## Q9 — (2,1,4) parameters and output for 10111

*(Encoding as Q1; adds parameter definitions.)*

**Restate:** A $(2,1,4)$ encoder has $g^{(1)}=01111$, $g^{(2)}=01101$. Find the constraint length, code rate, and codeword length; encode $10111$; write the generator polynomials and the output.

$n=2,\ k=1,\ m=4$, $L=5$.

**Step 1 — Parameters.**

- Constraint length $K=m+1=\boxed5$.
- Code rate $=k/n=\boxed{1/2}$.
- Codeword length $=n(L+m)=2(5+4)=\boxed{18\text{ bits}}$ (each stream $L+m=9$ bits).

**Step 2 — Generator polynomials** (bit at position $i\leftrightarrow x^i$):

$$g^{(1)}(x)=x+x^2+x^3+x^4,\qquad g^{(2)}(x)=x+x^2+x^4 .$$

**Step 3 — Encode $D=10111$** ($C_l^j=\sum_i d_{l-i}g_{i+1}^j$, $i=0..4$):

$$C^1 = 0\,1\,1\,0\,1\,1\,1\,0\,1,\qquad C^2 = 0\,1\,1\,1\,1\,0\,0\,1\,1$$

Multiplexing:

$$\boxed{C = 00\;11\;11\;01\;11\;10\;10\;01\;11 }$$

> **Steps explained:** Constraint length is just $m+1$; rate is $k/n$; length is $n(L+m)$. The leading `0` in each generator means the first output pair is `00` (a one-step delay). Slide the taps over the message for the rest.

---

## Q10 — (2,1,3) encoder, G and codeword for 10111

*(Matrix as Q2, transform as Q3.)*

**Restate:** For the encoder in the figure (a $(2,1,3)$ code: top adder from input + FF1 + FF2 + FF3; bottom adder from input + FF2 + FF3), find $G$ and the codeword for $d=10111$ by matrix and transform methods.

From the taps: $g^{(1)}=1111$, $g^{(2)}=1011$. $n=2,\ m=3,\ L=5$, codeword $=2(8)=16$ bits. Interleave: $11,10,11,11$.

**(A) Matrix method.**

![matrix](https://latex.codecogs.com/png.image?%5Cdpi%7B130%7D%5Cbg%7Bwhite%7DG%3D%5Cbegin%7Bbmatrix%7D%2011%2610%2611%2611%2600%2600%2600%2600%5C%5C00%2611%2610%2611%2611%2600%2600%2600%5C%5C00%2600%2611%2610%2611%2611%2600%2600%5C%5C00%2600%2600%2611%2610%2611%2611%2600%5C%5C00%2600%2600%2600%2611%2610%2611%2611%20%5Cend%7Bbmatrix%7D)

<details><summary>LaTeX source for G</summary>

```
$$G=\begin{bmatrix} 11&10&11&11&00&00&00&00 \\ 00&11&10&11&11&00&00&00 \\ 00&00&11&10&11&11&00&00 \\ 00&00&00&11&10&11&11&00 \\ 00&00&00&00&11&10&11&11 \end{bmatrix}$$
```

</details>

$$C = D\,G = \boxed{11\;10\;00\;10\;10\;10\;00\;11 }$$

**(B) Transform domain.** $D(x)=1+x^2+x^3+x^4$, $g^1(x)=1+x+x^2+x^3$, $g^2(x)=1+x^2+x^3$:

$$C^1(x)=D(x)g^1(x)=1+x+x^7\Rightarrow 11000001\ \text{(stream }C^1)$$
$$C^2(x)=D(x)g^2(x)=1+x^4+x^5+x^7\Rightarrow 10001101\ \text{(stream }C^2)$$

Multiplexing $C^1_lC^2_l$ gives $11\;10\;00\;10\;10\;10\;00\;11$ (matches).

**LaTeX (TikZ) — (2,1,3) encoder, $g^1=1111,\ g^2=1011$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm},
  xor/.style={draw,circle,inner sep=1.5pt}]
  \node (in) {$D$};
  \node[ff,right=8mm of in] (f1) {FF1};
  \node[ff,right=7mm of f1] (f2) {FF2};
  \node[ff,right=7mm of f2] (f3) {FF3};
  \draw[->] (in) -- (f1); \draw[->] (f1) -- (f2); \draw[->] (f2) -- (f3);
  % top adder C1 = in + f1 + f2 + f3
  \node[xor,above=12mm of f2] (a1) {$+$};
  \draw[->] ($(in)!0.5!(f1)$) |- (a1);
  \draw[->] (f1.north) -- (a1); \draw[->] (f2.north) -- (a1); \draw[->] (f3.north) |- (a1);
  \draw[->] (a1) -- ++(0,9mm) node[above]{$C^{(1)}$};
  % bottom adder C2 = in + f2 + f3
  \node[xor,below=12mm of f2] (a2) {$+$};
  \draw[->] ($(in)!0.5!(f1)+(0,-1mm)$) |- (a2);
  \draw[->] (f2.south) -- (a2); \draw[->] (f3.south) |- (a2);
  \draw[->] (a2) -- ++(0,-9mm) node[below]{$C^{(2)}$};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Read taps ⇒ $g^1=1111,\ g^2=1011$. Interleave into the first row of $G$, shift right by 2 each row; multiply by $D$. Transform domain confirms via two polynomial products.

---

# UNIT 2 — STATE, TREE, TRELLIS, VITERBI

## Q11 — Define state table, code tree, trellis, Viterbi decoder

**Restate:** Explain (i) State transition table (ii) Code tree (iii) Trellis diagram (iv) Viterbi decoder.

**(i) State transition table.** A table listing, for every present state (the register contents) and every input bit, the **output code bits** and the **next state**. It is the complete description of the encoder as a finite-state machine.

**(ii) Code tree.** A tree that grows one branch level per input bit: at each node the **upper branch = input 0**, **lower branch = input 1**, and each branch is labelled with the output bits. A message traces a unique path from the root; the concatenated branch labels are the codeword. The tree repeats its structure after $m$ levels.

**(iii) Trellis diagram.** A compact, folded version of the code tree: the states are drawn as a column and repeated left-to-right for each time step, with branches joining present states to next states. Because different tree nodes with the same state merge, the trellis has a **fixed number of states** ($2^{m}$) at every depth — this merging is what makes Viterbi decoding efficient.

**(iv) Viterbi decoder.** A maximum-likelihood decoder that walks the trellis, keeping at each state only the **survivor path** (minimum accumulated Hamming distance to the received sequence). At the end it traces back the global survivor to output the most likely transmitted message.

> **Steps explained:** Table = the rule book; tree = every possible path drawn out; trellis = the tree folded so states repeat; Viterbi = the efficient shortest-path search through the trellis.

---

## Q12 — (2,1,2) state table, state diagram, code tree

**Restate:** A $(2,1,2)$ encoder has $g^{(1)}=101$, $g^{(2)}=111$. Construct the state transition table, state diagram, and code tree.

Outputs: $x_1=m+m_2$ (from $g^1=101$), $x_2=m+m_1+m_2$ (from $g^2=111$). State $=(m_1,m_2)$: $a=00,\ b=10,\ c=01,\ d=11$. New state $=(m,m_1)$.

**State transition table.**

| PS | input | output $x_1x_2$ | next state |
|:--:|:--:|:--:|:--:|
| a | 0 | 00 | a |
| a | 1 | 11 | b |
| b | 0 | 01 | c |
| b | 1 | 10 | d |
| c | 0 | 11 | a |
| c | 1 | 00 | b |
| d | 0 | 10 | c |
| d | 1 | 01 | d |

**LaTeX (TikZ) — state diagram:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta,bending}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=3.2cm,on grid,auto,
  every state/.style={draw,minimum size=9mm,font=\small}]
  \node[state] (a) {a};
  \node[state] (b) [right=of a] {b};
  \node[state] (d) [below=of b] {d};
  \node[state] (c) [below=of a] {c};
  \path[->]
    (a) edge[loop left] node{0/00} (a)
    (a) edge[bend left] node{1/11} (b)
    (b) edge[bend left] node{0/01} (c)
    (b) edge[bend left] node{1/10} (d)
    (c) edge[bend left] node{0/11} (a)
    (c) edge node[near start]{1/00} (b)
    (d) edge[bend left] node{0/10} (c)
    (d) edge[loop right] node{1/01} (d);
\end{tikzpicture}
\end{document}
```

**LaTeX (TikZ) — code tree (first 3 levels from state a):**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=18mm, s sep=3mm, font=\footnotesize}
[a
  [a, edge label={node[midway,above,font=\tiny]{0/00}}
     [a, edge label={node[midway,above,font=\tiny]{0/00}}]
     [b, edge label={node[midway,below,font=\tiny]{1/11}}]]
  [b, edge label={node[midway,below,font=\tiny]{1/11}}
     [c, edge label={node[midway,above,font=\tiny]{0/01}}]
     [d, edge label={node[midway,below,font=\tiny]{1/10}}]]
]
\end{forest}
\end{document}
```

> **Steps explained:** Compute $x_1,x_2$ and the next state $(m,m_1)$ for each (state, input). The state diagram draws those 8 transitions as arcs; the code tree branches up=0 / down=1 with the same output labels.

---

## Q13 — (2,1,1) state table, state diagram, code tree for 10111

**Restate:** A $(2,1,1)$ encoder has $C^{(1)}=d_1+d_2$ and $C^{(2)}=d_1$ (where $d_1$ = current bit, $d_2$ = previous bit). Build the state table, state diagram and code tree, and encode $10111$.

$m=1$, so one register; state $=(d_2)$: $a=0,\ b=1$. Outputs $x_1=d_1+d_2$, $x_2=d_1$; next state $=d_1$.

**State transition table.**

| PS | input $d_1$ | output $x_1x_2$ | next state |
|:--:|:--:|:--:|:--:|
| a (0) | 0 | 00 | a |
| a (0) | 1 | 11 | b |
| b (1) | 0 | 10 | a |
| b (1) | 1 | 01 | b |

**Encode $10111$** (start at a): a→b `11`, b→a `10`, a→b `11`, b→b `01`, b→b `01`:

$$\boxed{C = 11\;10\;11\;01\;01 }$$

**LaTeX (TikZ) — state diagram:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta,bending}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=3.4cm,on grid,auto,
  every state/.style={draw,minimum size=9mm,font=\small}]
  \node[state] (a) {a};
  \node[state] (b) [right=of a] {b};
  \path[->]
    (a) edge[loop left] node{0/00} (a)
    (a) edge[bend left] node{1/11} (b)
    (b) edge[bend left] node{0/10} (a)
    (b) edge[loop right] node{1/01} (b);
\end{tikzpicture}
\end{document}
```

**LaTeX (TikZ) — code tree for 10111:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=16mm, s sep=3mm, font=\footnotesize}
[a
  [a, edge label={node[midway,above,font=\tiny]{0/00}}]
  [b, edge label={node[midway,below,font=\tiny]{1/11}}
     [a, edge label={node[midway,above,font=\tiny]{0/10}}
        [b, edge label={node[midway,below,font=\tiny]{1/11}}
           [b, edge label={node[midway,above,font=\tiny]{1/01}}
              [b, edge label={node[midway,below,font=\tiny]{1/01}}]]]]
]
\end{forest}
\end{document}
```

> **Steps explained:** One memory bit ⇒ 2 states. Output rule gives $x_1x_2$ and next state $=d_1$. Following $10111$ through the table produces $11\,10\,11\,01\,01$; the tree shows that exact path.

---

## Q14 — (2,1,2) encoder: tables and codeword for 10011

**Restate:** For the $(2,1,2)$ encoder in the figure (top adder $x_1=m+m_1+m_2$, bottom adder $x_2=m_1+m_2$), build the state table, state diagram and code tree, and find the codeword for input $10011$ using the state diagram.

State $=(m_1,m_2)$: $a=00,\ b=10,\ c=01,\ d=11$; next state $=(m,m_1)$.

**State transition table.**

| PS | input | output $x_1x_2$ | next state |
|:--:|:--:|:--:|:--:|
| a | 0 | 00 | a |
| a | 1 | 10 | b |
| b | 0 | 11 | c |
| b | 1 | 01 | d |
| c | 0 | 11 | a |
| c | 1 | 01 | b |
| d | 0 | 00 | c |
| d | 1 | 10 | d |

**Encode $10011$** (start a): a→b `10`, b→c `11`, c→a `11`, a→b `10`, b→d `01`:

$$\boxed{C = 10\;11\;11\;10\;01 }$$

**LaTeX (TikZ) — state diagram:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta,bending}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=3.2cm,on grid,auto,
  every state/.style={draw,minimum size=9mm,font=\small}]
  \node[state] (a) {a};
  \node[state] (b) [right=of a] {b};
  \node[state] (d) [below=of b] {d};
  \node[state] (c) [below=of a] {c};
  \path[->]
    (a) edge[loop left] node{0/00} (a)
    (a) edge[bend left] node{1/10} (b)
    (b) edge[bend left] node{0/11} (c)
    (b) edge[bend left] node{1/01} (d)
    (c) edge[bend left] node{0/11} (a)
    (c) edge node[near start]{1/01} (b)
    (d) edge[bend left] node{0/00} (c)
    (d) edge[loop right] node{1/10} (d);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Same $(2,1,2)$ machine, different tap pattern. Build the 8-row table, draw the arcs, and walk the path $a\!\to\!b\!\to\!c\!\to\!a\!\to\!b\!\to\!d$ for $10011$, reading branch outputs $10\,11\,11\,10\,01$.

---

## Q15 — (3,1,2) state table, state diagram, code tree

*(State-machine method as Q12, with 3 outputs.)*

**Restate:** A $(3,1,2)$ code has $g^{(1)}=110$, $g^{(2)}=101$, $g^{(3)}=111$. Write the state transition table, draw the state diagram, write the code tree.

Outputs: $x_1=m+m_1$, $x_2=m+m_2$, $x_3=m+m_1+m_2$. State $=(m_1,m_2)$; next $=(m,m_1)$.

**State transition table.**

| PS | input | output $x_1x_2x_3$ | next state |
|:--:|:--:|:--:|:--:|
| a | 0 | 000 | a |
| a | 1 | 111 | b |
| b | 0 | 100 | c |
| b | 1 | 011 | d |
| c | 0 | 010 | a |
| c | 1 | 101 | b |
| d | 0 | 110 | c |
| d | 1 | 001 | d |

**LaTeX (TikZ) — state diagram:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta,bending}
\begin{document}
\begin{tikzpicture}[>=Stealth,node distance=3.4cm,on grid,auto,
  every state/.style={draw,minimum size=9mm,font=\small}]
  \node[state] (a) {a};
  \node[state] (b) [right=of a] {b};
  \node[state] (d) [below=of b] {d};
  \node[state] (c) [below=of a] {c};
  \path[->]
    (a) edge[loop left] node{0/000} (a)
    (a) edge[bend left] node{1/111} (b)
    (b) edge[bend left] node{0/100} (c)
    (b) edge[bend left] node{1/011} (d)
    (c) edge[bend left] node{0/010} (a)
    (c) edge node[near start]{1/101} (b)
    (d) edge[bend left] node{0/110} (c)
    (d) edge[loop right] node{1/001} (d);
\end{tikzpicture}
\end{document}
```

**LaTeX (TikZ) — code tree (first 3 levels):**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=20mm, s sep=3mm, font=\footnotesize}
[a
  [a, edge label={node[midway,above,font=\tiny]{0/000}}
     [a, edge label={node[midway,above,font=\tiny]{0/000}}]
     [b, edge label={node[midway,below,font=\tiny]{1/111}}]]
  [b, edge label={node[midway,below,font=\tiny]{1/111}}
     [c, edge label={node[midway,above,font=\tiny]{0/100}}]
     [d, edge label={node[midway,below,font=\tiny]{1/011}}]]
]
\end{forest}
\end{document}
```

> **Steps explained:** Three adders give 3-bit branch labels. The state/next-state logic is identical to any $(2,1,2)$-shaped machine ($2^2=4$ states); only the outputs differ.

---

## Q16 — (2,1,2) state table and trellis for 10111

**Restate:** A $(2,1,2)$ code has $g^{(1)}=111$, $g^{(2)}=101$. Write the state transition table and draw the trellis for message $10111$.

Outputs: $x_1=m+m_1+m_2$, $x_2=m+m_2$. State $=(m_1,m_2)$; next $=(m,m_1)$.

**State transition table.**

| PS | input | output $x_1x_2$ | next state |
|:--:|:--:|:--:|:--:|
| a | 0 | 00 | a |
| a | 1 | 11 | b |
| b | 0 | 10 | c |
| b | 1 | 01 | d |
| c | 0 | 11 | a |
| c | 1 | 00 | b |
| d | 0 | 01 | c |
| d | 1 | 10 | d |

**Encode $10111$** (start a): a→b `11`, b→c `10`, c→b `00`, b→d `01`, d→d `10`:

$$\boxed{C = 11\;10\;00\;01\;10 }$$

**LaTeX (TikZ) — trellis for 10111 (5 stages):**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\scriptsize, x=24mm, y=11mm]
  % states a,b,c,d at y=0,-1,-2,-3 ; stages t=0..5
  \foreach \s/\y in {a/0,b/-1,c/-2,d/-3}{
    \foreach \t in {0,...,5}{ \node[circle,fill=black,inner sep=1pt] (\s\t) at (\t,\y){};
       \ifnum\t=0 \node[left] at (\s0){\s}\fi }}
  % survivor (bold) path for message 10111: a->b->c->b->d->d
  \draw[very thick,->] (a0)--(b1) node[midway,above,sloped]{11};
  \draw[very thick,->] (b1)--(c2) node[midway,above,sloped]{10};
  \draw[very thick,->] (c2)--(b3) node[midway,above,sloped]{00};
  \draw[very thick,->] (b3)--(d4) node[midway,above,sloped]{01};
  \draw[very thick,->] (d4)--(d5) node[midway,above,sloped]{10};
  % faint full trellis branches (input 0 solid-thin, input 1 dashed)
  \foreach \t/\tn in {0/1,1/2,2/3,3/4,4/5}{
    \draw[gray!40] (a\t)--(a\tn); \draw[gray!40,dashed] (a\t)--(b\tn);
    \draw[gray!40] (b\t)--(c\tn); \draw[gray!40,dashed] (b\t)--(d\tn);
    \draw[gray!40] (c\t)--(a\tn); \draw[gray!40,dashed] (c\t)--(b\tn);
    \draw[gray!40] (d\t)--(c\tn); \draw[gray!40,dashed] (d\t)--(d\tn);}
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Build the 8-row table. The trellis repeats the four states each stage; solid = input 0, dashed = input 1. The bold path $a\!\to\!b\!\to\!c\!\to\!b\!\to\!d\!\to\!d$ traces $10111$ and reads $11\,10\,00\,01\,10$.

---

## Q17 — (2,1,2) state table and state diagram

*(Same encoder as Q12: $g^{(1)}=101$, $g^{(2)}=111$.)*

**Restate:** A $(2,1,2)$ encoder has $g^{(1)}=101$, $g^{(2)}=111$. Construct the state table and state diagram.

This is exactly the machine of [Q12](#q12--212-state-table-state-diagram-code-tree). Repeating the table for convenience:

| PS | input | output $x_1x_2$ | next state |
|:--:|:--:|:--:|:--:|
| a | 0 | 00 | a |
| a | 1 | 11 | b |
| b | 0 | 01 | c |
| b | 1 | 10 | d |
| c | 0 | 11 | a |
| c | 1 | 00 | b |
| d | 0 | 10 | c |
| d | 1 | 01 | d |

The state diagram is the same TikZ figure as Q12 (states $a,b,c,d$ with the eight arcs above).

> **Steps explained:** Identical generators to Q12 ⇒ identical state table and diagram. (This is the encoder also used for the Viterbi question Q19.)

---

## Q18 — Steps of the Viterbi algorithm

**Restate:** Explain the steps involved in the Viterbi algorithm.

**Algorithm (maximum-likelihood trellis decoding).**

1. **Initialise.** Start at the known initial state (usually all-zero, state $a$) with path metric $0$; all other states $\infty$ (unreachable).
2. **Branch metrics.** At each time step, for every trellis branch compute the **Hamming distance** between the branch's expected output and the received $n$-bit group.
3. **Add–Compare–Select (ACS).** For each next-state node, add each incoming branch metric to its source's path metric, **compare** the candidates, and **select** the smallest as the survivor (store its predecessor).
4. **Discard** the non-survivors. Exactly one survivor path enters each state.
5. **Repeat** steps 2–4 for the whole received sequence.
6. **Traceback.** At the end pick the state with the smallest path metric and trace its survivor backwards to recover the input bits (and hence the most likely transmitted codeword).

> **Steps explained:** It is a shortest-path search on the trellis where "distance" = disagreement with what was received. Keeping only one best path per state at each step keeps the work linear in the message length while still finding the global maximum-likelihood sequence.

---

## Q19 — Viterbi decoding of R = 1100000111

**Restate:** A $(2,1,2)$ encoder has $g^{(1)}=101$, $g^{(2)}=111$. The received sequence is $R=11\,00\,00\,01\,11$. Decode it with the Viterbi algorithm.

Use the Q12/Q17 trellis (branch outputs: $a\!\to\!a$ 00, $a\!\to\!b$ 11, $b\!\to\!c$ 01, $b\!\to\!d$ 10, $c\!\to\!a$ 11, $c\!\to\!b$ 00, $d\!\to\!c$ 10, $d\!\to\!d$ 01). Received groups: $r=11,\,00,\,00,\,01,\,11$.

**Add–Compare–Select, stage by stage** (path metric = cumulative Hamming distance; only survivors kept):

| after group | survivor metrics $(a,b,c,d)$ | note |
|:--:|:--:|:--:|
| t1: $11$ | a:2, b:0, –, – | from a only |
| t2: $00$ | a:2, b:3, c:1, d:1 | |
| t3: $00$ | a:2, b:2, c:3, d:2 | |
| t4: $01$ | a:4, b:2, c:2, d:2 | |
| t5: $11$ | **a:1**, b:4, c:3, d:3 | best = state a, metric 1 |

**Traceback** from the minimum-metric state $a$ (metric $1$) gives the input path

$$\boxed{\hat m = 1\,0\,1\,0\,0 }$$

**Check.** Encoding $\hat m=10100$ gives $11\,01\,00\,01\,11$, which differs from $R=11\,00\,00\,01\,11$ in exactly **one** bit (the 4th) — so one error was corrected, consistent with the final metric of $1$.

**LaTeX (TikZ) — decoding trellis with survivor path:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\scriptsize, x=26mm, y=11mm]
  \foreach \s/\y in {a/0,b/-1,c/-2,d/-3}{
    \foreach \t in {0,...,5}{ \node[circle,fill=black,inner sep=1pt] (\s\t) at (\t,\y){};
       \ifnum\t=0 \node[left] at (\s0){\s}\fi }}
  % received groups along the top
  \foreach \t/\r in {1/11,2/00,3/00,4/01,5/11}{ \node[above] at (\t-0.5,0.6){\r}; }
  % faint full trellis
  \foreach \t/\tn in {0/1,1/2,2/3,3/4,4/5}{
    \draw[gray!35] (a\t)--(a\tn); \draw[gray!35,dashed] (a\t)--(b\tn);
    \draw[gray!35] (b\t)--(c\tn); \draw[gray!35,dashed] (b\t)--(d\tn);
    \draw[gray!35] (c\t)--(a\tn); \draw[gray!35,dashed] (c\t)--(b\tn);
    \draw[gray!35] (d\t)--(c\tn); \draw[gray!35,dashed] (d\t)--(d\tn);}
  % survivor path a->b->c->a->b->a  (message 1 0 1 0 0)
  \draw[very thick,->] (a0)--(b1);
  \draw[very thick,->] (b1)--(c2);
  \draw[very thick,->] (c2)--(a3);
  \draw[very thick,->] (a3)--(b4);
  \draw[very thick,->] (b4)--(a5);
  \node[below=6mm] at (2.5,-3){decoded message $=10100$, final metric $=1$};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** For each received pair, score every branch by Hamming distance, add to the running metric, and keep the best path into each state (ACS). After the last group the smallest metric (1, at state $a$) wins; tracing it back gives $10100$, which re-encodes to a codeword one bit from $R$.

---

## Q20 — BCH codes and Golay codes

**Restate:** Explain BCH codes and Golay codes.

**BCH codes (Bose–Chaudhuri–Hocquenghem).**

- A large, powerful class of **cyclic block codes** that can be designed for **any** desired error-correcting capability $t$.
- For any integers $m\ge 3$ and $t<2^{m-1}$ there is a binary BCH code with block length $n=2^m-1$, parity bits $n-k\le mt$, and minimum distance $d_{\min}\ge 2t+1$ (so it corrects up to $t$ errors).
- The generator polynomial $g(x)$ is the **LCM** of the minimal polynomials of $\alpha,\alpha^2,\dots,\alpha^{2t}$, where $\alpha$ is a primitive element of $GF(2^m)$.
- They are decoded algebraically (e.g. Berlekamp–Massey) and are widely used because the code parameters can be tuned precisely.

**Golay codes.**

- The binary **(23, 12) Golay code** is a perfect cyclic code with $d_{\min}=7$, correcting up to **3 errors**, generated by $g(x)=x^{11}+x^{10}+x^6+x^5+x^4+x^2+1$ (or its reciprocal).
- Adding one overall parity bit gives the **extended (24, 12) Golay code** with $d_{\min}=8$ — convenient because the rate is exactly $1/2$.
- It is one of only a few **perfect** codes (the Hamming spheres around codewords exactly fill the space), and is famous for its strong error correction and rich algebraic structure (used in deep-space communication, e.g. Voyager).

> **Steps explained:** BCH = a tunable family of cyclic codes (pick $t$, get the matching $g(x)$). Golay = a specific, exceptional perfect code (23,12) correcting 3 errors, often used in its extended (24,12) form.

---

# 📋 Formulas

### 1. Code parameters
$$\text{rate}=\frac{k}{n},\qquad \text{constraint length }K=m+1,\qquad \text{codeword length}=n(L+m)$$
- $n$ = outputs, $k$ = input bits/step, $m$ = register stages, $L$ = message length.
- **Used in:** Q1–Q10, Q9 (explicitly), all encoding questions.

### 2. Time-domain convolution
$$C_l^{\,j}=\sum_{i=0}^{m} d_{l-i}\,g_{i+1}^{\,j}\pmod 2,\qquad l=1,\dots,L+m$$
- $g^j$ = generator (impulse response) of adder $j$; $d_{l-i}=0$ outside $1\le l-i\le L$.
- **Used in:** Q1, Q5, Q6, Q9.

### 3. Matrix encoding
$$C=D\,G$$
- $G$ = interleaved generators, each row shifted right by $n$; size $L\times n(L+m)$ (or $kL$ rows for $k>1$).
- **Used in:** Q2, Q4, Q7, Q8, Q10.

### 4. Transform-domain encoding
$$C^j(x)=D(x)\,g^j(x)\pmod 2$$
- Polynomials in $x$; bit at position $i\leftrightarrow x^i$.
- **Used in:** Q3, Q6, Q7, Q8, Q9, Q10.

### 5. Multiplexing
$$C=C_1^1C_1^2\cdots C_1^n,\ C_2^1C_2^2\cdots C_2^n,\ \dots$$
- Interleave the $n$ output streams bit-by-bit.
- **Used in:** every encoding question.

### 6. State / next-state (rate-1/n, $m$ stages)
$$\text{state}=(m_1,\dots,m_m),\qquad \text{next state}=(m,m_1,\dots,m_{m-1})$$
- $m$ = current input bit; $m_i$ = stored bits.
- **Used in:** Q12, Q13, Q14, Q15, Q16, Q17.

### 7. Branch / path metric (Viterbi)
$$\text{branch metric}=d_H(\text{expected output},\ \text{received group}),\quad \text{path metric}=\sum \text{branch metrics}$$
- $d_H$ = Hamming distance. Survivor = minimum path metric per state (ACS).
- **Used in:** Q18, Q19.

### 8. BCH parameters
$$n=2^m-1,\quad n-k\le mt,\quad d_{\min}\ge 2t+1$$
- Corrects up to $t$ errors. Golay (23,12): $d_{\min}=7$, corrects 3; extended (24,12): $d_{\min}=8$.
- **Used in:** Q20.

### 9. Mod-2 (Ex-OR) addition
$$0+0=0,\quad 1+0=1,\quad 0+1=1,\quad 1+1=0$$
- Underlies every "+" in this module.
- **Used in:** all questions.

---

*Module 5 — Convolution Codes. All arithmetic is over GF(2). Generator matrices are rendered as images for reliable GitHub display (LaTeX source in the collapsible blocks). Every TikZ block is a standalone document — paste into Overleaf and compile with pdfLaTeX.*
