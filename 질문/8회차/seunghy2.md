# Chapter 7. 보안 이슈와 포인터의 오남용

포인터의 선언과 초기화
-

>"typedef"와 "#define"의 차이점

"typedef" : 새로운 type을 지정하는 데에 사용. 세미콜론(;)으로 끝나야 함. 컴파일러에 의해 수행됨. scope rule을 따름

"#define" : 값 등의 별칭을 위해 사용. 세미콜론이 필요없음. preprocessor 단계에서 단순히 복사 붙여넣기 정도의 역할 수행. scope rule을 신경쓰지 않음.

출처 : https://www.geeksforgeeks.org/typedef-versus-define-c/ 

>Scope Rule

scope rule : 변수 등이 함수 내에서 선언되었는지, 함수 밖에서 선언되었는지에 따라 함수 내에서 선언된 변수는 함수 내에서만 작동하도록 하는 규칙.

* "#define"이 scope rule을 따르지 않는 경우를 위한 테스트

코드

````
#include <stdio.h>

#define TEST int

void testfx(void)
{
	TEST a;

	#define TEST char

	TEST b;

	printf("%zu\n", sizeof(a));
	printf("%zu\n", sizeof(b));
}

int	main(void)
{
	TEST a;

	printf("%zu\n", sizeof(a));
	testfx();

	return 0;
}
````

해당 함수를 컴파일링 한 결과

````
btest.c:9:10: warning: 'TEST' macro redefined [-Wmacro-redefined]
        #define TEST char
                ^
btest.c:3:9: note: previous definition is here
#define TEST int
        ^
1 warning generated.
````

결과값

````
1
4
1
````

위에서 말했듯이 "#define"은 preprocessor 단계에서 TEST라는 문자열을 int로 바꾸며 코드를 내려감.

그러다가 두 번째 "#define TEST"를 만나면서 이후 아래의 코드들은 TEST라는 문자열을 char로 바꾸며 내려가게 됨.

그렇다보니, testfx함수 내에서만 작동하길 바랬던 "#define TEST char"은 main 함수의 TEST a 까지 영향을 끼치면서, char형이 되버림.

* "typedef"은 scope rule을 따른다

코드

````
#include <stdio.h>

typedef int TEST;

void testfx(void)
{
	TEST a;

	typedef char TEST;

	TEST b;

	printf("%zu\n", sizeof(a));
	printf("%zu\n", sizeof(b));
}

int	main(void)
{
	TEST a;

	printf("%zu\n", sizeof(a));
	testfx();

	return 0;
}
````

컴파일링 문제 없음.

결과값

````
4
4
1
````

main문의 TEST a와 testfx함수 내의 TEST a는 "typedef int TEST;" 의 영향을 받아 int형으로 되었고,

"typedef char TEST;" 이후에 나온 TEST b는 char형이 되었음.

포인터 사용 이슈
-
메모리 해제 이슈
-
