# Chapter 7. 보안 이슈와 포인터의 오남용

포인터의 선언과 초기화
-

>"typedef"와 "#define"의 차이점

"typedef" : 새로운 type을 지정하는 데에 사용. 세미콜론(;)으로 끝나야 함. 컴파일러에 의해 수행됨. scope rule을 따름

"#define" : 값 등의 별칭을 위해 사용. 세미콜론이 필요없음. preprocessor 단계에서 단순히 복사 붙여넣기 정도의 역할 수행. scope rule을 따르지 않을 수 있음.

출처 : https://www.geeksforgeeks.org/typedef-versus-define-c/ 

scope rule : 변수 등이 함수 내에서 선언되었는지, 함수 밖에서 선언되었는지에 따라 함수 내에서 선언된 변수는 함수 내에서만 작동하도록 하는 규칙.

* "#define"이 scope rule을 따르지 않는 경우를 위한 테스트

포인터 사용 이슈
-
메모리 해제 이슈
-
