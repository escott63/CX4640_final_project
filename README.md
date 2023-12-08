---
Name: Eli Scott
Topic: 13
Title: Background, Analysis, and Applications of the Power Method
----


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

Example: Given matrix A = [1 -2; -4 3] which has eigenvalues of -1 and 5

Initial vector, $x_0$, = $[1, -1]^T$.

|Kth Iteration|$x_k^T$       |Ratio|
|:-----------:|:------------:|:---:|
|0            |1,          -1|     |
|1            |3,          -7|7    |
|2            |17,        -33|4.714|
|3            |83,       -167|5.061|
|4            |417,      -833|4.988|
|5            |2083,    -4167|5.002|
|6            |10417,  -20833|4.999|
|7            |52083, -104167|5.000|

As shown, in only 7 iteration, the power method approached the dominant eigenvalue of 5. Additionally, $x_k^T$ converged to the corresponding dominant eigenvector $[-0.5, 1]^T$ as $\frac{52083}{-104167} = -0.500$.

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

Example: Given matrix A = [1 -2; -4 3] which has dominant eigenvalues of 5 with corresponding eigenvector of $[-0.5, 1]^T$

Initial vector, $x_0$, = $[1, -1]^T$.

|Kth Iteration|$x_k^T$       |Normalized $x_k^T$|
|:-----------:|:------------:|:----------------:|
|0            |1,          -1|1,              -1|
|1            |3,          -7|-0.429,          1|
|2            |-2.429,  4.714|-0.515,          1|
|3            |-2.515,  5.061|-0.497,          1|
|4            |-2.497,  4.988|-0.501,          1|
|5            |-2.501,  5.002|-0.5,            1|
|6            |-2.5,        5|-0.5,            1|

Thus, normalized power iteration works as it still approached the dominant eigenvector $[-0.5, 1]^T$ without the vector, $x_k^T$, greatly increasing in size.

Another alteration to the power method is the shifting of eigenvalues. If $Ax = \lambda x$, then the eigenvalues can be shifted by any number $\alpha$ through $(A-\alpha I)x = (\lambda-\alpha)x$ [5]. If the aproximate area of an eigenvalue is known for matrix A, then matrix A can be shifted by $\alpha$, and it will converge to $\sigma$ through shifted inverse power method such that $\sigma = \frac{1}{\lambda-\alpha}$ where $\lambda$ is the desired eigenvalue. Having started with a random vector $x_0$, the shifted inverse power method is defined as the iteration [5]:
$$y_k = (A-\alpha I)^{-1}x_k$$
$$x_{k+1} = \frac{y_k}{\lVert y_k \rVert_{\infty}}$$
$x_{k+1}$ will converge to the eigenvector which corresponds to the shifted eigenvalue $\sigma$. Then, the actual desired eigenvalue can be found by solving $\lambda = \frac{1}{\sigma} + \alpha$. Therefore, by choosing a random $\alpha$ which is close to a desired eigenvalue which is not the dominant eigenvalue, shifted inverse power method can be employed and will converge to a value $\sigma$ which can be used to find the desired eigenvalue and eigenvector [5].

## Strengths and Limitations
There are multiple reasons that will cause the power method to fail. If the initial random vector, $x_0$, does not have any component in the direction of dominant eigenvector, i.e. $a_1 = 0$ for $x_0 = a_1x_1 + a_2x_2 + ... + a_nx_n$, then the power method will never be able to converge to the largest eigenvector causing the algorithm to fail [6]. Although, in real world application, this problem often does not occur do the the fact that rounding errors in computers during iterations will cause the vector to have a very slight component in the direction of the dominant eigenvector allowing the power method to converge to the dominant eigenvector eventually [6]. Another reason for the power method to fail is the existance of multiple dominant eigenvalues in the given matrix A. If two eigenvalues of the same magnitude exist, then the power method will not converge to one of the matrix's eigenvectors, but instead it will converge to a linear combination of the eigenvectors which correspond to the multiple dominant eigenvalues [6]. The power method can also fail if imaginary numbers have not been taken into consideration when utilizing the power method. The dominant eigenvalue and dominant eigenvector could both potentially contain an imaginary number, in which case the power method algorithm will fail and never converge to dominant eigenvector if it is only using real numbers [6]. This can be avoided by including imaginary numbers in the random starting vector, $x_0$. Thus, the algorithm will be able to handle imaginary numbers if they are required for a given matrix. Finally, if the given matrix is ill-conditioned, then the power method can often fail and can have significant error.

Strengths of the power method include the ability to quickly find the dominant eigenvalue and eigenvector of a given matrix without having to do any calculations for the other eigenvalues and eigenvectors. This easily saves compuational power and memory. Additionally, the power method is efficient with sparse matrices, i.e. matrices which contain many zeros, as the most time consuming step of the power method is multiplying the matrix by a vector, and having many zeros in the matrix makes the multiplication much quicker and thus makes the power method much quicker overall for sparse matrices [7]. Overall, the power method converges linearly to the dominant eigenvector, but the rate depends on the magnitude of the dominant vector, $\lambda_1$ compared to the second largest eigenvalue in magnitude, $\lambda_2$ [8]. The power method also has a tradeoff for symmetric matrices. While for a symmetric matrix, the power method is gauranteed to converge, and it will have a simple error estimation [9]. Symmetric matrices will also cause the power method to converge slower, and it will be more sensitive to the starting random vector [9]. 
## Common Applications
Although the power method may often not be utilized because of its ability to only find the dominant eigenvalue and eigenvetor and because of its increased error amassed through its countless iterations, the power method is still used from some significant calculations which effect the day-to-day lives of many humans in the modern day. Google still employs a form of the power method for their PageRank algorithm which is used to rank websites in for search results [10]. Twitter also uses a form of the power method to rank suggestions for who people should follow [11].
## References
1. 0.3 POWER METHOD FOR APPROXIMATING EIGENVALUES. (n.d.). [https://ergodic.ugr.es/cphys/LECCIONES/FORTRAN/power_method.pdf](https://ergodic.ugr.es/cphys/LECCIONES/FORTRAN/power_method.pdf)
2. Lecture # 10 The Power Method for Eigenvalues. (n.d.). from [https://www.cse.psu.edu/~b58/cse456/lecture10.pdf](https://www.cse.psu.edu/~b58/cse456/lecture10.pdf)
3. File:Animation of the Power Iteration Algorithm.gif - Wikipedia. (2020, February 6). Commons.wikimedia.org. [https://en.wikipedia.org/wiki/File:Animation_of_the_Power_Iteration_Algorithm.gif#file](https://en.wikipedia.org/wiki/File:Animation_of_the_Power_Iteration_Algorithm.gif#file)
4. Vector iteration (power method) 7.1 Simple vector iteration. (n.d.). from [https://people.inf.ethz.ch/arbenz/ewp/Lnotes/chapter7.pdf](https://people.inf.ethz.ch/arbenz/ewp/Lnotes/chapter7.pdf)
5. Yu-Kai Honsg, An introduction to the Power Method and (shifted/Inverse) Power Method, 2007, from [http://www.math.nuk.edu.tw/jinnliu/Software_Engineering/IS_PowerMethod.pdf](http://www.math.nuk.edu.tw/jinnliu/Software_Engineering/IS_PowerMethod.pdf)
6. GT | GT Login. (n.d.). Sso.gatech.edu. from [https://gatech.instructure.com/courses/337286/files/folder/Post-class%20slides?preview=45927399](https://gatech.instructure.com/courses/337286/files/folder/Post-class%20slides?preview=45927399)
7. Ferronato, M. (n.d.). Eigenvalue computation for sparse matrices. from [https://dispense.dmsa.unipd.it/ferronato/MN-PhD/2008/eigen.pdf](https://dispense.dmsa.unipd.it/ferronato/MN-PhD/2008/eigen.pdf)
8. Wong, J. (2019). Math 361S Lecture notes Finding eigenvalues: The power method Topics covered. [https://services.math.duke.edu/~jtwong/math361-2019/lectures/Lec10eigenvalues.pdf](https://services.math.duke.edu/~jtwong/math361-2019/lectures/Lec10eigenvalues.pdf)
9. Lecture 14: Eigenvalue Computations. (n.d.). from [https://www.math.kent.edu/~reichel/courses/intr.num.comp.2/lecture21/evmeth.pdf](https://www.math.kent.edu/~reichel/courses/intr.num.comp.2/lecture21/evmeth.pdf)
10. Perra, N., & Fortunato, S. (2008). Spectral centrality measures in complex networks. Physical Review E, 78(3). [https://doi.org/10.1103/physreve.78.036107](https://doi.org/10.1103/physreve.78.036107)
11. Gupta, P., Goel, A., Lin, J., Sharma, A., Wang, D., & Zadeh, R. (2013). WTF. Proceedings of the 22nd International Conference on World Wide Web - WWW ’13. [https://doi.org/10.1145/2488388.2488433](https://doi.org/10.1145/2488388.2488433)
