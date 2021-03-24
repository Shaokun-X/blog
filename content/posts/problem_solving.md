---
title: "Problem-solving with State Graph"
date: 2021-03-10T09:17:19+01:00
description: "A brief description"
tags: ["State Graph", "AI", "Problem Solving"]
draft: true
---

## Typology of three types of problem
### 1. combinatoire
definition


trouver dans un ensemble X donne les elements x satisfaisant un ensemble de contraintes donnees K(x)

### 2. changement

### 3. décompositon en sou-problèmes

## General problem solver

The idea is to finda a way to reduce the difference between the initial state and the desired state.

1. transformation

    Build a list of differences between object A and object B

2. Reduction

    Find a operator to dimish the list of differences

3. Application of Operators

    Apply the operators to the element in the list of differences. The result can be applied with oeprators again

## State graph

Nodes represent the statea and the arcs represent the transitions.

The cost the a graph is the sum of the cost of all possible paths, which can be approximately denoted as b^n, where b is the cost of the sum of outgoing arc from a node, the n is the deepth of the graph.

## Heuristic search

### Classic
1. depth-first
2. broadth-first

### Heuristic
The information of a node:
```
F(n) = G(n) + H(n)

G(n) = coût nécessaire pour parvenir de ei à n. 
H(n) = estimation de la distance au nœud objectif. 
```
H(n) is the hyper-knoledge about the state graph, that is why the search is called *heuristic*.

#### General search algorithm in graph state
```
1. Put root node in the available list
2. Choose the node that has least F(n)
3. Find the children of the chosen node and calculate their H(n)
4. Repeat 2-3 till reach the objective
```

#### A<sup>*</sup> Algorithm
```python
OUVERT = {ei} ;
FERME = {} ;
Succès = Faux ;
Tant que OUVERT != {} et Succès == Faux
    # choose best node
    Choix d’un nœud n dans OUVERT tq f(n) minimum
    Supprimer n de OUVERT ;
    Ajouter n à FERME ;
    # update available nodes info
    pour tout successeur s de n
        # if the node is found first time
        si s in (OUVERT and FERME}
            père(s) = n
            g(s) = g(n) + c(n,s)
            f(s) = g(s) + h(s)
            ajouter s à OUVERT
        Sinon
        # if the node has been found before
            # only update when has better value
            Si (g(s) > g(n) + c(n,s) )
                # if the node hasn't been visited (not in the path), simply update the node
                si (s in OUVERT)) {
                    g(s) = g(n) + c(n,s)
                    père(s) = n
                # if the node is in the path, update the node, the path (by changing the parent), the available node set (treatSuccessor)
                sinon
                    g(s) = g(n) + c(n,s)
                    père(s) = n
                    traitementSuccesseur(s)
        
```