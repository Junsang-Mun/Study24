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

- getline함수 컴파일되는데 실행이 안됨...왜지?? 
````
/* error message: free(): double free detected in tcache 2 Aborted */

#include <stdio.h>
#include <stdlib.h>

char* getLine(void)
{
        const size_t sizeIncrement = 0;
        char* buffer = malloc(sizeIncrement);
        char* currentPosition = buffer;
        size_t maximumLength = sizeIncrement;
        size_t length = 0;
        int character;

        if (currentPosition == NULL) { return NULL; }

        while (1)
        {
                character = fgetc(stdin);
                if (character == '\n') { break; }

                if (++length >= maximumLength)
                {
                        char* newBuffer = realloc(buffer, maximumLength += sizeIncrement);

                        if (newBuffer == NULL)
                        {
                                free(buffer);
                                return NULL;
                        }

                        currentPosition = newBuffer + (currentPosition - buffer);
                        buffer = newBuffer;
                }
                *currentPosition++ = character;
        }
        *currentPosition = '\0';
        return buffer;
}

int main(void)
{
        char* result = getLine();

        printf("%s\n", result);

        // free(result);

        return 0;
}
````
   
Passing a One-Dimensional Array
---

- 1차원 배열을 함수로 넘겨줄 때 배열의 크기도 함께 넘겨주어야 함
- 크기를 넘겨주지 않으면 배열요소에 접근할 수 없고, 적은 요소를 사용하거나 배열의 범위를 넘어 사용할 위험이 있음 

Using Array Notation
---
- 배열의 크기를 넘겨줄 때 sizeof(arr)로 넘겨주면 안됨 -> sizeof(arr)/sizeof(arr[0])으로 넘겨준다면?? 

Using Pointer Notation
---
- 1차원 배열을 포인터로 넘겨받을 때 대괄호를 사용한 배열 표기법을 사용하면 안 됨 -> 포인터 표기법사용해야 함 
- 1차원 배열을 대괄호를 사용한 배열 표기법으로 넘겨받을 때 포인터 표기법 사용 가능

> 그 이유에 대해 설명한다면 어떻게 말할 수 있을까? 

Using a One-Dimensional Array of Pointers
---

Pointers and Multidimensional Arrays
---
> 2차원 배열이 다음과 같이 선언되었다고 했을때, 아래 3가지 표현이 의미하는 바는 무엇일까? 

````  
int matrix[2][5] = {{1, 2, 3, 4, 5}, {6, 7, 8, 9, 10}}; 

1. matrix + 1  
2. matrix[0] + 1
3. *(matrix[0] + 1)
````

<details>
 <summary></summary>
 <div markdown="1">
 1. 두 번째 배열(행)의 주솟값을 의미한다. 6으로 시작하는 두번째 배열의 주솟값이다. matrix[1]의 값과 동일하다.  
 2. 첫 번째 배열(행)의 두 번째 값의 주솟값을 의미한다.  
 3. 첫 번째 배열(행)의 두 번째 값인 2를 의미한다. 
 </div>
</details>  

> int (*pmatrix)[5] = matrix는 무엇을 의미할까?  

<details>
 <summary></summary>
 <div markdown="1">
 2차원 배열인 matrix를 가리키는 포인터 배열을 의미한다. 
 </div>
</details>    

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

