+++
title = "Specifications"
weight = 1

[extra]
mathjax = "tex-mml"
+++

## mell.delete

`mell.delete` is a construct for deleting a membrane with an indefinite number of free links.

```
mell.delete(X,A),{$p[X|*X]},{$a} :- $a[*X].
```

## mell.copy

`mell.copy` is a construct for copying a membrane with an indefinite number of free links.

```
mell.copy(X,A1,A2,A3,B1,B2,C1,C2),
{$p[X|*Z]},{$a[A1,A2,A3]},{$b[B1,B2]}
            :- {$p[X'|*Z']},{$p[X''|*Z'']}, 
               $a[*Z',*Z'',*Z],$b[X'',C1],$b[X'',C2].
```
