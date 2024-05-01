# Hierarchical clustering

## Linkage distances

Consider the following two sets of points: $S_1 = \{1,2,3\}$ and
$S_2 = \{4,5,6\}$. What is the min linkage (aka single linkage) distance
between $S_1$ and $S_2$? What is the max linkage (aka complete linkage)
distance between $S_1$ and $S_2$?

:::{note} Solutions
:class: dropdown
Min linkage distance is 1, max linkage distance is 5.
:::

## Leaf order

When we make a dendrogram, there is a leaf node for each of the original
data points. These leaf nodes are placed along the horizontal axis in a
special order. True or false: given a particular choice of linkage
distance, there is typically only one way the leaves can be ordered.

:::{note} Solutions
:class: dropdown
False. There are always many possibilities.
:::

## By hand

Consider this dissimilarity matrix. $$\Delta=\begin{pmatrix}
0 &1 &4 & 2 \\
1 &0 &3 & 5 \\
4 &3& 0 & 10 \\
2 & 5 & 10 & 0
\end{pmatrix}$$ Perform agglomerative clustering by hand using min
linkage. Sketch the dendrogram (you can upload a picture of something
sketched on paper). Be sure to clearly indicate height of each merge.

:::{note} Solutions
:class: dropdown
Let's give the samples names, A, B, C and D.

```
       A   B   C    D
  --- --- --- ---- ----
  A    0   1   4    2
  B    1   0   3    5
  C    4   3   0    10
  D    2   5   10   0
```

1.  At step one, distance matrix says A and B are closest (merge at
    height 1). Let's merge them into a new internal node we'll call E.
    Now recompute distances using min linkage: $d(E,C)=\min(4,3)=3$ and
    $d(E,D)=\min(2,5)=2$.

    ```
           E   C    D   
      --- --- ---- ---- 
      E    0   3    2   
      C    3   0    10  
      D    2   10   0   
    ```

2.  At step two, distance matrix says E and D are closest (merge at
    height 2). Let's merge them into a new internal node we'll call F.
    Now recompute distances using min linkage:
    $d(F,C)=\min(d(E,C),d(D,C))=\min(3,10)=3$. So

    ```
           F   C     
      --- --- ---
      F    0   3     
      C    3   0  
    ```   

    Final merge is at height 3.

```

    3             ----root----
                  |          |
    2      |------F---|      |
           |          |      |
    1   ---E----      |      |
        |      |      |      |
        A      B      D      C
```
:::

