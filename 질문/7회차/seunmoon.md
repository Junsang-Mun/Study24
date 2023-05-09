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
  
  
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
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
      
        
    - **선언한 구조체 변수가 정적인 저장 공간에 할당될 때**  
      ( 모든 함수의 범위를 벗어난 '전역 변수'로 선언된 경우 (위의 코드에서 case0)  
        & 함수 내에서 '정적 변수'로 선언된 경우(위의 코드에서 case3))  
          
        - 초기화가 된 경우  
          : ROM(Read Only Memory)에 들어감  
           (+ Q. ROM 영역은 값을 수정할 수 없는데 정적 변수는 왜 수정이 가능할까?  
                 A. Data 영역에 있는 변수들(런타임에서 수정되는 값들)은 RAM도 함께 쓰기 때문)  
        - 초기화가 되지 않은 경우 (즉, ROM에도 들어가지 않고 RAM을 사용하는 '수정'의 행위도 일어나지 X)  
          : BSS 영역(data 영역과 heap 영역의 사이)에 저장이 되며,  
            이 영역에서는 저장을 시작할 때 내장된 핸들러(reset)가 값을 모두 0으로 채움  
            -> 결론 : 정적 영역에 저장되는 변수는 자동으로 0으로(포인터의 경우 NULL로) 초기화 됨!!!  
      
        
    - **선언한 구조체 변수가 동적인 저장 공간에 할당될 때**    
       (스택(stack)에 올라오는 경우 (위의 코드에서 case1)  
         & 힙(heap)에 올라오는 경우 (위의 코드에서 case2))  
           
         - 초기화가 된 경우  
           : 알맞은 공간(stack 또는 heap)에 초기화되어 들어감  
         - 초기화가 되지 않은 경우  
           : 쓰레기(garbage) 값으로 채워짐 (즉, 자동으로 초기화되지 않음)  
  
    
- 주의할 점
    - ///code/// 구조체에 대한 포인터를 사용한 경우, 아래 코드처럼 포인터에 할당된 메모리를 해제하는 것 잊지 말기
        ```c  
        void processPerson(){  
            Person *ptrPerson;  
            ptkrPerson = (Person*)malloc(siezof(Person));  
            initializePerson(ptrPerson, "Peter", "Underwood", "Manager", 36);  
                ...  
            deallocatePerson(ptrPerson);  
            free(ptrPerson);  
        }  
        ```  
          
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――  
## malloc/free 오버헤드 피하기 (Avoiding malloc/free Overhead)  
  
    
- 오버 헤드(Overhead)이란?  
    - 프로그램의 실행흐름에서 나타나는 현상 중 하나  
    - ex) 프로그램의 실행 흐름 도중에 동떨어진 위치의 코드를 실행시켜야 할 때,  
      추가적으로 시간, 메모리, 자원이 사용됨  
    - 이러한 현상은 프로그래밍 시 외부 함수를 사용할 때 특히 나타남  
    - 매크로 함수, 인라인 함수 등을 사용하여 최적화 시키곤 함  
  
  
- 객체 풀(Object Pool)이란?  
    - 목적 :  객체를 매번 할당&해제하지 않고 고정 크기 풀에 들어있는 객체를 재사용함  
      -> 메모리 사용 성능 개선(메모리나 다른 자원 할당을 신경쓰지 않고 마음껏 객체를 생성 및 삭제 가능)
    - 객체  풀에 들어가는 객체는 자신이 '사용 중'인지 여부를 알 수 있는 방법을 제공해야함  
      (사용 중이 아니라면 객체 풀에 다시 집어 넣어야 하기 때문)  
    - 해당 풀에 객체를 미리 '사용 안함' 상태로 선언해놓고 필요할 때 객체 풀에 새로운 객체를 달라고 요구  
      -> 풀에서 객체를 선택하여 '사용 중'상태로 바꾼 뒤 객체를 반환해 줌  
  
    
- 객체 풀을 사용하여 오버 헤드 회피하는 방법  
    - step 1) ///code/// 위의 예시 코드에서 계속 사용되었던 person 객체를 담을 리스트 만들고 초기화  
    　　　　(초기화해주는 역할의 함수인 initializeList를 통해 배열의 각 요소에 NULL을 할당)    
            
            ```c  
            #define LIST_SIZE 10  
            Person *list[LIST_SIZE];  
            
            void initializeList() {  
                for (int i = 0; i < LIST_SIZE; i++)  
                    list[i] = NULL;  
            }  
            ```
              
    - step 2) 이제, person의 개체를 추가하거나 가져올 때 두 개의 함수(getPerson, returnPerson)이 사용 됨  
        - ///code/// getPerson 함수
            : list에서 person 객체의 개체가 사용 가능한 경우 개체를 가져오는 함수.  
            : 배열의 요소가 NULL인지 검사 -> 처음으로 NULL이 아닌 요소 반환 & 해당 위치의 값에 NULL 할당.  
            : 만약 사용 가능한 개체가 없다면 -> 새로운 Person 객체 생성 후 반환  
           
                ```c
                Person *getPerson() {
                    for (int i = 0; i < LIST_SIZE; i++) {
                        if (list[i] != NULL) {
                            Person *ptr = list[i];
                            list[i] = NULL;
                            return ptr;
                        }
                    }
                    Person *person = (Person*)malloc(sizeof(Person));
                    return person;
                }
                ```  
            
        - ///code/// returnPerson 함수
            : list에 person 개체를 반환하거나 해제하는 함수.  
            : 배열의 요소가 NULL인지 검사 -> NULL일 경우 반환된 person 인스턴스를 그 위치에 할당 & 포인터 반환.  
            : 만약 list가 가득 차있다면 -> person 안의 포인터를 deallocatePerson 함수를 이용하여 해제 후 NULL 반환  
            
                ```c  
                Person *returnPerson(Person *person) {  
                    for (int i = 0; i < LIST_SIZE; i++) {  
                        if (list[i] == NULL) {  
                            Person *ptr = list[i];  
                            list[i] = person;  
                            return person;  
                        }  
                    }  
                    deallocatePerson(person);  
                    free(person);  
                    return NULL;  
                }  
                ```   
                
                
    - ///code/// 위 step(과정) 총 정리 및 요약
        ```c
        initializeList();
        Person *ptrPerson;
        
        ptrPerson = getPerson();
        linitializePerson(ptrPerson, "Ralph", "Fisgerald", "Mr", 35);
        displayPerson(*ptrPerson);
        returnPerson(ptrPerson);
        ```  
              

    - 위 방법의 한 가지 문제점 : 리스트 크기  
        - 리스트가 너무 작을 경우 : 동적잉ㄴ 할당 및 해제가 더욱 자주 발생  
        - 리스트가 너무 클 경우 : 너무 많은 메모리가 선점되어 다른 애플리케이션의 실행에 영향을 줌  
        - 따라서, 좀 더 정교한 메모리 관리 기법을 이용해 리스트의 크기 조절 가능  
        
        
        
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――  
## 포인터와 데이터 구조 ( Using Pointers to Support Data Structures)  

- 단일 연결 리스트 (Single-Linked List)  

- 포인터와 큐 (Using Pointers to Support a Queue)  
 
- 포인터와 스택 (Using Pointers to Support a Stack)  
 
- 포인터와 트리 (Using Pointers to Support a Tree)  
 
