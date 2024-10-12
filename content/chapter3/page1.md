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
           {$x,+C3}, $b2, mell.delete({$c2}),
           {bang, get(C3,Y2,Body2),$c1},
           mell.copy({$b1[U|*V1]},A1,A2,A3,Copies), 
           {cp(A1,A2,A3)},copies(Copies,U,Body2,Z,Y2).
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

## Examples

Examples are taken from 

[1] Milner, R., The Polyadic pi-Calculus, a Tutorial ECS-LFCS-91-180

[2] Milner, R., Communicating and Mobile Systems: the Pi-Calculus Cambridge Univ. Press, 1999.


1. $(\nu z)\left(\bar{x} \langle y \rangle + \bar{z}(w).\bar{w} \langle y \rangle \right) \mid \bar{x}(u).\bar{u} \langle v \rangle \mid \bar{x} \langle z \rangle \quad \longrightarrow^* \quad (\nu z)\left(\bar{y}(v) \mid \bar{x}(z)\right) \text{ or } \bar{v}(y)$


```
{ example1. pi.use.
  {snd(X0,Y,{})},
  {once,get(X1,U,{{snd(U,V,{})}})},
  {snd(X2,Z,{})},
  {name(x),+X0,+X1,+X2}, {name(y),+Y}, {name(z),+Z}, {name(v),+V}
}.
```

2. $(\nu x)(\bar{x} \langle y \rangle .0 \mid \bar{x}(u).\bar{u} \langle v \rangle .0) \mid \bar{x} \langle z \rangle .0 \quad \longrightarrow^* \quad \bar{y} \langle v \rangle \mid \bar{x} \langle z \rangle$


```
{ example2. pi.use.
  {snd(LX0,Y,{})},
  {once,get(LX1,U,{{snd(U,V,{})}})},
  {snd(X2,Z,{})},
  {name,+LX0,+LX1},
  {name(x),+X2}, {name(y),+Y}, {name(z),+Z}, {name(v),+V}
}.
```

3. $(\nu z)(\bar{x} \langle y \rangle + \bar{z}(w).\bar{w} \langle y \rangle) \mid \bar{x}(u).\bar{u} \langle v \rangle \mid \bar{x} \langle z \rangle \quad \longrightarrow^* \quad (\nu z)(\bar{y}(v) \mid \bar{x}(z)) \text{ or } \bar{v}(y)$
 

```
{ example3. pi.use.
  {once,snd(X0,Y0,{}),get(Z0,W,{{snd(W,Y1,{})}})},
  {once,get(X1,U,{{snd(U,V,{})}})},
  {once,snd(X2,Z1,{})},
  {name(x),+X0,+X1,+X2}, {name(y),+Y0,+Y1}, {name,+Z0,+Z1},
  {name(v),+V}
}.
``` 

4. 

$(\nu b\ c\ m_1\ m_2)\left(\bar{c} \langle x_1, \bar{l l}, m_1 \rangle \mid \bar{b} \langle m_1, m_2 \rangle \mid \bar{c} \langle x_2, m_2, \bar{r r} \rangle \mid !\bar{b}(i, o).i(x).\bar{c} \langle x, i, o \rangle \mid !\bar{c}(y, i, o), o \langle y \rangle, \bar{b}(i, o)\right)$

$\quad \longrightarrow^* \quad \bar{l l}(x).\bar{c} \langle x, \bar{l l}, m_1 \rangle \mid m_2 \langle x_1 \rangle.\bar{b} \langle m_1, m_2 \rangle \mid \bar{r r} \langle x_2 \rangle.\bar{b} \langle m_2, \bar{r} \rangle$


```
{ example4. pi.use.
   {snd(C1,S1,{})}, S1=[x1,ll,M11],
   {snd(B1,S2,{})}, S2=[M12,M21],
   {snd(C2,S3,{})}, S3=[x2,M22,rr],
   {bang,get(B2,G1,
          {G1=[cp(I1,I2),O], 
             {once,get(I1,X,{{snd(C3,S4,{})}, S4=[X,I2,O]})}})},
   {bang,get(C4,G2,
          {G2=[Y,I,cp(O1,O2)], 
             {snd(O1,Y,{{snd(B3,S5,{})}, S5=[I,O2]})}})},
   {name,+B1,+B2,+B3}, {name,+C1,+C2,+C3,+C4},
   {name,+M11,+M12}, {name,+M21,+M22}.
}.
```
