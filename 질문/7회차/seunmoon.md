# Ch6. 포인터와 구조체 (Pointers and Structures)  
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――  
## 들어가기
- C언어에서 구조체란 무엇이며 포인터와의 관계는?  
  - 데이터 구조의 요소(연결 리스트, 트리의 노드 등)를 표현할 때 사용  
  - 포인터는 이 요소들은 연결하는 접착제 역할  
    
    
- 이 장에서 중점적으로 살펴볼 것  
  - 데이터 구조의 메모리 할당 원리 및 다양한 연
  - 구조체가 사용하는 메모리 관리 기법
  - 몇몇 일반적인 데이터 구조의 구현  
  - 여태까지 공부해온 포인터 개념의 확장  
    - 구조체와 함께 사용할 때 포인터의 표기법  
    - 함수 포인터  
  - 힙(heap) 관리의 오버헤드를 최소화할 수 있는 기법  
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――  
## 소개 (Introduction)
- 구조체 선언 방법
    - ///code/// strcut 키워드 이용  
        ```c
        struct _person{  
            char *firstName;  
            char *lastName;  
            char *title;  
            unsinged int age;  
        }  
        ```
    - ///code/// typedef 키워드 이용  
        ```c
        typedef struct _person{  
            char *firstName;  
            char *lastName;  
            char *title;  
            unsinged int age;  
        } Person;  
        
        /* person의 인스턴스 선언 방법  
        1) Person person; ---> 단순 구조체 선언, '.(dot)' 기호를 사용하여 필드에 접근  
        2) Person *person; ---> 구조체에 대한 포인터 이용, '->(points-to)' 연산자를 사용하여 필드로 접근  
            (이 경우에 '.'을 사용하려면 (*person).firstName 이런 식으로 역참조 사용해야 함)  
        ```
    - 주의할 점 (혼동하지 말 것)  
        - ///code/// 구조체 선언은 변수 선언이 아니다!!!  
          main()함수 내에서 변수를 선언해야 구조체 변수가 선언 된 것임.  
          ```c
          #include <stdio.h>
          #include <string.h>
          
          struct student{
            int number;
            double grade;
          } s1;
          
          int main(){
            s1.number = 12345;
            s1.grade = 4.5;
            
            return 0;
          }
          ```
          
- 구조체에 메모리가 할당되는 방법  
    - 구조체가 메모리에 할당될 때 메모리 양 = 최소한 구조체의 각 필드 크기의 합  
      (※ 단, 구조체 각 변수 사이에 패딩(padding)이 발생하는 경우 필드 크기의 합보다 더 많은 크기가 할당 됨 주의  
          (ex_ short 타입의 필드의 경우 2byte의 패딩 추가되어 나머지 필드와 같은 양의 메모리가 할당 됨))  

## 구조체 할당 해제 이슈 (Structure Deallocation Issues)
- 구조체에 메모리가 할당될 때/구조체가 삭제될 때 런타임 시스템(runtime system)의 작동  
    - 런타임 시스템은 구조체 내부에 선언된 포인터에 자동으로 메모리 할당하지 X  
    - 런타임 시스템은 구조체 내부의 포인터에 할당된 메모리 자동 해제 X  
  
- ///code/// 초기화에 대하여
```c
// 밑의 코드에는 person을 선언하는 4가지 경우 (case0 ~ 3, 주석으로 표시해둠)을 나타냄  
  
typedef struct _person{  
    char *name;  
    int user_id;  
} Person  
  
Person p0; // case0  
  
int main(){  
    Person p; // case1  
    Person *pp = malloc(sizeof(Person)); // case2  
    static Person p3; //case3  
}
```  
    - 선언한 구조체 변수가 동적인 저장 공간에 할당될 때
    - 선언한 구조체 변수가 정적인 저장 공간에 할당될 때
    

## malloc/free 오버헤드 피하기 (Avoiding malloc/free Overhead) 

## 포인터와 데이터 구조 ( Using Pointers to Support Data Structures)  

- 단일 연결 리스트 (Single-Linked List)  

- 포인터와 큐 (Using Pointers to Support a Queue)  
 
- 포인터와 스택 (Using Pointers to Support a Stack)  
 
- 포인터와 트리 (Using Pointers to Support a Tree)  
 
