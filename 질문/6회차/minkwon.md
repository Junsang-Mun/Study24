# Chapter5. Pointers and Strings 

> ## String Fundamentals
`````c
/* warning: format ‘%d’ expects argument of type ‘int’, but argument 2 has type ‘long unsigned int’ */
/* 형식지정자 %ld로 해결 */
printf("%d\n", sizeof(char));
printf("%d\n", sizeof('a')); 
`````
>> String Declaration 108  
- 문자열을 선언하는 3가지 방법  
  1. " "(큰따옴표)를 사용한 문자열 리터럴
  2. 문자 배열
  3. 문자 포인터  

>> The String Literal Pool 109  
- 문자열 리터럴이 선언되면 "문자열 리터럴 풀"이라는 공간에 할당 된다  
- 같은 문자열이 두 번이상 사용되어도 문자열 리터럴 풀에는 복사본이 하나만 저장된다.  
```c
char *testMessage1 = "I'm min-hee";
char *testMessage2 = "I'm min-hee";
```
- 위 코드에서 포인터가 달라도 "문자열 리터럴 풀"에는 복사본 하나만 저장하기 때문에 두 포인터는 같은 곳을 가리키게 된다. 주솟값을 확인해 보면 아래와 같이 같은 것을 볼 수 있다.  
````c
address of testMessage1: 0x55a3bc6c8004
address of testMessage2: 0x55a3bc6c8004
````
- 복사본을 하나만 만들고 그것을 재사용하므로 메모리 공간을 효율적으로 사용할 수 있다. 
- 책에는 아래와 같이 서술되어 있다.(*원문을 그대로 옮겨 적은 것은 아니며 이해를 바탕으로 작성한 것으로, 오역이 있을 수 있다*)
````   
대부분의 컴파일러가 메모리 사용을 최적화하기 위해 string pool을 사용하지만 프로그래머가 이러한 기능을 비활성화하는 경우가 있다.   
드물기는 하지만 이러한 풀링 동작은 예상치 못한 결과나 버그를 야기할 수 있기 때문이다.   
대부분의 컴파일러는 string pool기능을 비활성화하는 옵션을 제공한다.   
string pool이 비활성화되면 문자열은 복사될 수 있고, 각각 주솟값을 가지게 된다.   
gcc에서 -fwritable-strings 옵션을 사용하면 string pool을 비활성화할 수 있다.
`````
- **그러나 최신 버전의 gcc에선 -fwritable-strings 옵션을 더이상 제공하지 않는다** 
- 책에선 gcc컴파일러는 문자열 리터럴의 수정이 가능하다고 하며 아래의 소스코드 결과 "Sound"가 "Lound"로 수정된다고 하였으나   
  **현재 버전의 gcc에서는 불가능하다.** 

````c
#include <stdio.h>
int main(void)
{
        char *tabHeader = "Sound";
        *tabHeader = 'L';
        printf("%s\n", tabHeader);
        return 0;
}
````
- 위 코드는 컴파일은 되나, segmentation fault오류가 생긴다.
- 해당 오류가 생긴이유는 문자열 리터럴은 읽기 전용 메모리(read-only memory)에 할당되는데 그것에 접근하여 값을 변경하려고 했기 때문이다. 
- 오류없이 "Sound"를 "Lound"로 변경하고 싶다면 다음과 같이 코드를 작성해야 한다. 

````c
int main(void)
{
        char tabHeader[] = "Sound";
        *tabHeader = 'L';
        printf("%s\n", tabHeader);
        return 0;
}
````

- 위 코드로 segmentation fault오류를 해결할 수 있는 이유는"Sound"라는 문자열 리터럴을 문자 배열에 대입하여 값을 복사한 후에 배열의 값을 변경하였기 때문이다. 
- 포인터를 사용한다면 아래 코드와 같이 동적할당하여 "Sound"를 "Lound"로 변경할 수 있다.
  
`````c
#include <string.h>
#include <stdlib.h>
int main(void)
{
        char *tabHeader =(char*)malloc(sizeof(char) * 6);
        strcpy(tabHeader, "Sound");
        *tabHeader = 'L';
        printf("%s\n", tabHeader);
        return 0;
}
`````

>> String Initialization 110  

  1. char배열로 문자열 초기화
  `````c
  char testCharArray1[] = "Awesome day!";
  /* 배열의 크기를 지정하지 않은 방식. 배열은 널문자를 포함하여 13byte할당되고 "Awesome day!" 문자열 리터럴이 배열에 복사됨 */

  char testCharArray2[13];
  strcpy(testCharArray2, "Awesome day!");
  /* 배열 크기를 지정한 방식. 배열 크기를 널문자를 포함하여 문자열 리터럴의 문자개수보다 1개 더 많은 13개로 지정한 후 strcpy함수를 사용해 배열에 복사함 */ 
  `````
  **주의!**  
  **배열명에 문자열 리터럴을 대입할 수 없다. 왜냐하면 배열명은 배열 첫번째 요소의 주솟값으로, 상수이기 때문이다. 상수는 Lvalue가 될 수 없다.** 

 2. char포인터로 문자열 초기화   
 `````c

 char* testPointer;  
 char* testPointer = (char*)malloc(strlen("Awesome day!")+1);
 strcpy(testPointer, "Awesome day!"); 
 /*문자열 리터럴 문자개수에 널문자를 더한 만큼 동적할당한후 해당 공간에 strcpy함수로 문자열을 복사함*/ 
 `````
 **주의!**  
 **동적할당 시 문자길이를 결정할 때 반드시 널문자를 함께 고려해야 한다**  
 **문자열 리터럴의 문자 개수 셀 때 sizeof연산자 사용하면 안되고 strlen함수 사용해야 한다. 왜냐하면 sizeof연산자는 문자열의 길이를 반환하는 것이 아니라 배열이나 포인터의 크기를 반환하기 때문이다**  

 `````c

#include <stdio.h>
#include <string.h>

int main(void)
{

        printf("sizeof of \"Awesome day!\": %ld\n", sizeof("Awesome day!")); //null문자 포함하여 결괏값 13
        printf("strlen of \"Awesome day!\": %ld\n", strlen("Awesome day!")); //null문자 포함하지 않아서 결괏값 12

        return 0;
}

`````

*애초에 동적할당할 때 널문자를 위한 1을 더하지 않고 sizeof연산자만 사용하면 되지 않나??* 
`````c
int main(void)
{
        char* testPointer = (char*)malloc(sizeof("Awesome day!"));
        strcpy(testPointer, "Awesome day!");

        printf("%s\n", testPointer);

        free(testPointer);

        return 0;
}
/* 위 코드도 컴파일 오류없이 잘 실행됨 */
`````

개인적 의견으로는 문자열길이만큼 동적할당 해야하는 것이므로 목적에 맞게 strlen함수를 사용하고 널문자를 위한 1을 연산하는 것이 보다 더 정확한 코드 구현일 거라 생각한다.  

**문자**리터럴을 포인터로 초기화 할 때 주의해야 한다.  
````c
        char* thisIsPlus = '+';
        printf("%c\n", *thisIsPlus); // 컴파일 오류. warning: initialization of ‘char *’ from ‘int’ makes pointer from integer without a cast 

        char* thisIsString = "string";
        printf("%c\n", *thisIsString); // 결괏값 s 
````  

첫 번째 코드에서 컴파일 오류가 나는 이유는 **문자**리터럴은 주솟값이 아니라 **int형**이기 때문이다.   

3. 표준입출력을 통한 문자열 초기화 

scanf함수를 사용하여 사용자로부터 입력을 받아 문자열을 초기화 할 수도 있으나 입력받은 문자를 저장할 공간이 할당되지 않은 상태에서 입력을 받게되므로 잠재적인 오류가 발생할 가능성이 있다.  

> ## Standard String Operations

>> Comparing Strings 115  

strcmp함수로 2개의 문자열을 비교한다. 매개변수로 받은 첫 번째 문자열의 알파벳 순서가 두 번째 문자열의 알파벳 순서보다 먼저이면 음수를 리턴한다. 그 반대이면 양수를 반환한다. 두 문자열이 같으면 0을 반환한다.  

>> Copying Strings 116  

strcpy함수로 첫 번째 매개변수로 주어진 문자열에 두 번째 매개변수로 주어진 문자열을 복사한다.

>> Concatenating Strings 118  

strcmp함수로 첫 번째 매개변수로 주어진 문자열 뒤에 두 번째 매개변수로 주어진 문자열을 이어붙인다. 반환값은 첫 번째 인수의 주솟값이다.   

````c
        char* prac1 = "To be or not to be, ";
        char* prac2 = "that is the question.";

        char* buffer = (char*)malloc(strlen(prac1)+strlen(prac2)+1);
        strcpy(buffer, prac1);
        strcat(buffer, prac2);

        printf("%s\n", buffer);
        printf("%s\n", prac1);
        printf("%s\n", prac2);
````
위 코드에서 포인터 prac1과 prac2는 각각 string literal pool에 있는 문자열을 가리키고 있다. buffer는 널문자를 포함하여 붙여질 두 문장이 저장될 동적할당 된 공간을 가리키고 있다. 우선 prac1이 가리키고 있는 문자열을 strcpy함수를 사용하여 buffer에 복사한다.  

**prac1에서 buffer로 복사할 때, strcpy함수 대신 strcat함수를 사용할 수 있을까?**   
````c
        char* buffer = (char*)malloc(strlen(error)+strlen(errorMessage) +1);
        strcat(buffer, prac1);
        strcat(buffer, prac2);
````
결론부터 말하면 **안 된다**. 왜나하면 strcmp함수는 널문자의 위치를 먼저 찾은 후 그 뒤에 문자열을 붙이므로 반드시 초기화를 해주어야 한다. 그렇지 않으면 쓰레기 값의 중간부터 붙여넣을 가능성이 크다. 위 buffer는 공간만 할당되었을 뿐이다. 따라서 초기화 되지 않은 buffer에 prac1 문자열을 붙여넣을 수 없다.  

그럼 컴파일도 되지 않을까?  
아니다.  
아래와 같이 컴파일 오류 없이 strcpy함수를 사용했을 때와 동일한 결과물이 나온다.  

````c
To be or not to be, that is the question.
To be or not to be,
that is the question. 
````
chatGPT는 이런 결과에 대해 다음과 같이 말했다.  

````
Although the code you wrote is incorrect and can lead to undefined behavior, it is possible that your compiler did not catch the error and allowed the code to compile and run without issue. This can happen because C does not provide automatic runtime checking for many types of errors, including buffer overflow and uninitialized memory access.

When you call malloc(), it returns a pointer to a block of uninitialized memory. The value of the memory is undefined, and it may contain arbitrary data that was previously stored in that location.  
````

버퍼 오버플로우나 초기화 되지 않은 메모리 접근과 같은 오류를 못잡는 경우가 있다고 한다. 실행이 되었다고해서 안심해서는 안 될 거 같다. 그럼 strcat함수는 어디서 널문자를 찾은 것일까? 추측해 보건대 이전에 사용했던 어떤 임의의 데이터값-쓰레기 값이라고 할 수 있는?-에서 널문자를 찾은거 아닐까.   

결론은 strcat함수를 사용할 때 붙여 넣어질 공간은 반드시 초기화 되어야 한다는 것이다. 그렇지 않으면 훗날 예상치 못한 문제가 발생할 확률이 높아진다.  

````c

        char* prac1 = "To be or not to be, ";
        char* prac2 = "that is the question.";

        strcat(prac1, prac2);

        printf("%s\n", prac1);
        printf("%s\n", prac2);
````

위 코드는 컴파일은 되지만 **segmentation fault 오류**가 생긴다. 이유는 앞에서도 학습했듯이, 읽기 전용 메모리에 저장된 문자열을 바꾸려는 시도를 했기 때문이다.  

그럼 아래 코드는 어떨까?  

````c
        char prac1[] = "To be or not to be, ";
        char* prac2 = "that is the question.";

        strcat(prac1, prac2);

        printf("%s\n", prac1);
        printf("%s\n", prac2);
````
prac1을 포인터에서 배열로만 바꿨다. 해당 코드는 문자열을 배열에 대입한 후 배열의 값을 바꾸는 것이므로 문제가 없지 않을까?  

결론은 아니다. 해당 코드를 실행하면 아래와 같은 오류를 발견하게 된다.  

````
To be or not to be, that is the question.
that is the question.
*** stack smashing detected ***: terminated
Aborted  
````
컴파일도 되고 결괏값도 확인할 수 있으나 이상한 메시지가 따라 붙는다. 해당 메시지는 **스택 버퍼 오버플로우**가 발생했을 때 나타난다. stack buffer overflow가 발생한 이유는 prac1의 배열의 크기가 문자열이 붙여졌을 때를 대비할 만큼 충분히 크지 못하기 때문이다.  

문자열이 붙여지기 전 prac1의 배열의 크기는 "To be or not to be, "에 널문자를 하나 더한 만큼이다. 배열 초기화시 그 크기를 정해주지 않았기 때문이다. 그런데 strcat함수를 호출한 후 prac1의 크기는 "To be or not to be, that is the question."에 널문자를 하나 더한 만큼이 된다. 문자열을 붙인 후의 크기가 붙이기 전보다 크기 때문에 오버플로우가 발생한 것이다.  

이를 해결하려면 아래와 같이 prac1의 배열의 크기를 붙여질 문장을 포함할 수 있도록 충분히 지정해 주어야 한다.

````c
        char prac1[100] = "To be or not to be, ";
        char* prac2 = "that is the question.";

        strcat(prac1, prac2);

        printf("%s\n", prac1);
        printf("%s\n", prac2);
````

아래 코드의 결과를 예상해 보고, 그 결괏값이 나온 이유에 대해 설명한다면?  

````c
        char prac1[] = "To be or not to be, ";
        char prac2[] = "that is the question.";

        strcat(prac1, prac2);

        printf("%s\n", prac1);
        printf("%s\n", prac2);
````

<details>
 <summary></summary>
 <div markdown="1">
 해당 코드는 앞선 이유와 마찬가지로 prac1배열의 크기가 strcat함수 호출 후 붙여진 문자열을 담기에 충분하지 않기 때문에 잘못된 코드다. 그러나 gcc로 컴파일한 결과 앞에서 나왔던 '*** stack smashing detected ***: terminated
 Aborted'와 같은 오류없이 실행결과가 나왔다. prac2를 출력한 결과 앞의 문자열이 잘리고 "question"부분만 출력되었다. 이는 책에서 아래와 같이 설명한 부분과 비슷하다. prac2를 출력했을 때 해당 문자열이 잘리지 않고 그대로 나오려면 prac1 배열의 크기를 충분히 주어야 한다. 
 </div>
</details>  

````
The errorMessage string has been shifted one character to the left. This is because the
resulting  concatenated  string  is  written  over  errorMessage.  Since  the  literal  “Not
enough memory” follows the first literal, the second literal is overwritten. (p.120 Figure 5-10 위쪽)
````

결론적으로 중요한 것은 strcat함수 사용으로 다른 메모리 공간을 침범하지 않아야 한다는 것이다.   
strcat함수를 통해 붙여진 문자열의 길이에 널문자를 더한 만큼의 공간을 확보한 후 해당 공간에 문자열을 채워 넣는 것이 안전하다.

> Passing Strings 
### Passing a Simple String 121
### Passing a Pointer to a Constant char 123
### Passing a String to Be Initialized 123
### Passing Arguments to an Application 125

> Returning Strings
### Returning the Address of a Literal 126
### Returning the Address of Dynamically Allocated Memory 128

> Function Pointers and Strings 130
