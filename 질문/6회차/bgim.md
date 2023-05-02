# 포인터와 문자열
 ### 이 장의 목적
 - 문자열 선언과 초기화하는 다양한 방법에 대해 알아봄
 - C 앱에서 리터럴 풀의 활용과 그 영향에 대한 조사
 - 일반적인 문자열 조작(비교, 복사, 연결 등) 대해 알아봄
 - 함수 포인터의 사용 방식과 정렬 연산에 어떻게 활용되는지
 - 위의 상황에서 포인터가 어떻게 동작하는지 이해하는 것

## 문자열 기초
 - 문자열은 아스키 NUL 문자로 끝나는 연속된 문자의 집합
 - 문자열은 일반적으로 배열 또는 힙에 할당된 메모리에 저장됨
 - 모든 문자의 배열이 문자열인 것은 아니다. 문자 배열에는 NUL이 포함되지 않을 수 있다
 - NULL과 NUL이 다르다는 사실을 기억하자. NULL은 특별한 포인터로 사용되며, 일반적으로 ((void*)0)으로 정의한다. NUL은 문자이며, '\0'으로 정의한다. 이들 둘은 서로 바뀌어 사용되어서는 안 된다.
 - <mark>Q. C 언어에서 문자 상수는 아래 예제와 같이 정수(int) 타입이라고 한다. 이런 변칙 현상은 언어 설계에 의도적으로 반영되었다고 하는데, 왜지? 어차피 1바이트만으로 ascii 표현 가능한 것이 아니었던가?</mark>
  ```c
  printf("%d\n", sizeof(char));   // 1 byte
  printf("%d\n", sizeof('a'));	  // 4 bytes
  ```
  - <span style = "color:green">※참고 : C++ 에서는 문자 리터럴이 char와 동일하게 1바이트임)</span>
  - <span style = "color:green">※ literal 이란? "소스코드에 쓴 그대로의 값"</span>
  - <span style = "color:red">Q. 패킹과 다중바이트란?</span>
 
 ### 문자열 선언
 - 방법 세 가지 : 리터럴로, 문자의 배열로, 문자에 대한 포인터로
 #### 문자열 리터럴 풀
 - (읽기 전용)메모리 영역에 문자열 리터럴 풀이라는 게 존재하는구나.
 #### 문자열 리터럴이 상수가 아니라면
 ```c
 char *tabHeader = "Sound";
 *tabHeader = 'L';
 printf("%s\n", tabHeader); // "Lound" 출력
 ```
 - <mark>Q. 나는 왜 예제처럼 'L'로 바꾸고 출력 안 되지?</mark>
 ### 문자열 초기화
 #### char 배열 초기화하기
 ```c
    char header[] = "Media Player";
 ```
 - 위와 같은 코드가 있다면, (내부 구현 과정에서 자동으로) 배열에 13바이트가 할당되고, 초기화 과정에서 리터럴의 문자열이 header 배열로 복사(strcpy?)된다고 한다. 평소 무심코 사용하고 있는 것들이 내부적으로 어떤 원리와 흐름으로 동작하는지 궁금하곤 했는데 이런 식으로 조금씩 알게 되니 재밌다.

 #### char 포인터 초기화하기
 - malloc 함수에 전달할 문자열의 길이를 결정 할 때 유의사항
    - NUL 문자를 위해 1바이트를 추가로 할당
    - 문자열의 길이를 계산할 때 sizeof 연산자를 사용하지 말고, strlen 함수를 이용한다.(※ sizeof 연산자는 문자열의 길이가 아닌 배열이나 포인터의 크기를 변환하는 것을 인지할 것)
 - char 포인터는 문자 상수로 초기화 할 수 없다. 문자 상수는 정수형(int)이기 때문에, char 포인터에 정수를 할당하려는 시도와 같다. 이렇게 하면 포인터가 역참조될 때 앱의 비정상 종료가 빈번하게 발생한다.
 ```c
    char* prefix = '+';  // 유효하지 않음
    
    prefix = (char*)malloc(2); // malloc 함수를 이용한 올바른 방식
    *prefix = '+';
    *(prefix+1) = 0;
 ```
 #### 표준 입력(stdin)으로 문자열 초기화하기
 #### 문자열 위치 요약
 ```c
    char *globalHeader = "Chapter";
    char globalArrayHeader = "Chapter";

    void	displayHeader()
    {
        static char* staticHeader = "Chapter";
        char* localHeader = "Chapter";
        static char staticArrayHeader[] = "Chapter";
        char localArrayHeader[] = "Chapter";
        char* heapHeader = (char*)malloc(strlen("Chapter")+1);
        strcpy(heapHeader, "Chapter");
    }
 ```
 - 책의 '그림 5-5 선언 방식에 따른 (메모리 영역의)문자열 위치'를 한 번 머리에 넣어두자.

 #### <span style = 'color:green'>※※ 마크업, 마크다운이란?</span>
 - <span style = 'color:green'>마크다운을 이 문서를 작성하면서 처음으로 찾아보게 되었는데 문득 궁금했다.</span>
 > “마크업 언어(Markup Language)는 문서가 화면에 표시되는 형식을 나타내거나 데이터의 논리적인 구조를 명시하기 위한 규칙들을 정의한 언어의 일종이다. 데이터를 기술한 언어라는 점에서 프로그래밍 언어와는 차이가 있다.”
 - <span style = 'color:green'>그러면 Markdown은 무엇일까?</span>
 > 마크다운은 마크업 언어가 태그(<div> </div>과 같은 구조)로 이루어져 사용하기 힘들어서 만들어진 마크업의 파생형이다. 읽기도 쓰기도 쉬운 문서 양식을 지향하기 때문에, 복잡한 태그 구조가 사라지고 간단한 텍스트들과 몇 가지 문법만 알면 작성할 수 있게 했다.
 - <span style = 'color:green'>지금 작성되고 있는 확장자명 .md 뜻이 markdown 이었구나!</span>

## 표준 문자열 연산
 - 이 절에서는 일반적인 문자열 연산(문자열 비교, 복사, 연결하기 등)에서의 포인터 사용에 대해 알아보자.
 ### 문자열 비교하기
 - strcmp 반갑다. 피신에서 보았었지. 뜻 : string compare
 - strcmp 함수의 원형 : int strcmp(const char *s1, const char *s2);
 - 반환값 : 음수(s1이 s2보다 사전적으로 앞에 있을 때), 0, 양수
 - 문자열 비교할 때 잘못된 방법의 예
 ```c
    char command[16];
    if(strcmp(command, "QUIT") == 0) // 맞음.
    if(command == "QUIT") // 틀림. 배열의 이름이나 문자열 상수 자체는 내용이 아닌 주소를 반환한다.
 ```
 ### 문자열 복하사기
 - char* strcpy(char *s1, const char *s2);
 - Q. 그림 5-8 에서 예시로 든 문자열 amorphous compounds 의 뜻이 비정질 화합물이란다. 저자는 이 단어를 어떻게 사용하게 되었을까?
 ### 문자열 연결하기
 - char *strcat(char *s1, const char *s2);
 - 이 함수는 두 문자열의 포인터를 입력받고, 연결된 결과 문자열에 대한 포인터를 반환한다.
 - 잘 이해가 안 된다. 한 번 더 읽자.
 
## 문자열 전달하기
 - 메모 : 후위 증가 연산자가(++) 참조 연산자(*)보다 우선순위가 높다.
 ### 단순 문자열 전달하기
 ### 상수 문자에 대한 포인터 전달하기
 ### 전달한 문자열 초기화하기
 ### 애플리케이션에 인자 전달하기
 - argc, argv

## 문자열 반환하기
 ### 리터럴 문자열의 주소 반환하기
 ### 동적으로 할당된 메모리 주소 반환하기
 ### 로컬 문자열의 주소 반환하기

## 함수 포인터와 문자열

# 요약
 - 흐음.
