|NAME     |TOPIC       |TITLE                                                    |
|:-------:|:----------:|:-------------------------------------------------------:|
|Eli Scott|Power Method|Background, Analysis, and Application of the Power Method|

# Power Method
## Table of Contents
* [Overview](#overview)
* [Background](#background)
* [Power Method Overview](#power-method-overview)
* [Formulation](#formulation)
* [Improvements](#improvements)
* [Strengths and Limitations](#strengths-and-limitations)
* [Common Applications](#common-applications)
* [References](#references)
## Overview
The power method is an algorthim which is utilized in solving eigenvalue problems. The power method is an iterative algorithm that finds the dominant eigenvalue and eigenvector of a given matrix [1]. This becomes an effective algorithm when only the dominant eigenvalue and eigenvector is desired.
## Background
An eigenvalue problem for which this algorithm is used can be defind as [1]:
$$(A-\lambda I)x = 0$$
where A is an abritrary square matrix, I is the identity matrix, and $\lambda$ and and x are the corresonding eigenvalues and eigenvectors which solve the equation. Eigenvalues are the values for which $A\lambda = Ax$ is a true statement. Thus, mulitplying an eigenvector by the matrix is equivalent to multiplying the eigenvector with its corresponding eigenvector. If A is an nxn matrix (having n number rows and n number columns), then matrix A can have up to n number eigenvalues and eigenvectors. Therefore, as matrix A gets arbitrarily large, it can become computationaly expensive to compute all of the eigenvalues and eigenvectors for a given matrix. Consequently, for many problems, only the dominant eigenvalue and eigenvector is desired. Henceforth, the power method was devised which cuts out much unneccesary computation by only solving for the dominant eigenvector and eigenvalue. 
## Power Method Overview

## Formulation

## Improvements

## Strengths and Limitations

## Common Applications

## References
1. 0.3 POWER METHOD FOR APPROXIMATING EIGENVALUES. (n.d.). https://ergodic.ugr.es/cphys/LECCIONES/FORTRAN/power_method.pdf
