# Chapter8. 기타 (Odds and Ends)  
  
  
<br/>
<details>
<summary>part 0. 들어가기</summary>

## part 0. 들어가기
  
  
</details>

<br/>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 
<br/>
<details>
<summary>part 1. 포인터 캐스팅 (Casting Pointers)</summary>

## part 1. 포인터 캐스팅 (Casting Pointers)
  
  
- 포인터 캐스팅이 유용한 이유
    - 특수 목적 주소에 접근 가능
    - 포트(port)에 대응하는 주소를 할당 가능
    - 시스템의 엔디안(endian) 타입을 알 수 있음
<br/>  
    
### 특수 목적 주소 접근하기 (Accessing a Special Purpose Address)  
- 저수준 커널 프로그램에서 주소 0에 접근할 때 사용할 수 있는 기법들  
    - 포인터를 0으로 설정 (이 방법이 항상 동작하는 것은 아님)  
    - 정수 변수에 0을 할당, 변수를 정수 포인터로 캐스팅  
    - 유니언 이용  
    - memset 함수를 이용해 포인터에 0을 할당  
    
<br/>  
    
### 포트에 접근하기 (Accessing a Port)  
- 포트란?
    - 소프트웨어적인 개념
        - 서버가 컴퓨터로 전달된 특정한 메시지를 자신이 받는다는 것을 알리기 위해 사용하는 것  
        - 운영체제의 일부
    - 하드웨어적인 개념  
        - 일반적으로 외부 장치에 연결된 물리적인 입/출력 시스템 컴포넌트   
        - 하드웨어 포트에서의 읽기/쓰기 작업을 통해 프로그램에서의 정보와 명령 처리 가능  
    
<br/>  
    
### DMA로 메모리 접근하기 (Accessing Memory using DMA)
- DMA란?
    - 직접 메모리 접근 (Direct Memory Access)
    - 메인 메모리와 특정 장치 간 데이터 전송을 보조하는 저수준 동작
    - 주로 CPU와 병행으로 동작이 이루어짐
    
<br/>  
 
### 시스템의 엔디안 종류 알아내기 (Determining the Endianness of a Machine)  
  
</details>

<br/>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 
<br/>
<details>
<summary>part 2. 에일리어싱, 엄격한 에일리어싱, 그리고 restrict 키워드 
(Aliasing, Strict Aliasing, and the restrict Keyword)</summary>

## part 2. 에일리어싱, 엄격한 에일리어싱, 그리고 restrict 키워드 
(Aliasing, Strict Aliasing, and the restrict Keyword)
    
<br/>  
    
### 유니언을 사용해 여러 방법으로 값 표현하기 (Using a Union to Represent a Value in Multiple Ways)
    
<br/>  
    
### 엄격한 에일리어싱 규칙 (Strict Aliasing)
    
<br/>  
    
### restrict 키워드 사용하기 (Using the restrict Keyword)
  
</details>

<br/>  
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 
<br/>
<details>
<summary>part 3. 스레드와 포인터 (Threads and Pointers)</summary>

## part 3. 스레드와 포인터 (Threads and Pointers)
    
<br/>  
    
### 스레드 간의 포인터 공유 (Sharing Pointers Between Threads)
    
<br/>  
    
### 함수 포인터를 이용한 콜백 함수 지원 (Using Function Pointers to Support Callbacks)
  
</details>

<br/>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 
<br/>
<details>
<summary>part 4. 객체 지향 기법 (Object-Oriented Techniques)</summary>

## part 4. 객체 지향 기법 (Object-Oriented Techniques)
    
<br/>  
    
### 불투명 포인터 생성하고 사용하기 (Creating and Using an Opaque Pointer)
    
<br/>  
    
### C언어에서의 다형성 (Polymorphism in C)
  
</details>

<br/>
