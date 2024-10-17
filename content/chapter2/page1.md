+++
title = "Proof Net Example"
weight = 1

[extra]
mathjax = "tex-mml"
+++

## 1. $(\lambda f\mathord{:} n \to n .\lambda x \mathord{:} n . f x)\:(\lambda x \mathord{:}n . x) \to (\lambda x \mathord{:} n . x)$ 


```
ax{ax_(A1),ax_(A2)},'?d'(A1,A3),'?w'(A4).
{
ax{ax_(B1),ax_(B2)},'?d'(B1,B3),'?w'(B4),'!'(B2,B5)
}.
'?c'({'?c_'(A3),'?c_'(B4)},C1).
'?c'({'?c_'(A4),'?c_'(B3)},C2).
tensor(B5,T1,D2),ax{ax_(T1),ax_(T2)}.
cut{cut_(A2),cut_(D2)}.

par(C2,T2,P1).
par(C1,P1,F).

{
ax{ax_(E1),ax_(E2)},'?d'(E1,E3),par(E3,E2,E4),'!'(E4,E5).

}.
tensor(E5,T3,D4),ax{ax_(T3),ax_(T4)}.
cut{cut_(F),cut_(D4)}.
formula(T4).
```

## 2. $(\lambda f \mathord{:}  n \to n .  \lambda x \mathord{:} n . f (f x) )(\lambda x \mathord{:} n . x)$

```
{
  ax{ax_(A1),ax_(A2)},'?d'(A1,A3).
  par(A3,A2,A5).
  '!'(A5,A7).
}.

tensor(A7,I1,I2).
ax{ax_(I1),ax_(I3)}.

{
  {ax{ax_(B1),ax_(B2)},'?d'(B1,B3),'?w'(B4),'!'(B2,B5)}.
  tensor(B5,E1,E2).
  ax{ax_(E1),ax_(E3)}.

  ax{ax_(C1),ax_(C2)},'?d'(C1,C3),'?w'(C4).
  '?c'({'?c_'(B4), '?c_'(C3)},D1).
  '?c'({'?c_'(B3),'?c_'(C4)},D2).
  cut{cut_(E2),cut_(C2)}.
  '!'(E3,E4).
}.

ax{ax_(F1),ax_(F2)},'?d'(F1,F3),'?w'(F4).
'?c'({'?c_'(F4),'?c_'(D2)},G1).
'?c'({'?c_'(F3),'?c_'(D1)},G2).

tensor(E4,H1,H2).
ax{ax_(H1),ax_(H3)}.
cut{cut_(H2),cut_(F2)}.

par(G1,H3,P1).
par(G2,P1,P2).

cut{cut_(I2),cut_(P2)}.
formula(I3).
```
