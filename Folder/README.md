                           CS553 -- Evaluation lab 1


Instructor: Louis-Noel Pouchet <pouchet@colostate.edu>
===============================================================================


Computation specification:

Implement the multiplication of two matrices A (size NI x NK) and 
B (size NK x NJ) added to the resulting matrix C (size NI x NJ), after 
scaling the input matrix C by the scalar beta. That is, one 
element of C is computed as:

C[i,j] = C[i,j] * beta + \sum_{k = 0}^{NK-1} (A[i,k] * B[k,j])

And you have to execute this computation for every i,j cell of C.
This is known as the GEMM kernel from BLAS, where alpha = 1 and matrices are 
not transposed. In matrix notation, the operation to implement is:

C = C * beta + A ** B

where beta is a scalar, A, B and C are matrices, * denotes the scalar 
(pointwise) multiplication, and ** the matrix multiplication.


Implementation specification:

Edit only the function gemm/gemm.c:kernel_gemm and replace the current code
(the addition of two matrices A and B into matrix C) by a code implementing
the above matrix multiplication.

Note NI is stored in the variable _PB_NI, similarly for NJ and NK in _PB_NJ
and _PB_NK. You must use these variable names to refer to the matrices sizes.

You are not allowed to modify any other part of the code.


To compile and measure time:

$> gcc -I utilities -I gemm utilities/polybench.c gemm/gemm.c -DPOLYBENCH_TIME -o my_binary
$> ./my_binary

You are allowed to add any other option of your choosing to this compilation 
command, /EXCEPT/ any flag related to changing the problem size (e.g., flags 
like -DNI=... are forbidden, similarly -DSMALL_DATASET, -DMEDIUM_DATASET, etc.
are forbidden).


Restrictions and submission:

Remember this lab is not graded, it is only used to assess your progresses 
during this class. We trust you to follow the rules:
 - absolutely no internet search to get the answer. Do it by yourself, with 
   your existing knowledge and any info from man pages (e.g., man gcc) you need.
 - you are given 30 minutes to complete this lab, not more.
 - once you are done, submit your file by email to pouchet@colostate.edu 
   along with the best execution time you observed. Please use the subject
   "CS553 - Evaluation lab 1" for your email.


