< Chapter4. Pointers and Arrays>
===

Quick Review of Arrays
---
> One-Dimensional Arrays  
> Two-Dimensional Arrays  
> Multidimensional Arrays  
- 1차원, 2차원, 다차원 배열 각각의 선언방법과 메모리에 어떻게 할당되는지 알 수 있음 

- <details>
   <summary>sizeof연산자를 활용해서 배열요소 개수를 구하는 방법은?(p.81)</summary>
   <div markdown="1">
    sizeof(array) / sizeof(element)
   </div>
  </details>


Pointer Notation and Arrays
---
- 배열 표기법과 포인터 표기법은 서로 바꿔 사용할 수 있음   
  (But, 완전히 동일하진 않음)  
- 배열 이름 자체는 주소값을 가지고 있으므로 포인터에 대입할 수 있음
- 배열 이름은 배열 그 자체의 주소값이 아니라, 배열 첫 번째 요소의 주소값
- 배열 이름에 주소연산자를 사용하면 배열 자체에 대한 포인터를 반환함 


Differences Between Arrays and Pointers
---
- 배열 이름과 배열 이름을 저장한 포인터는 다름 
- 포인터는 lvalue이기 때문에 바뀔 수 있으나, 배열 이름은 주소값으로 상수이기 때문에 바꿀 수 없음
- sizeof로 배열이름을 연산하면 배열 전체 크기가 값이 되나 배열이름을 저장한 포인터를 연산하면 포인터 크기가 값이 됨 

Using malloc to Create a One-Dimensional Array
---
- 힙에 메모리를 할당하고 그 주소값을 포인터에 대입하면 이 메모리를 배열로 사용할 수 있음 

Using the realloc Function to Resize an Array
---
- <details>
   <summary>(복습)realloc 함수 원형</summary>
   <div markdown="1">
      void* realloc(void* ptr, size_t size);
   </div>
   </details>
   
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

