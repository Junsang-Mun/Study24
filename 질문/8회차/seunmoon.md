# Chapter 7. 보안 이슈와 포인터의 오남용 (Security Issues and the Improper Use of Pointers)
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
<br/>
<br/>
<details>
<summary>part 1. 포인터의 선언과 초기화 (Pointer Declaration and Initialization)</summary>

## part 1. 포인터의 선언과 초기화 (Pointer Declaration and Initialization)  
- 포인터의 선언 / 포인터를 초기화 하지 않는다면 발생할 수 있는 문제에 대해 알아보기
  
   
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
        - 타입 정의 : 컴파일러가 범위 규칙*_1_(scope rule)에 대해 확인하도록 함  
        - 매크로 정의 : 컴파일러에 따라 범위 규칙(scope rule)에 대해 확인을 못하는 경우도 존재

  
</details>
<br/>
<br/>
<br/>
<details>
<summary>part 2. 포인터의 사용 이슈 (Pointer Usage Issues)</summary>

## part 2. 포인터의 사용 이슈 (Pointer Usage Issues)
~내용~
</details>
<br/>
<br/>
<br/>
<details>
<summary>part 3. 메모리 해제 이슈 (Memory Deallocation Issues)</summary>

## part 3. 메모리 해제 이슈 (Memory Deallocation Issues)
~내용~
</details>
<br/>
<br/>
<br/>
<details>
<summary>part 4. 메모리 해제 이슈 (Using Static Analysis Tools)</summary>

## part 4. 메모리 해제 이슈 (Using Static Analysis Tools)
~내용~
</details>
<br/>
<br/>
<br/>
<details>
<summary>추가 공부 메모</summary>

## 추가 공부 메모
      
    
### _1_ 범위 규칙 (Scope Type)

    
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
    
### _2_ 뭐시기
</details>
