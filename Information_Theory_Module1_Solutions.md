# Module 1 — Information Theory · Question Bank Solutions

> Complete, step-by-step solutions to all 20 questions (Unit 1 & Unit 2), faithful to the class notes by **Dr. Madhavi M (ECE, GAT)**.
> Each question includes a worked solution, a short **Steps explained** note, and **LaTeX (TikZ / pgfplots)** code for every diagram.
> A full **formula sheet** is given at the end.

> **Handy constant used throughout:** $\log_2 x = 3.3219 \times \log_{10} x$
> Quick values: $\log_2 2 = 1$, $\log_2 3 = 1.585$, $\log_2 4 = 2$, $\log_2 5 = 2.322$, $\log_2 6 = 2.585$, $\log_2 7 = 2.807$, $\log_2 10 = 3.322$, $\log_2 13 = 3.70$, $\log_2 52 = 5.70$.

---

## Table of Contents

**Unit 1**
- [Q1 — Explain the five terms](#q1--explain-the-five-terms-theory)
- [Q2 — Reasons for log measure + derivation](#q2--reasons-for-logarithmic-measure--derive-average-information-theory--derivation)
- [Q3 — Card: spade / ace / ace of spades](#q3--card-spade--ace--ace-of-spades)
- [Q4 — Black & white TV picture](#q4--black--white-tv-picture)
- [Q5 — Telegraph source](#q5--telegraph-source)
- [Q6 — Card: red suit](#q6--card-red-suit)
- [Q7 — Binary source entropy plot](#q7--binary-source-entropy-plot)
- [Q8 — Dot–dash code](#q8--dotdash-code)
- [Q9 — Source x, y: efficiency & redundancy](#q9--source-x-y-with-p--055-045-efficiency--redundancy)
- [Q10 — A, B, C: efficiency, redundancy, Hartley & Nats](#q10--a-b-c-with-p--07-015-015)

**Unit 2 — Markov Sources**
- [Q11 — 3-state symmetric source](#q11--3-state-symmetric-source-pi13)
- [Q12 — Car heading reporter](#q12--car-heading-reporter)
- [Q13 — Students abroad / MBA / industry](#q13--students-abroad--mba--industry)
- [Q14 — Markov source (X, Y, Z)](#q14--markov-source-x-y-z)
- [Q15 — Diamond 4-state source](#q15--diamond-4-state-source)
- [Q16 — Triangular asymmetric 3-state](#q16--triangular-asymmetric-3-state)
- [Q17 — Two-state, find H, G₁, G₂, G₃](#q17--two-state-find-h-g₁-g₂-g₃)
- [Q18 — Two-state A, B](#q18--two-state-a-b)
- [Q19 — Second-order Markov (00, 01, 10, 11)](#q19--second-order-markov-00-01-10-11)
- [Q20 — Two-state asymmetric, find H, G₁, G₂, G₃](#q20--two-state-asymmetric-find-h-g₁-g₂-g₃)

- [📋 Master Formula Sheet](#-master-formula-sheet)
- [Quick Answer Table](#quick-answer-table)

---

# UNIT 1

## Q1 — Explain the five terms (Theory)

**(i) Self-information** — The information conveyed by a single message/symbol. A rare message carries more information; a certain message carries none.

$$I_k = \log_2\frac{1}{P_k} = -\log_2 P_k \quad \text{bits}$$

where $P_k$ = probability of message $k$.

**(ii) Average self-information (Entropy)** — The average information per symbol over all symbols of the source.

$$H(S) = \sum_{k=1}^{N} P_k \log_2\frac{1}{P_k}\quad \text{bits/symbol}$$

**(iii) Information rate (R)** — Average information produced by the source **per second**.

$$R = r_s \, H(S)\quad \text{bits/sec}$$

**(iv) Symbol rate ($r_s$)** — Number of symbols emitted by the source **per second** (symbols/sec).

**(v) Redundancy** — The fraction of the source's capacity that is "wasted" because the symbols are not equally likely.

$$\text{Redundancy} = 1 - \eta = 1 - \frac{H(S)}{H_{max}} \qquad H_{max}=\log_2 N$$

where $\eta$ = efficiency, $N$ = number of symbols.

> **Steps explained:** These are pure definitions. The linking idea: information $\uparrow$ as probability $\downarrow$ (i); average it $\rightarrow$ entropy (ii); multiply by speed $\rightarrow$ rate (iii, iv); compare actual entropy to the maximum possible $\rightarrow$ efficiency/redundancy (v).

---

## Q2 — Reasons for logarithmic measure + derive average information (Theory + Derivation)

**Reasons for choosing a logarithmic measure:**

1. **Information cannot be negative** — since $0 \le P_k \le 1$, $\log(1/P_k) \ge 0$.
2. **A sure event carries zero information** — if $P_k = 1$, then $\log(1/1) = 0$.
3. **A less likely message carries more information** — $\log(1/P_k)$ increases as $P_k$ decreases.
4. **Additivity for independent messages** — $\log\dfrac{1}{P_1 P_2}=\log\dfrac{1}{P_1}+\log\dfrac{1}{P_2}$, so $I(m_1,m_2)=I(m_1)+I(m_2)$. Only the logarithm gives this property.

**Derivation of average information (entropy) of a long independent sequence:**

A zero-memory source emits symbols $S=\{s_1,s_2,\dots,s_N\}$ with probabilities $\{P_1,P_2,\dots,P_N\}$. Consider a long message of **L** symbols.

- Number of times $s_1$ occurs $= P_1 L$, contributing $P_1 L \log_2\dfrac{1}{P_1}$ bits
- Number of times $s_2$ occurs $= P_2 L$, contributing $P_2 L \log_2\dfrac{1}{P_2}$ bits
- … and so on up to $s_N$.

Total information:

$$I_{Total}= L\left[P_1\log_2\tfrac{1}{P_1}+P_2\log_2\tfrac{1}{P_2}+\dots+P_N\log_2\tfrac{1}{P_N}\right]$$

Average information per symbol = entropy:

$$H(S)=\frac{I_{Total}}{L}=\sum_{k=1}^{N}P_k\log_2\frac{1}{P_k}\ \text{bits/symbol}$$

> **Steps explained:** State the 4 properties that only "log" satisfies. Then count how many of each symbol appear in L symbols, multiply each count by its self-information, sum, and divide by L. The L cancels, leaving entropy.

---

## Q3 — Card: spade / ace / ace of spades

*(Same style as Example 8 in notes)*

Deck = 52 cards (13 each of spade, heart, diamond, club).

**(i) Told it is a spade:** $P = \dfrac{13}{52}=\dfrac14$

$$I_{spade}=\log_2\frac{1}{1/4}=\log_2 4 = 2\ \text{bits}$$

**(ii) Told it is an ace:** 4 aces, $P=\dfrac{4}{52}=\dfrac{1}{13}$

$$I_{ace}=\log_2 13 = 3.70\ \text{bits}$$

**(iii) Told it is the ace of spades:** only 1 such card, $P=\dfrac{1}{52}$

$$I_{ace\text{-}spade}=\log_2 52 = 5.70\ \text{bits}$$

**(iv)** $I_{spade}+I_{ace}=2+3.70=5.70=I_{ace\text{-}spade}$. **Yes** — because "being a spade" and "being an ace" are **independent** events, so their information adds.

> **Steps explained:** Each part: probability = (favourable cards)/52, then apply $I=\log_2(1/P)$. Part (iv) checks additivity of independent events.

---

## Q4 — Black & white TV picture

*(Same as Example 13 in notes)*

Given: 525 lines × 525 pixels, 256 brightness levels/pixel, 30 frames/sec.

**Pixels per frame:** $525\times525 = 275{,}625$

**Possible frames (all equally likely):** $q=(256)^{275625}$

**Information per frame (maximum entropy):**

$$I=\log_2 q = 275625 \times \log_2 256 = 275625 \times 8 = 22.05\times10^{5}\ \text{bits/frame}$$

**Information rate:**

$$R=r_s \, I = 30 \times 22.05\times10^{5} = 66.15\times10^{5}\ \text{bits/sec}$$

> **Steps explained:** Count total pixels $\rightarrow$ total possible frames is $(\text{levels})^{(\text{pixels})}$. Equally likely $\Rightarrow$ info/frame $= \log_2 q = \text{pixels}\times\log_2(\text{levels}) = 275625\times8$. Multiply by frame rate (30/s) for $R$.

---

## Q5 — Telegraph source

**Dash twice as long as a dot and half as probable, $\tau_{dot}=0.2$ s.**

**Probabilities:** dash is half as probable: $P_{dash}=\tfrac12 P_{dot}$, and $P_{dot}+P_{dash}=1$:

$$P_{dot}+\tfrac12 P_{dot}=1 \Rightarrow P_{dot}=\tfrac23 \quad P_{dash}=\tfrac13$$

**Entropy:**

$$H=\tfrac23\log_2\tfrac32+\tfrac13\log_2 3 = \tfrac23(0.585)+\tfrac13(1.585)=0.390+0.528=0.918\ \text{bits/symbol}$$

**Durations:** $\tau_{dot}=0.2$ s, $\tau_{dash}=2\times0.2=0.4$ s. Average time per symbol:

$$\bar\tau = P_{dot}\tau_{dot}+P_{dash}\tau_{dash}=\tfrac23(0.2)+\tfrac13(0.4)=0.2667\ \text{s}$$

$$r_s=\frac{1}{\bar\tau}=3.75\ \text{symbols/sec}$$

**Entropy (information) rate:**

$$R=r_s\,H=3.75\times0.918=3.44\ \text{bits/sec}$$

> **Steps explained:** "Half as probable" + "sum = 1" gives the two probabilities. Compute $H$. Then average symbol duration (weighted by probabilities), invert to get symbol rate, multiply $H \times r_s$.

---

## Q6 — Card: red suit

**Told it is red:** 26 red cards, $P=\dfrac{26}{52}=\dfrac12$

$$I_{red}=\log_2 2 = 1\ \text{bit}$$

**Information needed to fully specify the card:** total $= \log_2 52 = 5.70$ bits.

$$\text{More info needed}=5.70-1=4.70\ \text{bits}$$

(Check: given it is red, picking 1 of 26 $= \log_2 26 = 4.70$ bits ✓)

> **Steps explained:** Red $\Rightarrow P=1/2 \Rightarrow 1$ bit. To pin down the exact card needs $\log_2 52 = 5.70$ bits total; subtract the 1 bit already known $\rightarrow$ 4.70 bits remain.

---

## Q7 — Binary source entropy plot

*(Same as the binary plot Problem in notes)*

$$H = P\log_2\frac{1}{P}+(1-P)\log_2\frac{1}{1-P}$$

| P    | 0 | 0.2   | 0.4   | 0.5   | 0.6   | 0.8   | 1 |
|------|---|-------|-------|-------|-------|-------|---|
| H    | 0 | 0.721 | 0.970 | **1.0** | 0.970 | 0.721 | 0 |

The curve is concave ($\cap$-shaped), symmetric about $P = 0.5$, with **maximum $H = 1$ bit at $P = 0.5$**, falling to 0 at $P = 0$ and $P = 1$.

**LaTeX (pgfplots) for the plot:**

```latex
\documentclass{standalone}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
\begin{document}
\begin{tikzpicture}
\begin{axis}[
    xlabel=$P$, ylabel=$H(P)$,
    xmin=0, xmax=1, ymin=0, ymax=1.1,
    samples=200, domain=0.0001:0.9999,
    grid=both, width=10cm, height=7cm,
    title={Entropy of a binary source}]
\addplot[blue,thick]{-x*ln(x)/ln(2)-(1-x)*ln(1-x)/ln(2)};
\addplot[dashed,red] coordinates {(0.5,0)(0.5,1)};
\addplot[dashed,red] coordinates {(0,1)(0.5,1)};
\node[label={[font=\small]above:$(0.5,\,1)$}] at (axis cs:0.5,1){};
\end{axis}
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Plug several $P$ values into the binary entropy formula, tabulate, and plot. The peak is always at $P=0.5$ (equally likely $\Rightarrow$ maximum uncertainty $\Rightarrow H=1$ bit).

---

## Q8 — Dot–dash code

*(Same as Q7 in notes)*

**Probabilities:** $P_{dash}=\tfrac13 P_{dot}$, sum $= 1 \Rightarrow P_{dot}=\tfrac34,\ P_{dash}=\tfrac14$.

**(i)**

$$I_{dot}=\log_2\tfrac{4}{3}=0.415\ \text{bits} \qquad I_{dash}=\log_2 4=2\ \text{bits}$$

**(ii)**

$$H=\tfrac34\log_2\tfrac43+\tfrac14\log_2 4 = 0.311+0.5=0.811\ \text{bits/symbol}$$

**(iii)** dot = 10 ms, dash = 30 ms, gap = 10 ms. For every 4 symbols (3 dots + 1 dash): $3(10)+1(30)+4(10)=100$ ms.

$$r_s=\frac{4}{100\ \text{ms}}=40\ \text{symbols/sec}$$

$$R=r_s H = 40\times0.811=32.45\ \text{bits/sec}$$

> **Steps explained:** Same probability trick as Q5 but with "one-third." Self-info of each, then weighted average for $H$. For the rate, count total time for 4 symbols including gaps (100 ms) $\rightarrow r_s = 40$/s $\rightarrow R = r_s H$.

---

## Q9 — Source x, y with P = 0.55, 0.45: efficiency & redundancy

**Entropy:**

$$H=-0.55\log_2 0.55-0.45\log_2 0.45=0.55(0.8625)+0.45(1.1520)=0.4744+0.5184=0.9928\ \text{bits/sym}$$

**Maximum entropy:** $H_{max}=\log_2 2 = 1$ bit.

**Efficiency:**

$$\eta=\frac{H}{H_{max}}=\frac{0.9928}{1}=0.9928=99.28\%$$

**Redundancy:**

$$=1-\eta=0.72\%$$

> **Steps explained:** Compute $H$. Since 2 symbols, $H_{max}=\log_2 2 =1$. Efficiency $= H/H_{max}$; redundancy $= 1 - \eta$.

---

## Q10 — A, B, C with P = 0.7, 0.15, 0.15

**Efficiency, redundancy, entropy in Hartley & Nats.**

**Entropy (bits):**

$$H=-0.7\log_2 0.7-2(0.15)\log_2 0.15=0.7(0.5146)+0.3(2.737)=0.3602+0.8211=1.1813\ \text{bits/sym}$$

**Maximum entropy:** $H_{max}=\log_2 3 = 1.585$ bits.

**Efficiency:** $\eta=\dfrac{1.1813}{1.585}=0.7453=74.53\%$

**Redundancy:** $1-\eta=25.47\%$

**In Hartley** (1 Hartley $= \log_2 10 = 3.3219$ bits): $\dfrac{1.1813}{3.3219}=0.356\ \text{Hartley/sym}$

**In Nats** (1 Nat $= 1.4427$ bits): $\dfrac{1.1813}{1.4427}=0.819\ \text{Nats/sym}$

> **Steps explained:** Compute $H$ in bits. 3 symbols $\Rightarrow H_{max}=\log_2 3$. $\eta$ and redundancy as before. Convert bits $\rightarrow$ Hartley ($\div 3.3219$) and bits $\rightarrow$ Nats ($\div 1.4427$, i.e. $\times 0.693$).

---

# UNIT 2 — MARKOV SOURCES

> **General method for every Markov question:**
> 1. **State probabilities** — write one balance equation per state ( $P(\text{state}) = \sum (\text{incoming } P \times \text{transition})$ ), use $\sum P = 1$, solve.
> 2. **Entropy of each state** — $H_i=\sum_j p_{ij}\log_2\dfrac{1}{p_{ij}}$ (only the **outgoing** branches of that state).
> 3. **Source entropy** — $H=\sum_i P_i H_i$.
> 4. **$G_N$** (when asked) — find probabilities of all length-$N$ symbol sequences, compute $H(S^N)=\sum P\log_2\dfrac1P$, then $G_N=\dfrac{H(S^N)}{N}$. Property: $G_1 \ge G_2 \ge G_3 \ge \dots \ge H$.

---

## Q11 — 3-state symmetric source, p(i)=1/3

**Self-loop ½, two ¼ branches per state.** Symbol = the state entered. By symmetry $P_1=P_2=P_3=\tfrac13$.

**(i) Entropy of each state** (same for all):

$$H_i=\tfrac12\log_2 2+\tfrac14\log_2 4+\tfrac14\log_2 4=0.5+0.5+0.5=1.5\ \text{bits/state}$$

**(ii) Source entropy:**

$$H=\sum P_i H_i = 3\times\tfrac13(1.5)=1.5\ \text{bits/symbol}$$

**(iii) $G_1$:** symbols A, B, C each have probability $\tfrac13$ $\Rightarrow$

$$G_1=\log_2 3=1.585\ \text{bits/sym}$$

**$G_2$ — build the pair-probability tree.** Start from each state ($P_i=\tfrac13$), branch by the transition probabilities (self $\tfrac12$, others $\tfrac14$). Each leaf = a state-pair; its probability $= P_i\times p_{ij}$:

| Pair | Prob | Pair | Prob | Pair | Prob |
|------|------|------|------|------|------|
| 11 | $\tfrac16$ | 12 | $\tfrac1{12}$ | 13 | $\tfrac1{12}$ |
| 21 | $\tfrac1{12}$ | 22 | $\tfrac16$ | 23 | $\tfrac1{12}$ |
| 31 | $\tfrac1{12}$ | 32 | $\tfrac1{12}$ | 33 | $\tfrac16$ |

(3 "same" pairs at $\tfrac16$, 6 "different" pairs at $\tfrac1{12}$; sum $=3\cdot\tfrac16+6\cdot\tfrac1{12}=1$ ✓)

$$H(S^2)=3\Big(\tfrac16\log_2 6\Big)+6\Big(\tfrac1{12}\log_2 12\Big)=\tfrac12(2.585)+\tfrac12(3.585)=3.085$$

$$G_2=\frac{H(S^2)}{2}=\frac{3.085}{2}=1.5425\ \text{bits/sym}$$

**Verification:** $G_1(1.585)\ge G_2(1.5425)\ge H(1.5)$ ✓

**LaTeX (TikZ/forest) — pair-probability tree:**

```latex
\documentclass{standalone}
\usepackage{forest}
\usepackage{amsmath}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west,
          edge={-stealth}, l sep=16mm, s sep=2mm, font=\footnotesize}
[{Start}
  [{$1$ $\tfrac13$}, edge label={node[midway,above,font=\tiny]{$\tfrac13$}}
    [{$11=\tfrac16$},   edge label={node[midway,above,font=\tiny]{$\tfrac12$}}]
    [{$12=\tfrac1{12}$},edge label={node[midway,above,font=\tiny]{$\tfrac14$}}]
    [{$13=\tfrac1{12}$},edge label={node[midway,below,font=\tiny]{$\tfrac14$}}]
  ]
  [{$2$ $\tfrac13$}, edge label={node[midway,above,font=\tiny]{$\tfrac13$}}
    [{$21=\tfrac1{12}$},edge label={node[midway,above,font=\tiny]{$\tfrac14$}}]
    [{$22=\tfrac16$},   edge label={node[midway,above,font=\tiny]{$\tfrac12$}}]
    [{$23=\tfrac1{12}$},edge label={node[midway,below,font=\tiny]{$\tfrac14$}}]
  ]
  [{$3$ $\tfrac13$}, edge label={node[midway,below,font=\tiny]{$\tfrac13$}}
    [{$31=\tfrac1{12}$},edge label={node[midway,above,font=\tiny]{$\tfrac14$}}]
    [{$32=\tfrac1{12}$},edge label={node[midway,above,font=\tiny]{$\tfrac14$}}]
    [{$33=\tfrac16$},   edge label={node[midway,below,font=\tiny]{$\tfrac12$}}]
  ]
]
\end{forest}
\end{document}
```

**LaTeX (TikZ) — triangular 3-state diagram:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=3.5cm,
   every state/.style={thick,minimum size=1cm}]
  \node[state] (S2) {2 (C)};
  \node[state] (S1) [below left=2.5cm and 1.8cm of S2] {1 (A)};
  \node[state] (S3) [below right=2.5cm and 1.8cm of S2] {3 (B)};
  \path
    (S1) edge[loop left]  node{$\tfrac12$} (S1)
    (S2) edge[loop above] node{$\tfrac12$} (S2)
    (S3) edge[loop right] node{$\tfrac12$} (S3)
    (S1) edge[bend left=12]  node{$\tfrac14$} (S2)
    (S2) edge[bend left=12]  node{$\tfrac14$} (S1)
    (S2) edge[bend left=12]  node{$\tfrac14$} (S3)
    (S3) edge[bend left=12]  node{$\tfrac14$} (S2)
    (S1) edge[bend left=12]  node{$\tfrac14$} (S3)
    (S3) edge[bend left=12]  node{$\tfrac14$} (S1);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Perfect symmetry $\Rightarrow$ all state probabilities $\tfrac13$ and all $H_i$ equal (1.5), so $H=1.5$. For $G_1$ all symbols are equally likely $\Rightarrow \log_2 3$. For $G_2$, pair prob $=$ (state prob) $\times$ (transition prob); group same/different pairs.

---

## Q12 — Car heading reporter

*(Same style as Example 5 / Q13)*

States: S (straight), L (left), R (right). Build the model from the data:

- From S: $P(S\!\to\!S)=\tfrac12,\ P(S\!\to\!L)=\tfrac14,\ P(S\!\to\!R)=\tfrac14$
- From L: $P(L\!\to\!L)=\tfrac12,\ P(L\!\to\!S)=\tfrac12$ (cannot go L→R)
- From R: $P(R\!\to\!R)=\tfrac12,\ P(R\!\to\!S)=\tfrac12$ (cannot go R→L)

**State equations:**

$$P(S)=\tfrac12P(S)+\tfrac12P(L)+\tfrac12P(R) \quad P(L)=\tfrac12P(L)+\tfrac14P(S) \quad P(R)=\tfrac12P(R)+\tfrac14P(S)$$

Solving (with $\sum P=1$): $P(S)=\tfrac12,\ P(L)=\tfrac14,\ P(R)=\tfrac14$

**Entropy of each state:**

$$H_S=\tfrac12\log_2 2+\tfrac14\log_2 4+\tfrac14\log_2 4=1.5\ \text{bits}$$

$$H_L=\tfrac12\log_2 2+\tfrac12\log_2 2=1\ \text{bit} \qquad H_R=1\ \text{bit}$$

**Source entropy:**

$$H=\tfrac12(1.5)+\tfrac14(1)+\tfrac14(1)=1.25\ \text{bits/symbol}$$

**Rate** (reported every second $\Rightarrow r_s=1$/s):

$$R=r_s H=1.25\ \text{bits/sec}$$

**LaTeX (TikZ):**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=3.2cm,
   every state/.style={thick,minimum size=1cm}]
  \node[state] (S) {S};
  \node[state] (L) [left=of S] {L};
  \node[state] (R) [right=of S] {R};
  \path
   (S) edge[loop above] node{$\tfrac12$} (S)
   (L) edge[loop left]  node{$\tfrac12$} (L)
   (R) edge[loop right] node{$\tfrac12$} (R)
   (S) edge[bend right=12] node[below]{$\tfrac14$} (L)
   (L) edge[bend right=12] node[above]{$\tfrac12$} (S)
   (S) edge[bend left=12]  node[below]{$\tfrac14$} (R)
   (R) edge[bend left=12]  node[above]{$\tfrac12$} (S);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Translate each data sentence into transition probabilities (note "cannot swap L↔R" gives the two zero transitions). Then standard 3-step method. Rate $= H \times 1$ since one report/second.

---

## Q13 — Students abroad / MBA / industry

*(Identical to Example 5 in notes)*

States: A (abroad), B (MBA/Civil), C (Industry).

$$P(A)=\tfrac12P(A)+\tfrac12P(B)+\tfrac12P(C) \quad P(B)=\tfrac12P(B)+\tfrac14P(A) \quad P(C)=\tfrac12P(C)+\tfrac14P(A)$$

**Solving:** $P(A)=\tfrac12,\ P(B)=\tfrac14,\ P(C)=\tfrac14$

$$H_A=\tfrac12\log_2 2+\tfrac14\log_2 4+\tfrac14\log_2 4=1.5 \qquad H_B=1 \qquad H_C=1$$

$$H=\tfrac12(1.5)+\tfrac14(1)+\tfrac14(1)=1.25\ \text{bits/symbol}$$

**LaTeX (TikZ) — state diagram (A = abroad, B = MBA/Civil, C = Industry):**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usepackage{amsmath}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=3.2cm,
   every state/.style={thick,minimum size=1cm}]
  \node[state] (A) {A};
  \node[state] (B) [left=of A] {B};
  \node[state] (C) [right=of A] {C};
  \path
   (A) edge[loop above] node{$\tfrac12$} (A)
   (B) edge[loop left]  node{$\tfrac12$} (B)
   (C) edge[loop right] node{$\tfrac12$} (C)
   (A) edge[bend right=12] node[below]{$\tfrac14$} (B)
   (B) edge[bend right=12] node[above]{$\tfrac12$} (A)
   (A) edge[bend left=12]  node[below]{$\tfrac14$} (C)
   (C) edge[bend left=12]  node[above]{$\tfrac12$} (A);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Same source structure as Q12. Build transitions from the (a)–(d) data, solve balance equations, compute $H_i$ and $H$.

---

## Q14 — Markov source (X, Y, Z)

*(Identical to Example 6 in notes)*

States: 1(X) and 2(Y). Transitions: $1\!\to\!1=\tfrac23(X),\ 1\!\to\!2=\tfrac13(Z),\ 2\!\to\!1=\tfrac14(Z),\ 2\!\to\!2=\tfrac34(Y)$.

**State probabilities:** from $\tfrac13P(1)=\tfrac14P(2)$ and sum $= 1$: $P(1)=\tfrac37,\ P(2)=\tfrac47$

**Entropy of each state:**

$$H_1=\tfrac23\log_2\tfrac32+\tfrac13\log_2 3=0.9183 \qquad H_2=\tfrac14\log_2 4+\tfrac34\log_2\tfrac43=0.8113$$

**Source entropy:**

$$H=\tfrac37(0.9183)+\tfrac47(0.8113)=0.8572\ \text{bits/sym}$$

### Building $G_N$ — the probability tree

Start from the stationary state probabilities $P(1)=\tfrac37,\ P(2)=\tfrac47$. At each step, a branch **multiplies by its transition probability** and **emits the symbol** on that transition. Reading every root→leaf path gives a symbol sequence and its probability. The cumulative probability is shown at each node.

**LaTeX (TikZ/forest) — probability tree (3 levels):**

```latex
\documentclass{standalone}
\usepackage{forest}
\usepackage{amsmath}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west,
          edge={-stealth}, l sep=20mm, s sep=1.5mm, font=\footnotesize}
[{Start}
  [{$1$ $\tfrac{3}{7}$}, edge label={node[midway,above,font=\tiny]{$\tfrac37$}}
    [{$X{:}1$ $\tfrac{2}{7}$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac23$}}
      [{$X{:}1$ $\tfrac{4}{21}$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac23$}}
        [{$XXX=\tfrac{8}{63}$},  edge label={node[midway,above,font=\tiny]{$X\,\tfrac23$}}]
        [{$XXZ=\tfrac{4}{63}$},  edge label={node[midway,below,font=\tiny]{$Z\,\tfrac13$}}]]
      [{$Z{:}2$ $\tfrac{2}{21}$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac13$}}
        [{$XZZ=\tfrac{1}{42}$},  edge label={node[midway,above,font=\tiny]{$Z\,\tfrac14$}}]
        [{$XZY=\tfrac{1}{14}$},  edge label={node[midway,below,font=\tiny]{$Y\,\tfrac34$}}]]]
    [{$Z{:}2$ $\tfrac{1}{7}$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac13$}}
      [{$Z{:}1$ $\tfrac{1}{28}$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac14$}}
        [{$ZZX=\tfrac{1}{42}$},  edge label={node[midway,above,font=\tiny]{$X\,\tfrac23$}}]
        [{$ZZZ=\tfrac{1}{84}$},  edge label={node[midway,below,font=\tiny]{$Z\,\tfrac13$}}]]
      [{$Y{:}2$ $\tfrac{3}{28}$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac34$}}
        [{$ZYZ=\tfrac{3}{112}$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac14$}}]
        [{$ZYY=\tfrac{9}{112}$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac34$}}]]]]
  [{$2$ $\tfrac{4}{7}$}, edge label={node[midway,below,font=\tiny]{$\tfrac47$}}
    [{$Z{:}1$ $\tfrac{1}{7}$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac14$}}
      [{$X{:}1$ $\tfrac{2}{21}$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac23$}}
        [{$ZXX=\tfrac{4}{63}$},  edge label={node[midway,above,font=\tiny]{$X\,\tfrac23$}}]
        [{$ZXZ=\tfrac{2}{63}$},  edge label={node[midway,below,font=\tiny]{$Z\,\tfrac13$}}]]
      [{$Z{:}2$ $\tfrac{1}{21}$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac13$}}
        [{$ZZZ=\tfrac{1}{84}$},  edge label={node[midway,above,font=\tiny]{$Z\,\tfrac14$}}]
        [{$ZZY=\tfrac{1}{28}$},  edge label={node[midway,below,font=\tiny]{$Y\,\tfrac34$}}]]]
    [{$Y{:}2$ $\tfrac{3}{7}$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac34$}}
      [{$Z{:}1$ $\tfrac{3}{28}$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac14$}}
        [{$YZX=\tfrac{1}{14}$},  edge label={node[midway,above,font=\tiny]{$X\,\tfrac23$}}]
        [{$YZZ=\tfrac{1}{28}$},  edge label={node[midway,below,font=\tiny]{$Z\,\tfrac13$}}]]
      [{$Y{:}2$ $\tfrac{9}{28}$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac34$}}
        [{$YYZ=\tfrac{9}{112}$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac14$}}]
        [{$YYY=\tfrac{27}{112}$},edge label={node[midway,below,font=\tiny]{$Y\,\tfrac34$}}]]]]
]
\end{forest}
\end{document}
```

**$G_1$ — first-order symbols** (the symbol at the *first* step):

| Symbol | How it arises | Probability |
|--------|---------------|-------------|
| X | $P(1)\cdot\tfrac23$ | $\tfrac37\cdot\tfrac23=\tfrac27$ |
| Y | $P(2)\cdot\tfrac34$ | $\tfrac47\cdot\tfrac34=\tfrac37$ |
| Z | $P(1)\cdot\tfrac13+P(2)\cdot\tfrac14$ | $\tfrac17+\tfrac17=\tfrac27$ |

$$G_1=H(\bar S)=\tfrac27\log_2\tfrac72+\tfrac37\log_2\tfrac73+\tfrac27\log_2\tfrac72=\mathbf{1.557\ bits/sym}$$

**$G_2$ — second-order** (depth-2 nodes; $ZZ$ appears from both roots and merges: $\tfrac1{28}+\tfrac1{21}=\tfrac1{12}$):

| Seq | Prob | Seq | Prob | Seq | Prob |
|-----|------|-----|------|-----|------|
| XX | $\tfrac{4}{21}$ | XZ | $\tfrac{2}{21}$ | ZX | $\tfrac{2}{21}$ |
| ZY | $\tfrac{3}{28}$ | YZ | $\tfrac{3}{28}$ | YY | $\tfrac{9}{28}$ |
| ZZ | $\tfrac{1}{12}$ |  |  |  |  |

$$H(S^2)=\tfrac{4}{21}\log_2\tfrac{21}{4}+2\Big(\tfrac{2}{21}\log_2\tfrac{21}{2}\Big)+2\Big(\tfrac{3}{28}\log_2\tfrac{28}{3}\Big)+\tfrac{9}{28}\log_2\tfrac{28}{9}+\tfrac{1}{12}\log_2 12$$
$$= 0.4557+0.6461+0.6905+0.5263+0.2988 = 2.6174 \;\Rightarrow\; G_2=\frac{2.6174}{2}=\mathbf{1.3087}$$

**$G_3$ — third-order** (the 15 leaves; $ZZZ$ merges: $\tfrac1{84}+\tfrac1{84}=\tfrac1{42}$):

| Seq | Prob | Seq | Prob | Seq | Prob |
|-----|------|-----|------|-----|------|
| XXX | $\tfrac{8}{63}$ | XXZ | $\tfrac{4}{63}$ | ZXX | $\tfrac{4}{63}$ |
| ZXZ | $\tfrac{2}{63}$ | XZZ | $\tfrac{1}{42}$ | ZZX | $\tfrac{1}{42}$ |
| ZZZ | $\tfrac{1}{42}$ | XZY | $\tfrac{1}{14}$ | YZX | $\tfrac{1}{14}$ |
| ZYZ | $\tfrac{3}{112}$ | ZYY | $\tfrac{9}{112}$ | YYZ | $\tfrac{9}{112}$ |
| ZZY | $\tfrac{1}{28}$ | YZZ | $\tfrac{1}{28}$ | YYY | $\tfrac{27}{112}$ |

$$H(S^3)=0.3781+2(0.2525)+0.1580+3(0.1284)+2(0.2720)+0.1399+2(0.2923)+2(0.1717)+0.4948 = 3.533$$
$$G_3=\frac{3.533}{3}=\mathbf{1.178}$$

**Verification:** $G_1(1.557)>G_2(1.309)>G_3(1.178)>H(0.857)$ ✓

**LaTeX (TikZ) — two-state with emitted symbols:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=4cm,
   every state/.style={thick,minimum size=1.1cm}]
  \node[state] (s1) {1 (X)};
  \node[state] (s2) [right=of s1] {2 (Y)};
  \path
   (s1) edge[loop left]  node{$\tfrac23\,(X)$} (s1)
   (s2) edge[loop right] node{$\tfrac34\,(Y)$} (s2)
   (s1) edge[bend left=18] node[above]{$\tfrac13\,(Z)$} (s2)
   (s2) edge[bend left=18] node[below]{$\tfrac14\,(Z)$} (s1);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** This is exactly Example 6. State probs from the balance equation. $G_N$ needs the tree diagram: list all $N$-length symbol sequences, multiply probabilities along each branch, sum $P\log(1/P)$, divide by $N$.

---

## Q15 — Diamond 4-state source

*(Identical to Example 4 in notes)*

Transitions: $A\!\to\!A=0.6,\ A\!\to\!B=0.4;\ B\!\to\!C=0.5,\ B\!\to\!D=0.5;\ C\!\to\!C=0.6,\ C\!\to\!D=0.4;\ D\!\to\!A=0.5,\ D\!\to\!B=0.5$.

**(i) State probabilities** — solving the balance equations (all in terms of $P(D)$):

$$P(A)=1.25P(D) \quad P(B)=P(D) \quad P(C)=1.25P(D) \quad \text{sum}\Rightarrow 4.5P(D)=1$$

$$P(A)=P(C)=\tfrac{5}{18} \qquad P(B)=P(D)=\tfrac29$$

**(ii) Entropy of each state:**

$$H_A=0.6\log_2\tfrac1{0.6}+0.4\log_2\tfrac1{0.4}=0.971 \qquad H_C=0.971$$

$$H_B=0.5\log_2 2+0.5\log_2 2=1 \qquad H_D=1$$

**(iii) Source entropy:**

$$H=\tfrac{5}{18}(0.971)+\tfrac29(1)+\tfrac{5}{18}(0.971)+\tfrac29(1)=0.9839\ \text{bits/sym}$$

**LaTeX (TikZ) — diamond layout:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=2.8cm,
   every state/.style={thick,minimum size=1cm}]
  \node[state] (C) {C};
  \node[state] (D) [below left=2cm and 2cm of C] {D};
  \node[state] (B) [below right=2cm and 2cm of C] {B};
  \node[state] (A) [below right=2cm and 2cm of D] {A};
  \path
   (A) edge[loop left]  node{$0.6$} (A)
   (C) edge[loop right] node{$0.6$} (C)
   (A) edge[bend left=10] node[left]{$0.4$} (B)
   (B) edge[bend left=10] node{$0.5$} (C)
   (C) edge[bend left=10] node{$0.4$} (D)
   (D) edge[bend left=10] node[left]{$0.5$} (A)
   (B) edge[bend left=10] node[above]{$0.5$} (D)
   (D) edge[bend left=10] node[below]{$0.5$} (B);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Exactly Example 4. Write 4 balance equations, express all in terms of $P(D)$, apply sum $= 1$. Then $H_i$ per state (two states give 0.971, two give 1), and $H=\sum P_i H_i$.

---

## Q16 — Triangular asymmetric 3-state

**Transitions given; $G_1, G_2$ with initial $P(i)=\tfrac13$.**

Transitions: A→A 0.6, A→B 0.2, A→C 0.2; B→A 0.3, B→B 0.5, B→C 0.2; C→A 0.3, C→B 0.3, C→C 0.4.

**(i) Stationary distribution:**

$$0.4P(A)=0.3\bigl(1-P(A)\bigr)\Rightarrow P(A)=\tfrac37=0.4286$$

Solving the rest: $P(A)=0.4286,\ P(B)=0.3214,\ P(C)=0.25$

**(ii) Entropy of each state:**

$$H_A=0.6\log_2\tfrac1{0.6}+2(0.2)\log_2\tfrac1{0.2}=0.442+0.929=1.371$$

$$H_B=0.3\log_2\tfrac1{0.3}+0.5\log_2 2+0.2\log_2\tfrac1{0.2}=0.521+0.5+0.464=1.486$$

$$H_C=2(0.3)\log_2\tfrac1{0.3}+0.4\log_2\tfrac1{0.4}=1.042+0.529=1.571$$

**Source entropy:**

$$H=0.4286(1.371)+0.3214(1.486)+0.25(1.571)=1.458\ \text{bits/sym}$$

**(iii) $G_1, G_2$ with initial $P(1)=P(2)=P(3)=\tfrac13$:**

**$G_1$ — first-symbol probabilities** (each $=\tfrac13\times$ column sum of the transition matrix):

$$P(A)=\tfrac13(0.6+0.3+0.3)=0.4 \quad P(B)=\tfrac13(0.2+0.5+0.3)=0.333 \quad P(C)=\tfrac13(0.2+0.2+0.4)=0.267$$

$$G_1=0.4\log_2\tfrac1{0.4}+0.333\log_2\tfrac1{0.333}+0.267\log_2\tfrac1{0.267}=0.5288+0.5283+0.5086=\mathbf{1.566}$$

**$G_2$ — pair-probability tree.** Each pair probability $=\tfrac13\,p_{ij}$ (initial state uniform):

| Pair | Prob | Pair | Prob | Pair | Prob |
|------|------|------|------|------|------|
| AA | 0.2000 | AB | 0.0667 | AC | 0.0667 |
| BA | 0.1000 | BB | 0.1667 | BC | 0.0667 |
| CA | 0.1000 | CB | 0.1000 | CC | 0.1333 |

$$H(S^2)=0.2\log_2\tfrac1{0.2}+3\Big(0.0667\log_2\tfrac1{0.0667}\Big)+3\Big(0.1\log_2\tfrac1{0.1}\Big)+0.1667\log_2\tfrac1{0.1667}+0.1333\log_2\tfrac1{0.1333}$$
$$= 0.4644+0.7814+0.9966+0.4308+0.3876 = 3.061 \;\Rightarrow\; G_2=\frac{3.061}{2}=\mathbf{1.531}$$

**Verification:** $G_1(1.566)\ge G_2(1.531)\ge H(1.458)$ ✓

**LaTeX (TikZ/forest) — pair-probability tree:**

```latex
\documentclass{standalone}
\usepackage{forest}
\usepackage{amsmath}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west,
          edge={-stealth}, l sep=16mm, s sep=2mm, font=\footnotesize}
[{Start}
  [{$A$ $\tfrac13$}, edge label={node[midway,above,font=\tiny]{$\tfrac13$}}
    [{$AA=0.200$}, edge label={node[midway,above,font=\tiny]{$0.6$}}]
    [{$AB=0.067$}, edge label={node[midway,above,font=\tiny]{$0.2$}}]
    [{$AC=0.067$}, edge label={node[midway,below,font=\tiny]{$0.2$}}]]
  [{$B$ $\tfrac13$}, edge label={node[midway,above,font=\tiny]{$\tfrac13$}}
    [{$BA=0.100$}, edge label={node[midway,above,font=\tiny]{$0.3$}}]
    [{$BB=0.167$}, edge label={node[midway,above,font=\tiny]{$0.5$}}]
    [{$BC=0.067$}, edge label={node[midway,below,font=\tiny]{$0.2$}}]]
  [{$C$ $\tfrac13$}, edge label={node[midway,below,font=\tiny]{$\tfrac13$}}
    [{$CA=0.100$}, edge label={node[midway,above,font=\tiny]{$0.3$}}]
    [{$CB=0.100$}, edge label={node[midway,above,font=\tiny]{$0.3$}}]
    [{$CC=0.133$}, edge label={node[midway,below,font=\tiny]{$0.4$}}]]
]
\end{forest}
\end{document}
```

**LaTeX (TikZ) — triangular, asymmetric:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=3.6cm,
   every state/.style={thick,minimum size=1cm}]
  \node[state] (A) {A};
  \node[state] (B) [below left=2.6cm and 1.8cm of A] {B};
  \node[state] (C) [below right=2.6cm and 1.8cm of A] {C};
  \path
   (A) edge[loop above] node{$0.6$} (A)
   (B) edge[loop left]  node{$0.5$} (B)
   (C) edge[loop right] node{$0.4$} (C)
   (A) edge[bend right=12] node[left]{$0.2$} (B)
   (B) edge[bend right=12] node[right]{$0.3$} (A)
   (A) edge[bend left=12]  node[right]{$0.2$} (C)
   (C) edge[bend left=12]  node[left]{$0.3$} (A)
   (B) edge[bend left=12]  node[above]{$0.2$} (C)
   (C) edge[bend left=12]  node[below]{$0.3$} (B);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Transitions are NOT symmetric, so solve the balance equations properly for the stationary distribution (used in part ii). Parts (iii) $G_1/G_2$ explicitly use the *uniform initial* $P(i)=\tfrac13$: first-symbol probs come from $\tfrac13\times(\text{columns})$, and pair probs from $\tfrac13 p_{ij}$.

---

## Q17 — Two-state, find H, G₁, G₂, G₃

**$P_1=P_2=\tfrac12$.** Transitions: $1\!\to\!1=\tfrac34(A),\ 1\!\to\!2=\tfrac14(C),\ 2\!\to\!1=\tfrac14(C),\ 2\!\to\!2=\tfrac34(B)$.

**State probs:** symmetry $\Rightarrow P(1)=P(2)=\tfrac12$ ✓

**Entropy of each state:**

$$H_1=\tfrac34\log_2\tfrac43+\tfrac14\log_2 4=0.311+0.5=0.811=H_2$$

$$H=\tfrac12(0.811)+\tfrac12(0.811)=0.811\ \text{bits/sym}$$

### Building $G_N$ — the probability tree

Start from $P(1)=P(2)=\tfrac12$; each branch multiplies by its transition probability and emits its symbol.

**LaTeX (TikZ/forest) — probability tree (3 levels):**

```latex
\documentclass{standalone}
\usepackage{forest}
\usepackage{amsmath}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west,
          edge={-stealth}, l sep=20mm, s sep=1.5mm, font=\footnotesize}
[{Start}
  [{$1$ $\tfrac12$}, edge label={node[midway,above,font=\tiny]{$\tfrac12$}}
    [{$A{:}1$ $\tfrac38$}, edge label={node[midway,above,font=\tiny]{$A\,\tfrac34$}}
      [{$A{:}1$ $\tfrac{9}{32}$}, edge label={node[midway,above,font=\tiny]{$A\,\tfrac34$}}
        [{$AAA=\tfrac{27}{128}$}, edge label={node[midway,above,font=\tiny]{$A\,\tfrac34$}}]
        [{$AAC=\tfrac{9}{128}$},  edge label={node[midway,below,font=\tiny]{$C\,\tfrac14$}}]]
      [{$C{:}2$ $\tfrac{3}{32}$}, edge label={node[midway,below,font=\tiny]{$C\,\tfrac14$}}
        [{$ACC=\tfrac{3}{128}$},  edge label={node[midway,above,font=\tiny]{$C\,\tfrac14$}}]
        [{$ACB=\tfrac{9}{128}$},  edge label={node[midway,below,font=\tiny]{$B\,\tfrac34$}}]]]
    [{$C{:}2$ $\tfrac18$}, edge label={node[midway,below,font=\tiny]{$C\,\tfrac14$}}
      [{$C{:}1$ $\tfrac{1}{32}$}, edge label={node[midway,above,font=\tiny]{$C\,\tfrac14$}}
        [{$CCA=\tfrac{3}{128}$},  edge label={node[midway,above,font=\tiny]{$A\,\tfrac34$}}]
        [{$CCC=\tfrac{1}{128}$},  edge label={node[midway,below,font=\tiny]{$C\,\tfrac14$}}]]
      [{$B{:}2$ $\tfrac{3}{32}$}, edge label={node[midway,below,font=\tiny]{$B\,\tfrac34$}}
        [{$CBC=\tfrac{3}{128}$},  edge label={node[midway,above,font=\tiny]{$C\,\tfrac14$}}]
        [{$CBB=\tfrac{9}{128}$},  edge label={node[midway,below,font=\tiny]{$B\,\tfrac34$}}]]]]
  [{$2$ $\tfrac12$}, edge label={node[midway,below,font=\tiny]{$\tfrac12$}}
    [{$C{:}1$ $\tfrac18$}, edge label={node[midway,above,font=\tiny]{$C\,\tfrac14$}}
      [{$A{:}1$ $\tfrac{3}{32}$}, edge label={node[midway,above,font=\tiny]{$A\,\tfrac34$}}
        [{$CAA=\tfrac{9}{128}$},  edge label={node[midway,above,font=\tiny]{$A\,\tfrac34$}}]
        [{$CAC=\tfrac{3}{128}$},  edge label={node[midway,below,font=\tiny]{$C\,\tfrac14$}}]]
      [{$C{:}2$ $\tfrac{1}{32}$}, edge label={node[midway,below,font=\tiny]{$C\,\tfrac14$}}
        [{$CCC=\tfrac{1}{128}$},  edge label={node[midway,above,font=\tiny]{$C\,\tfrac14$}}]
        [{$CCB=\tfrac{3}{128}$},  edge label={node[midway,below,font=\tiny]{$B\,\tfrac34$}}]]]
    [{$B{:}2$ $\tfrac38$}, edge label={node[midway,below,font=\tiny]{$B\,\tfrac34$}}
      [{$C{:}1$ $\tfrac{3}{32}$}, edge label={node[midway,above,font=\tiny]{$C\,\tfrac14$}}
        [{$BCA=\tfrac{9}{128}$},  edge label={node[midway,above,font=\tiny]{$A\,\tfrac34$}}]
        [{$BCC=\tfrac{3}{128}$},  edge label={node[midway,below,font=\tiny]{$C\,\tfrac14$}}]]
      [{$B{:}2$ $\tfrac{9}{32}$}, edge label={node[midway,below,font=\tiny]{$B\,\tfrac34$}}
        [{$BBC=\tfrac{9}{128}$},  edge label={node[midway,above,font=\tiny]{$C\,\tfrac14$}}]
        [{$BBB=\tfrac{27}{128}$}, edge label={node[midway,below,font=\tiny]{$B\,\tfrac34$}}]]]]
]
\end{forest}
\end{document}
```

**$G_1$ — first-order symbols:** $P(A)=P(1)\cdot\tfrac34=\tfrac38$, $P(B)=P(2)\cdot\tfrac34=\tfrac38$, $P(C)=P(1)\cdot\tfrac14+P(2)\cdot\tfrac14=\tfrac14$.

$$G_1=2\Big(\tfrac38\log_2\tfrac83\Big)+\tfrac14\log_2 4=1.061+0.5=\mathbf{1.561}$$

**$G_2$ — second-order** (depth-2 nodes; $CC$ merges: $\tfrac1{32}+\tfrac1{32}=\tfrac2{32}$):

| Seq | Prob | Seq | Prob | Seq | Prob |
|-----|------|-----|------|-----|------|
| AA | $\tfrac{9}{32}$ | BB | $\tfrac{9}{32}$ | AC | $\tfrac{3}{32}$ |
| CB | $\tfrac{3}{32}$ | CA | $\tfrac{3}{32}$ | BC | $\tfrac{3}{32}$ |
| CC | $\tfrac{2}{32}$ |  |  |  |  |

$$H(S^2)=2\Big(\tfrac9{32}\log_2\tfrac{32}{9}\Big)+4\Big(\tfrac3{32}\log_2\tfrac{32}{3}\Big)+\tfrac2{32}\log_2 16 = 1.0294+1.2806+0.25 = 2.560$$
$$G_2=\frac{2.560}{2}=\mathbf{1.280}$$

**$G_3$ — third-order** (the 15 leaves; $CCC$ merges: $\tfrac1{128}+\tfrac1{128}=\tfrac2{128}$):

| Prob | Sequences | Count |
|------|-----------|-------|
| $\tfrac{27}{128}$ | AAA, BBB | 2 |
| $\tfrac{9}{128}$ | AAC, ACB, CBB, CAA, BCA, BBC | 6 |
| $\tfrac{3}{128}$ | ACC, CCA, CBC, CAC, CCB, BCC | 6 |
| $\tfrac{2}{128}$ | CCC | 1 |

$$H(S^3)=2\Big(\tfrac{27}{128}\log_2\tfrac{128}{27}\Big)+6\Big(\tfrac{9}{128}\log_2\tfrac{128}{9}\Big)+6\Big(\tfrac{3}{128}\log_2\tfrac{128}{3}\Big)+\tfrac{2}{128}\log_2 64$$
$$= 0.9471+1.6158+0.7615+0.0938 = 3.418 \;\Rightarrow\; G_3=\frac{3.418}{3}=\mathbf{1.139}$$

**Verification:** $G_1(1.561)>G_2(1.280)>G_3(1.139)>H(0.811)$ ✓

**LaTeX (TikZ):** same as the Q14 diagram, with labels — loops $\tfrac34(A)$ and $\tfrac34(B)$, cross-links $\tfrac14(C)$ both ways:

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=4cm,
   every state/.style={thick,minimum size=1.1cm}]
  \node[state] (s1) {1};
  \node[state] (s2) [right=of s1] {2};
  \path
   (s1) edge[loop left]  node{$\tfrac34\,(A)$} (s1)
   (s2) edge[loop right] node{$\tfrac34\,(B)$} (s2)
   (s1) edge[bend left=18] node[above]{$\tfrac14\,(C)$} (s2)
   (s2) edge[bend left=18] node[below]{$\tfrac14\,(C)$} (s1);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Identical machinery to Example 6/Q14, just different numbers. Symmetric $\Rightarrow P=\tfrac12$ each. For each $G_N$ build the tree to $N$ levels, collect repeated sequences, sum $P\log(1/P)$, divide by $N$. Confidence check: the values must strictly decrease toward $H$.

---

## Q18 — Two-state A, B

**A→A=⅓, A→B=⅔, B→A=⅔, B→B=⅓.**

**State probabilities:** $P(A)=\tfrac13P(A)+\tfrac23P(B)\Rightarrow \tfrac23P(A)=\tfrac23P(B)\Rightarrow P(A)=P(B)$

$$P(A)=P(B)=\tfrac12$$

**Entropy of each state:**

$$H_A=\tfrac13\log_2 3+\tfrac23\log_2\tfrac32=0.528+0.390=0.918=H_B$$

**Source entropy:**

$$H=\tfrac12(0.918)+\tfrac12(0.918)=0.918\ \text{bits/sym}$$

**LaTeX (TikZ):**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=4cm,
   every state/.style={thick,minimum size=1.1cm}]
  \node[state] (A) {A};
  \node[state] (B) [right=of A] {B};
  \path
   (A) edge[loop left]  node{$\tfrac13$} (A)
   (B) edge[loop right] node{$\tfrac13$} (B)
   (A) edge[bend left=18] node[above]{$\tfrac23$} (B)
   (B) edge[bend left=18] node[below]{$\tfrac23$} (A);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Symmetric two-state $\Rightarrow P(A)=P(B)=\tfrac12$. Both states have the identical branch set $\{\tfrac13, \tfrac23\}$ $\Rightarrow$ identical $H_i=0.918$, so $H=0.918$.

---

## Q19 — Second-order Markov (00, 01, 10, 11)

Transitions: 00→00 0.8, 00→01 0.2; 01→10 0.5, 01→11 0.5; 10→00 0.5, 10→01 0.5; 11→10 0.2, 11→11 0.8.

**State probabilities:** From 00→00 balance, $0.2P(00)=0.5P(10)\Rightarrow P(00)=2.5P(10)$; similarly $P(11)=2.5P(01)$. By symmetry $P(00)=P(11)$, $P(01)=P(10)$. Let $P(01)=P(10)=x$, $P(00)=P(11)=2.5x$. Sum: $7x=1$.

$$P(00)=P(11)=\tfrac{5}{14}=0.357 \qquad P(01)=P(10)=\tfrac17=0.143$$

**Entropy of each state:**

$$H(00)=0.8\log_2\tfrac1{0.8}+0.2\log_2\tfrac1{0.2}=0.722=H(11)$$

$$H(01)=0.5\log_2 2+0.5\log_2 2=1=H(10)$$

**Source entropy:**

$$H=2(0.357)(0.722)+2(0.143)(1)=0.516+0.286=0.801\ \text{bits/sym}$$

**LaTeX (TikZ) — diamond layout:**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=2.8cm,
   every state/.style={thick,minimum size=1cm}]
  \node[state] (a) {00};
  \node[state] (b) [below left=2cm and 2cm of a] {01};
  \node[state] (c) [below right=2cm and 2cm of a] {10};
  \node[state] (d) [below right=2cm and 2cm of b] {11};
  \path
   (a) edge[loop above] node{$0.8$} (a)
   (d) edge[loop below] node{$0.8$} (d)
   (a) edge[bend right=10] node[left]{$0.2$} (b)
   (b) edge[bend left=10]  node{$0.5$} (c)
   (c) edge[bend left=10]  node{$0.5$} (b)
   (c) edge[bend left=10]  node{$0.5$} (a)
   (b) edge[bend right=10] node[left]{$0.5$} (d)
   (d) edge[bend left=10]  node[right]{$0.2$} (c);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Same 3-step method but with 4 states. Spot the symmetry (00↔11, 01↔10) to solve quickly: $P=\tfrac{5}{14},\tfrac{5}{14},\tfrac17,\tfrac17$. Two states give $H_i=0.722$, two give $H_i=1$. Combine for $H\approx0.801$.

---

## Q20 — Two-state asymmetric, find H, G₁, G₂, G₃

**1(X): self 5/6, 1→2 1/6 (Z); 2(Y): self 2/5, 2→1 3/5 (Z).**

**State probabilities:** $\tfrac16P(1)=\tfrac35P(2)\Rightarrow P(1)=3.6P(2)$; sum $= 1 \Rightarrow 4.6P(2)=1$

$$P(1)=\tfrac{18}{23}=0.783 \qquad P(2)=\tfrac{5}{23}=0.217$$

**Entropy of each state:**

$$H_1=\tfrac56\log_2\tfrac65+\tfrac16\log_2 6=0.219+0.431=0.650$$

$$H_2=\tfrac35\log_2\tfrac53+\tfrac25\log_2\tfrac52=0.442+0.529=0.971$$

**Source entropy:**

$$H=0.783(0.650)+0.217(0.971)=0.509+0.211=0.720\ \text{bits/sym}$$

### Building $G_N$ — the probability tree

Start from $P(1)=\tfrac{18}{23}=0.783,\ P(2)=\tfrac{5}{23}=0.217$; each branch multiplies by its transition probability and emits its symbol. (Probabilities shown as decimals since the denominators are awkward.)

**LaTeX (TikZ/forest) — probability tree (3 levels):**

```latex
\documentclass{standalone}
\usepackage{forest}
\usepackage{amsmath}
\begin{document}
\begin{forest}
for tree={grow'=east, parent anchor=east, child anchor=west,
          edge={-stealth}, l sep=20mm, s sep=1.5mm, font=\footnotesize}
[{Start}
  [{$1$ $0.783$}, edge label={node[midway,above,font=\tiny]{$0.783$}}
    [{$X{:}1$ $0.652$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac56$}}
      [{$X{:}1$ $0.5435$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac56$}}
        [{$XXX=0.4529$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac56$}}]
        [{$XXZ=0.0906$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac16$}}]]
      [{$Z{:}2$ $0.1087$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac16$}}
        [{$XZZ=0.0652$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac35$}}]
        [{$XZY=0.0435$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac25$}}]]]
    [{$Z{:}2$ $0.1304$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac16$}}
      [{$Z{:}1$ $0.0783$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac35$}}
        [{$ZZX=0.0652$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac56$}}]
        [{$ZZZ=0.0130$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac16$}}]]
      [{$Y{:}2$ $0.0522$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac25$}}
        [{$ZYZ=0.0313$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac35$}}]
        [{$ZYY=0.0209$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac25$}}]]]]
  [{$2$ $0.217$}, edge label={node[midway,below,font=\tiny]{$0.217$}}
    [{$Z{:}1$ $0.1304$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac35$}}
      [{$X{:}1$ $0.1087$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac56$}}
        [{$ZXX=0.0906$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac56$}}]
        [{$ZXZ=0.0181$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac16$}}]]
      [{$Z{:}2$ $0.0217$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac16$}}
        [{$ZZZ=0.0130$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac35$}}]
        [{$ZZY=0.0087$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac25$}}]]]
    [{$Y{:}2$ $0.0870$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac25$}}
      [{$Z{:}1$ $0.0522$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac35$}}
        [{$YZX=0.0435$}, edge label={node[midway,above,font=\tiny]{$X\,\tfrac56$}}]
        [{$YZZ=0.0087$}, edge label={node[midway,below,font=\tiny]{$Z\,\tfrac16$}}]]
      [{$Y{:}2$ $0.0348$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac25$}}
        [{$YYZ=0.0209$}, edge label={node[midway,above,font=\tiny]{$Z\,\tfrac35$}}]
        [{$YYY=0.0139$}, edge label={node[midway,below,font=\tiny]{$Y\,\tfrac25$}}]]]]
]
\end{forest}
\end{document}
```

**$G_1$ — first-order symbols:**

| Symbol | How it arises | Probability |
|--------|---------------|-------------|
| X | $P(1)\cdot\tfrac56$ | $\tfrac{18}{23}\cdot\tfrac56=\tfrac{15}{23}=0.652$ |
| Y | $P(2)\cdot\tfrac25$ | $\tfrac{5}{23}\cdot\tfrac25=\tfrac{2}{23}=0.087$ |
| Z | $P(1)\cdot\tfrac16+P(2)\cdot\tfrac35$ | $\tfrac{3}{23}+\tfrac{3}{23}=\tfrac{6}{23}=0.261$ |

$$G_1=0.652\log_2\tfrac1{0.652}+0.261\log_2\tfrac1{0.261}+0.087\log_2\tfrac1{0.087}=0.402+0.506+0.306=\mathbf{1.214}$$

**$G_2$ — second-order** (depth-2 nodes; $ZZ$ merges: $0.0783+0.0217=0.1$):

| Seq | Prob | Seq | Prob | Seq | Prob |
|-----|------|-----|------|-----|------|
| XX | 0.5435 | XZ | 0.1087 | ZX | 0.1087 |
| ZY | 0.0522 | YZ | 0.0522 | YY | 0.0348 |
| ZZ | 0.1000 |  |  |  |  |

$$H(S^2)=0.4781+0.3480+0.3480+0.2223+0.2223+0.1686+0.3322 = 2.119$$
$$G_2=\frac{2.119}{2}=\mathbf{1.060}$$

> ⚠️ *(Correction: an earlier draft listed $G_2=1.080$ — the correct value is $\mathbf{1.060}$, from $H(S^2)=2.119$.)*

**$G_3$ — third-order** (the 15 leaves; $ZZZ$ merges: $0.0130+0.0130=0.0261$):

| Seq | Prob | Seq | Prob | Seq | Prob |
|-----|------|-----|------|-----|------|
| XXX | 0.4529 | XXZ | 0.0906 | ZXX | 0.0906 |
| XZZ | 0.0652 | ZZX | 0.0652 | XZY | 0.0435 |
| YZX | 0.0435 | ZYZ | 0.0313 | ZZZ | 0.0261 |
| ZYY | 0.0209 | YYZ | 0.0209 | ZXZ | 0.0181 |
| YYY | 0.0139 | ZZY | 0.0087 | YZZ | 0.0087 |

$$H(S^3)=2.889 \;\Rightarrow\; G_3=\frac{2.889}{3}=\mathbf{0.963}$$

**Verification:** $G_1(1.214)>G_2(1.060)>G_3(0.963)>H(0.720)$ ✓

**LaTeX (TikZ):**

```latex
\documentclass{standalone}
\usepackage{tikz}
\usetikzlibrary{automata,positioning,arrows.meta}
\begin{document}
\begin{tikzpicture}[->,>=Stealth,auto,node distance=4cm,
   every state/.style={thick,minimum size=1.1cm}]
  \node[state] (s1) {1 (X)};
  \node[state] (s2) [right=of s1] {2 (Y)};
  \path
   (s1) edge[loop left]  node{$\tfrac56\,(X)$} (s1)
   (s2) edge[loop right] node{$\tfrac25\,(Y)$} (s2)
   (s1) edge[bend left=18] node[above]{$\tfrac16\,(Z)$} (s2)
   (s2) edge[bend left=18] node[below]{$\tfrac35\,(Z)$} (s1);
\end{tikzpicture}
\end{document}
```

> **Steps explained:** Same method as Q14/Q17. Asymmetric $\Rightarrow$ solve for $P(1)=\tfrac{18}{23},\ P(2)=\tfrac{5}{23}$. Symbol X comes only from the 1→1 loop, Y only from the 2→2 loop, Z from both cross transitions. Build trees to depth 2 and 3 for $G_2, G_3$. The strictly-decreasing chain confirms the work.

---

# 📋 Master Formula Sheet

Memorize these — every question above is just an application of one or more.

### 1. Self-information

$$I_k=\log_2\frac{1}{P_k}=-\log_2 P_k\ \text{bits}$$

- $I_k$ = information in message $k$; $P_k$ = its probability.
- **Used in:** Q1, Q3, Q6, (and the dot/dash info parts of Q5, Q8).

### 2. Entropy (average self-information)

$$H(S)=\sum_{k=1}^{N}P_k\log_2\frac{1}{P_k}\ \text{bits/symbol}$$

- $N$ = number of symbols; $P_k$ = probability of symbol $k$.
- **Used in:** Q1, Q2, Q5, Q7, Q8, Q9, Q10, and every Markov question (for $H_i$, $H$, $G$).

### 3. Binary entropy (special case of #2 with two symbols)

$$H=P\log_2\tfrac1P+(1-P)\log_2\tfrac1{1-P}$$

- **Used in:** Q7 (plot), and any 2-symbol source (Q5, Q8, Q18).

### 4. Symbol rate

$$r_s=\frac{\text{number of symbols}}{\text{time}}=\frac{1}{\bar\tau}\ \text{symbols/sec}$$

- $\bar\tau$ = average duration of one symbol.
- **Used in:** Q1, Q4, Q5, Q8, Q12.

### 5. Information rate

$$R=r_s\,H(S)\ \text{bits/sec}$$

- **Used in:** Q1, Q4, Q5, Q8, Q12.

### 6. Maximum entropy (equally likely symbols)

$$H_{max}=\log_2 N$$

- $N$ = number of symbols.
- **Used in:** Q4 (max-info per frame), Q9 ($N$=2 ⇒ 1), Q10 ($N$=3 ⇒ 1.585), Q11.

### 7. Efficiency & Redundancy

$$\eta=\frac{H(S)}{H_{max}}\qquad \text{Redundancy}=1-\eta$$

- **Used in:** Q1, Q9, Q10.

### 8. Unit conversions

$$1\ \text{bit}=0.693\ \text{Nats}\quad(1\ \text{Nat}=1.4427\ \text{bits})$$

$$1\ \text{Hartley}=\log_2 10=3.3219\ \text{bits}$$

$$\log_2 x=\frac{\log_{10}x}{\log_{10}2}=3.3219\,\log_{10}x$$

- **Used in:** Q10 (Hartley & Nats), every numeric calculation.

### 9. Independent / composite messages (additivity)

$$I(m_1,m_2)=I(m_1)+I(m_2)$$

- Only when events are **independent**.
- **Used in:** Q2 (reason for log), Q3(iv).

### --- MARKOV SOURCE FORMULAS ---

### 10. State (stationary) probability balance

$$P(\text{state }j)=\sum_i P(\text{state }i)\,p_{ij} \qquad \sum_j P_j=1$$

- $p_{ij}$ = transition probability from state $i$ to state $j$.
- **Used in:** Q11–Q20 (step 1 of every Markov problem).

### 11. Entropy of a single state

$$H_i=\sum_{j}p_{ij}\log_2\frac{1}{p_{ij}}$$

- Sum over the **outgoing** branches of state $i$ only.
- **Used in:** Q11–Q20.

### 12. Entropy of the Markov source

$$H=\sum_i P_i\,H_i\ \text{bits/symbol}$$

- $P_i$ = state probability (from #10); $H_i$ = state entropy (from #11).
- **Used in:** Q11–Q20.

### 13. N-th order average information $G_N$

$$G_N=\frac{1}{N}\,H(S^N)=\frac{1}{N}\sum_{\text{all }N\text{-seq}}P(\text{seq})\log_2\frac{1}{P(\text{seq})}$$

- $G_1=H(\bar S)$ (entropy of single-symbol probabilities).
- $P(\text{seq})$ = product of probabilities along the tree path.
- **Property to verify:** $G_1\ge G_2\ge G_3\ge\dots\ge H$.
- **Used in:** Q11 ($G_1,G_2$), Q14 ($G_1,G_2,G_3$), Q16 ($G_1,G_2$), Q17 ($G_1,G_2,G_3$), Q20 ($G_1,G_2,G_3$).

---

# Quick Answer Table

| Q  | Key answers |
|----|-------------|
| 3  | 2 / 3.70 / 5.70 bits; (iv) yes (additive) |
| 4  | $R = 66.15\times10^5$ bits/s |
| 5  | $H=0.918$, $R\approx3.44$ bits/s |
| 6  | 1 bit; 4.70 bits more |
| 8  | $I_{dot}=0.415$, $I_{dash}=2$; $H=0.811$; $R=32.45$ bits/s |
| 9  | $\eta=99.28\%$, redundancy $=0.72\%$ |
| 10 | $\eta=74.53\%$, red $=25.47\%$; $H=1.181$ bits $=0.356$ Hartley $=0.819$ Nats |
| 11 | $H_i=1.5$, $H=1.5$, $G_1=1.585$, $G_2=1.5425$ |
| 12 | $P=\tfrac12,\tfrac14,\tfrac14$; $H=1.25$ bits/s |
| 13 | $P=\tfrac12,\tfrac14,\tfrac14$; $H=1.25$ |
| 14 | $H=0.857$, $G_1=1.557$, $G_2=1.309$, $G_3=1.178$ |
| 15 | $P(A)=P(C)=\tfrac{5}{18}$, $P(B)=P(D)=\tfrac29$; $H=0.984$ |
| 16 | $P=0.429,0.321,0.25$; $H=1.458$; $G_1=1.566$, $G_2=1.531$ |
| 17 | $H=0.811$, $G_1=1.561$, $G_2=1.280$, $G_3=1.139$ |
| 18 | $P(A)=P(B)=\tfrac12$; $H=0.918$ |
| 19 | $P=\tfrac{5}{14},\tfrac17,\tfrac17,\tfrac{5}{14}$; $H=0.801$ |
| 20 | $P(1)=\tfrac{18}{23}$, $P(2)=\tfrac{5}{23}$; $H=0.720$, $G_1=1.214$, $G_2=1.060$, $G_3=0.963$ |

---

### Exam tips

- For **any Markov problem**: always do them in order → state probabilities → state entropies → source entropy → ($G_N$ if asked). Same 4 steps every time.
- For **$G_N$**: draw the tree, multiply along branches, watch for **repeated sequences** (like ZZ) — add their probabilities.
- Always do the **sum = 1 check** on probabilities, and the **$G_1 \ge G_2 \ge G_3 \ge H$** check — they catch arithmetic slips instantly.
- The TikZ blocks are each a complete standalone document — paste into Overleaf and compile directly.

---

*Solutions compiled from notes by Dr. Madhavi M, ECE, GAT. Math renders on GitHub via native LaTeX ($\dots$ / $$\dots$$) support.*
