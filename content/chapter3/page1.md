+++
title = "Synchronous π-calculus"
weight = 1

[extra]
mathjax = "tex-mml"
+++

## Core Rules

### Communication Rule:

$$  (x(y).P + M \mid \bar{x} \langle z \rangle.Q + N) \to P[z/y] \mid Q $$


```
comm@@ 
{$x,+C1,+C2},{get(C1,Y,{$p[Y|*V1]}),$m}, {snd(C2,Z,{$q}),$n}
  :- {$x},$p[Z|*V1],$q,mell.delete({$m,$n},d).
```

## Full

```
{ module(pi).
  comm@@ {$x,+C1,+C2}, {once, get(C1,Y,{$b1[Y|*V1]}), $c1},
                       {snd(C2,Z,{$b2}),  $c2} :-
           {$x}, $b1[Z|*V1], $b2, mell.delete({$c1,$c2},d).
  repl@@ {$x,+C1,+C2}, {bang, get(C1,Y,{$b1[Y|*V1]}), $c1},
                       {snd(C2,Z,{$b2}),  $c2} :-
           {$x,+C3}, $b2, nlmem.kill({$c2}),
           {bang, get(C3,Y2,Body2),$c1},
           nlmem.copy({$b1[U|*V1]},cp,Copies), 
           copies(Copies,U,Body2,Z,Y2).
  aux1@@ copies(cp(C1,C2),cp(D1,D2),Body2,Z,Y2),
         {$c1[D1|*C1],+C1}, {$c2[D2|*C2],+C2} :-
           $c1[Z|*C1], {$c2[Y2|*C2], enter(Body2)}.
  aux2@@ {$m[X|*L]}, {enter(X),$c} :- {$m[X|*L], {+X,$c}}.
  kill@@ {+C,$c}, killed(C) :- {$c}.
  copy@@ {+C,$c}, cp(C1,C2,C) :- {+C1,+C2,$c}.
  gc1@@  {name($n)} :- unary($n) | .
  gc2@@  {name} :- .
  gc3@@  {} :- .
  gc4@@  pi.use :- .
  gc5@@  { module(pi), @a} :- .

  unif1@@ [] = [] :- .
  unif2@@ [A1|D1] = [A2|D2] :- A1=A2, D1=D2.
}.


```
