# CHAPTER 4. 포인터와 배열 (Pointers and Arrays)
> 모든 self Q, Q에 대한 정답은 접어두도록 하겠다. 질문만 보고 스스로 생각해본 뒤 정답 보기!
> 만약 정답과 비교해봤을 때 부족한 부분이 있었거나, 정답 내용을 모두 떠올리긴 했어도 우다다다 대답하지 못하였다면 그건 아직 모르는 것임!
## 들어가기
<details>
<summary>self Q1. 문자 배열과 문자열를 비교하시오.</summary>



</details>
<details>
<summary>self Q1. 문자열을 선언하는 방법은 어떠한 것들이 있나? 또한 그 방법들의 차이점을 아는가?</summary>

방법1) 포인터 형태 선언  
    
```
    // ex)
    char *a = "hello!";  
``` 
      
방법2) 배열 형태로 선언 (크기 자동 할당)  
    
    ```
    // ex)  
    char a[] = "hello!";
    ```  
    
    /* 방법2의 진행 순서  
        1. 메모리 어딘가에 "hello!"가 할당된다.  
        2. char *a 변수가 선언된다.  
        3. a는 h의 주소값을 가리킨다. (*string = 'h') */
      
 
- 두 방법의 공톰점
    
    - 포맷팅 양식이 %s로 같다.
    
    - 문자열 전체 출력 결과가 같다.
- 두 방법의 차이점
    
   - 할당된 메모리 영역의 크기가 다르다.  
        -> sizeof함수를 적용해봤을 때,방법1의 경우 8(포인터 자료형 크기)이고 방법2의 경우 6(비열 크기)임
    
   - 할당된 메모리 영역의 위치가 다르다.  
    
   - 배열 형태는 내용 수정 가능, 포인터 형태는 내용 수정 불가능 (리터럴 상수)
    
    
  

</details>


## 배열 (array)


## 포인터 표기법과 배열 (Pointer Notation and Arrays)

<details>
<summary>self Q1. lvalue란 무엇인지 구체적이고 정확하게 설명할 수 있는가? 또한 rvalue와도 비교하여라. </summary>
Lvalue와 Rvalue는 소스코드상에서만 볼 수 있는 문법적 요소로, 소스코드가 컴파일 된 후 프로그램이 실행되는 시점에서는 L/Rvalue를 논하는 것은 의미없는 행위임    
    
1) Lvalue(left value 또는 locator value) : 메모리 위치를 참조하는 식
    
    - object를 표기할 수 있는 것. (void 타입의 object 제외)  
    ```
    // ex)  
    int a = 10;  
    
    // a라는 것은 10이라는 값이 들어있는 공간인 object를 의미.  
    // 식별자 a라는 것이 10이 들어있는 object를 표기하기 위해 사용된다고 말할 수 있음.
    ``` 
    - 주로 Lvalue가 식별자(변수, 함수, 클래스 등)를 의미  
    - 모든 Lvalue는 Rvlue이지만, Rvalue는 Lvalue가 아닐 수 있다.  
    - 존재하지 않는 object를 표기하는 p[10]이나 *(p+4) 또한 Lvalue임.  
          이러한 Lvalue를 사용하는 것은 미정의 동작(undefined behavior)임.  
    
2) Rvalue(right value) : 해당 표현식이 끝나면 더 이상 참조 불가능 


</details>

## malloc 함수로 1차원 배열 생성하기 (Using malloc to Create a One-Dimensional Array)


## realloc 함수로 배열 크기 조정하기 (Using the realloc Function to Resize an Array)
<details>
<summary>self Q1. {NULL과 0}과 {NU과과 '/0'}과 undefined를 비교할 수 있는가?</summary>  
  
1) NULL과 0
    * 널 포인터, (void*)0을 가리킴. '값이 없다', '비어있다'를 의미하며 정의되었지만 가리키는 것이 없음.  
    * NULL 매크로는 <stdio.h>, <stdlib.h> 등 여러 헤더파일에서 이미 정의되어 있음.    
    * Null pointer과 정수 0은 완전 같은 것임.  
      (왜? 두 데이터 타입 모두 4byte로 c언어에서 pointer과 정수는 서로 형 변환이 가능하기 때문)  
    *즉, Null pointer이란? 메모리 주소 0을 가리킨큰 pointer (0번지는 일반적으로 접근 불가 메모리 영역)  
    * pointer이 아무 것도 가르키지 않을 때 Null pointer(0x00000000)로 초기화  
  
2) NUL과 '\0'
    * NUL == '\0'임.
    * 문자상수, ascii code의 첫 번째 문자 0.
    * char형임.
    * 문자열은 항상 NUL로 끝나야 하며 NUL을 만나기 전까지의 문자들을 문자열이라고 함.  
  
3) undefined
    * 아예 정의된 적 조차 없는 변수를 의미.


</details>


## 1차원 배열 전달하기 (Passing a One-Dimensional Array)


## 1차원 포인터 배열 이용하기 (Using a One-Dimensional Array of Pointers)


## 포인터와 다차원 배열 (Pointers and Multidimensional Arrays)


## 다차원 배열 전달하기 (Passing a Multidimensional Array)


## 2차원 배열 동적으로 할당하기 (Dynamically Allocating a Two-Dimensional Array)


## 가변 배열과 포인터 (Jagged Arrays and Pointers)

