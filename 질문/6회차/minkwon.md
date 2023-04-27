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
### String Initialization 110

> Standard String Operations 

### Comparing Strings 115
### Copying Strings 116
### Concatenating Strings 118

> Passing Strings 
### Passing a Simple String 121
### Passing a Pointer to a Constant char 123
### Passing a String to Be Initialized 123
### Passing Arguments to an Application 125

> Returning Strings
### Returning the Address of a Literal 126
### Returning the Address of Dynamically Allocated Memory 128

> Function Pointers and Strings 130
