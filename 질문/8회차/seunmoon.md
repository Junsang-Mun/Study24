# Chapter 7. 보안 이슈와 포인터의 오남용 (Security Issues and the Improper Use of Pointers)

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
   int *ptr1, ptr2;
   ``` 
    
-     

  
</details>
  
  
  
  
<details>
<summary>part 2. 포인터의 사용 이슈 (Pointer Usage Issues)</summary>

## part 2. 포인터의 사용 이슈 (Pointer Usage Issues)
~내용~
</details>
  
  
  
  
<details>
<summary>part 3. 메모리 해제 이슈 (Memory Deallocation Issues)</summary>

## part 3. 메모리 해제 이슈 (Memory Deallocation Issues)
~내용~
</details>
  
  
  
  
<details>
<summary>part 4. 메모리 해제 이슈 (Using Static Analysis Tools)</summary>

## part 4. 메모리 해제 이슈 (Using Static Analysis Tools)
~내용~
</details>
