+++
title = "Ambient calculus"
weight = 2

[extra]
mathjax = "tex-mml"
+++

## Core Rules

### Replication of Open

$$ !(\mathrm{open} m.P) \mid m[Q]  \to  P \mid Q \mid !(\mathrm{open} m.P) $$


```
open_repl(M,{$p}), {amb(M1),{id,+M1,-M2,$mm},$q,@q}, {id,+M,+M2,$m}
  :- mell.copy({$p},A1,A2,A3,B1,B2,remove,P), {cp(A1,A2,A3)},
     {B1=B2}, $q, {id,+M3,$m,$mm}, open_repl(M3,P).

open_repl_aux@@ remove({$p}):- $p.
```


## Full

```
{ module(amb).

/* n[in m.P | Q] | m[R] --> m[n[P|Q] | R] */
in@@
{amb(N0), {id,+N0,$n}, {id,+M0,-M1,$m0}, in(M0,{$p}), $q,@q},
{amb(M2), {id,+M2,-M3,$m1}, $r,@r},
{id,+M1,+M3,$m2} :-
   {amb(M4), {id,+M4,+M5,-M,$m1},
      {amb(N2), {id,+N2,$n}, {id,-M5,$m0}, $p,$q,@q},
   $r,@r},
   {id,+M,$m2}.

/* m[n[out m.P | Q] | R] --> n[P|Q] | m[R] */
out@@
{amb(M0), {id,+M0,+M2,$m1}, {id,+N1,$n2},
  {amb(N0), {id,+M1,-M2,$m0}, {id,+N0,-N1,$n}, out(M1,{$p}), $q,@q},
  $r,@r} :-
    {amb(N2), {id,-M3,$m0}, {id,+N2,-N3,$n}, $p,$q,@q},
    {amb(M4), {id,+M3,+M4,$m1}, {id,+N3,$n2}, $r,@r}.

/* open m.P | m[Q] --> P|Q */
open@@
open(M,{$p}), {amb(M1), {id,+M1,-M2,$mm}, $q,@q}, {id,+M,+M2,$m} :-
   $p, $q, {id,$m,$mm}.

/* !(open m) | m[Q] --> Q | !(open m) */
open_repl@@  /* special case of !open */
open_repl(M,{$p}), {amb(M1), {id,+M1,-M2,$mm}, $q,@q}, {id,+M,+M2,$m} :-
   mell.copy({$p},A1,A2,A3,B1,B2,remove,P),{cp(A1,A2,A3)},{B1=B2},
   $q, {id,+M3,$m,$mm}, open_repl(M3,P).

open_repl_aux@@
remove({$p}) :- {$p}.

proxy_enter@@
{$p[M0,M1|*P],@p}, {id,+M0,+M1,$m} :-
   {$p[M0,M1|*P],@p, {id,+M0,+M1,-M}}, {id,+M,$m}.

proxy_resolve@@
{id,-M,$m0}, {id,+M,$m1} :- {id,$m0,$m1}.

proxy_insert_middle@@
{{{id,-M,$m},$p,@p},$q,@q} :- {{id,+M0,-M}, {{id,-M0,$m},$p,@p},$q,@q}.

proxy_insert_outer@@
{{id,+M0,$m0},$p,@p} :- {{id,-M,$m0},$p,@p}, {id,+M0,+M}.

proxy_merge_outer@@
{id,+M0,$m0}, {id,+M1,$m1}, {{id,-M0,-M1,$m2},$p,@p} :-
     {id,+M,$m0,$m1}, {{id,-M,$m2},$p,@p}.

local_name_in@@
{$p[M|*P],@p}, {id,+M} :- {{id,+M}, $p[M|*P],@p}.

global_name_out@@
{{id,name($n),+M0},{$p[M0|*M],@p},$q,@q} :- unary($n) |
    {{id,+M0,-M},{$p[M0|*M],@p},$q,@q}, {id,name($n),+M}.

root_merge@@
{id,name($n0),$m0}, {id,name($n1), $m1} :-
  unary($n0), unary($n1),
  $n0=$n1 |
  {id,name($n0),$m0,$m1}.

gc1@@ {id} :- .
gc2@@ {id,name($n)} :- unary($n) | .
gc3@@ {id,+X,$m}, {{id,-X}, $p,@p} :- {id,$m}, {$p,@p}.
gc4@@ amb.use :- .

}.
```

## Use

1.
 

```
{ amb.use. locks.
acquire(N,P) :- open(N,P).
release(N,{$p}) :- {amb.use, amb(N0), {id,+N0,-N}}, $p.

acquire(N0,{release(M0,{pp})}),
release(N1,{acquire(M1,{qq})}),
{id,+N0,+N1}, {id,+M0,+M1}.
}.
```

2. 

```
{ amb.use.  authentication.
{id,name(home),+H4}, {id,name(agent),+A4},
{amb.use. amb(H0), {id,+N0,+N3}, {id,+H0,+H3,-H4}, {id,+A3,-A4},
  open(N0,{}),
  {amb.use. amb(A), {id,+H1,+H2,-H3}, {id,+A,+A1,+A2,-A3}, {id,+N2,-N3},
   out(H1,{in(H2,{{amb(N1), {id,+N1,-N2}, 
                   out(A1,{open(A2,{pp})})}})})}}.
}.
```


