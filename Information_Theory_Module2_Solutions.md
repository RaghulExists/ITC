# Module 2 — Source Coding · Question Bank Solutions

> Complete, step-by-step solutions to all 20 questions (Unit 1 & Unit 2), faithful to the class notes (ITC, Module 2).
> Each question includes a worked solution, a short **Steps explained** note, and **LaTeX (TikZ)** code for every diagram.
> A full **formula sheet** is given at the end.

> **Handy constant used throughout:** $\log_2 x = 3.3219 \times \log_{10} x$
> Quick values: $\log_2 2 = 1$, $\log_2 3 = 1.585$, $\log_2 4 = 2$, $\log_2 5 = 2.322$, $\log_2 6.667 = 2.737$, $\log_2 8 = 3$, $\log_2 10 = 3.322$, $\log_2 20 = 4.322$.

---

## Table of Contents

**Unit 1 — Shannon & Shannon-Fano**
- [Q1 — Shannon's encoding algorithm (theory)](#q1--shannons-encoding-algorithm-theory)
- [Q2 — Shannon-Fano coding procedure (theory)](#q2--shannon-fano-coding-procedure-theory)
- [Q3 — Shannon code: 0.2, 0.5, 0.3](#q3--shannon-code-02-05-03)
- [Q4 — Shannon code: 0.4, 0.25, 0.15, 0.12, 0.08](#q4--shannon-code-04-025-015-012-008)
- [Q5 — Shannon code: 2-symbol messages](#q5--shannon-code-2-symbol-messages)
- [Q6 — Shannon-Fano: 0.4, 0.2, 0.2, 0.1, 0.07, 0.03](#q6--shannon-fano-04-02-02-01-007-003)
- [Q7 — Shannon-Fano with 2nd & 3rd extensions](#q7--shannon-fano-with-2nd--3rd-extensions)
- [Q8 — Shannon-Fano: efficiency ≥ 65%](#q8--shannon-fano-efficiency--65)
- [Q9 — Shannon-Fano: 100% efficiency case](#q9--shannon-fano-100-efficiency-case)
- [Q10 — Shannon-Fano Ternary code](#q10--shannon-fano-ternary-code)

**Unit 2 — Prefix, Huffman, Arithmetic, LZ**
- [Q11 — Prefix code & its properties (theory)](#q11--prefix-code--its-properties-theory)
- [Q12 — Huffman procedure for r = 2, 3, 4 (theory)](#q12--huffman-procedure-for-r--2-3-4-theory)
- [Q13 — Huffman: 0.4, 0.3, 0.2, 0.1](#q13--huffman-04-03-02-01)
- [Q14 — Huffman: 0.7, 0.15, 0.15 + 2nd extension](#q14--huffman-07-015-015--2nd-extension)
- [Q15 — Huffman binary & ternary (8 symbols)](#q15--huffman-binary--ternary-8-symbols)
- [Q16 — Huffman binary & quaternary](#q16--huffman-binary--quaternary)
- [Q17 — Huffman: 100% efficiency case](#q17--huffman-100-efficiency-case)
- [Q18 — Huffman: composite high vs low + variance](#q18--huffman-composite-high-vs-low--variance)
- [Q19 — Arithmetic coding of 'YXZ'](#q19--arithmetic-coding-of-yxz)
- [Q20 — Lempel-Ziv encoding](#q20--lempel-ziv-encoding)

- [📋 Master Formula Sheet](#-master-formula-sheet)
- [Quick Answer Table](#quick-answer-table)

---

# UNIT 1

## Q1 — Shannon's encoding algorithm (theory)

**Shannon's binary encoding algorithm** assigns variable-length codes based on cumulative probabilities. Steps:

1. **List the source symbols in order of decreasing probability:**

$$P_1 \geq P_2 \geq P_3 \geq \dots \geq P_q$$

2. **Compute the cumulative sequences $\alpha_i$:**

$$\alpha_1 = 0,\quad \alpha_2 = P_1,\quad \alpha_3 = P_2 + \alpha_2,\quad \dots,\quad \alpha_{q+1} = P_q + \alpha_q$$

3. **Find the smallest integer $l_i$** (codeword length) satisfying:

$$2^{l_i} \geq \frac{1}{P_i} \qquad i = 1, 2, \dots, q$$

4. **Expand each decimal $\alpha_i$ into binary** up to $l_i$ places (multiply-by-2 method).

5. **Remove the binary point** — the remaining bits form the codeword.

> **Steps explained:** The cumulative probabilities $\alpha_i$ give each symbol a unique starting point on the number line $[0,1)$; the length rule $2^{l_i}\ge 1/P_i$ guarantees each interval is wide enough that the truncated binary expansion never overlaps a neighbour — producing a prefix-free (instantaneous) code.

---

## Q2 — Shannon-Fano coding procedure (theory)

**Shannon-Fano** builds a variable-length, near-optimal, minimum-redundancy code by repeated splitting:

1. Arrange the symbols in **non-increasing order** of probability.
2. **Divide** the list into two groups of **approximately equal total probability**.
3. Assign **`0`** to the top group and **`1`** to the bottom group.
4. **Subdivide** each group again into two near-equal halves.
5. Append **`0`** to the top sub-group and **`1`** to the bottom sub-group.
6. Repeat until **each group has only one symbol** — the accumulated bits form each codeword.

> **Steps explained:** Each binary split contributes one bit. Splitting into equal-probability halves makes both bit values ≈ equally likely, which maximises the information per bit and pushes the average length $L$ close to the entropy $H(S)$ (i.e. high efficiency, low redundancy).

---

## Q3 — Shannon code: 0.2, 0.5, 0.3

*(Same style as Example 2.13 in notes — identical probability set, reordered.)*

**Step 1 — Order by decreasing probability:** $P_1=0.5,\ P_2=0.3,\ P_3=0.2$

**Step 2 — Cumulative $\alpha$'s:**

$$\alpha_1 = 0,\quad \alpha_2 = 0.5,\quad \alpha_3 = 0.8,\quad \alpha_4 = 1$$

**Step 3 — Lengths ($2^{l_i}\ge 1/P_i$):**

| Symbol | $P_i$ | $1/P_i$ | $l_i$ |
|--------|-------|---------|-------|
| $s_1$  | 0.5   | 2       | 1     |
| $s_2$  | 0.3   | 3.33    | 2     |
| $s_3$  | 0.2   | 5       | 3     |

**Step 4 & 5 — Binary expansion → code:**

- $\alpha_1 = 0 \Rightarrow$ code `0`
- $\alpha_2 = 0.5 = (.1)_2 \Rightarrow$ code `10`
- $\alpha_3 = 0.8 = (.110\ldots)_2 \Rightarrow$ code `110`

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 0.5   | 0    | 1     |
| $s_2$  | 0.3   | 10   | 2     |
| $s_3$  | 0.2   | 110  | 3     |

**Average length:**

$$L = \sum P_i l_i = 0.5(1) + 0.3(2) + 0.2(3) = 1.7 \text{ binits/symbol}$$

**Entropy:**

$$H(S) = 0.5\log_2 2 + 0.3\log_2\tfrac{1}{0.3} + 0.2\log_2\tfrac{1}{0.2} = 0.5 + 0.5211 + 0.4644 = 1.4855 \text{ bits/symbol}$$

**Efficiency & redundancy:**

$$\eta_c = \frac{H(S)}{L} = \frac{1.4855}{1.7} = 87.38\% \qquad R_{\eta_c} = 1 - \eta_c = 12.62\%$$

> **Steps explained:** Sort → cumulate → length rule → binary-expand → read off codes → compute $L$ and $H$ → ratio gives efficiency. Identical to Example 2.13.

---

## Q4 — Shannon code: 0.4, 0.25, 0.15, 0.12, 0.08

*(Same style as Example 2.14 in notes.)*

**Step 1 — Order:** $P_1=0.4,\ P_2=0.25,\ P_3=0.15,\ P_4=0.12,\ P_5=0.08$

**Step 2 — Cumulative $\alpha$'s:**

$$\alpha_1=0,\quad \alpha_2=0.4,\quad \alpha_3=0.65,\quad \alpha_4=0.8,\quad \alpha_5=0.92,\quad \alpha_6=1$$

**Step 3 — Lengths:**

| Symbol | $P_i$ | $1/P_i$ | $l_i$ |
|--------|-------|---------|-------|
| $s_1$  | 0.4   | 2.5     | 2     |
| $s_2$  | 0.25  | 4       | 2     |
| $s_3$  | 0.15  | 6.67    | 3     |
| $s_4$  | 0.12  | 8.33    | 4     |
| $s_5$  | 0.08  | 12.5    | 4     |

**Step 4 & 5 — Binary expansion → code:**

- $\alpha_1 = 0 \Rightarrow$ `00`
- $\alpha_2 = 0.4 = (.011)_2 \Rightarrow$ `01`
- $\alpha_3 = 0.65 = (.101)_2 \Rightarrow$ `101`
- $\alpha_4 = 0.8 = (.1100)_2 \Rightarrow$ `1100`
- $\alpha_5 = 0.92 = (.1110)_2 \Rightarrow$ `1110`

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 0.4   | 00   | 2     |
| $s_2$  | 0.25  | 01   | 2     |
| $s_3$  | 0.15  | 101  | 3     |
| $s_4$  | 0.12  | 1100 | 4     |
| $s_5$  | 0.08  | 1110 | 4     |

**Average length:**

$$L = 0.4(2)+0.25(2)+0.15(3)+0.12(4)+0.08(4) = 2.55 \text{ binits/symbol}$$

**Entropy:**

$$H(S) = 0.4\log_2\tfrac{1}{0.4} + 0.25\log_2 4 + 0.15\log_2\tfrac{1}{0.15} + 0.12\log_2\tfrac{1}{0.12} + 0.08\log_2\tfrac{1}{0.08}$$

$$= 0.5288 + 0.5 + 0.4105 + 0.3671 + 0.2915 = 2.098 \text{ bits/symbol}$$

**Efficiency & redundancy:**

$$\eta_c = \frac{2.098}{2.55} = 82.27\% \qquad R_{\eta_c} = 17.73\%$$

> **Steps explained:** Same 5-step Shannon procedure. The notes stop at the code table (Table 2.20); here we additionally compute $L$, $H$ and the efficiency the question asks for.

---

## Q5 — Shannon code: 2-symbol messages

Given messages and probabilities:

| Symbol | AA | AC | CC | CB | CA | BC | BB |
|--------|-----|-----|-----|-----|-----|-----|-----|
| Prob   | 9/32 | 3/32 | 1/16 | 3/32 | 3/32 | 3/32 | 9/32 |

**Step 1 — Order by decreasing probability** (note $1/16 = 2/32$):

| Order | $P_1$ | $P_2$ | $P_3$ | $P_4$ | $P_5$ | $P_6$ | $P_7$ |
|-------|-------|-------|-------|-------|-------|-------|-------|
| Symbol| AA    | BB    | AC    | CB    | CA    | BC    | CC    |
| $P_i$ | 9/32  | 9/32  | 3/32  | 3/32  | 3/32  | 3/32  | 2/32  |

**Step 2 — Cumulative $\alpha$'s (in /32):**

$$\alpha_1=0,\ \alpha_2=\tfrac{9}{32},\ \alpha_3=\tfrac{18}{32},\ \alpha_4=\tfrac{21}{32},\ \alpha_5=\tfrac{24}{32},\ \alpha_6=\tfrac{27}{32},\ \alpha_7=\tfrac{30}{32},\ \alpha_8=1$$

i.e. $0,\ 0.28125,\ 0.5625,\ 0.65625,\ 0.75,\ 0.84375,\ 0.9375,\ 1$

**Step 3 — Lengths ($2^{l_i}\ge 1/P_i$):**

| Symbol | $P_i$ | $1/P_i$ | $l_i$ |
|--------|-------|---------|-------|
| AA     | 9/32  | 3.56    | 2     |
| BB     | 9/32  | 3.56    | 2     |
| AC     | 3/32  | 10.67   | 4     |
| CB     | 3/32  | 10.67   | 4     |
| CA     | 3/32  | 10.67   | 4     |
| BC     | 3/32  | 10.67   | 4     |
| CC     | 2/32  | 16      | 4     |

**Step 4 & 5 — Binary expansion → code:**

- $\alpha_1=0 \Rightarrow$ `00`
- $\alpha_2=0.28125=(.01)_2 \Rightarrow$ `01`
- $\alpha_3=0.5625=(.1001)_2 \Rightarrow$ `1001`
- $\alpha_4=0.65625=(.1010)_2 \Rightarrow$ `1010`
- $\alpha_5=0.75=(.1100)_2 \Rightarrow$ `1100`
- $\alpha_6=0.84375=(.1101)_2 \Rightarrow$ `1101`
- $\alpha_7=0.9375=(.1111)_2 \Rightarrow$ `1111`

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| AA     | 9/32  | 00   | 2     |
| BB     | 9/32  | 01   | 2     |
| AC     | 3/32  | 1001 | 4     |
| CB     | 3/32  | 1010 | 4     |
| CA     | 3/32  | 1100 | 4     |
| BC     | 3/32  | 1101 | 4     |
| CC     | 2/32  | 1111 | 4     |

**Average length:**

$$L = \frac{1}{32}\big[9(2)+9(2)+3(4)+3(4)+3(4)+3(4)+2(4)\big] = \frac{92}{32} = 2.875 \text{ binits/symbol}$$

**Entropy:**

$$H(S) = 2\left(\tfrac{9}{32}\log_2\tfrac{32}{9}\right) + 4\left(\tfrac{3}{32}\log_2\tfrac{32}{3}\right) + \tfrac{2}{32}\log_2 16 = 1.0294 + 1.2806 + 0.25 = 2.560 \text{ bits/symbol}$$

**Efficiency & redundancy:**

$$\eta_c = \frac{2.560}{2.875} = 89.05\% \qquad R_{\eta_c} = 10.95\%$$

> **Steps explained:** Converting all probabilities to a common denominator (/32) makes ordering and cumulating easy. The rest is the standard 5-step Shannon procedure.

---

## Q6 — Shannon-Fano: 0.4, 0.2, 0.2, 0.1, 0.07, 0.03

*(Identical to Shannon-Fano Problem 1 in notes.)*

**Splitting table:**

| Symbol | $P_i$ | Div 1 | Div 2 | Div 3 | Div 4 | Code | $l_i$ |
|--------|-------|-------|-------|-------|-------|------|-------|
| $x_1$  | 0.4   | 0     | 0     |       |       | 00   | 2     |
| $x_2$  | 0.2   | 0     | 1     |       |       | 01   | 2     |
| $x_3$  | 0.2   | 1     | 0     |       |       | 10   | 2     |
| $x_4$  | 0.1   | 1     | 1     | 0     |       | 110  | 3     |
| $x_5$  | 0.07  | 1     | 1     | 1     | 0     | 1110 | 4     |
| $x_6$  | 0.03  | 1     | 1     | 1     | 1     | 1111 | 4     |

**Average length:**

$$L = 0.4(2)+0.2(2)+0.2(2)+0.1(3)+0.07(4)+0.03(4) = 2.3 \text{ binits/symbol}$$

**Entropy:**

$$H(S) = 0.4\log_2\tfrac{1}{0.4}+2(0.2)\log_2\tfrac{1}{0.2}+0.1\log_2\tfrac{1}{0.1}+0.07\log_2\tfrac{1}{0.07}+0.03\log_2\tfrac{1}{0.03} = 2.209 \text{ bits/symbol}$$

**Efficiency & redundancy:**

$$\eta_c = \frac{2.209}{2.3} = 96.04\% \qquad R_{\eta_c} = 3.96\%$$

> **Steps explained:** First split: {$x_1,x_2$} = 0.6 gets bit `0`, {$x_3,x_4,x_5,x_6$} = 0.4 gets bit `1`. Within the top group, $x_1$→`0`, $x_2$→`1`. Within the bottom group, $x_3$ = 0.2 →`0` while {$x_4,x_5,x_6$} →`1`, and so on. Each division adds one bit; continue until single symbols remain. Then $L$, $H$, and $\eta_c = H/L$.

---

## Q7 — Shannon-Fano with 2nd & 3rd extensions

*(Identical to Shannon-Fano Problem 3 in notes.)*

Source $S=\{s_1,s_2\}$ with $P=\{3/4,\ 1/4\}$.

**Basic source:**

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 3/4   | 0    | 1     |
| $s_2$  | 1/4   | 1    | 1     |

$$L = \tfrac34(1)+\tfrac14(1) = 1 \qquad H(S) = \tfrac34\log_2\tfrac43 + \tfrac14\log_2 4 = 0.8113$$

$$\eta_c^{(1)} = \frac{0.8113}{1} = 81.13\%$$

**2nd extension** ($2^2=4$ symbols):

| Symbol   | $P_i$ | Code | $l_i$ |
|----------|-------|------|-------|
| $s_1s_1$ | 9/16  | 0    | 1     |
| $s_1s_2$ | 3/16  | 10   | 2     |
| $s_2s_1$ | 3/16  | 110  | 3     |
| $s_2s_2$ | 1/16  | 111  | 3     |

$$L_2 = \tfrac{9}{16}(1)+\tfrac{3}{16}(2)+\tfrac{3}{16}(3)+\tfrac{1}{16}(3) = 1.6875$$

$$H(S^2) = 2H(S) = 1.6226 \qquad \eta_c^{(2)} = \frac{1.6226}{1.6875} = 96.15\%$$

**3rd extension** ($2^3=8$ symbols):

| Symbol      | $P_i$  | Code   | $l_i$ |
|-------------|--------|--------|-------|
| $s_1s_1s_1$ | 27/64  | 0      | 1     |
| $s_1s_1s_2$ | 9/64   | 100    | 3     |
| $s_1s_2s_1$ | 9/64   | 101    | 3     |
| $s_2s_1s_1$ | 9/64   | 110    | 3     |
| $s_1s_2s_2$ | 3/64   | 1110   | 4     |
| $s_2s_1s_2$ | 3/64   | 11110  | 5     |
| $s_2s_2s_1$ | 3/64   | 111110 | 6     |
| $s_2s_2s_2$ | 1/64   | 111111 | 6     |

$$L_3 = 2.484375 \qquad H(S^3) = 3H(S) = 2.4339 \qquad \eta_c^{(3)} = \frac{2.4339}{2.484375} = 97.97\%$$

> **Steps explained:** As the extension order $n$ rises, $H(S^n)=nH(S)$ grows in proportion but $L_n$ grows a little slower, so efficiency improves: $81.13\% \to 96.15\% \to 97.97\%$. As $n\to\infty$, $\eta\to 100\%$ (source coding theorem).

---

## Q8 — Shannon-Fano: efficiency ≥ 65%

Source $A=0.05,\ B=0.95$. We extend the source until $\eta_c \ge 65\%$.

**Basic source entropy:**

$$H(S) = 0.05\log_2\tfrac{1}{0.05} + 0.95\log_2\tfrac{1}{0.95} = 0.2161 + 0.0703 = 0.2864 \text{ bits/symbol}$$

**Basic source:** $A\to 0,\ B\to 1$, so $L=1$ and $\eta_c^{(1)} = 0.2864/1 = 28.64\%$ ❌

**2nd extension** ($P$: BB=0.9025, AB=0.0475, BA=0.0475, AA=0.0025):

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| BB     | 0.9025| 0    | 1     |
| AB     | 0.0475| 10   | 2     |
| BA     | 0.0475| 110  | 3     |
| AA     | 0.0025| 111  | 3     |

$$L_2 = 0.9025(1)+0.0475(2)+0.0475(3)+0.0025(3) = 1.1475$$
$$H(S^2)=2H(S)=0.5728 \qquad \eta_c^{(2)} = \frac{0.5728}{1.1475} = 49.9\% \text{ ❌}$$

**3rd extension** ($P$: BBB=0.857375, then three at 0.045125, three at 0.002375, AAA=0.000125):

| Symbol | $P_i$    | Code  | $l_i$ |
|--------|----------|-------|-------|
| BBB    | 0.857375 | 0     | 1     |
| BBA    | 0.045125 | 100   | 3     |
| BAB    | 0.045125 | 101   | 3     |
| ABB    | 0.045125 | 110   | 3     |
| BAA    | 0.002375 | 11100 | 5     |
| ABA    | 0.002375 | 11101 | 5     |
| AAB    | 0.002375 | 11110 | 5     |
| AAA    | 0.000125 | 11111 | 5     |

$$L_3 = 0.857375(1) + 3(0.045125)(3) + 3(0.002375)(5) + 0.000125(5) = 1.2998$$
$$H(S^3)=3H(S)=0.8592 \qquad \eta_c^{(3)} = \frac{0.8592}{1.2998} = 66.1\% \text{ ✓}$$

**Answer: the 3rd extension achieves $\eta_c \approx 66.1\% \ge 65\%$.**

> **Steps explained:** A highly skewed source (0.05 / 0.95) has tiny entropy, so a 1-bit-per-symbol basic code is very wasteful. Encoding *blocks* of symbols (extensions) lets the average length per original symbol approach $H(S)$. We test extensions in turn until the target efficiency is met — here, $n=3$.

---

## Q9 — Shannon-Fano: 100% efficiency case

Symbols and probabilities (all powers of ½):

| Symbol | S0 | S1 | S2 | S3 | S4 | S5 | S6 |
|--------|------|------|-------|-------|-------|--------|--------|
| Prob   | 0.25 | 0.25 | 0.125 | 0.125 | 0.125 | 0.0625 | 0.0625 |

**Shannon-Fano splitting:**

| Symbol | $P_i$  | Code | $l_i$ |
|--------|--------|------|-------|
| S0     | 0.25   | 00   | 2     |
| S1     | 0.25   | 01   | 2     |
| S2     | 0.125  | 100  | 3     |
| S3     | 0.125  | 101  | 3     |
| S4     | 0.125  | 110  | 3     |
| S5     | 0.0625 | 1110 | 4     |
| S6     | 0.0625 | 1111 | 4     |

**Average length:**

$$L = 0.25(2)+0.25(2)+0.125(3)+0.125(3)+0.125(3)+0.0625(4)+0.0625(4) = 2.625$$

**Entropy:**

$$H(S) = 2(0.25)\log_2 4 + 3(0.125)\log_2 8 + 2(0.0625)\log_2 16 = 0.5+0.5+1.125+0.5 = 2.625$$

**Efficiency:**

$$\eta_c = \frac{2.625}{2.625} = \mathbf{100\%}$$

> **Why 100%?** Every probability is an exact negative power of two, $P_i = 2^{-l_i}$. The length rule then gives $l_i = \log_2(1/P_i)$ as an **exact integer**, so each codeword length equals the symbol's self-information. Hence $L = H(S)$ exactly and efficiency is 100%.

---

## Q10 — Shannon-Fano Ternary code

Probabilities: 0.3, 0.3, 0.12, 0.12, 0.06, 0.06, 0.04. Code alphabet $X=\{0,1,2\}$ ($r=3$).

**Ternary splitting** (divide into **three** near-equal groups each step, assign 0/1/2):

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 0.3   | 0    | 1     |
| $s_2$  | 0.3   | 1    | 1     |
| $s_3$  | 0.12  | 20   | 2     |
| $s_4$  | 0.12  | 21   | 2     |
| $s_5$  | 0.06  | 220  | 3     |
| $s_6$  | 0.06  | 221  | 3     |
| $s_7$  | 0.04  | 222  | 3     |

**Average length:**

$$L = 0.3(1)+0.3(1)+0.12(2)+0.12(2)+0.06(3)+0.06(3)+0.04(3) = 1.56 \text{ trinits/symbol}$$

**Entropy (bits):**

$$H(S) = 2(0.3)\log_2\tfrac{1}{0.3} + 2(0.12)\log_2\tfrac{1}{0.12} + 2(0.06)\log_2\tfrac{1}{0.06} + 0.04\log_2\tfrac{1}{0.04} = 2.4493 \text{ bits/symbol}$$

**Entropy in ternary units:**

$$H_3(S) = \frac{H(S)}{\log_2 3} = \frac{2.4493}{1.585} = 1.5453 \text{ trinits/symbol}$$

**Efficiency & redundancy:**

$$\eta_c = \frac{H_3(S)}{L} = \frac{1.5453}{1.56} = 99.06\% \qquad R_{\eta_c} = 0.94\%$$

> **Steps explained:** For an $r$-ary code, split into **$r$** near-equal groups per stage and convert entropy to base-$r$ units via $H_r(S)=H(S)/\log_2 r$. Then $\eta_c = H_r(S)/L$.

---

# UNIT 2

## Q11 — Prefix code & its properties (theory)

**Prefix code:** A code in which **no complete codeword is a prefix of any other codeword**. (A *prefix* of a codeword $X = X_1X_2\dots X_m$ is any leading sub-sequence $X_1\dots X_j$ for $j\le m$.)

**Properties:**

1. **Instantaneous / self-punctuating** — the end of each codeword is recognised immediately, without looking at following symbols (no decoding delay).
2. **Uniquely decodable** — any received bit-stream decodes to exactly one message sequence.
3. **Satisfies the Kraft inequality** — for word lengths $l_1,\dots,l_q$ over an $r$-symbol alphabet:

$$K = \sum_{i=1}^{q} r^{-l_i} \leq 1$$

4. A prefix code with given lengths **exists if and only if** the Kraft inequality holds.

**Example — test for the prefix property:**

| Symbols | S1 | S2 | S3  | S4   |
|---------|----|----|-----|------|
| Code    | 0  | 10 | 110 | 1110 |

The prefixes of `1110` are `1`, `11`, `111` — none is a codeword → **prefix (instantaneous) code**. By contrast, codes `{0, 01, 011, 0111}` fail, since `0`, `01`, `011` are themselves codewords.

**LaTeX (TikZ/forest) — code tree of the prefix code {0, 10, 110, 1110}** (every codeword sits at a *leaf*, never on the path to another — that is what "prefix-free" looks like):

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=4mm, font=\footnotesize}
[{root}
  [{$S_1=0$}, edge label={node[midway,above,font=\tiny]{0}}]
  [{$\bullet$}, edge label={node[midway,below,font=\tiny]{1}}
    [{$S_2=10$}, edge label={node[midway,above,font=\tiny]{0}}]
    [{$\bullet$}, edge label={node[midway,below,font=\tiny]{1}}
      [{$S_3=110$}, edge label={node[midway,above,font=\tiny]{0}}]
      [{$S_4=1110$}, edge label={node[midway,below,font=\tiny]{1}}]]]
]
\end{forest}
\end{document}
```

> **Steps explained:** State the definition, then the three consequences (instantaneous, uniquely decodable, Kraft). The worked check shows how to verify the property: list the prefixes of the longest codeword(s) and confirm none appears as another codeword. In the code tree, a prefix-free code has **all codewords at leaves** — no codeword lies on the branch leading to another.

---

## Q12 — Huffman procedure for r = 2, 3, 4 (theory)

**Number of stages / dummy symbols:**

$$n = \frac{N - r}{r - 1}$$

where $N$ = number of source symbols, $r$ = size of code alphabet. **$n$ must be an integer**; if not, add **dummy symbols of probability 0** until it is.

**Procedure:**

1. Compute $n$; pad with zero-probability dummies if needed.
2. Arrange symbols in **decreasing** order of probability.
3. **Combine the last $r$ symbols** into one composite symbol (sum their probabilities); re-sort.
4. Repeat step 3 on the reduced source until only **$r$ symbols** remain.
5. Assign the **$r$ code symbols** $(0,1,\dots,r-1)$ to these final $r$ symbols.
6. **Work backwards**, expanding each composite symbol by appending the next code symbol, until every original symbol has a codeword. Discard dummies.

**For each $r$:**
- **$r=2$ (binary):** combine the **2** lowest each stage; alphabet $\{0,1\}$. $n=\frac{N-2}{1}=N-2$ (always integer).
- **$r=3$ (ternary):** combine the **3** lowest; alphabet $\{0,1,2\}$. Need $\frac{N-3}{2}$ integer.
- **$r=4$ (quaternary):** combine the **4** lowest; alphabet $\{0,1,2,3\}$. Need $\frac{N-4}{3}$ integer.

> **Steps explained:** Huffman is a *bottom-up* merge: always merge the $r$ least-probable nodes so that rare symbols sink to the deepest (longest) codewords. The dummy-symbol rule guarantees the final reduction lands on exactly $r$ symbols.

---

## Q13 — Huffman: 0.4, 0.3, 0.2, 0.1

$n=\frac{N-r}{r-1}=\frac{4-2}{1}=2$.

**Reduction (combine two lowest each stage):**

```
0.4 ───────────────── 0.4 ──────── 0.6 ─┐
0.3 ──────── 0.3 ─┐   0.3 ──┐       0.4 ─┴─ 1.0
0.2 ──┐      0.3 ─┴── 0.6              
0.1 ──┴─ 0.3                          
```

**Codewords (read back from the tree):**

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 0.4   | 1    | 1     |
| $s_2$  | 0.3   | 00   | 2     |
| $s_3$  | 0.2   | 010  | 3     |
| $s_4$  | 0.1   | 011  | 3     |

**Average length:**

$$L = 0.4(1)+0.3(2)+0.2(3)+0.1(3) = 1.9 \text{ binits/symbol}$$

**Entropy:**

$$H(S) = 0.4\log_2\tfrac{1}{0.4}+0.3\log_2\tfrac{1}{0.3}+0.2\log_2\tfrac{1}{0.2}+0.1\log_2\tfrac{1}{0.1} = 1.846 \text{ bits/symbol}$$

**Efficiency & redundancy:**

$$\eta_c = \frac{1.846}{1.9} = 97.18\% \qquad R_{\eta_c} = 2.82\%$$

**LaTeX (TikZ) — Huffman tree:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{trees}
\begin{document}
\begin{tikzpicture}[level distance=14mm,
  level 1/.style={sibling distance=30mm},
  level 2/.style={sibling distance=16mm},
  every node/.style={draw,circle,inner sep=1.5pt,minimum size=7mm,font=\small}]
\node {1.0}
  child {node {0.6}
    child {node {0.3} edge from parent node[left,draw=none]{0}}   % s2 = 00
    child {node {0.3}                                              % -> 010,011
      child {node {$s_3$} edge from parent node[left,draw=none]{0}}
      child {node {$s_4$} edge from parent node[right,draw=none]{1}}
      edge from parent node[right,draw=none]{1}}
    edge from parent node[left,draw=none]{0}}
  child {node {$s_1$} edge from parent node[right,draw=none]{1}};  % s1 = 1
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Merge 0.2+0.1, then 0.3+0.3, then 0.6+0.4. Label the two branches of every merge 0/1; reading root→leaf gives each codeword. Smaller probability ⇒ deeper leaf ⇒ longer code.

---

## Q14 — Huffman: 0.7, 0.15, 0.15 + 2nd extension

**Basic source** — $n=\frac{3-2}{1}=2$:

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 0.7   | 0    | 1     |
| $s_2$  | 0.15  | 10   | 2     |
| $s_3$  | 0.15  | 11   | 2     |

$$L = 0.7(1)+0.15(2)+0.15(2) = 1.3 \qquad H(S) = 0.7\log_2\tfrac{1}{0.7}+2(0.15)\log_2\tfrac{1}{0.15} = 1.1813$$

$$\eta_c^{(1)} = \frac{1.1813}{1.3} = 90.87\%$$

**LaTeX (TikZ/forest) — basic-source Huffman tree** (internal nodes = combined probability, edges = bit):

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=16mm, s sep=5mm, font=\small}
[{$1.0$}
  [{$s_1$ (0.7) $\to$ 0}, edge label={node[midway,above,font=\tiny]{0}}]
  [{$0.3$}, edge label={node[midway,below,font=\tiny]{1}}
    [{$s_2$ (0.15) $\to$ 10}, edge label={node[midway,above,font=\tiny]{0}}]
    [{$s_3$ (0.15) $\to$ 11}, edge label={node[midway,below,font=\tiny]{1}}]]
]
\end{forest}
\end{document}
```

**2nd extension** ($3^2 = 9$ symbols). With $A=0.7,B=0.15,C=0.15$, the codewords are read from the extended-source Huffman tree below:

| Symbol | $P_i$  | Code | $l_i$ |
|--------|--------|--------|-------|
| AA     | 0.49   | 0      | 1     |
| AB     | 0.105  | 100    | 3     |
| AC     | 0.105  | 101    | 3     |
| BA     | 0.105  | 110    | 3     |
| CA     | 0.105  | 1110   | 4     |
| BB     | 0.0225 | 111100 | 6     |
| BC     | 0.0225 | 111101 | 6     |
| CB     | 0.0225 | 111110 | 6     |
| CC     | 0.0225 | 111111 | 6     |

$$L_2 = 0.49(1) + 3(0.105)(3) + 0.105(4) + 4(0.0225)(6) = 2.395$$

$$H(S^2) = 2H(S) = 2.3626 \qquad \eta_c^{(2)} = \frac{2.3626}{2.395} = 98.65\%$$

**Improvement** $= 98.65\% - 90.87\% = \mathbf{7.78\%}$

**LaTeX (TikZ/forest) — 2nd-extension Huffman tree:**

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=2mm, font=\footnotesize}
[{$1.0$}
  [{AA (0.49)}, edge label={node[midway,above,font=\tiny]{0}}]
  [{$0.51$}, edge label={node[midway,below,font=\tiny]{1}}
    [{$0.21$}, edge label={node[midway,above,font=\tiny]{0}}
      [{AB (0.105)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{AC (0.105)}, edge label={node[midway,below,font=\tiny]{1}}]]
    [{$0.30$}, edge label={node[midway,below,font=\tiny]{1}}
      [{BA (0.105)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{$0.195$}, edge label={node[midway,below,font=\tiny]{1}}
        [{CA (0.105)}, edge label={node[midway,above,font=\tiny]{0}}]
        [{$0.09$}, edge label={node[midway,below,font=\tiny]{1}}
          [{$0.045$}, edge label={node[midway,above,font=\tiny]{0}}
            [{BB (0.0225)}, edge label={node[midway,above,font=\tiny]{0}}]
            [{BC (0.0225)}, edge label={node[midway,below,font=\tiny]{1}}]]
          [{$0.045$}, edge label={node[midway,below,font=\tiny]{1}}
            [{CB (0.0225)}, edge label={node[midway,above,font=\tiny]{0}}]
            [{CC (0.0225)}, edge label={node[midway,below,font=\tiny]{1}}]]]]]]
]
\end{forest}
\end{document}
```

> **Steps explained:** Build the 9-symbol extended source ($P$ = products of the basic probabilities), run Huffman on it, then compare $\eta_c$. Encoding pairs of symbols squeezes more redundancy out, raising efficiency by ~7.8%.

---

## Q15 — Huffman binary & ternary (8 symbols)

Probabilities (sorted): 0.22, 0.20, 0.18, 0.15, 0.10, 0.08, 0.05, 0.02.

First, the **entropy** (used by both parts):

$$H(S) = \sum P_i\log_2\tfrac{1}{P_i} = 2.7536 \text{ bits/symbol}$$

### (i) Binary code — $n=\frac{8-2}{1}=6$

| Symbol | $P_i$ | Code  | $l_i$ |
|--------|-------|-------|-------|
| A      | 0.22  | 10    | 2     |
| B      | 0.20  | 11    | 2     |
| C      | 0.18  | 000   | 3     |
| D      | 0.15  | 001   | 3     |
| E      | 0.10  | 011   | 3     |
| F      | 0.08  | 0100  | 4     |
| G      | 0.05  | 01010 | 5     |
| H      | 0.02  | 01011 | 5     |

$$L = 0.22(2)+0.20(2)+0.18(3)+0.15(3)+0.10(3)+0.08(4)+0.05(5)+0.02(5) = 2.80$$

$$\eta_c = \frac{2.7536}{2.80} = 98.34\%$$

**LaTeX (TikZ/forest) — binary Huffman tree** (internal nodes = combined probability, edges = bit):

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=2mm, font=\footnotesize}
[{$1.0$}
  [{$0.58$}, edge label={node[midway,above,font=\tiny]{0}}
    [{$0.33$}, edge label={node[midway,above,font=\tiny]{0}}
      [{C (0.18)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{D (0.15)}, edge label={node[midway,below,font=\tiny]{1}}]]
    [{$0.25$}, edge label={node[midway,below,font=\tiny]{1}}
      [{$0.15$}, edge label={node[midway,above,font=\tiny]{0}}
        [{F (0.08)}, edge label={node[midway,above,font=\tiny]{0}}]
        [{$0.07$}, edge label={node[midway,below,font=\tiny]{1}}
          [{G (0.05)}, edge label={node[midway,above,font=\tiny]{0}}]
          [{H (0.02)}, edge label={node[midway,below,font=\tiny]{1}}]]]
      [{E (0.10)}, edge label={node[midway,below,font=\tiny]{1}}]]]
  [{$0.42$}, edge label={node[midway,below,font=\tiny]{1}}
    [{A (0.22)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{B (0.20)}, edge label={node[midway,below,font=\tiny]{1}}]]
]
\end{forest}
\end{document}
```

### (ii) Ternary code — $n=\frac{8-3}{3-1}=2.5$ ❌ → add **1 dummy** (prob 0) → $n=\frac{9-3}{2}=3$ ✓

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| A      | 0.22  | 0    | 1     |
| B      | 0.20  | 10   | 2     |
| C      | 0.18  | 11   | 2     |
| D      | 0.15  | 12   | 2     |
| E      | 0.10  | 20   | 2     |
| F      | 0.08  | 21   | 2     |
| G      | 0.05  | 220  | 3     |
| H      | 0.02  | 221  | 3     |
| (dummy)| 0     | 222  | —     |

$$L_3 = 0.22(1)+0.20(2)+0.18(2)+0.15(2)+0.10(2)+0.08(2)+0.05(3)+0.02(3) = 1.85 \text{ trinits/symbol}$$

$$H_3(S) = \frac{H(S)}{\log_2 3} = \frac{2.7536}{1.585} = 1.7373 \qquad \eta_c = \frac{1.7373}{1.85} = 93.91\%$$

**LaTeX (TikZ/forest) — ternary Huffman tree** (3-way merges; edges = ternary digit 0/1/2):

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=2mm, font=\footnotesize}
[{$1.0$}
  [{A (0.22)}, edge label={node[midway,above,font=\tiny]{0}}]
  [{$0.53$}, edge label={node[midway,above,font=\tiny]{1}}
    [{B (0.20)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{C (0.18)}, edge label={node[midway,above,font=\tiny]{1}}]
    [{D (0.15)}, edge label={node[midway,below,font=\tiny]{2}}]]
  [{$0.25$}, edge label={node[midway,below,font=\tiny]{2}}
    [{E (0.10)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{F (0.08)}, edge label={node[midway,above,font=\tiny]{1}}]
    [{$0.07$}, edge label={node[midway,below,font=\tiny]{2}}
      [{G (0.05)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{H (0.02)}, edge label={node[midway,above,font=\tiny]{1}}]
      [{dummy (0)}, edge label={node[midway,below,font=\tiny]{2}}]]]
]
\end{forest}
\end{document}
```

> **Steps explained:** Binary merges 2-at-a-time; ternary merges 3-at-a-time but only after padding with a dummy so the reduction ends on exactly 3 symbols. Note the ternary code is **less** efficient — the next question explains why.

---

## Q16 — Huffman binary & quaternary

Source A–H: 0.22, 0.20, 0.18, 0.15, 0.10, 0.08, 0.05, 0.02 (same set as Q15). $H(S)=2.7536$ bits/symbol.

### (i) Binary Huffman

Same as Q15(i): $L = 2.80$ binits/symbol, $\eta_c = \mathbf{98.34\%}$.

### (ii) Quaternary Huffman ($r=4$)

$n=\frac{8-4}{4-1}=\frac{4}{3}=1.33$ ❌ → add **2 dummies** → $n=\frac{10-4}{3}=2$ ✓

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| A      | 0.22  | 0    | 1     |
| B      | 0.20  | 1    | 1     |
| C      | 0.18  | 2    | 1     |
| D      | 0.15  | 30   | 2     |
| E      | 0.10  | 31   | 2     |
| F      | 0.08  | 32   | 2     |
| G      | 0.05  | 330  | 3     |
| H      | 0.02  | 331  | 3     |
| (2 dummies) | 0 | 332, 333 | — |

$$L_4 = 0.22(1)+0.20(1)+0.18(1)+0.15(2)+0.10(2)+0.08(2)+0.05(3)+0.02(3) = 1.47 \text{ quaternary units/symbol}$$

$$H_4(S) = \frac{H(S)}{\log_2 4} = \frac{2.7536}{2} = 1.3768 \qquad \eta_c = \frac{1.3768}{1.47} = 93.66\%$$

**Conclusion:** quaternary $\eta_c = 93.66\% <$ binary $98.34\%$ — the quaternary code is **worse**.

*(The binary Huffman tree for part (i) is the one drawn in [Q15(i)](#q15--huffman-binary--ternary-8-symbols) — same 8 symbols.)*

**LaTeX (TikZ/forest) — quaternary Huffman tree** (4-way merges; edges = quaternary digit 0/1/2/3):

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=2mm, font=\footnotesize}
[{$1.0$}
  [{A (0.22)}, edge label={node[midway,above,font=\tiny]{0}}]
  [{B (0.20)}, edge label={node[midway,above,font=\tiny]{1}}]
  [{C (0.18)}, edge label={node[midway,above,font=\tiny]{2}}]
  [{$0.40$}, edge label={node[midway,below,font=\tiny]{3}}
    [{D (0.15)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{E (0.10)}, edge label={node[midway,above,font=\tiny]{1}}]
    [{F (0.08)}, edge label={node[midway,above,font=\tiny]{2}}]
    [{$0.07$}, edge label={node[midway,below,font=\tiny]{3}}
      [{G (0.05)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{H (0.02)}, edge label={node[midway,above,font=\tiny]{1}}]
      [{dummy (0)}, edge label={node[midway,above,font=\tiny]{2}}]
      [{dummy (0)}, edge label={node[midway,below,font=\tiny]{3}}]]]
]
\end{forest}
\end{document}
```

> **Why worse?** A larger $r$ forces each symbol into fewer, "wider" digits, and the padding dummies waste part of the code space. As $r$ increases, the granularity of length choices gets coarser, so $L$ cannot hug $H_r(S)$ as tightly ⇒ efficiency drops.

---

## Q17 — Huffman: 100% efficiency case

Symbols (all powers of ½): S0=0.25, S1=0.25, S2=0.125, S3=0.125, S4=0.125, S5=0.0625, S6=0.0625. Combine **as high as possible**.

| Symbol | $P_i$  | Code | $l_i$ |
|--------|--------|------|-------|
| S0     | 0.25   | 00   | 2     |
| S1     | 0.25   | 10   | 2     |
| S2     | 0.125  | 010  | 3     |
| S3     | 0.125  | 011  | 3     |
| S4     | 0.125  | 110  | 3     |
| S5     | 0.0625 | 1110 | 4     |
| S6     | 0.0625 | 1111 | 4     |

**Average length:**

$$L = 2(0.25)(2) + 3(0.125)(3) + 2(0.0625)(4) = 1.0 + 1.125 + 0.5 = 2.625$$

**Entropy:**

$$H(S) = 2(0.25)\log_2 4 + 3(0.125)\log_2 8 + 2(0.0625)\log_2 16 = 2.625$$

**Efficiency:** $\eta_c = \dfrac{2.625}{2.625} = \mathbf{100\%}$

**LaTeX (TikZ/forest) — Huffman tree (combine as high as possible):**

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=2mm, font=\footnotesize}
[{$1.0$}
  [{$0.50$}, edge label={node[midway,above,font=\tiny]{0}}
    [{S0 (0.25)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{$0.25$}, edge label={node[midway,below,font=\tiny]{1}}
      [{S2 (0.125)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{S3 (0.125)}, edge label={node[midway,below,font=\tiny]{1}}]]]
  [{$0.50$}, edge label={node[midway,below,font=\tiny]{1}}
    [{S1 (0.25)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{$0.25$}, edge label={node[midway,below,font=\tiny]{1}}
      [{S4 (0.125)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{$0.125$}, edge label={node[midway,below,font=\tiny]{1}}
        [{S5 (0.0625)}, edge label={node[midway,above,font=\tiny]{0}}]
        [{S6 (0.0625)}, edge label={node[midway,below,font=\tiny]{1}}]]]]
]
\end{forest}
\end{document}
```

> **Why 100%?** Each $P_i = 2^{-l_i}$ is an exact power of ½, so the optimal (Huffman) length equals the self-information $l_i = \log_2(1/P_i)$ — an integer. Therefore $L = H(S)$ exactly, giving 100% efficiency. (Same reason as the Shannon-Fano case in Q9.)

---

## Q18 — Huffman: composite high vs low + variance

Probabilities: 0.25, 0.20, 0.15, 0.15, 0.10, 0.05, 0.05, 0.05. $n=\frac{8-2}{1}=6$.

Both placements give the **same** average length:

$$L = 2.80 \text{ binits/symbol}$$

### (i) Composite placed **as LOW as possible** (skewed tree)

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 0.25  | 00    | 2     |
| $s_2$  | 0.20  | 01    | 2     |
| $s_3$  | 0.15  | 100   | 3     |
| $s_4$  | 0.15  | 101   | 3     |
| $s_5$  | 0.10  | 110   | 3     |
| $s_8$  | 0.05  | 1110  | 4     |
| $s_6$  | 0.05  | 11110 | 5     |
| $s_7$  | 0.05  | 11111 | 5     |

$$\text{Var}(l) = \sum P_i (l_i - L)^2$$
$$= 0.25(0.64)+0.20(0.64)+0.15(0.04)+0.15(0.04)+0.10(0.04)+0.05(4.84)+0.05(4.84)+0.05(1.44)$$
$$= \mathbf{0.86}$$

**LaTeX (TikZ/forest) — "as low as possible" Huffman tree** (composite pushed down → one deep tail):

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=2mm, font=\footnotesize}
[{$1.0$}
  [{$0.45$}, edge label={node[midway,above,font=\tiny]{0}}
    [{$s_1$ (0.25)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{$s_2$ (0.20)}, edge label={node[midway,below,font=\tiny]{1}}]]
  [{$0.55$}, edge label={node[midway,below,font=\tiny]{1}}
    [{$0.30$}, edge label={node[midway,above,font=\tiny]{0}}
      [{$s_3$ (0.15)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{$s_4$ (0.15)}, edge label={node[midway,below,font=\tiny]{1}}]]
    [{$0.25$}, edge label={node[midway,below,font=\tiny]{1}}
      [{$s_5$ (0.10)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{$0.15$}, edge label={node[midway,below,font=\tiny]{1}}
        [{$s_8$ (0.05)}, edge label={node[midway,above,font=\tiny]{0}}]
        [{$0.10$}, edge label={node[midway,below,font=\tiny]{1}}
          [{$s_6$ (0.05)}, edge label={node[midway,above,font=\tiny]{0}}]
          [{$s_7$ (0.05)}, edge label={node[midway,below,font=\tiny]{1}}]]]]]
]
\end{forest}
\end{document}
```

### (ii) Composite placed **as HIGH as possible** (balanced tree)

| Symbol | $P_i$ | Code | $l_i$ |
|--------|-------|------|-------|
| $s_1$  | 0.25  | 00   | 2     |
| $s_2$  | 0.20  | 01   | 2     |
| $s_3$  | 0.15  | 100  | 3     |
| $s_4$  | 0.15  | 110  | 3     |
| $s_5$  | 0.10  | 1010 | 4     |
| $s_6$  | 0.05  | 1110 | 4     |
| $s_7$  | 0.05  | 1111 | 4     |
| $s_8$  | 0.05  | 1011 | 4     |

$$\text{Var}(l) = 0.25(0.64)+0.20(0.64)+0.15(0.04)+0.15(0.04)+0.10(1.44)+0.05(1.44)+0.05(1.44)+0.05(1.44)$$
$$= \mathbf{0.66}$$

> **Comment:** Both codes are optimal ($L=2.80$), but placing the composite symbol **as high as possible gives the smaller variance** (0.66 vs 0.86). Lower variance ⇒ more uniform codeword lengths ⇒ steadier buffer/transmission behaviour. This is the preferred placement.

**LaTeX (TikZ/forest) — "as high as possible" Huffman tree** (balanced; all four rare symbols at depth 4):

```latex
\documentclass{standalone}
\usepackage{forest}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west, edge={-Stealth},
          l sep=14mm, s sep=2mm, font=\footnotesize}
[{$1.0$}
  [{$0.45$}, edge label={node[midway,above,font=\tiny]{0}}
    [{$s_1$ (0.25)}, edge label={node[midway,above,font=\tiny]{0}}]
    [{$s_2$ (0.20)}, edge label={node[midway,below,font=\tiny]{1}}]]
  [{$0.55$}, edge label={node[midway,below,font=\tiny]{1}}
    [{$0.30$}, edge label={node[midway,above,font=\tiny]{0}}
      [{$s_3$ (0.15)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{$0.15$}, edge label={node[midway,below,font=\tiny]{1}}
        [{$s_5$ (0.10)}, edge label={node[midway,above,font=\tiny]{0}}]
        [{$s_8$ (0.05)}, edge label={node[midway,below,font=\tiny]{1}}]]]
    [{$0.25$}, edge label={node[midway,below,font=\tiny]{1}}
      [{$s_4$ (0.15)}, edge label={node[midway,above,font=\tiny]{0}}]
      [{$0.10$}, edge label={node[midway,below,font=\tiny]{1}}
        [{$s_6$ (0.05)}, edge label={node[midway,above,font=\tiny]{0}}]
        [{$s_7$ (0.05)}, edge label={node[midway,below,font=\tiny]{1}}]]]]
]
\end{forest}
\end{document}
```

> **Steps explained:** When a merged (composite) node ties in probability with an original symbol, the "high" rule keeps the tree balanced, so all four rare symbols sit at depth 4 (lengths $\{2,2,3,3,4,4,4,4\}$, variance 0.66). The "low" rule delays merges, creating one deep tail (lengths $\{2,2,3,3,3,4,5,5\}$, variance 0.86). The mean length $L=2.80$ is unchanged either way.

---

## Q19 — Arithmetic coding of 'YXZ'

Source $S=(X,Y,Z)$ with $P=(0.6, 0.2, 0.2)$. Cumulative intervals on $[0,1)$:

| Symbol | Probability | Interval        |
|--------|-------------|-----------------|
| X      | 0.6         | $[0.0,\ 0.6)$   |
| Y      | 0.2         | $[0.6,\ 0.8)$   |
| Z      | 0.2         | $[0.8,\ 1.0)$   |

Encode **Y → X → Z**, each time narrowing to the sub-interval. For symbol with sub-interval $[c,\,d)$ inside current $[\text{low},\text{high})$ with range $= \text{high}-\text{low}$:

$$\text{new low} = \text{low} + \text{range}\times c, \qquad \text{new high} = \text{low} + \text{range}\times d$$

**Step 1 — Y** on $[0,1)$, range $=1$, Y$=[0.6,0.8)$:

$$\text{low}=0+1(0.6)=0.6,\quad \text{high}=0+1(0.8)=0.8 \;\Rightarrow\; [0.6,\ 0.8),\ \text{range}=0.2$$

**Step 2 — X** on $[0.6,0.8)$, range $=0.2$, X$=[0,0.6)$:

$$\text{low}=0.6+0.2(0)=0.6,\quad \text{high}=0.6+0.2(0.6)=0.72 \;\Rightarrow\; [0.6,\ 0.72),\ \text{range}=0.12$$

**Step 3 — Z** on $[0.6,0.72)$, range $=0.12$, Z$=[0.8,1.0)$:

$$\text{low}=0.6+0.12(0.8)=0.696,\quad \text{high}=0.6+0.12(1.0)=0.72 \;\Rightarrow\; [0.696,\ 0.72)$$

**Final tag:** any number in $[0.696,\ 0.72)$ encodes 'YXZ'. Taking the lower limit:

$$\boxed{\text{codeword} = 0.696}$$

(the midpoint $0.708$ is an equally valid representative value).

**LaTeX (TikZ) — interval-subdivision diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\begin{document}
\begin{tikzpicture}[xscale=12,yscale=1]
% Stage 0: [0,1)
\draw (0,3) -- (1,3);
\foreach \x/\l in {0/0,0.6/0.6,0.8/0.8,1/1}
  \draw (\x,3.05)--(\x,2.95) node[below,font=\scriptsize]{\l};
\node[font=\scriptsize] at (0.3,3.2){X}; \node[font=\scriptsize] at (0.7,3.2){Y};
\node[font=\scriptsize] at (0.9,3.2){Z};
% Stage 1: Y -> [0.6,0.8)
\draw[blue,very thick] (0.6,3)--(0.8,3);
\draw (0.6,2) -- (0.8,2);
\foreach \x/\l in {0.6/0.6,0.72/0.72,0.76/0.76,0.8/0.8}
  \draw (\x,2.05)--(\x,1.95) node[below,font=\scriptsize]{\l};
\node[font=\scriptsize] at (0.66,2.2){X}; \node[font=\scriptsize] at (0.74,2.2){Y};
\node[font=\scriptsize] at (0.78,2.2){Z};
\draw[->,gray] (0.7,2.9)--(0.7,2.1);
% Stage 2: X -> [0.6,0.72)
\draw[blue,very thick] (0.6,2)--(0.72,2);
\draw (0.6,1) -- (0.72,1);
\foreach \x/\l in {0.6/0.6,0.696/0.696,0.72/0.72}
  \draw (\x,1.05)--(\x,0.95) node[below,font=\scriptsize]{\l};
\node[font=\scriptsize] at (0.78,1){$\leftarrow$ Z $=[0.696,0.72)$};
\draw[->,gray] (0.66,1.9)--(0.66,1.1);
\draw[red,very thick] (0.696,1)--(0.72,1);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Arithmetic coding maps the whole message to a single sub-interval of $[0,1)$. Each symbol rescales the current interval to its own probability slice. After all symbols, **any** real number inside the final interval uniquely identifies the message.

---

## Q20 — Lempel-Ziv encoding

Encode `THIS_IS_HIS_HIT` (with `_` = space) using the **LZ78** dictionary method.

**Parsing into phrases** (scan left→right; each new phrase = longest match already in the dictionary + one new symbol):

| # | Phrase | = (prefix index, new char) | Dictionary entry |
|---|--------|----------------------------|------------------|
| 1 | `T`    | (0, T)                     | T   |
| 2 | `H`    | (0, H)                     | H   |
| 3 | `I`    | (0, I)                     | I   |
| 4 | `S`    | (0, S)                     | S   |
| 5 | `_`    | (0, _)                     | _   |
| 6 | `IS`   | (3, S)                     | I·S |
| 7 | `_H`   | (5, H)                     | _·H |
| 8 | `IS_`  | (6, _)                     | IS·_ |
| 9 | `HI`   | (2, I)                     | H·I |
| 10| `T`    | (1, —)  *(end; T already entry 1)* | T |

**Encoded output (sequence of tokens):**

$$(0,T)\,(0,H)\,(0,I)\,(0,S)\,(0,\_)\,(3,S)\,(5,H)\,(6,\_)\,(2,I)\,(1,-)$$

**Binary form:** each token = (dictionary pointer in binary) + (new symbol). The pointer for the $k$-th token needs $\lceil \log_2 k\rceil$ bits, e.g. token 6 `(3,S)` → pointer `011` followed by the codeword for `S`.

> **Steps explained:** LZ78 grows a dictionary on the fly. Repeated substrings (`IS`, `_H`, `IS_`, `HI`) are coded as a short pointer to an earlier phrase plus one new character, so longer/repeated text compresses more as the dictionary fills. The decoder rebuilds the identical dictionary from the same token stream.

---

# 📋 Master Formula Sheet

### 1. Self-information / Entropy

$$H(S) = \sum_{i=1}^{q} P_i \log_2\frac{1}{P_i} \ \text{bits/symbol}$$

- $P_i$ = probability of symbol $i$; $q$ = number of symbols.
- **Used in:** Q3, Q4, Q5, Q6, Q7, Q8, Q9, Q10, Q13, Q14, Q15, Q16, Q17, Q18.

### 2. Average codeword length

$$L = \sum_{i=1}^{q} P_i\, l_i$$

- $l_i$ = length of the codeword for symbol $i$ (binits / trinits / etc.).
- **Used in:** every numerical question (Q3–Q10, Q13–Q18).

### 3. Code efficiency

$$\eta_c = \frac{H(S)}{L} \quad\text{(binary)} \qquad \eta_c = \frac{H_r(S)}{L} \quad(r\text{-ary})$$

- **Used in:** Q3, Q4, Q5, Q6, Q7, Q8, Q9, Q10, Q13, Q14, Q15, Q16, Q17.

### 4. Code redundancy

$$R_{\eta_c} = 1 - \eta_c$$

- **Used in:** Q3, Q4, Q5, Q6, Q10, Q13.

### 5. Entropy in $r$-ary units

$$H_r(S) = \frac{H(S)}{\log_2 r}$$

- Converts bits → trinits ($r=3$) / quaternary units ($r=4$).
- **Used in:** Q10, Q15(ii), Q16(ii).

### 6. Shannon length rule

$$2^{l_i} \geq \frac{1}{P_i} \quad\Rightarrow\quad l_i = \left\lceil \log_2\tfrac{1}{P_i} \right\rceil$$

- Smallest integer $l_i$ satisfying the inequality.
- **Used in:** Q1, Q3, Q4, Q5.

### 7. Shannon cumulative probabilities

$$\alpha_1 = 0, \qquad \alpha_{i+1} = P_i + \alpha_i$$

- $\alpha_i$ is expanded in binary to $l_i$ places to get the codeword.
- **Used in:** Q1, Q3, Q4, Q5.

### 8. Extension-source entropy

$$H(S^n) = n\,H(S)$$

- $n$ = order of extension. Efficiency improves with $n$; $\eta\to 100\%$ as $n\to\infty$.
- **Used in:** Q7, Q8, Q14.

### 9. Huffman number of stages / dummies

$$n = \frac{N - r}{r - 1}$$

- $N$ = number of source symbols, $r$ = code-alphabet size. Pad with **zero-probability dummies** until $n$ is an integer.
- **Used in:** Q12, Q13, Q14, Q15, Q16, Q17, Q18.

### 10. Variance of codeword length

$$\text{Var}(l) = \sum_{i=1}^{q} P_i\,(l_i - L)^2$$

- Measures spread of codeword lengths; smaller is better ("composite high").
- **Used in:** Q18.

### 11. Kraft–McMillan inequality

$$K = \sum_{i=1}^{q} r^{-l_i} \leq 1$$

- Necessary & sufficient for an **instantaneous (prefix)** code to exist with lengths $l_i$.
- **Used in:** Q11.

### 12. Source coding theorem (bound on $L$)

$$H_r(S) \leq L \leq 1 + H_r(S) \qquad\Rightarrow\qquad L \geq H_r(S)$$

- The average length can never be less than the (base-$r$) entropy.
- **Used in:** Q2, Q11 (background), Q7/Q8 (why efficiency improves).

### 13. Arithmetic-coding interval update

$$\text{low}' = \text{low} + \text{range}\times c, \qquad \text{high}' = \text{low} + \text{range}\times d$$

- $[c,d)$ = cumulative interval of the current symbol; range $=\text{high}-\text{low}$.
- **Used in:** Q19.

### 14. Probability of 0's and 1's in a code

$$P(0) = \frac{1}{L}\sum_i (\text{no. of 0's in code }i)\,P_i, \qquad P(1) = \frac{1}{L}\sum_i (\text{no. of 1's in code }i)\,P_i$$

- **Used in:** (Shannon-Fano Problem 2 in notes; handy if asked.)

---

# Quick Answer Table

| Q  | Key answers |
|----|-------------|
| 3  | Codes 0/10/110; $L=1.7$, $H=1.4855$, $\eta_c=87.38\%$, $R=12.62\%$ |
| 4  | Codes 00/01/101/1100/1110; $L=2.55$, $H=2.098$, $\eta_c=82.27\%$, $R=17.73\%$ |
| 5  | $L=2.875$, $H=2.560$, $\eta_c=89.05\%$, $R=10.95\%$ |
| 6  | $L=2.3$, $H=2.209$, $\eta_c=96.04\%$, $R=3.96\%$ |
| 7  | $\eta^{(1)}=81.13\%$, $\eta^{(2)}=96.15\%$, $\eta^{(3)}=97.97\%$ |
| 8  | Basic 28.64%, 2nd ext 49.9%, **3rd ext 66.1% ≥ 65%** |
| 9  | $L=H=2.625$, $\eta_c=100\%$ (probs are powers of ½) |
| 10 | $L=1.56$, $H_3=1.5453$, $\eta_c=99.06\%$, $R=0.94\%$ |
| 13 | Codes 1/00/010/011; $L=1.9$, $H=1.846$, $\eta_c=97.18\%$, $R=2.82\%$ |
| 14 | $\eta^{(1)}=90.87\%$, $\eta^{(2)}=98.65\%$, improvement $=7.78\%$ |
| 15 | Binary $L=2.8$, $\eta=98.34\%$; Ternary $L=1.85$, $\eta=93.91\%$ |
| 16 | Binary $\eta=98.34\%$; Quaternary $L=1.47$, $\eta=93.66\%$ (worse) |
| 17 | $L=H=2.625$, $\eta_c=100\%$ |
| 18 | $L=2.8$ both; Var(low)$=0.86$, Var(high)$=0.66$ → high is better |
| 19 | Final interval $[0.696, 0.72)$; codeword $=0.696$ |
| 20 | Tokens $(0,T)(0,H)(0,I)(0,S)(0,\_)(3,S)(5,H)(6,\_)(2,I)(1,-)$ |

---

### Exam tips

- **Shannon vs Shannon-Fano vs Huffman:** all need the same final trio — compute $L$, compute $H(S)$, then $\eta_c = H/L$. Only the *codeword construction* differs.
- **Extensions:** remember $H(S^n) = nH(S)$; compute $L_n$ from the extended table, then $\eta = nH(S)/L_n$. Efficiency always rises with $n$.
- **$r$-ary codes:** convert entropy with $H_r = H/\log_2 r$ before dividing by $L$, and check the dummy-symbol rule $n=(N-r)/(r-1)$.
- **100% efficiency** happens **iff** every $P_i$ is a power of $1/r$ (then $l_i = \log_r(1/P_i)$ is an integer).
- **Variance:** "composite as high as possible" ⇒ lower variance (more uniform lengths); $L$ is identical either way.
- The TikZ blocks are complete standalone documents — paste into Overleaf and compile directly.

---

*Module 2 — Source Coding. Math renders on GitHub via native LaTeX ($\dots$ / $$\dots$$) support.*
