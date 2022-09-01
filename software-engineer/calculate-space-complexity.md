# Methods for Calculating Space Complexity

With an example, you will go over how to calculate space complexity in this section. Here is an example of computing the multiplication of array elements:

```c
    int mul, i
    While i < = n do
       mul <- mul * array[i]
       i <- i + 1
    end while
    return mul
```
Let S(n) denote the algorithm's space complexity. In most systems, an integer occupies 4 bytes of memory. As a result, the number of allocated bytes would be the space complexity.

Line 1 allocates memory space for two integers, resulting in S(n) = 4 bytes multiplied by 2 = 8 bytes. Line 2 represents a loop. Lines 3 and 4 assign a value to an already existing variable. As a result, there is no need to set aside any space. The return statement in line 6 will allocate one more memory case. As a result, S(n)= 4 times 2 + 4 = 12 bytes.

Because the array is used in the algorithm to allocate n cases of integers, the final space complexity will be fS(n) = n + 12 = O (n).

