+++
title = "Additional Rules"
weight = 3

[extra]
mathjax = "tex-mml"
+++

## Pull-Equivalence: 

```
{
system_ruleset.
  
box_contraction_pull@@
{'!'(X1,X2),'?c'({'?c_'(X3),'?c_'(X4)},X5),$p[X1,X3,X4|*X]}
  :- {'!'(X1,X2),$p[X1,X3,X4|*X]},'?c'({'?c_'(X3),'?c_'(X4)},X5).

box_contraction_push@@
/{'!'(X1,X2),$p[X1,X3,X4|*X]},'?c'({'?c_'(X3),'?c_'(X4)},X5)
  :- {'!'(X1,X2),'?c'({'?c_'(X3),'?c_'(X4)},X5),$p[X1,X3,X4|*X]}.

box_weakening_pull@@
{'!'(X1,X2),'?w'(X3),$p[X1|*X]}
  :- {'!'(X1,X2),$p[X1|*X]},'?w'(X3).

box_weakening_push@@
{'!'(X1,X2),$p[X1|*X]},'?w'(X3)
  :- {'!'(X1,X2),'?w'(X3),$p[X1|*X]}.

}.
```

## $?c$-$?w$ fusion

```
{
system_ruleset.

contraction_weakening_fusion@@
'?c'({'?c_'(C1),'?c_'(C2)},C3),'?w'(C2)
  :- C1=C3.
}
```
