
[![Build Status](https://travis-ci.com/vrunge/M2algorithmique.svg?branch=main)](https://travis-ci.com/vrunge/M2Algorithmique)

# M2algorithmique Vignette

### Vincent Runge

#### LaMME, Evry Paris-Saclay University

### November 3, 2020

> [Quick Start](#qs)

> [Comparing time complexity of the 4 algorithms](#com)

> [Coming soon: other simulations...](#oth)

<a id="qs"></a>

## Quick Start

The `M2algorithmique` R package is an **example package** developed for students building their own R/Rcpp package as part of the **algorithmic M2 courses**. This package contains two algorithmic strategies (**insertion sort** and **heap sort**) implemented in R and in Rcpp.

Insertion sort is of time complexity *O*(*n*<sup>2</sup>) as heap sort is *O*(*n*log(*n*)) (worst case complexity). We aim at highlighting two important features with this package:

1.  Rcpp algorithms are **much more efficient** than their R counterpart
2.  Time complexities **can be compared to** one another

*All the simulations presented in this README file are available in the `mesTests.R` file in the tests folder.*

### Package installation

You first need to install the `devtools` package, it can be done easily from Rstudio. We install the package from Github (remove the \# sign):

``` r
#devtools::install_github("vrunge/M2algorithmique")
library(M2algorithmique)
```

### A first simple test

We simulate simple data as follows, with `v` a vector as size `n` containing all the integers from `1` to `n` (exactly one time) in any order.

``` r
n <- 10
v <- sample(n)
```

We've implemeted 4 algorithms:

-   `insertion_sort`
-   `heap_sort`
-   `insertion_sort_Rcpp`
-   `heap_sort_Rcpp`

They all have a simple argument: the unsorted vector `v`.

``` r
v
```

    ##  [1]  8  5  9  7  2  6  3  1  4 10

``` r
insertion_sort(v)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10

`insertion_sort(v)` returns the sorted vector from `v`.

<a id="com"></a>

## Comparing time complexity of the 4 algorithms

We run all the following example at a fixed vector length `n = 10000`.

### One simulation

We define a function `one.simu` to simplify the simulation study for time complexity.

``` r
one.simu <- function(n, type = "sample", func = "insertion_sort")
{
  if(type == "sample"){v <- sample(n)}else{v <- n:1}
  if(func == "insertion_sort"){t <- system.time(insertion_sort(v))[[1]]}
  if(func == "heap_sort"){t <- system.time(heap_sort(v))[[1]]} 
  if(func == "insertion_sort_Rcpp"){t <- system.time(insertion_sort_Rcpp(v))[[1]]}
  if(func == "heap_sort_Rcpp"){t <- system.time(heap_sort_Rcpp(v))[[1]]}
  return(t)
}
```

We evaluate the time with a given n over the 4 algorithms

``` r
n <- 10000
one.simu(n, func = "insertion_sort")
```

    ## [1] 3.381

``` r
one.simu(n, func = "heap_sort")
```

    ## [1] 0.269

``` r
one.simu(n, func = "insertion_sort_Rcpp")
```

    ## [1] 0.028

``` r
one.simu(n, func = "heap_sort_Rcpp")
```

    ## [1] 0.002

### Some comparisons

we compare the running time at a given length n with repeated executions (nbSimus times)

``` r
nbSimus <- 10
time1 <- 0
time2 <- 0
time3 <- 0
time4 <- 0
for(i in 1:nbSimus){time1 <- time1 + one.simu(n, func = "insertion_sort")}
for(i in 1:nbSimus){time2 <- time2 + one.simu(n, func = "heap_sort")}
for(i in 1:nbSimus){time3 <- time3 + one.simu(n, func = "insertion_sort_Rcpp")}
for(i in 1:nbSimus){time4 <- time4 + one.simu(n, func = "heap_sort_Rcpp")}

#gain R -> Rcpp
time1/time3
```

    ## [1] 87.8981

``` r
time2/time4
```

    ## [1] 164.8636

``` r
#gain insertion -> heap
time1/time2
```

    ## [1] 10.22691

``` r
time3/time4
```

    ## [1] 19.18182

``` r
#max gain
time1/time4
```

    ## [1] 1686.045

<a id="oth"></a>

## Coming soon: other simulations...
