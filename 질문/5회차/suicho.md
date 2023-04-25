Pointers and Arrays
===

1. Vector란 무엇인가?
2. Jagged 배열?

Quick Review of Arrays
---

One-Dimensional Arrays
---

아래 코드에서 printf에서 크기 불일치 오류가 뜨는데 왜 안맞는지 궁금합니다.(값이 출력되기는 합니다)
<pre>
<code>
  #include <stdio.h>

int main(void)
{
	int vector[5] = { 1, 2, 3, 4, 5 };
	printf("%d\n", sizeof(vector) / sizeof(int));
}
</code>
</pre>

Two-Dimensional Arrays
---

Multidimensional Arrays
---

Pointer Notation and Arrays
---

Differences Between Arrays and Pointers
---

Using malloc to Create a One-Dimensional Array
---

Using the realloc Function to Resize an Array
---

Passing a One-Dimensional Array
---

Using Array Notation
---

Using Pointer Notation
---

Using a One-Dimensional Array of Pointers
---

Pointers and Multidimensional Arrays
---

Passing a Multidimensional Array
---

Dynamically Allocating a Two-Dimensional Array
---

Allocating Potentially Noncontiguous Memory
---

Allocating Contiguous Memory
---

Jagged Arrays and Pointers
---

Jagged 배열은 노력이 아까우니 자제(?)합시다..

Summary
---

다중배열을 동적할당 할 경우, 연속적으로 배열이 할당 되도록 유의해야함.
