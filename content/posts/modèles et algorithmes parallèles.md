---
title: "Modèles Et Algorithmes Parallèles"
date: 2021-03-04T09:22:07+01:00
description: "A brief description"
tags: ["High Performance Computing"]
draft: false
---
## Classification de Flynn
- SISD *mono-core PC*
- SIMD *some multi-dimensional data, image/matrix etc.*
- MISD *not exist*
- MIMD *mulitple cores, paralle calculation*
    - shared memory
    - distributed memory

## Matrice to measure perfomance

### Definition
```
T1: The time for one core sequential calculation.
Tn: The time for parallel calculation. N signifies the number of processors.
speed_up = T1/Tn
O: The overhead, the cost of communication between processors, synchronization, etc..
```
### Fixed-sized model (Loi d’Amdahl)
The proportion `f` part is fixed, which means it is independent from `n`.
Define that:
```
f: The proportion of a task to which parallel calculation can be applied.
```
```
Evidently,
speed_up < n, speedup > 1
If we neglect O, then
Tn = (1-f)T1 + fT1/n
Assume we have infinite cores, that is n -> ∞, we can have
speed_up = 1 / (1-f)

Therefore,
speed_up < n and speed_up < 1 / (1-f)
```

### Scaled-sized model (Loi de Gustafson)
The proportion `f` is a linear function to `n`.
```
Tn: sequential time
Tp: parallel time
```
```
In this case, T1 = Ts + Tp, Tn = Ts + nTp, we can easily deduct that (neglect O)
SSpeadup = 1 + (n-1)f
Still, the range is (1, n)
```

### Shared memory V.S. Distributed memory
#### Shared memory
- problem of the coherence between cache and memory
- wide addressing range
- need extra algorithm to access to the shared memory

#### Distributed memory
- hard to synchronize

## Modèle PRAM

### Polynomial time
T(n) = O(n<sup>k</sup>)

States that polynomial time is a synonym for **tractable**, **feasible**, **efficient**, or **fast**.

### P versus NP problem
The general class of questions for which some algorithm can provide an answer in polynomial time is called "class P".

The class of questions for which no polynominal time algorithm can be used to find an answer, **but can be verified in ploynominal time**.

### Une (machine) PRAM
- Has unlimited CPUs/cores
- Has infinite global shared memory, the time of accessing any unit of the memory is equal
- Has a set of finite instructions executed synchronously

### CREW, EREW, CRCW
```
C - concurrent
E - exclusive
R - read
W - write
```

1. EREW
   - At any time only one processor can read a memory unit
2. CREW
   - Concurrently read a memory unit is okay, but not write
3. CRCW
   - Common-mode-CRCW: all processors have to write the same value
   - Arbitraty-mode-CRCW: picks one value of the processors to write
   - Priority-mode-CRCW: write the value that belongs to the most prioritized processor

### Indicators of complexity
```
An: The sequential algorithm with given input of scale n
number of processors: H(An)
execution time: T(An)
work: H(An) * T(An)
```

[slides](https://ecampus.emse.fr/pluginfile.php/9175/mod_resource/content/0/cours_modeles.pdf) | 
[NC complexity](https://en.wikipedia.org/wiki/NC_%28complexity%29)
