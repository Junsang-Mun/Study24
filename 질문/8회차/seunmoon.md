# Chapter 7. 보안 이슈와 포인터의 오남용 (Security Issues and the Improper Use of Pointers)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  
- 마냥 모든 내용이 동시에 보이게 작성하면 너무 길어져서 가독성이 떨어질 것 같아 각 part마다 접어두기를 하였다. 원하는 파트를 클릭하여 자유롭게 열고 닫으며 읽으면 된다.  
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  
<br/>
<details>
<summary>part 0. 들어가기</summary>

## part 0. 들어가기

- 이 단원의 중요 포인트  
    - 포인터의 선언과 초기화  
    -  부적절한 포인터 사용  
    - 메모리 해제 문제  
    
    
- 포인터의 사용에 관한 보안 이슈에 대해 집중   
    
    
- 여태까지 '프로그래밍 습관의 관점'에서 포인터와 그 적절한 사용 방법에 대해 이해하였다면 이 단원에서는 '보안 관점'에서 바라보기  
    
    
- 운영체제 보안 개선 사항 (이 단원에서는 두 가지만 알아봄)  
    - 주소 영역 배치 랜덤화 (Address Space Layout Randomization)  
    - 데이터 실행 방지 (Data Execution Prevention)  
    - 주소 영역 배치 랜덤화(ASLR) 절차 : 메모리 내 애플리케이션의 데이터 영역(code, stack, heap 포함)을 랜덤하게 배치 함  
        -> 영역 배치를 랜덤화함으로써 공격자가 메모리가 어디에 위치할지 예측하기 어렵게 만들어서 데이터 영역에 접근을 힘들게 할 수 있음  
    - 데이터 실행 방지(DEP) 기법 : 코드가 메모리의 실행 불가능한 영역(stack, heap)에 있을 때 실행 차단  
  
    
- C언어가 안전한 애플리케이션을 작성하기에 쉽지 않은 주요 원인  
    - C언어는 배열의 영억을 넘어선 영역에 데이터를 기록하는 것을 막지 않음.  
        -> 메모리가 손상되어 보안에 잠재적 취약점이 됨.  
    - 포인터의 부적절한 사용으로 인해 보안 문제를 야기함.  
  
  
- CERT : C와 다른 언어에서의 보안 이슈를 더 포괄적으로 다루며 인터넷 보안 취약점에 대해 연구하는 조직    
</details>

<br/>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   
<br/>  
<details>
<summary>part 1. 포인터의 선언과 초기화 (Pointer Declaration and Initialization)</summary>

## part 1. 포인터의 선언과 초기화 (Pointer Declaration and Initialization)  
- 포인터의 선언 / 포인터를 초기화 하지 않는다면 발생할 수 있는 문제에 대해 알아보기  
    
<br/>  
    
### 부적절한 포인터 선언 (Improper Pointer Declaration)  
    
- ///code/// 한 줄에 두 개의 포인터를 선언하고 싶은 경우  
  
    ```c
    // 올바른 예시
    int *ptr1, *ptr2;
   
    // 잘못된 예시
    /* 얼핏 보기에 int형 포인터 두 개를 선언한 것처럼 착각할 수 있으나,
       아래와 같이 쓴 경우는 포인터는 ptr1 뿐이고, ptr2는 그냥 int형이다. */
    int *ptr1, ptr2;
    ``` 
    
    
- 타입 정의 (type definition)을 사용한 정의
    - 매크로 정의 대신 타입 정의 이용하는 것은 좋은 습관
    - 타입 정의 vs 매크로 정의
        - 타입 정의 : 컴파일러가 범위 규칙*1__(scope rule)에 대해 확인하도록 함  
        - 매크로 정의 : 컴파일러에 따라 범위 규칙(scope rule)에 대해 확인을 못하는 경우도 존재
    - ///code/// typedef를 이용한 선언(올바른 예시) & 지시자(directive)를 이용한 선언(잘못된 예시)
  
       ```c
       // 올바른 예시
       typedef int* PINT
       PINT ptr1, ptr2;

       // 잘못된 예시 -> 위의 예시 코드와 동일한 이유로 틀림
       #define PINT int*
       PINT ptr1, ptr2;
       ```
    
<br/>
    
### 초기화되지 않은 포인터 (Failure to Initialize a Pointer Before It Is Used)
- 포인터를 초기화 하기 전 사용한다면, 런타임 에러 발생 가능성 O  
    
    
- 와일드 포인터 (wild pointer) : 초기화되지 않은 포인터를 지칭하는 용어  
    
    
- 와일드 포인터 사용시 발생할 수 있는 경우
    - 포인터에 지정된 메모리 주소가 애플리케이션의 유효한 영역 밖에 있다면  
        -> 코드 실행 중단 됨  
    - 우연히 유효 영역 안에 있다면  
        -> 포인터가 int형인 경우, 메모리 영역에 있던 랜덤 값(쓰레기 값)이 정수형으로 출력됨.  
           또한 char형 포인터의 경우에는 NUL 종료 문자가 나타낼 때까지 괴상한 문자들 출력.

    <br/>
    
### 초기화되지 않은 포인터 처리하기 (Dealing with Uninitialized Pointers)
- 초기화되지 않은 포인터를 다루는 세 가지 접근 방법에 대해 알아보도록 하겠다.
    
    
- 방법 1) 포인터는 언제나 NULL로 초기화하기
    - ///code/// 사용 예제
    
        ```c
        int *pi = NULL;
        ... ...
        if (pi == NULL) {
            // 여기에서 pi 역참조 금지
        }
        else {
            // 여기에 pi 사용하는 코드 작성
        }
        ```  
    
    
- 방법 2)assert 함수 활용하기
    - assert 함수란
        - 디버깅을 위해서 사용하는 함수로
        - 정해진 조건을 위반하는지를 검사하기 위한 목적으로 사용
            -> 정해진 조건에 맞지 않는 경우 프로그램을 중단
        - 헤더파일 : <assert.h>
        - Visual Studio에서는 Debug 모드에서만 작동하며 Realease 모드에서는 동작하지 X
        - ///code/// 사용 예제
        
            ```c
            #define _CRT_SECURE_NO_WARNINGS
            #include <stdio.h>
            #include <string.h>
            #include <assert.h> 

            void copy(char *dest, char *src)
            {
                assert(dest != NULL);    // dest이 NULL이면 프로그램 중단
                assert(src != NULL);     // src가 NULL이면 프로그램 중단

                strcpy(dest, src);       // 문자열 복사
            } 

            int main()
            {
                char s1[100];
                char *s2 = "Hi, I'm lunash0";

                copy(s1, s2);     // 정상 동작

                copy(NULL, s2);   // 프로그램 중단
                // 출력되는 에러 메세지 --> Assertion failed: dest != NULL

                return 0;
            }
            ```  
            
    
- 서드파티 도구 활용하기
    - 서드파티(Third Party)란?
        - 프로그래밍을 도와주는 plug_in이나 library 등을 만드는 회사를 칭함
        - 제조사와 사용자 이외 외부의 생산자를 가리키는 뜻으로 쓰임 - 위키
  
</details>
<br/>  
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  
<br/>  
<details>
<summary>part 2. 포인터의 사용 이슈 (Pointer Usage Issues)</summary>

## part 2. 포인터의 사용 이슈 (Pointer Usage Issues)
- 이 절에서 공부할 내용
    - 역참조 연산자
    - 배열 첨자(subscript)
    - 문자열, 구조체, 함수 포인터에 관한 문제
    
<br/> 
    
- 버퍼 오버플로우 (buffer overflow)
    - 보안 관점에서의 의미  
        - 객체의 영역을 벗어난 영역의 메모리가 덮어 쓰일 때 발생하는 현상  
    - 발생 원인
        - 배열 요소에 접근할 때 사용하는 인덱스 값을 확인하지 X  
        - 배열 포인터에 대한 포인터 연산을 할 때 주의를 기울이지 X  
        - 표준 입력(stdin)에서 문자열을 읽어 들일 때 gets 같은 함수를 사용함  
            -> 초기 설계의 문제로 버퍼 오버플로우가 발생한다는 치명적 오류로 인해 현재는 fgets를 용대신 사용  
        - strcpy, strcat 등의 함수를 부적절하게 사용  
    - 버퍼 오버플로우가 애플리케이션 외부에서 발생하는 경우  
        : 즉, 덮어 쓰이는 메모리 영역이 타프로그램의 주소 공간일 경우  
        -> OS가 segmentation fault를 발생시킴 & 프로그램 강제 종료  
    - 버퍼 오버플로우가 애플리케이션 내부에서 발생하는 경우  
        -> 데이터에 대해 허가되지 않은 접근 발생 or 코드의 다른 세그먼트로 제어가 넘어가 시스템 파괴  
    - 버퍼 오버플로우가 스택 프레임 안에서 발생하는 경우
        -> 스택 프레임의 복귀 주소 부분을 같은 시점에 생성된 악성 코드 주소로 덮어쓸 수 O
    - 함수 반환 때 반환 주소가 악성 코드를 가리키는 경우
        -> 제어가 악성 함수에 주어지면서 그 함수가 현재 사용자 권한 레벨에서 할 수 있는 모든 동작 가능
   
<br/>  

### NULL 확인하기 (Test for NULL)
- malloc 부류 함수 사용시 항상 '반환 값 확인하기'를 세트로 생각하기!!  
    
    
- ///code/// 일반적인 접근 방식을 설명하는 사용 예시  
    
    ```c
    float *vector = malloc(sizeof(float) * 20);
    if (vector == NULL) {
        // malloc 함수가 메모리 할당에 실패
    }
    else {
        // vector를 여기서 처리
    }
    ```
    
<br/>  
    
### 역참조 연산자의 잘못된 사용 (Misuse of the Dereference Operator)
- ///code/// 변수 num의 주소를 pi로 설정하고 싶은 경우로 알아보는 예시 코드  
    
    ```c
    // 올바른 코드  
    int num;
    int *pi = &num;
    
    /* 잘못된 코드  
        -> num 변수의 주소를 pi 포인터에 할당한 것이 아니라  
           pi에 의해 지정된 메모리 영역에 할당하려고 하는 코드가 되어버림.  
           또한 pi가 초기화 되지 않은 상태에서 사용하려고 하는 것이기 때문에 말이 안됨. */
    int num;
    int *pi;
    *pi = &num; // 맨 앞의 *을 때고 pi = &num;이라고 써야 의도대로 작성된 것임
    ```
    
<br/>  
    
### 댕글링 포인터 (Dangling Pointers)  
- 포인터가 해제된 메모리를 참조할 때 발생  
- 해제된 이후에 이 메모리 영역에 접근하려 하면 메모리의 내용이 변경 되었을 가능성 O  
- 이 메모리에 write 연산 실행 -> 메모리 손상 가능성 O  
- 이 메모리에 read 연산 실행 -> 유효하지 않은 데이터 반환 가능성 O  
- 댕글링 포인터를 이용한 잠재적 취약점 공격 방법(exploit) 존재  
    : C++의 VTable(Virtual Table, 가상 method 기능 지원을 위해 사용되는 함수 포인터의 배열)의 취악졈 공격에 기반  
        -> 즉, VTable에 관련해서만 보안 이슈가 되므로 C언어와는 상관 X  
    
<br/>  
    
### 배열의 범위를 벗어난 메모리 접근 (Accessing Memory Outside the Bounds of an Array)  
- 프로그램이 배열에 할당된 범위를 벗어난 메모리 영역에 접근하는 것을 막을 방법은 없음  
- 첨자(subscript)로 계산한 주소는 인덱스 값이 유효한지 확인하지 않음 -> 대표적 버퍼 오버플로우  
    
<br/>  
    
### 배열 크기 계산 오류 (Calculating the Array Size Incorrectly)
- ///code/// 함수의 배열을 매개변수로 넘길 때는 항상 배열의 크기도 함께 넘기자!!  
    
    ```c
    // 예시 함수(buffer 안의 데이터를 replacement라는 문자로 대체)의 프로토타입  
       (size 매개변수가 함수에서 버퍼의 끝을 넘어서서 문자를 기록하지 않도록 방지해줌)  
        -> void replace(char buffer[], char replacement, size_t size);
    ```  
    
    
- 위의 예시 함수와 달리 strcpy는 버퍼 오버플로우를 허용 -> 주의하자.

    
<br/>  
    
### sizeof 연산자 오용 (Misusing the sizeof Operator)
- ///code/// 예시 코드 : int buffer[20]이라는 배열의 각 요소를 0으로 초기화 하고 싶을 때  
    
  ```c
  // 올바른 코드
  int buffer[20];
  int *pbuffer = buffer;
  for (int i = 0; i < sizeof(buffer) / sizeof(int) ; i++) // / 'sizeof(int)' 해주는게 포인트
      *(buffer++) = 0;  
    
  // 잘못된(sizeof 연산자를 오용한) 코드
  // 위의 for문에서 '/ sizeof(int)'를 안쓰고 for (int i = 0; i < sizeof(buffer); i++)로 작성
  ```  
    
<br/>  
    
### 항상 포인터 타입 일치시키기 (Always Match Pointer Types)
~~~~~~~~~~~~~~~~~~ 조금 더 공부하고 정리할 것 ~~~~~~~~~~~~~~~~~~

    
<br/>  
    
### 유계 포인터 (Bounded Pointers)
- 정의 : 유효한 영역 내에서만 사용이 가능한 포인터  
        ex) 배열이 32개의 요소를 갖도록 선언했다면, 이 배열에 대해 사용되는 포인터는 배열 범위의 이전 또는 이후의 메모리 영역에 접근할 수 없도록 함  
    
    
- C언어에서의 유계포인터  
    - 이런 접근 방식을 직접적으로 지원하지는 X
    - ///code/// 그러나 개발자가 명시적으로 포인터 범위 제한 가능  
    
    ```c
    #define SIZE 32
    
    char name[SIZE];
    char *p = name;
    
    if(name != NULL) {
        if (p >= name && p < name + SIZE) {
            //포인터가 유효함 - 정상 진행
        }
        else {
            // 포인터가 유효하지 않음 - 조건이 맞지 않음
        }
    }
    ```  
    
    
- 유계 포인터의 다른 변형  
    - 포인터 유효성 검증 함수 (pointer validation function) 만들기  
      : 이 함수를 만들려면 초기 위치 & 범위 알아야 함  
    - Bounded Model Checking for ANSI-C and C++ (CBMC) 사용  
      : CBMC는 분석 도구로, 배열 범위나 포인터 안전성에 대한 검증을 도와주는 도구  
      : 이 애플리케이션은 C 프로그램에서의 다양한 안전성 & 보안 이슈에 대해 테스트하고 배열 범위와 버퍼 오버플로우 문제점 찾아줌  
    
<br/>  
    
### 문자열 보안 이슈 (String Security Issues)
    
<br/>  
    
### 포인터 연산과 구조체 (Pointer Arithmetic and Structures)
    
<br/>  
    
### 함수 포인터 이슈 (Function Pointer Issues)
    
<br/>  

</details>
<br/>  
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  
<br/>  
<details>
<summary>part 3. 메모리 해제 이슈 (Memory Deallocation Issues)</summary>

## part 3. 메모리 해제 이슈 (Memory Deallocation Issues)
~내용~
</details>
<br/>  
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  
<br/>  
<details>
<summary>part 4. 메모리 해제 이슈 (Using Static Analysis Tools)</summary>

## part 4. 메모리 해제 이슈 (Using Static Analysis Tools)
~내용~
</details>
<br/>  
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  
<br/>  
<details>
<summary>추가 공부 메모</summary>

## 추가 공부 메모
      
    
### 1__ 범위 규칙 (Scope Type)

    
- 범위 규칙이란?
    - 동일한 이름(identifier)의 변수나 함수가 여러 곳에 선언되어 있을 때, 가장 가까운 범위에 선언된 이름을 사용하는 규칙
    - class나 block 내에 선언도니 이름과 동일한 이름이 전역 범위(global area)에 선언되면, 전역 범위에 선언도니 이름은 class나 block으로부터 숨겨지게(hidden) 됨.
    
- 스코프(Scope)란?
    -쉽게 말하자면, 어떤 변수의 범위(스코프)란 프로그램 중에서 그 변수가 효력을 발행하는 부분  
    - 프로그램에서 바인딩(binding, 프로그램의 어떤 기본 단위가 가질 수 있는 구성 요소의 구체적인 값이나 성격을 확정하는 행위) 동작을 하는 Textual Region (변수, 함수 등의 유효 범위)    
    - 바인딩 바뀌지 않는 영역 (Static 기준으로)  
    
    
-  동작 스코프의 종류  
    - 정적 스코프 (Static Scope) : 컴파일 시점에서 스코프 확정 (코드를 보고 스코프 구별 가능)  
    - 동적 스코프 (Dynamic Scope) : 실행 시점에 스코프 확정 (실행 흐름을 따라가봐야 스코프 구별 가능)  
    
    
- 레벨 스코프의 종류  
    - 함수 스코프  
    - 블록 스코프  
    - 전역 스코프  
    - 지역 스코프  
    
    
- Referencing Environment란?  
    - 프로그램 실행 특정 포인트에서 활성화 되어 있는 바인딩들의 집합  
    
<br/>
    
### 2__
</details>
