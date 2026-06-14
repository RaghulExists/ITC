# Module 4 — Error Control Coding · Question Bank Solutions

> Complete, exam-ready solutions to all 20 questions (Unit 1: Linear Block Codes, Unit 2: Cyclic Codes), faithful to the class notes.
> Each question is restated, marked **(Same method as Qx)** when it reuses a method, solved in ordered steps, and followed by a short **Steps explained**.
> Every diagram has compilable **LaTeX (TikZ)**. A full **Formulas** sheet is at the end.

> **GitHub rendering note:** matrices are written one per math block in multi-line form (the form GitHub renders reliably). All XOR / modulo-2 additions use `+` meaning **mod-2 (Ex-OR)**: `0+0=0, 1+0=1, 0+1=1, 1+1=0`.

---

## Table of Contents

**Unit 1 — Linear Block Codes**
- [Q1 — Block diagram of a system with error control coding](#q1--block-diagram-of-a-system-with-error-control-coding)
- [Q2 — Methods of controlling errors; types of errors and codes](#q2--methods-of-controlling-errors-types-of-errors-and-codes)
- [Q3 — Generalized encoder for a linear block code](#q3--generalized-encoder-for-a-linear-block-code)
- [Q4 — Encoder and decoder circuit for a (6,3) code](#q4--encoder-and-decoder-circuit-for-a-63-code)
- [Q5 — (8,4) code: G, H, code vectors, minimum weight](#q5--84-code-g-h-code-vectors-minimum-weight)
- [Q6 — (6,3) code from given G: code vectors, weights, distances](#q6--63-code-from-given-g-code-vectors-weights-distances)
- [Q7 — Prove GHᵀ = 0 and CHᵀ = 0](#q7--prove-ghᵀ--0-and-chᵀ--0)
- [Q8 — (7,4) code: G, H, syndrome circuit](#q8--74-code-g-h-syndrome-circuit)
- [Q9 — (6,3) code: detect and correct a single error](#q9--63-code-detect-and-correct-a-single-error)
- [Q10 — Code from syndrome equations: G, H, circuits, decoding](#q10--code-from-syndrome-equations-g-h-circuits-decoding)

**Unit 2 — Cyclic Codes**
- [Q11 — Code vectors in systematic form](#q11--code-vectors-in-systematic-form)
- [Q12 — Encoding an (n,k) cyclic code with an (n−k)-bit register](#q12--encoding-an-nk-cyclic-code-with-an-nk-bit-register)
- [Q13 — (7,3) cyclic encoder, message 011](#q13--73-cyclic-encoder-message-011)
- [Q14 — (7,4) cyclic encoder, message 1011](#q14--74-cyclic-encoder-message-1011)
- [Q15 — Syndrome calculation procedure and circuit](#q15--syndrome-calculation-procedure-and-circuit)
- [Q16 — (15,?) code from a factored g(x): G and H](#q16--15-code-from-a-factored-gx-g-and-h)
- [Q17 — (7,4) cyclic: syndrome circuit and error correction](#q17--74-cyclic-syndrome-circuit-and-error-correction)
- [Q18 — (15,5) cyclic: encoder/syndrome, systematic codeword, codeword test](#q18--155-cyclic-encodersyndrome-systematic-codeword-codeword-test)
- [Q19 — (15,11) cyclic encoder, message 10010110111](#q19--1511-cyclic-encoder-message-10010110111)
- [Q20 — (15,7) cyclic: encoder/syndrome, systematic codeword, error syndrome](#q20--157-cyclic-encodersyndrome-systematic-codeword-error-syndrome)

- [📋 Formulas](#-formulas)

---

# UNIT 1 — LINEAR BLOCK CODES

## Q1 — Block diagram of a system with error control coding

**Restate:** Explain the block diagram of a digital communication system that uses error control coding.

**Answer.** The message bit stream $\{b_k\}$ at rate $r_b$ enters the **channel encoder**, which groups the bits into blocks of $k$ and appends $(n-k)$ check bits to form $n$-bit codewords $\{d_k\}$ at the higher rate

$$r_c = r_b\left(\frac{n}{k}\right)\ \text{bits/sec}.$$

The codewords pass through the **modulator → noisy channel → demodulator** (together the "modem"), which introduces a **channel bit-error probability** $q_c = P\{d_k \neq \hat d_k\}$. The **channel decoder** uses the structure added by the redundant bits to detect/correct errors and outputs the recovered message $\{\hat b_k\}$ with **message bit-error probability** $P_e = P\{b_k \neq \hat b_k\}$.

The redundant (check) bits carry **no information** but let the decoder reduce the overall error probability $P_e$ well below $q_c$.

**LaTeX (TikZ) — block diagram:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  blk/.style={draw, minimum width=22mm, minimum height=11mm, align=center}]
  \node[blk] (enc) {Channel\\Encoder};
  \node[blk, right=14mm of enc] (mod) {Modulator};
  \node[blk, right=12mm of mod] (ch) {Noisy\\Channel};
  \node[blk, right=12mm of ch] (dem) {Demodulator};
  \node[blk, right=12mm of dem] (dec) {Channel\\Decoder};
  \draw[<-] (enc.west) -- ++(-14mm,0) node[left]{$\{b_k\},\ r_b$};
  \draw[->] (enc) -- node[above]{$\{d_k\},\ r_c$} (mod);
  \draw[->] (mod) -- (ch);
  \draw[->] (ch) -- (dem);
  \draw[->] (dem) -- node[above]{$q_c$} (dec);
  \draw[->] (dec.east) -- ++(14mm,0) node[right]{$\{\hat b_k\},\ r_b$};
  \node[below=1mm of ch, align=center, font=\scriptsize] {noise};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Follow the data left to right: encoder adds redundancy (rate rises from $r_b$ to $r_c$); modem + channel add errors at rate $q_c$; decoder removes errors and restores the message at rate $r_b$ with a much smaller error probability $P_e$.

---

## Q2 — Methods of controlling errors; types of errors and codes

**Restate:** What are the methods of controlling errors? Explain the types of errors and the types of codes.

**Methods of controlling errors**

1. **Forward Error Correction (FEC):** the receiver/decoder *corrects* the noise-induced errors on its own using the redundancy in the codeword. No return path is needed.
2. **Error Detection with retransmission (ARQ):** the decoder only *detects* errors. If the received block is not a valid codeword, it is discarded and a **retransmission** is requested over a reverse channel until a correct block arrives.

**Types of errors**

1. **Random errors** — caused by **white Gaussian noise** (thermal/shot noise, antenna pickup). Errors hit bits independently.
2. **Burst errors** — caused by **impulse noise** (lightning, switching transients, man-made noise). A single disturbance corrupts **several adjacent** bits.

**Types of codes**

1. **Linear block codes** — fixed $k$-bit blocks mapped to $n$-bit codewords; sum of any two codewords is a codeword.
2. **Cyclic codes** — a subclass of linear block codes; any cyclic shift of a codeword is also a codeword (easy to build with shift registers).
3. **Convolution codes** — output depends on the current and previous message bits (covered in Module 5).

> **Steps explained:** Two control strategies (correct vs. detect-and-resend), two noise-driven error patterns (isolated vs. clustered), and three code families used in practice.

---

## Q3 — Generalized encoder for a linear block code

**Restate:** Draw the generalized encoder diagram for a linear block code.

**Answer.** For a systematic $(n,k)$ code, the $k$ message bits $d_1,\dots,d_k$ pass straight through to the output, and each of the $(n-k)$ check bits is the **mod-2 sum** of selected message bits (defined by the parity matrix $P$):

$$C_{k+j} = P_{1j}d_1 + P_{2j}d_2 + \dots + P_{kj}d_k,\qquad j = 1,\dots,(n-k).$$

So the encoder is just the $k$ message lines plus $(n-k)$ XOR (mod-2 adder) trees.

**LaTeX (TikZ) — generalized systematic encoder:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  xor/.style={draw,circle,inner sep=1.5pt}]
  % message bus
  \foreach \i in {1,2,3} \node (d\i) at (0,-\i*0.9) {$d_{\i}$};
  \node at (0,-3.6) {$\vdots$};
  \node (dk) at (0,-4.5) {$d_k$};
  % straight-through message outputs
  \foreach \i/\c in {1/1,2/2,3/3} \draw[->] (d\i) -- ++(7,0) node[right]{$C_{\c}=d_{\i}$};
  \draw[->] (dk) -- ++(7,0) node[right]{$C_k=d_k$};
  % check-bit adders
  \node[xor] (p1) at (4,-5.6) {$+$};
  \node[xor] (p2) at (5,-6.4) {$+$};
  \draw[->] (p1) -- ++(3,0) node[right]{$C_{k+1}$};
  \draw[->] (p2) -- ++(2,0) node[right]{$C_{n}$};
  \node at (4.5,-7.1) {\scriptsize (each $C_{k+j}=\sum_i P_{ij}d_i$, mod-2)};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Message bits are copied to the output (systematic part); every check bit is one XOR tree summing the message bits picked out by a column of $P$.

---

## Q4 — Encoder and decoder circuit for a (6,3) code

**Restate:** Draw the encoder and decoder (syndrome) circuit for a $(6,3)$ linear block code.

Use the standard $(6,3)$ code with parity matrix

$$P=\begin{bmatrix} 1 & 0 & 1 \\ 0 & 1 & 1 \\ 1 & 1 & 0 \end{bmatrix}$$

so that $G=[I_3\mid P]$ gives the check bits

$$C_4 = d_1+d_3,\qquad C_5 = d_2+d_3,\qquad C_6 = d_1+d_2 .$$

The decoder forms the syndrome $S=RH^T$ with $H=[P^T\mid I_3]$:

$$S_1 = r_1+r_3+r_4,\qquad S_2 = r_2+r_3+r_5,\qquad S_3 = r_1+r_2+r_6 .$$

**LaTeX (TikZ) — encoder:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small, xor/.style={draw,circle,inner sep=1.5pt}]
  \node (d1) at (0,0) {$d_1$};
  \node (d2) at (0,-1) {$d_2$};
  \node (d3) at (0,-2) {$d_3$};
  \draw[->] (d1) -- ++(6,0) node[right]{$C_1=d_1$};
  \draw[->] (d2) -- ++(6,0) node[right]{$C_2=d_2$};
  \draw[->] (d3) -- ++(6,0) node[right]{$C_3=d_3$};
  \node[xor] (a4) at (3,-3) {$+$}; \draw[->] (a4)--++(3,0) node[right]{$C_4=d_1{+}d_3$};
  \node[xor] (a5) at (3,-4) {$+$}; \draw[->] (a5)--++(3,0) node[right]{$C_5=d_2{+}d_3$};
  \node[xor] (a6) at (3,-5) {$+$}; \draw[->] (a6)--++(3,0) node[right]{$C_6=d_1{+}d_2$};
\end{tikzpicture}
\end{document}
```

**LaTeX (TikZ) — decoder (syndrome) circuit:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small, xor/.style={draw,circle,inner sep=1.5pt}]
  \foreach \i in {1,...,6}\node (r\i) at (\i*1.4,0) {$r_{\i}$};
  \node[xor] (s1) at (3,-2) {$+$}; \draw[->](s1)--++(0,-1) node[below]{$S_1=r_1{+}r_3{+}r_4$};
  \node[xor] (s2) at (5,-2.8){$+$}; \draw[->](s2)--++(0,-1) node[below]{$S_2=r_2{+}r_3{+}r_5$};
  \node[xor] (s3) at (7,-3.6){$+$}; \draw[->](s3)--++(0,-1) node[below]{$S_3=r_1{+}r_2{+}r_6$};
  \draw[->](r1)--(s1); \draw[->](r3)--(s1); \draw[->](r4)--(s1);
  \draw[->](r2)--(s2); \draw[->](r3)--(s2); \draw[->](r5)--(s2);
  \draw[->](r1)--(s3); \draw[->](r2)--(s3); \draw[->](r6)--(s3);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** The encoder copies $d_1,d_2,d_3$ to the output and builds three check bits with three XOR gates from $P$. The decoder XORs the received bits exactly as the rows of $H$ dictate to produce the 3-bit syndrome $S$.

---

## Q5 — (8,4) code: G, H, code vectors, minimum weight

**Restate:** A systematic $(8,4)$ code has check bits $C_5=d_2+d_3+d_4$, $C_6=d_1+d_2+d_4$, $C_7=d_2+d_3+d_4$, $C_8=d_1+d_3+d_4$. (i) Find $G$ and $H$. (ii) List all code vectors. (iii) Find the minimum weight.

**Step 1 — Parity matrix $P$ (rows = $d_1..d_4$, columns = $C_5..C_8$).** Read each check equation as a column:

$$P=\begin{bmatrix} 0 & 1 & 0 & 1 \\ 1 & 1 & 1 & 0 \\ 1 & 0 & 1 & 1 \\ 1 & 1 & 1 & 1 \end{bmatrix}$$

**Step 2 — Generator matrix $G=[I_4\mid P]$:**

$$G=\begin{bmatrix} 1 & 0 & 0 & 0 & 0 & 1 & 0 & 1 \\ 0 & 1 & 0 & 0 & 1 & 1 & 1 & 0 \\ 0 & 0 & 1 & 0 & 1 & 0 & 1 & 1 \\ 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \end{bmatrix}$$

**Step 3 — Parity-check matrix $H=[P^T\mid I_4]$:**

$$H=\begin{bmatrix} 0 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\ 1 & 1 & 0 & 1 & 0 & 1 & 0 & 0 \\ 0 & 1 & 1 & 1 & 0 & 0 & 1 & 0 \\ 1 & 0 & 1 & 1 & 0 & 0 & 0 & 1 \end{bmatrix}$$

**Step 4 — Code vectors** $C=[\,d_1\,d_2\,d_3\,d_4\;C_5\,C_6\,C_7\,C_8\,]$:

| $d_1d_2d_3d_4$ | $C_5C_6C_7C_8$ | weight | $d_1d_2d_3d_4$ | $C_5C_6C_7C_8$ | weight |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 0000 | 0000 | 0 | 1000 | 0101 | 3 |
| 0001 | 1111 | 5 | 1001 | 1010 | 4 |
| 0010 | 1011 | 4 | 1010 | 1110 | 5 |
| 0011 | 0100 | 3 | 1011 | 0001 | 4 |
| 0100 | 1110 | 4 | 1100 | 1011 | 5 |
| 0101 | 0001 | 3 | 1101 | 0100 | 4 |
| 0110 | 0101 | 4 | 1110 | 0000 | 3 |
| 0111 | 1010 | 5 | 1111 | 1111 | 8 |

**Step 5 — Minimum weight.** The smallest non-zero weight is

$$\boxed{W_{t,\min}=d_{\min}=3.}$$

So this code detects $d_{\min}-1=2$ errors and corrects $\tfrac{d_{\min}-1}{2}=1$ error.

> **Note:** As written, $C_5$ and $C_7$ are identical equations ($d_2+d_3+d_4$), so columns 1 and 3 of $P$ are equal — likely a typo for $C_7=d_1+d_3+d_4$. Solved exactly as given; the minimum weight is still 3.

> **Steps explained:** Turn each check-bit equation into a column of $P$; stack with $I_4$ to get $G$; transpose $P$ and append $I_4$ to get $H$. Encode all 16 messages with the check equations, count 1s, and take the smallest non-zero count as $d_{\min}$.

---

## Q6 — (6,3) code from given G: code vectors, weights, distances

**Restate:** A $(6,3)$ code has

$$G=\begin{bmatrix} 1 & 0 & 0 & 1 & 0 & 1 \\ 0 & 1 & 0 & 1 & 1 & 0 \\ 0 & 0 & 1 & 0 & 1 & 1 \end{bmatrix}$$

(i) all code vectors, (ii) all Hamming weights and distances, (iii) minimum Hamming distance.

**Step 1 — Read the check equations** from $G=[I_3\mid P]$:

$$C_4=d_1+d_2,\qquad C_5=d_2+d_3,\qquad C_6=d_1+d_3 .$$

**Step 2 — Code vectors** $C=[\,d_1d_2d_3\;C_4C_5C_6\,]$:

| $d_1d_2d_3$ | code vector | weight |
|:---:|:---:|:---:|
| 000 | 000 000 | 0 |
| 001 | 001 011 | 3 |
| 010 | 010 110 | 3 |
| 011 | 011 101 | 4 |
| 100 | 100 101 | 3 |
| 101 | 101 110 | 4 |
| 110 | 110 011 | 4 |
| 111 | 111 000 | 3 |

**Step 3 — Hamming weights and distances.** The weights of the non-zero codewords are $\{3,3,4,3,4,4,3\}$. For a **linear** code the set of pairwise Hamming distances equals the set of non-zero codeword weights, so the distances present are $3$ and $4$.

**Step 4 — Minimum Hamming distance:**

$$\boxed{d_{\min}=W_{t,\min}=3.}$$

> **Steps explained:** Extract the check rules from the $P$ part of $G$, encode all 8 messages, and count 1s for weights. Because the code is linear, $d_{\min}$ equals the minimum non-zero weight — no need to compare every pair.

---

## Q7 — Prove GHᵀ = 0 and CHᵀ = 0

**Restate:** Prove that $GH^T=0$ and $CH^T=0$ for a systematic linear block code.

**Setup.** $G=[\,I_k\mid P\,]$ and $H=[\,P^T\mid I_{n-k}\,]$, so

$$H^T=\begin{bmatrix} P \\ I_{n-k} \end{bmatrix}.$$

**Proof that $GH^T=0$:** multiply the block matrices (mod 2):

$$GH^T=[\,I_k\mid P\,]\begin{bmatrix} P \\ I_{n-k} \end{bmatrix}=I_k\,P \;+\; P\,I_{n-k}=P+P=0 .$$

(In mod-2 arithmetic any matrix added to itself is $0$.)

**Proof that $CH^T=0$:** every codeword is $C=DG$ for some message $D$, hence

$$CH^T=(DG)H^T=D\,(GH^T)=D\cdot 0=0 .$$

This is exactly the test used by the decoder: a received word is a valid codeword **iff** $CH^T=0$.

> **Steps explained:** Write $H^T$ as $P$ stacked on $I$. Block-multiply $G$ by it: the identity blocks pull out $P$ twice, which cancel mod 2. Since each codeword is a row combination of $G$ ($C=DG$), it inherits $CH^T=0$.

---

## Q8 — (7,4) code: G, H, syndrome circuit

**Restate:** For a $(7,4)$ linear block code, write the generator matrix $G$, the parity-check matrix $H$, and draw the syndrome calculation circuit.

Take the standard $(7,4)$ Hamming code with check bits

$$C_5=d_1+d_3+d_4,\quad C_6=d_1+d_2+d_3,\quad C_7=d_2+d_3+d_4 .$$

**Step 1 — Parity matrix** (rows $d_1..d_4$, columns $C_5,C_6,C_7$):

$$P=\begin{bmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 0 & 1 \end{bmatrix}$$

**Step 2 — Generator matrix $G=[I_4\mid P]$:**

$$G=\begin{bmatrix} 1 & 0 & 0 & 0 & 1 & 1 & 0 \\ 0 & 1 & 0 & 0 & 0 & 1 & 1 \\ 0 & 0 & 1 & 0 & 1 & 1 & 1 \\ 0 & 0 & 0 & 1 & 1 & 0 & 1 \end{bmatrix}$$

**Step 3 — Parity-check matrix $H=[P^T\mid I_3]$:**

$$H=\begin{bmatrix} 1 & 0 & 1 & 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 0 & 0 & 1 & 0 \\ 0 & 1 & 1 & 1 & 0 & 0 & 1 \end{bmatrix}$$

**Step 4 — Syndrome equations** $S=RH^T$ (a $1$ in row $j$ of $H$ means that bit feeds $S_j$):

$$S_1=r_1+r_3+r_4+r_5,\quad S_2=r_1+r_2+r_3+r_6,\quad S_3=r_2+r_3+r_4+r_7 .$$

**LaTeX (TikZ) — syndrome circuit:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small, xor/.style={draw,circle,inner sep=1.5pt}]
  \foreach \i in {1,...,7}\node (r\i) at (\i*1.4,0){$r_{\i}$};
  \node[xor] (s1) at (4,-2)  {$+$}; \draw[->](s1)--++(0,-1) node[below]{$S_1=r_1{+}r_3{+}r_4{+}r_5$};
  \node[xor] (s2) at (6,-2.9){$+$}; \draw[->](s2)--++(0,-1) node[below]{$S_2=r_1{+}r_2{+}r_3{+}r_6$};
  \node[xor] (s3) at (8,-3.8){$+$}; \draw[->](s3)--++(0,-1) node[below]{$S_3=r_2{+}r_3{+}r_4{+}r_7$};
  \draw[->](r1)--(s1);\draw[->](r3)--(s1);\draw[->](r4)--(s1);\draw[->](r5)--(s1);
  \draw[->](r1)--(s2);\draw[->](r2)--(s2);\draw[->](r3)--(s2);\draw[->](r6)--(s2);
  \draw[->](r2)--(s3);\draw[->](r3)--(s3);\draw[->](r4)--(s3);\draw[->](r7)--(s3);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Convert the check equations into $P$; build $G=[I_4|P]$ and $H=[P^T|I_3]$. Each syndrome bit is the XOR of the received bits marked by a row of $H$.

---

## Q9 — (6,3) code: detect and correct a single error

*(Same method as Q4 — same $(6,3)$ code, now used to decode.)*

**Restate:** For the systematic $(6,3)$ code with

$$P=\begin{bmatrix} 1 & 0 & 1 \\ 0 & 1 & 1 \\ 1 & 1 & 0 \end{bmatrix}$$

the received vector is $R=110010$. Detect and correct the single error and draw the syndrome circuit.

**Step 1 — Build $H^T$.** Since $H=[P^T\mid I_3]$, the matrix $H^T$ is $P$ stacked on top of $I_3$:

$$H^T=\begin{bmatrix} 1 & 0 & 1 \\ 0 & 1 & 1 \\ 1 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**Step 2 — Syndrome $S=RH^T$** with $R=(r_1\dots r_6)=110010$:

$$S_1=r_1+r_3+r_4=1+0+0=1,\quad S_2=r_2+r_3+r_5=1+0+1=0,\quad S_3=r_1+r_2+r_6=1+1+0=0 .$$

$$\boxed{S=[\,1\ 0\ 0\,]\neq 0 \Rightarrow \text{error present.}}$$

**Step 3 — Locate the error.** $S=100$ matches the **4th row** of $H^T$, so bit $r_4$ is wrong.

**Step 4 — Correct.** Flip bit 4 of $R=110\mathbf{0}10$:

$$\boxed{C=110110.}$$

**Syndrome circuit:** identical to the decoder circuit drawn in [Q4](#q4--encoder-and-decoder-circuit-for-a-63-code) ($S_1=r_1+r_3+r_4,\ S_2=r_2+r_3+r_5,\ S_3=r_1+r_2+r_6$).

> **Steps explained:** Stack $P$ on $I$ to get $H^T$, multiply the received row by it to get $S$. A non-zero $S$ means an error; matching $S$ to a row of $H^T$ tells which bit; flip that bit to recover the codeword.

---

## Q10 — Code from syndrome equations: G, H, circuits, decoding

**Restate:** A linear block code has syndromes $S_1=r_1+r_2+r_3+r_5$, $S_2=r_1+r_2+r_4+r_6$, $S_3=r_1+r_3+r_4+r_7$. Find $G$ and $H$; draw the encoder and syndrome circuits; state how many errors it detects/corrects; find $S$ for $R=1011011$.

**Step 1 — Identify the code.** Seven received bits and three syndromes $\Rightarrow n=7$, $n-k=3$, $k=4$: a $(7,4)$ code.

**Step 2 — Read $H^T$** (row $i$ = coefficients of $r_i$ in $[S_1\,S_2\,S_3]$):

$$H^T=\begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 0 \\ 1 & 0 & 1 \\ 0 & 1 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

**Step 3 — Parity-check matrix $H=[P^T\mid I_3]$** (transpose of $H^T$):

$$H=\begin{bmatrix} 1 & 1 & 1 & 0 & 1 & 0 & 0 \\ 1 & 1 & 0 & 1 & 0 & 1 & 0 \\ 1 & 0 & 1 & 1 & 0 & 0 & 1 \end{bmatrix}$$

**Step 4 — Generator matrix $G=[I_4\mid P]$**, where $P$ is the transpose of the first four columns of $H$:

$$G=\begin{bmatrix} 1 & 0 & 0 & 0 & 1 & 1 & 1 \\ 0 & 1 & 0 & 0 & 1 & 1 & 0 \\ 0 & 0 & 1 & 0 & 1 & 0 & 1 \\ 0 & 0 & 0 & 1 & 0 & 1 & 1 \end{bmatrix}$$

i.e. check bits $C_5=d_1+d_2+d_3,\ C_6=d_1+d_2+d_4,\ C_7=d_1+d_3+d_4$.

**Step 5 — Error capability.** The seven columns of $H$ are the seven distinct non-zero 3-bit patterns, so this is a Hamming code with $d_{\min}=3$:

$$\text{detects } d_{\min}-1=2 \text{ errors},\qquad \text{corrects } \tfrac{d_{\min}-1}{2}=1 \text{ error}.$$

**Step 6 — Syndrome of $R=1011011$** ($r_1..r_7=1,0,1,1,0,1,1$):

$$S_1=r_1+r_2+r_3+r_5=1+0+1+0=0$$
$$S_2=r_1+r_2+r_4+r_6=1+0+1+1=1$$
$$S_3=r_1+r_3+r_4+r_7=1+1+1+1=0$$

$$\boxed{S=[\,0\ 1\ 0\,].}$$

$S=010$ is the **6th row** of $H^T$, so bit $r_6$ is in error; the corrected codeword is $1011001$ and the message is $d_1d_2d_3d_4=1011$.

**LaTeX (TikZ) — encoder ($C_5,C_6,C_7$ from $G$):**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small, xor/.style={draw,circle,inner sep=1.5pt}]
  \foreach \i in {1,2,3,4}\node (d\i) at (0,-\i*0.9){$d_{\i}$};
  \foreach \i in {1,2,3,4}\draw[->](d\i)--++(6,0) node[right]{$C_{\i}=d_{\i}$};
  \node[xor](c5) at (3,-4.5){$+$}; \draw[->](c5)--++(3,0) node[right]{$C_5=d_1{+}d_2{+}d_3$};
  \node[xor](c6) at (3,-5.3){$+$}; \draw[->](c6)--++(3,0) node[right]{$C_6=d_1{+}d_2{+}d_4$};
  \node[xor](c7) at (3,-6.1){$+$}; \draw[->](c7)--++(3,0) node[right]{$C_7=d_1{+}d_3{+}d_4$};
\end{tikzpicture}
\end{document}
```

**LaTeX (TikZ) — syndrome circuit:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small, xor/.style={draw,circle,inner sep=1.5pt}]
  \foreach \i in {1,...,7}\node (r\i) at (\i*1.5,0){$r_{\i}$};
  \node[xor](s1) at (4,-2)  {$+$}; \draw[->](s1)--++(0,-1) node[below]{$S_1=r_1{+}r_2{+}r_3{+}r_5$};
  \node[xor](s2) at (6,-2.9){$+$}; \draw[->](s2)--++(0,-1) node[below]{$S_2=r_1{+}r_2{+}r_4{+}r_6$};
  \node[xor](s3) at (8,-3.8){$+$}; \draw[->](s3)--++(0,-1) node[below]{$S_3=r_1{+}r_3{+}r_4{+}r_7$};
  \draw[->](r1)--(s1);\draw[->](r2)--(s1);\draw[->](r3)--(s1);\draw[->](r5)--(s1);
  \draw[->](r1)--(s2);\draw[->](r2)--(s2);\draw[->](r4)--(s2);\draw[->](r6)--(s2);
  \draw[->](r1)--(s3);\draw[->](r3)--(s3);\draw[->](r4)--(s3);\draw[->](r7)--(s3);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** The syndrome equations *are* the rows of $H^T$. Transpose to get $H$, read off $P$, and build $G=[I_4|P]$. Distinct non-zero columns of $H$ ⇒ Hamming code ($d_{\min}=3$). Plug $R$ into the three equations to get $S$; match $S$ to a row of $H^T$ to locate the bad bit.

---

# UNIT 2 — CYCLIC CODES

## Q11 — Code vectors in systematic form

**Restate:** Explain the generation of code vectors in systematic form (for a cyclic code).

**Answer.** A systematic cyclic codeword places the $(n-k)$ parity bits first and the $k$ message bits last:

$$V=(\,r_0\,r_1\,\cdots\,r_{n-k-1}\;\mid\;d_0\,d_1\,\cdots\,d_{k-1}\,).$$

**Procedure** (generator polynomial $g(x)$ of degree $n-k$):

1. Write the message polynomial $D(x)=d_0+d_1x+\dots+d_{k-1}x^{k-1}$.
2. Shift it up by $(n-k)$: form $x^{n-k}D(x)$.
3. Divide by $g(x)$:

$$\frac{x^{n-k}D(x)}{g(x)}=Q(x)+\frac{R(x)}{g(x)},\qquad \deg R(x)<(n-k).$$

4. The remainder $R(x)$ gives the parity bits. The systematic code polynomial is

$$\boxed{V(x)=x^{n-k}D(x)+R(x).}$$

Because $V(x)=x^{n-k}D(x)+R(x)$ is exactly divisible by $g(x)$, it is a valid codeword, and the top $k$ positions still hold $D$ unchanged (systematic).

> **Steps explained:** Multiplying by $x^{n-k}$ reserves the low positions for parity; the division remainder is chosen so the whole thing becomes a multiple of $g(x)$ (a legal codeword) while leaving the message bits visible.

---

## Q12 — Encoding an (n,k) cyclic code with an (n−k)-bit register

**Restate:** Explain encoding of an $(n,k)$ cyclic code in systematic form using an $(n-k)$-bit shift register.

**Answer.** The encoder is a feedback shift register that performs the division $x^{n-k}D(x)/g(x)$; the leftover in the register is the remainder $R(x)$ (the parity bits). Connections follow $g(x)=g_0+g_1x+\dots+g_{n-k}x^{n-k}$: a tap (XOR) exists wherever $g_i=1$.

**Operation (two phases):**

1. **Gate ON, switch at position 1** — the $k$ message bits are shifted in (highest-order bit first) and simultaneously sent to the channel. After all $k$ bits, the register holds the parity bits $r_0\dots r_{n-k-1}$.
2. **Gate OFF, switch at position 2** — the register is shifted out to the channel, appending the parity bits.

The transmitted codeword is $(\,r_0\dots r_{n-k-1}\mid d_0\dots d_{k-1}\,)$.

**Hardware:** $(n-k)$ flip-flops, up to $(n-k)$ XOR adders, one AND gate, and a counter.

**LaTeX (TikZ) — general $(n-k)$-stage encoder:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm},
  add/.style={draw,circle,inner sep=1.2pt}]
  \node[add](a0){$+$};
  \node[ff,right=7mm of a0](r0){$r_0$};
  \node[add,right=7mm of r0](a1){$+$};
  \node[ff,right=7mm of a1](r1){$r_1$};
  \node[right=7mm of r1](dots){$\cdots$};
  \node[ff,right=7mm of dots](rl){$r_{n-k-1}$};
  \draw[->](a0)--(r0); \draw[->](r0)--(a1); \draw[->](a1)--(r1);
  \draw[->](r1)--(dots); \draw[->](dots)--(rl);
  % feedback bus along the top
  \coordinate (out) at ($(rl.east)+(8mm,0)$);
  \draw[->](rl.east)--(out);
  \draw (out)|-($(a0.north)+(0,10mm)$);
  \draw[->]($(a0.north)+(0,10mm)$)-|(a0.north);
  \draw[->]($(a1.north)+(0,10mm)$)-- node[left]{\scriptsize $g_1$} (a1.north);
  \node at ($(a0.north)+(-3mm,5mm)$){\scriptsize $g_0$};
  % message input switch
  \draw[->]($(a0.west)+(-12mm,-6mm)$) node[left]{$D(x)$} -- ($(a0.south)+(0,-6mm)$) -- (a0.south);
  \node[below=8mm of rl]{\scriptsize gate (AND) opens/closes feedback; switch selects message vs.\ parity};
\end{tikzpicture}
\end{document}
```

> **Steps explained:** The shift register *is* a polynomial divider. Feeding the message (×$x^{n-k}$ is implicit in where bits enter) leaves the division remainder in the flip-flops; that remainder is the parity. Phase 1 sends the message and computes parity; phase 2 flushes the parity out.

---

## Q13 — (7,3) cyclic encoder, message 011

**Restate:** A $(7,3)$ code has $g(x)=1+x+x^2+x^4$. Devise the encoder and list the shift-register contents for the message $011$.

Here $n=7$, $k=3$, $n-k=4$, so a 4-stage register; $g_0=g_1=g_2=1,\ g_3=0,\ g_4=1$.

**Step 1 — Message polynomial.** $D=011\Rightarrow d_0=0,d_1=1,d_2=1$, so $D(x)=x+x^2$.

**Step 2 — Divide $x^{4}D(x)=x^5+x^6$ by $g(x)=x^4+x^2+x+1$.** Long division gives quotient $x^2+x+1$ and

$$R(x)=x^2+1\Rightarrow (r_0\,r_1\,r_2\,r_3)=(1\,0\,1\,0).$$

**Step 3 — Register trace** (feedback $fb=\text{input}+r_3$; updates $r_0{=}fb,\ r_1{=}r_0{+}fb,\ r_2{=}r_1{+}fb,\ r_3{=}r_2$; input order $d_2,d_1,d_0$):

| Shift | Input | $r_0\,r_1\,r_2\,r_3$ |
|:---:|:---:|:---:|
| init | – | 0 0 0 0 |
| 1 | $d_2=1$ | 1 1 1 0 |
| 2 | $d_1=1$ | 1 0 0 1 |
| 3 | $d_0=0$ | 1 0 1 0 |

Register $=1010 \Rightarrow R(x)=1+x^2$ (matches Step 2).

**Step 4 — Codeword** $V=(r_0r_1r_2r_3\mid d_0d_1d_2)$:

$$\boxed{V=1\,0\,1\,0\,\mid\,0\,1\,1\;=\;1010011,\qquad V(x)=1+x^2+x^5+x^6.}$$

**LaTeX (TikZ) — (7,3) encoder, $g(x)=1+x+x^2+x^4$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm}, add/.style={draw,circle,inner sep=1.2pt}]
  \node[add](a0){$+$};
  \node[ff,right=7mm of a0](r0){$r_0$};
  \node[add,right=7mm of r0](a1){$+$};
  \node[ff,right=7mm of a1](r1){$r_1$};
  \node[add,right=7mm of r1](a2){$+$};
  \node[ff,right=7mm of a2](r2){$r_2$};
  \node[ff,right=7mm of r2](r3){$r_3$};
  \draw[->](a0)--(r0);\draw[->](r0)--(a1);\draw[->](a1)--(r1);
  \draw[->](r1)--(a2);\draw[->](a2)--(r2);\draw[->](r2)--(r3);
  \coordinate(out) at ($(r3.east)+(8mm,0)$); \draw[->](r3.east)--(out) node[right]{$V$};
  \draw (out)|-($(a0.north)+(0,9mm)$);
  \draw[->]($(a0.north)+(0,9mm)$)-|(a0.north);
  \draw[->]($(a1.north)+(0,9mm)$)--(a1.north);
  \draw[->]($(a2.north)+(0,9mm)$)--(a2.north);
  \node[font=\scriptsize] at ($(a0.north)+(0,6mm)$){$g_0$};
  \node[font=\scriptsize] at ($(a1.north)+(0,6mm)$){$g_1$};
  \node[font=\scriptsize] at ($(a2.north)+(0,6mm)$){$g_2$};
  \draw[->]($(a0.south)+(0,-7mm)$) node[below]{$D(x)$} -- (a0.south);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Degree of $g(x)$ = 4 ⇒ 4 flip-flops with XOR taps at $g_0,g_1,g_2$. Shift the 3 message bits in; the register ends holding the parity $1010$. Prefix the parity to the message to form the 7-bit codeword.

---

## Q14 — (7,4) cyclic encoder, message 1011

*(Same method as Q13 — cyclic shift-register encoding, here $n-k=3$.)*

**Restate:** Design the encoder for a $(7,4)$ cyclic code with $g(x)=1+x+x^3$ and verify it with message $1011$.

$n=7$, $k=4$, $n-k=3$; $g_0=1,g_1=1,g_2=0,g_3=1$ (3-stage register).

**Step 1 — Message polynomial.** $D=1011\Rightarrow d_0=1,d_1=0,d_2=1,d_3=1$, so $D(x)=1+x^2+x^3$.

**Step 2 — Divide $x^3D(x)=x^3+x^5+x^6$ by $g(x)=x^3+x+1$.** Quotient $x^3+x^2+x+1$, remainder

$$R(x)=1\Rightarrow (r_0\,r_1\,r_2)=(1\,0\,0).$$

**Step 3 — Register trace** ($fb=\text{input}+r_2$; $r_0{=}fb,\ r_1{=}r_0{+}fb,\ r_2{=}r_1$; input order $d_3,d_2,d_1,d_0$):

| Shift | Input | $r_0\,r_1\,r_2$ |
|:---:|:---:|:---:|
| init | – | 0 0 0 |
| 1 | $d_3=1$ | 1 1 0 |
| 2 | $d_2=1$ | 1 0 1 |
| 3 | $d_1=0$ | 1 0 0 |
| 4 | $d_0=1$ | 1 0 0 |

Register $=100\Rightarrow R(x)=1$ (matches Step 2).

**Step 4 — Codeword** $V=(r_0r_1r_2\mid d_0d_1d_2d_3)$:

$$\boxed{V=1\,0\,0\,\mid\,1\,0\,1\,1\;=\;1001011,\qquad V(x)=1+x^3+x^5+x^6.}$$

**LaTeX (TikZ) — (7,4) encoder, $g(x)=1+x+x^3$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm}, add/.style={draw,circle,inner sep=1.2pt}]
  \node[add](a0){$+$};
  \node[ff,right=7mm of a0](r0){$r_0$};
  \node[add,right=7mm of r0](a1){$+$};
  \node[ff,right=7mm of a1](r1){$r_1$};
  \node[ff,right=10mm of r1](r2){$r_2$};
  \draw[->](a0)--(r0);\draw[->](r0)--(a1);\draw[->](a1)--(r1);\draw[->](r1)--(r2);
  \coordinate(out) at ($(r2.east)+(8mm,0)$); \draw[->](r2.east)--(out) node[right]{$V$};
  \draw (out)|-($(a0.north)+(0,9mm)$);
  \draw[->]($(a0.north)+(0,9mm)$)-|(a0.north);
  \draw[->]($(a1.north)+(0,9mm)$)--(a1.north);
  \node[font=\scriptsize] at ($(a0.north)+(0,6mm)$){$g_0$};
  \node[font=\scriptsize] at ($(a1.north)+(0,6mm)$){$g_1$};
  \draw[->]($(a0.south)+(0,-7mm)$) node[below]{$D(x)$} -- (a0.south);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Same machine as Q13 but 3 stages with taps at $g_0,g_1$. After the 4 message bits, the register holds parity $100$; codeword $=100\,1011$. The polynomial division confirms remainder $1$.

---

## Q15 — Syndrome calculation procedure and circuit

**Restate:** Explain the syndrome-calculation procedure and draw the $(n-k)$-stage syndrome circuit for an $(n,k)$ cyclic code.

**Procedure.** Let $R(x)$ be the received polynomial. Divide it by $g(x)$; the remainder is the **syndrome**:

$$s(x)=\text{remainder}\left[\frac{R(x)}{g(x)}\right].$$

- If $s(x)=0$, then $R(x)$ is a valid codeword ($R=C$, no detectable error).
- If $s(x)\neq 0$, an error is present and $s(x)$ identifies the error pattern.

**Circuit operation.** The syndrome circuit is the same $(n-k)$-stage feedback register as the encoder:

1. **Gate 2 ON, Gate 1 OFF** — shift the entire received vector $R$ into the register. After $n$ shifts the register holds the syndrome.
2. **Gate 2 OFF, Gate 1 ON** — shift the syndrome out.

**LaTeX (TikZ) — general syndrome circuit:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm}, add/.style={draw,circle,inner sep=1.2pt}]
  \node[add](a0){$+$};
  \node[ff,right=7mm of a0](s0){$s_0$};
  \node[add,right=7mm of s0](a1){$+$};
  \node[ff,right=7mm of a1](s1){$s_1$};
  \node[right=7mm of s1](dots){$\cdots$};
  \node[ff,right=7mm of dots](sl){$s_{n-k-1}$};
  \draw[->](a0)--(s0);\draw[->](s0)--(a1);\draw[->](a1)--(s1);
  \draw[->](s1)--(dots);\draw[->](dots)--(sl);
  \draw[->]($(a0.west)+(-12mm,0)$) node[left]{$R(x)$} -- (a0.west);
  \coordinate(out) at ($(sl.east)+(8mm,0)$); \draw[->](sl.east)--(out) node[right]{$s(x)$};
  \draw (out)|-($(a0.north)+(0,10mm)$);
  \draw[->]($(a0.north)+(0,10mm)$)-|(a0.north);
  \draw[->]($(a1.north)+(0,10mm)$)--(a1.north);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Dividing $R(x)$ by $g(x)$ in the shift register leaves the remainder (syndrome) in the flip-flops. Zero ⇒ clean; non-zero ⇒ error, and the pattern tells you where.

---

## Q16 — (15,?) code from a factored g(x): G and H

**Restate:** A length-15 binary cyclic code has $g(x)=(x^4+x+1)(x^4+x^3+x^2+x+1)$. Find a generator matrix and a parity-check matrix.

**Step 1 — Multiply out $g(x)$ (mod 2):**

$$g(x)=x^8+x^7+x^6+x^4+1\quad\Rightarrow\quad \deg g=8.$$

So $n-k=8$, $n=15$, $k=7$: a $(15,7)$ cyclic code.

**Step 2 — Generator matrix** (rows $=g(x),x g(x),\dots,x^{6}g(x)$). With $g=1\,0\,0\,0\,1\,0\,1\,1\,1$ (bits for $x^0\dots x^8$):

$$G=\begin{bmatrix} 1&0&0&0&1&0&1&1&1&0&0&0&0&0&0 \\ 0&1&0&0&0&1&0&1&1&1&0&0&0&0&0 \\ 0&0&1&0&0&0&1&0&1&1&1&0&0&0&0 \\ 0&0&0&1&0&0&0&1&0&1&1&1&0&0&0 \\ 0&0&0&0&1&0&0&0&1&0&1&1&1&0&0 \\ 0&0&0&0&0&1&0&0&0&1&0&1&1&1&0 \\ 0&0&0&0&0&0&1&0&0&0&1&0&1&1&1 \end{bmatrix}$$

**Step 3 — Parity polynomial** $h(x)=\dfrac{x^{15}+1}{g(x)}$. Long division gives

$$h(x)=x^7+x^6+x^4+1.$$

**Step 4 — Parity-check matrix** (rows $=x^{j}h(x^{-1})$ for $j=7,8,\dots,14$). The reciprocal pattern of $h$ is $1+x+x^3+x^7$, shifted down each row:

$$H=\begin{bmatrix} 1&1&0&1&0&0&0&1&0&0&0&0&0&0&0 \\ 0&1&1&0&1&0&0&0&1&0&0&0&0&0&0 \\ 0&0&1&1&0&1&0&0&0&1&0&0&0&0&0 \\ 0&0&0&1&1&0&1&0&0&0&1&0&0&0&0 \\ 0&0&0&0&1&1&0&1&0&0&0&1&0&0&0 \\ 0&0&0&0&0&1&1&0&1&0&0&0&1&0&0 \\ 0&0&0&0&0&0&1&1&0&1&0&0&0&1&0 \\ 0&0&0&0&0&0&0&1&1&0&1&0&0&0&1 \end{bmatrix}$$

> **Steps explained:** Multiply the two factors to get $g(x)$ (degree 8 ⇒ $(15,7)$). Shifting $g(x)$ across 7 rows builds $G$. Dividing $x^{15}+1$ by $g(x)$ gives $h(x)$; shifting the reciprocal of $h(x)$ across 8 rows builds $H$. (Both can be row-reduced to systematic form if required.)

---

## Q17 — (7,4) cyclic: syndrome circuit and error correction

*(Same syndrome method as Q15.)*

**Restate:** For a $(7,4)$ cyclic code with $g(x)=1+x+x^3$, the received vector is $Z=1110101$. Draw the syndrome circuit and correct the single error.

**Step 1 — Received polynomial.** $Z=1110101\Rightarrow Z(x)=1+x+x^2+x^4+x^6$.

**Step 2 — Syndrome by register trace** ($fb=\text{input}+s_2$; $s_0{=}fb,\ s_1{=}s_0{+}fb,\ s_2{=}s_1$; feed $Z$ left-to-right):

| Shift | In | $s_0\,s_1\,s_2$ |
|:---:|:---:|:---:|
| init | – | 0 0 0 |
| 1 | 1 | 1 0 0 |
| 2 | 1 | 1 1 0 |
| 3 | 1 | 1 1 1 |
| 4 | 0 | 1 0 1 |
| 5 | 1 | 1 0 0 |
| 6 | 0 | 0 1 0 |
| 7 | 1 | 1 1 1 |

After 7 shifts the syndrome is read out; equivalently divide $Z(x)$ by $g(x)$:

$$s(x)=\text{rem}\left[\frac{Z(x)}{g(x)}\right]=x^2\neq 0.$$

**Step 3 — Locate the error.** For a single error $e(x)=x^i$, the syndrome is $x^i \bmod g(x)$. Since $s(x)=x^2$, the error is at position $i=2$ (bit $z_2$).

**Step 4 — Correct.** $Z=11\mathbf{1}0101$, flip $z_2$:

$$\boxed{C=1100101,\qquad C(x)=1+x+x^4+x^6.}$$

(Check: $C(x)$ is exactly divisible by $g(x)$, so it is a valid codeword.)

**LaTeX (TikZ) — syndrome circuit, $g(x)=1+x+x^3$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm}, add/.style={draw,circle,inner sep=1.2pt}]
  \node[add](a0){$+$};
  \node[ff,right=7mm of a0](s0){$s_0$};
  \node[add,right=7mm of s0](a1){$+$};
  \node[ff,right=7mm of a1](s1){$s_1$};
  \node[ff,right=10mm of s1](s2){$s_2$};
  \draw[->](a0)--(s0);\draw[->](s0)--(a1);\draw[->](a1)--(s1);\draw[->](s1)--(s2);
  \draw[->]($(a0.west)+(-12mm,0)$) node[left]{$Z(x)$} -- (a0.west);
  \coordinate(out) at ($(s2.east)+(8mm,0)$); \draw[->](s2.east)--(out) node[right]{$s(x)$};
  \draw (out)|-($(a0.north)+(0,9mm)$);
  \draw[->]($(a0.north)+(0,9mm)$)-|(a0.north);
  \draw[->]($(a1.north)+(0,9mm)$)--(a1.north);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Shifting $Z$ through the divider leaves $s(x)=x^2$. A single error gives syndrome $x^i\bmod g$; matching $x^2$ pins the error to bit 2. Flip it to recover the codeword $1100101$.

---

## Q18 — (15,5) cyclic: encoder/syndrome, systematic codeword, codeword test

*(Encoder/syndrome built like Q12 and Q15; systematic encoding like Q11/Q13.)*

**Restate:** A $(15,5)$ cyclic code has $g(x)=1+x+x^2+x^4+x^5+x^8+x^{10}$. (i) Draw the encoder and syndrome circuit. (ii) Find the systematic code polynomial for $D(x)=1+x^2+x^4$. (iii) Is $v(x)=1+x^4+x^6+x^8+x^{14}$ a code polynomial?

Here $n=15$, $k=5$, $n-k=10$; taps where $g_i=1$: $g_0,g_1,g_2,g_4,g_5,g_8,g_{10}$.

**Part (ii) — Systematic codeword for $D(x)=1+x^2+x^4$.**

Step 1: $x^{n-k}D(x)=x^{10}(1+x^2+x^4)=x^{10}+x^{12}+x^{14}$.

Step 2: divide by $g(x)=x^{10}+x^8+x^5+x^4+x^2+x+1$. Quotient $Q(x)=x^4+1$, remainder

$$R(x)=x^9+x^6+x^2+x+1\Rightarrow (r_0\dots r_9)=1\,1\,1\,0\,0\,0\,1\,0\,0\,1 .$$

Step 3: $V(x)=x^{10}D(x)+R(x)$:

$$\boxed{V(x)=1+x+x^2+x^6+x^9+x^{10}+x^{12}+x^{14}}$$
$$\boxed{V=1\,1\,1\,0\,0\,0\,1\,0\,0\,1\;\mid\;1\,0\,1\,0\,1 .}$$

**Part (iii) — Is $v(x)=1+x^4+x^6+x^8+x^{14}$ a codeword?** Divide by $g(x)$:

$$\text{remainder}=x^9+x^8+x^7+x^6+x^3+x\neq 0 .$$

$$\boxed{\text{No — }v(x)\text{ is NOT a valid code polynomial.}}$$

**LaTeX (TikZ) — (15,5) encoder, $g(x)=1+x+x^2+x^4+x^5+x^8+x^{10}$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\scriptsize,
  ff/.style={draw,minimum width=7mm,minimum height=7mm}, add/.style={draw,circle,inner sep=1pt}]
  \node[add](a0){$+$};
  \foreach \i [remember=\i as \p (initially 0)] in {0,...,9}{
     \ifnum\i=0 \node[ff,right=5mm of a0](r0){$r_0$};
     \else \node[ff,right=6mm of r\p](r\i){$r_\i$}; \fi }
  % taps (XOR) before stages where g_i=1 for i=1,2,4,5,8 (g0 at a0, g10 feedback)
  \draw[->](a0)--(r0);
  \foreach \i [remember=\i as \p (initially 0)] in {1,...,9}{ \draw[->](r\p)--(r\i); }
  \coordinate(out) at ($(r9.east)+(7mm,0)$); \draw[->](r9.east)--(out) node[right]{$V$};
  \draw (out)|-($(a0.north)+(0,10mm)$); \draw[->]($(a0.north)+(0,10mm)$)-|(a0.north);
  \draw[->]($(a0.south)+(0,-7mm)$) node[below]{$D(x)$}--(a0.south);
  \node[align=center,below=10mm of r5]{\scriptsize XOR taps at stages $g_1,g_2,g_4,g_5,g_8$ (feedback from $r_9$ via $g_{10}$)};
\end{tikzpicture}
\end{document}
```

The **syndrome circuit** is the identical 10-stage register with $R(x)$ fed in instead of $D(x)$ (see [Q15](#q15--syndrome-calculation-procedure-and-circuit)).

> **Steps explained:** Multiply $D(x)$ by $x^{10}$, divide by $g(x)$; the remainder $R(x)$ is the 10 parity bits placed ahead of the 5 message bits. For part (iii) a non-zero remainder when dividing $v(x)$ by $g(x)$ proves it is not a codeword.

---

## Q19 — (15,11) cyclic encoder, message 10010110111

*(Same shift-register method as Q13/Q14, 4 stages.)*

**Restate:** A $(15,11)$ cyclic code has $g(x)=1+x+x^4$. Devise the feedback-register encoder and trace the registers for message $10010110111$.

$n=15$, $k=11$, $n-k=4$; $g_0=1,g_1=1,g_2=0,g_3=0,g_4=1$ (4 stages, taps at $g_0,g_1$).

Update rule: $fb=\text{input}+r_3$; $r_0{=}fb,\ r_1{=}r_0{+}fb,\ r_2{=}r_1,\ r_3{=}r_2$. Message $D=10010110111\Rightarrow d_0..d_{10}=1,0,0,1,0,1,1,0,1,1,1$ (feed $d_{10}$ first).

| Shift | Input | $r_0\,r_1\,r_2\,r_3$ |
|:---:|:---:|:---:|
| init | – | 0 0 0 0 |
| 1 | $d_{10}=1$ | 1 1 0 0 |
| 2 | $d_{9}=1$ | 1 0 1 0 |
| 3 | $d_{8}=1$ | 1 0 0 1 |
| 4 | $d_{7}=0$ | 1 0 0 0 |
| 5 | $d_{6}=1$ | 1 0 0 0 |
| 6 | $d_{5}=1$ | 1 0 0 0 |
| 7 | $d_{4}=0$ | 0 1 0 0 |
| 8 | $d_{3}=1$ | 1 1 1 0 |
| 9 | $d_{2}=0$ | 0 1 1 1 |
| 10 | $d_{1}=0$ | 1 1 1 1 |
| 11 | $d_{0}=1$ | 0 1 1 1 |

**Parity** $=(r_0r_1r_2r_3)=0\,1\,1\,1$, i.e. $R(x)=x+x^2+x^3$ (verified by $x^4D(x)\bmod g(x)$).

$$\boxed{V=0\,1\,1\,1\;\mid\;1\,0\,0\,1\,0\,1\,1\,0\,1\,1\,1 .}$$

**LaTeX (TikZ) — (15,11) encoder, $g(x)=1+x+x^4$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\small,
  ff/.style={draw,minimum width=9mm,minimum height=8mm}, add/.style={draw,circle,inner sep=1.2pt}]
  \node[add](a0){$+$};
  \node[ff,right=7mm of a0](r0){$r_0$};
  \node[add,right=7mm of r0](a1){$+$};
  \node[ff,right=7mm of a1](r1){$r_1$};
  \node[ff,right=7mm of r1](r2){$r_2$};
  \node[ff,right=7mm of r2](r3){$r_3$};
  \draw[->](a0)--(r0);\draw[->](r0)--(a1);\draw[->](a1)--(r1);\draw[->](r1)--(r2);\draw[->](r2)--(r3);
  \coordinate(out) at ($(r3.east)+(8mm,0)$); \draw[->](r3.east)--(out) node[right]{$V$};
  \draw (out)|-($(a0.north)+(0,9mm)$); \draw[->]($(a0.north)+(0,9mm)$)-|(a0.north);
  \draw[->]($(a1.north)+(0,9mm)$)--(a1.north);
  \node[font=\scriptsize] at ($(a0.north)+(0,6mm)$){$g_0$};
  \node[font=\scriptsize] at ($(a1.north)+(0,6mm)$){$g_1$};
  \draw[->]($(a0.south)+(0,-7mm)$) node[below]{$D(x)$}--(a0.south);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** 4-stage register, taps at $g_0,g_1$. After all 11 message bits, the register holds parity $0111$; prepend it to the message for the 15-bit codeword. Polynomial division ($x^4D(x)\bmod g$) gives the same $x+x^2+x^3$.

---

## Q20 — (15,7) cyclic: encoder/syndrome, systematic codeword, error syndrome

*(Systematic encoding like Q11/Q18; syndrome like Q15/Q17.)*

**Restate:** A $(15,7)$ code has $g(x)=1+x^4+x^6+x^7+x^8$. (i) Draw the encoder and syndrome circuit. (ii) Find the systematic code polynomial for $D(x)=x^2+x^3+x^4$. (iii) If the **first and last** bits of that codeword are received in error, find the syndrome.

$n=15$, $k=7$, $n-k=8$; taps where $g_i=1$: $g_0,g_4,g_6,g_7$ (feedback via $g_8$).

**Part (ii) — Systematic codeword.**

Step 1: $x^{8}D(x)=x^{8}(x^2+x^3+x^4)=x^{10}+x^{11}+x^{12}$.

Step 2: divide by $g(x)=x^8+x^7+x^6+x^4+1$. Quotient $Q(x)=x^4+1$, remainder

$$R(x)=x^7+x^6+1\Rightarrow (r_0\dots r_7)=1\,0\,0\,0\,0\,0\,1\,1 .$$

Step 3: $V(x)=x^{8}D(x)+R(x)$:

$$\boxed{V(x)=1+x^6+x^7+x^{10}+x^{11}+x^{12}}$$
$$\boxed{V=1\,0\,0\,0\,0\,0\,1\,1\;\mid\;0\,0\,1\,1\,1\,0\,0 .}$$

**Part (iii) — Syndrome when bit 0 and bit 14 are in error.**

The error polynomial is $e(x)=x^{0}+x^{14}=1+x^{14}$. Since the syndrome of a received word is $s(x)=e(x)\bmod g(x)$ (the valid part contributes nothing):

$$1\bmod g=1,\qquad x^{14}\bmod g(x)=x^{7}+x^{6}+x^{5}+x^{3}.$$

$$s(x)=1+x^{3}+x^{5}+x^{6}+x^{7}\Rightarrow \boxed{S=1\,0\,0\,1\,0\,1\,1\,1 .}$$

**LaTeX (TikZ) — (15,7) encoder, $g(x)=1+x^4+x^6+x^7+x^8$:**

```latex
\documentclass[border=4mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc}
\begin{document}
\begin{tikzpicture}[>=Stealth, font=\scriptsize,
  ff/.style={draw,minimum width=7mm,minimum height=7mm}, add/.style={draw,circle,inner sep=1pt}]
  \node[add](a0){$+$};
  \node[ff,right=5mm of a0](r0){$r_0$};
  \node[ff,right=6mm of r0](r1){$r_1$};
  \node[ff,right=6mm of r1](r2){$r_2$};
  \node[ff,right=6mm of r2](r3){$r_3$};
  \node[add,right=5mm of r3](a4){$+$};
  \node[ff,right=5mm of a4](r4){$r_4$};
  \node[ff,right=6mm of r4](r5){$r_5$};
  \node[add,right=5mm of r5](a6){$+$};
  \node[ff,right=5mm of a6](r6){$r_6$};
  \node[add,right=5mm of r6](a7){$+$};
  \node[ff,right=5mm of a7](r7){$r_7$};
  \draw[->](a0)--(r0);\draw[->](r0)--(r1);\draw[->](r1)--(r2);\draw[->](r2)--(r3);
  \draw[->](r3)--(a4);\draw[->](a4)--(r4);\draw[->](r4)--(r5);\draw[->](r5)--(a6);
  \draw[->](a6)--(r6);\draw[->](r6)--(a7);\draw[->](a7)--(r7);
  \coordinate(out) at ($(r7.east)+(6mm,0)$); \draw[->](r7.east)--(out) node[right]{$V$};
  \draw (out)|-($(a0.north)+(0,11mm)$); \draw[->]($(a0.north)+(0,11mm)$)-|(a0.north);
  \draw[->]($(a4.north)+(0,11mm)$)--(a4.north);
  \draw[->]($(a6.north)+(0,11mm)$)--(a6.north);
  \draw[->]($(a7.north)+(0,11mm)$)--(a7.north);
  \node at ($(a0.north)+(0,7mm)$){$g_0$};\node at ($(a4.north)+(0,7mm)$){$g_4$};
  \node at ($(a6.north)+(0,7mm)$){$g_6$};\node at ($(a7.north)+(0,7mm)$){$g_7$};
  \draw[->]($(a0.south)+(0,-7mm)$) node[below]{$D(x)$}--(a0.south);
\end{tikzpicture}
\end{document}
```

The **syndrome circuit** is the same 8-stage register fed with the received vector $R(x)$ (output is $s(x)$, as in [Q15](#q15--syndrome-calculation-procedure-and-circuit)).

> **Steps explained:** Multiply $D(x)$ by $x^8$, divide by $g(x)$; remainder $R(x)=1+x^6+x^7$ gives the 8 parity bits. For part (iii), the syndrome depends only on the error: reduce $e(x)=1+x^{14}$ modulo $g(x)$ to get $S=10010111$.

---

# 📋 Formulas

### 1. Coded bit rate
$$r_c=r_b\left(\frac{n}{k}\right)\ \text{bits/sec}$$
- $r_b$ = message bit rate, $r_c$ = coded bit rate, $n$ = codeword length, $k$ = message length.
- **Used in:** Q1.

### 2. Channel capacity (feasibility check)
$$C=B\log_2\!\left(1+\frac{S}{N}\right)\ \text{bits/sec}$$
- $B$ = bandwidth, $S/N$ = power ratio. Coding helps only if $r_c<C$.
- **Used in:** Q1 (worked example background).

### 3. Repetition-code error probability (majority logic, n = 3)
$$P_e=q_c^2(3-2q_c),\qquad {}^nC_r=\frac{n!}{(n-r)!\,r!}$$
- $q_c$ = channel bit-error probability.
- **Used in:** Q1 (error-control example).

### 4. Codeword generation
$$C=DG$$
- $D$ = $k$-bit message row vector, $G$ = generator matrix, $C$ = $n$-bit codeword.
- **Used in:** Q4, Q5, Q6, Q8, Q10 (and all cyclic codewords via matrices).

### 5. Generator matrix (systematic)
$$G=[\,I_k\mid P\,]$$
- $I_k$ = $k\times k$ identity, $P$ = $k\times(n-k)$ parity matrix.
- **Used in:** Q3, Q4, Q5, Q6, Q8, Q10, Q16.

### 6. Parity-check matrix
$$H=[\,P^T\mid I_{n-k}\,],\qquad H^T=\begin{bmatrix}P\\ I_{n-k}\end{bmatrix}$$
- **Used in:** Q5, Q7, Q8, Q9, Q10, Q16.

### 7. Codeword / generator orthogonality
$$GH^T=0,\qquad CH^T=0$$
- A received word is a valid codeword iff $CH^T=0$.
- **Used in:** Q7 (proof), Q9, Q10 (decoding basis).

### 8. Received vector and syndrome
$$R=C\oplus E,\qquad S=RH^T=EH^T$$
- $E$ = error pattern, $S$ = syndrome. $S=0$ ⇒ no (detectable) error; $S\neq0$ ⇒ error, and $S$ = the row of $H^T$ at the error position (single error).
- **Used in:** Q9, Q10.

### 9. Minimum distance and capability
$$d_{\min}=W_{t,\min}$$
$$\text{detect}=d_{\min}-1,\quad \text{correct}=\tfrac{d_{\min}-1}{2}\ (d_{\min}\text{ odd}),\ \tfrac{d_{\min}}{2}-1\ (d_{\min}\text{ even})$$
- $W_{t,\min}$ = smallest non-zero codeword weight.
- **Used in:** Q5, Q6, Q8, Q10.

### 10. Hamming weight and distance
$$w(C)=\#\{\text{1's in }C\},\qquad d(C_1,C_2)=\#\{\text{positions where they differ}\}$$
- **Used in:** Q5, Q6.

### 11. Single-error-correcting Hamming bound
$$n\le 2^{n-k}-1,\qquad k\le n-\log_2(n+1)$$
- **Used in:** Q8, Q10, Q13, Q16 (recognising Hamming/length conditions).

### 12. Cyclic code — non-systematic
$$V(x)=D(x)\,g(x)$$
- $g(x)$ = generator polynomial (degree $n-k$), $D(x)$ = message polynomial.
- **Used in:** (background for) Q11–Q20.

### 13. Cyclic code — systematic
$$R(x)=\text{remainder}\left[\frac{x^{n-k}D(x)}{g(x)}\right],\qquad V(x)=x^{n-k}D(x)+R(x)$$
- $R(x)$ = parity polynomial (degree $<n-k$).
- **Used in:** Q11, Q13, Q14, Q18, Q19, Q20.

### 14. Parity polynomial
$$h(x)=\frac{x^{n}+1}{g(x)}$$
- Used to build $H$ for cyclic codes.
- **Used in:** Q16.

### 15. Cyclic syndrome
$$s(x)=\text{remainder}\left[\frac{R(x)}{g(x)}\right]=e(x)\bmod g(x)$$
- $s(x)=0$ ⇒ valid codeword; otherwise identifies the error.
- **Used in:** Q15, Q17, Q18, Q20.

### 16. Cyclic G and H rows
$$\text{rows of }G:\ x^{i}g(x),\ i=0,\dots,k-1;\qquad \text{rows of }H:\ x^{j}h(x^{-1}),\ j=k,\dots,n-1$$
- **Used in:** Q16.

### 17. Mod-2 (Ex-OR) addition
$$0+0=0,\quad 1+0=1,\quad 0+1=1,\quad 1+1=0$$
- Underlies every "+" in this module.
- **Used in:** all questions.

---

*Module 4 — Error Control Coding. All polynomial divisions are over GF(2). Every TikZ block is a standalone document — paste into Overleaf and compile with pdfLaTeX.*
