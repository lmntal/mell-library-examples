+++
title = "Cut Elimination Rules"
weight = 2

[extra]
mathjax = "tex-mml"
+++

## Core Rules:

### $!$-$?w$

```
promotion_weakening@@
{'!'(X1,X2),$p[X1|*X]},cut{cut_(X2),cut_(X3)},'?w'(X3) 
  :- mell.delete(X1,W),{$p[X1|*X]},{'?w'(W)}.
```

### $!$-$?c$

```
promotion_contraction@@
{'!'(X1,X2),$p[X1|*X]},cut{cut_(X2),cut_(X3)},'?c'({'?c_'(C1),'?c_'(C2)},X3)
  :- mell.copy(X2,A1,A2,A3,B1,B2,C1,C2),{'!'(X1,X2),$p[X1|*X]},
      {'?c'({'?c_'(A1),'?c_'(A2)},A3)},{cut{cut_(B1),cut_(B2)}}.
```

## Full: 

```
{ 
system_ruleset.
// basic
//// ax-cut
ax_cut@@
cut{cut_(X),cut_(Y)},ax{ax_(Y),ax_(Z)} 
  :- X=Z.

//// tensor-par
tensor_par@@
tensor(X1,Y1,XY1),par(X2,Y2,XY2),cut{cut_(XY1),cut_(XY2)} 
  :- cut{cut_(X1),cut_(X2)},cut{cut_(Y1),cut_(Y2)}.

// box
//// !-?d
promotion_dereliction@@
{'!'(X1,X2),$p[X1|*X],@r},cut{cut_(X2),cut_(X3)},'?d'(X4,X3) 
  :- cut{cut_(X1),cut_(X4)}, $p[X1|*X],@r.

//// !-!
promotion_promotion@@
{'!'(X1,X2),$p[X1|*X],@r1},cut{cut_(X2),cut_(X3)},{$q[X3|*X],@r2}
  :- {{'!'(X1,X2),$p[X1|*X],@r1},cut{cut_(X2),cut_(X3)},$q[X3|*X],@r2}.

//// !-?w
promotion_weakening@@
{'!'(X1,X2),$p[X1|*X]},cut{cut_(X2),cut_(X3)},'?w'(X3) 
  :- mell.delete(X1,W),{$p[X1|*X]},{'?w'(W)}.

//// !-?c
promotion_contraction@@
{'!'(X1,X2),$p[X1|*X]},cut{cut_(X2),cut_(X3)},'?c'({'?c_'(C1),'?c_'(C2)},X3)
  :- mell.copy(X2,A1,A2,A3,B1,B2,C1,C2),{'!'(X1,X2),$p[X1|*X]},
      {'?c'({'?c_'(A1),'?c_'(A2)},A3)},{cut{cut_(B1),cut_(B2)}}.

}.
```
