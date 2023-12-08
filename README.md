|NAME     |TOPIC       |TITLE                                                    |
|:-------:|:----------:|:-------------------------------------------------------:|
|Eli Scott|Power Method|Background, Analysis, and Application of the Power Method|

# Power Method
## Table of Contents
* [Overview](#overview)
* [Background](#background)
* [Power Method Overview](#power-method-overview)
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
The power method starts by choosing a random vector, $x_0$, such that [2]:
$$x_0 = a_1x_1 + a_2x_2 + ... + a_nx_n$$
where $a_1, a_2, ..., a_n$ are random numbers and $x_1, x_2, ..., x_n$ are the eigenvectors of A correspoding respectively to the eigenvalues $\lambda_1, \lambda_2, ..., \lambda_n$ such that $|\lambda_1| >= |\lambda_2| >= ... >= |\lambda_n|$. Power method utilizes the mathmatic property [2]:
$$Ax_0 = a_1\lambda_1x_1 + a_2\lambda_2x_2 + ... + a_n\lambda_nx_n$$
Then, by iteratively multiplying a given matrix A to the random vector, $x_0$, the resulting vector will converge to the dominant eigenvector [2]:
$$A^kx_0 = a_1\lambda_1^kx_1 + a_2\lambda_2^kx_2 + ... + a_n\lambda^k_nx_n$$
$$A^kx_0 = \lambda_1^k(a_1x_1 + a_2(\frac{\lambda_2}{\lambda_1})^kx_2 + ... + a_n(\frac{\lambda_n}{\lambda_1})^kx_n)$$
As k gets arbitrarily large, $\lim_{k\to\infty} (\frac{\lambda_2}{\lambda_1})^k = 0$ and $\lim_{k\to\infty} (\frac{\lambda_n}{\lambda_1})^k = 0$ since $|\lambda_1| >= |\lambda_2| >= ... >= |\lambda_n|$. This simplifies the equation leaving:
$$A^kx_0 = \lambda_1^ka_1x_1$$
From this equation, $\lambda_1$ and $x_1$, which are the dominant eigenvalue and eigenvector, are obtained. The process of approaching the dominant eigenvector through each iteration can be seen by:

![](Animation_of_the_Power_Iteration_Algorithm.gif) [3]

The algorithm stops when consecutive iterations have been mutliplied by the same number with this number being the dominant eigenvalue.
## Improvements
One of the obvious problems with the initial algorithm for the power method is that, as k increases, $\lambda_1^ka_1x_1$ can increase to an extremely large value or an extremely small value if $|\lambda_1| < 1$. This will cause an overflow or an underflow as the measured vector gets either too large or small for the computer to do calculations. Therefore, normalization or scaling in each iteration is required so that the measured vector is able to continually be utilized [2]. Normalization is done by dividing the vector at iteration by its largest value, the infinity norm, so that the new largest value in the vector is 1. If $x_k$ is the kth iteration of the algorithm, then $x_k = \frac{Ax_{k-1}}{\lVert Ax_{k-1} \rVert_{\infty}}$. Normalization or scaling keeps the vector after each iteration from becoming either too large or too small. It is also good practice to normalize the random starting vector, $x_0$, such that its greatest value, $\lVert x_0 \rVert_{\infty}$, becomes 1. 

Pseudocode for power method with normalization [4]:
```
x0 = arbitrary nonzero starting vector
x0 = x0/max(x0)                            {Normalize the starting vector}
x = A*x0/max(A*x0)                         {First iteration with normalization}
while x ~= x0                              {Loop until the vectors are equal}
  x0 = x                                   {Overwrite x0 for next iteration}
  x = A*x0/max(A*x0)                       {Find next iteration with normalization}
end
eigVal = A*x/x                             {Having found eigenvector, x, solve for corresponding eigenvalue}
```

Another alteration to the power method is the shifting of eigenvalues. If $Ax = \lambda x$, then the eigenvalues can be shifted by any number $\alpha$ through $(A-\alpha I)x = (\lambda-\alpha)x$ [5]. If the aproximate area of an eigenvalue is known for matrix A, then matrix A can be shifted by $\alpha$, and it will converge to $\sigma$ through shifted inverse power method such that $\sigma = \frac{1}{\lambda-\alpha}$ where $\lambda$ is the desired eigenvalue. Having started with a random vector $x_0$, the shifted inverse power method is defined as the iteration [5]:
$$y_k = (A-\alpha I)^{-1}x_k$$
$$x_{k+1} = \frac{y_k}{\lVert y_k \rVert_{\infty}}$$
$x_{k+1}$ will converge to the eigenvector which corresponds to the shifted eigenvalue $\sigma$. Then, the actual desired eigenvalue can be found by solving $\lambda = \frac{1}{\sigma} + \alpha$. Therefore, by choosing a random $\alpha$ which is close to a desired eigenvalue which is not the dominant eigenvalue, shifted inverse power method can be employed and will converge to a value $\sigma$ which can be used to find the desired eigenvalue and eigenvector.
## Strengths and Limitations

## Common Applications

## References
1. 0.3 POWER METHOD FOR APPROXIMATING EIGENVALUES. (n.d.). [https://ergodic.ugr.es/cphys/LECCIONES/FORTRAN/power_method.pdf](https://ergodic.ugr.es/cphys/LECCIONES/FORTRAN/power_method.pdf)
2. Lecture # 10 The Power Method for Eigenvalues. (n.d.). from [https://www.cse.psu.edu/~b58/cse456/lecture10.pdf](https://www.cse.psu.edu/~b58/cse456/lecture10.pdf)
3. File:Animation of the Power Iteration Algorithm.gif - Wikipedia. (2020, February 6). Commons.wikimedia.org. [https://en.wikipedia.org/wiki/File:Animation_of_the_Power_Iteration_Algorithm.gif#file](https://en.wikipedia.org/wiki/File:Animation_of_the_Power_Iteration_Algorithm.gif#file)
4. Vector iteration (power method) 7.1 Simple vector iteration. (n.d.). from [https://people.inf.ethz.ch/arbenz/ewp/Lnotes/chapter7.pdf](https://people.inf.ethz.ch/arbenz/ewp/Lnotes/chapter7.pdf)
5. Yu-Kai Hong, An introduction to the Power Method and (shifted/Inverse) Power Method, 2007, from [http://www.math.nuk.edu.tw/jinnliu/Software_Engineering/IS_PowerMethod.pdf](http://www.math.nuk.edu.tw/jinnliu/Software_Engineering/IS_PowerMethod.pdf)
6. 
