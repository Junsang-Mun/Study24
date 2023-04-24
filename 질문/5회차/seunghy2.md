
 Quick Review of Arrays 80
-
 One-Dimensional Arrays 80
-
 Two-Dimensional Arrays 81
-
 Multidimensional Arrays 82
-
 Pointer Notation and Arrays 83
-
 vector[i]와 *(vector + i)가 같다는 것을 아래 Differences Between Arrays and Pointers (85) 파트에 작성한 것처럼 확인하였다.
 추가로 i[vector]의 형태 또한 *(vector + i)의 형태로 변한다고 가정했을 때 이 또한 같지 않을 까 확인하였고, 역시 같음을 확인하였다.
 그럼에도 책에서 또한 이는 언급되지 않아 있는데, 이는 가시성의 문제이지 않을까 추측했다.
 혹시 그 외의 다른 문제점이 있을까?
 
 참고 : https://stackoverflow.com/questions/7181504/why-does-iarr-work-as-well-as-arri-in-c-with-larger-data-types 
 
 vector가 다루는 자료형이 인덱스로 사용하는 int 자료형 i보다 큰 경우 혹시 자료형의 크기 * i를 하는 과정에서 문제가 생길 수도 있지 않을까해서 테스트 해보았다. ( 물론 자료형의 크기sizeof()가 int형이 아니므로 문제가 되진 않을 것 같다. unsigned long int 라고 함. )

````
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

struct large {
	char data[2147483648];
};

int	main(void)
{
	int i;
	struct large *arr;
	i = 1;
	arr	= (struct large *)malloc(sizeof(struct large) * 2);
	if (!arr)
		return (-1);
	memset(arr, 0, sizeof(arr[0]));
	memset(arr, 0, sizeof(arr[1]));
	printf("%lu\n", sizeof(arr[0]) + sizeof(arr[1]));
	strcpy(arr[0].data, "You are wrong (kk)");
	strcpy(arr[1].data, "Hello World!");
	printf("%s\n%s\n%s\n", arr[i].data, i[arr].data, (*(arr + i)).data);

	free (arr);
	return (0);
}
````

````
./a.out
4294967296
Hello World!
Hello World!
Hello World!
````

그렇다면, unsigned long int 의 범위를 벗어난 크기의 구조체를 만든다면? (진짜로 sizeof() * i 형식으로 되는지, 아니면 sizeof()만큼의 byte를 i번 각각 뛰어넘는지 등...)

vector[i] 와 i[vector]를 비교하는 것과는 연관이 아예 없다.

 Differences Between Arrays and Pointers 85
-
 vector[i]와 *(vector + i)의 머신코드가 다르다고 나와있다. 그런데 이를 확인하기 위해 두 파일을 gcc -S를 통해 어셈블리어로 컴파일링 하고, 이를 diff를 활용하여 비교해보았는데, 어떠한 차이도 발견하지 못 하였다. 혹시 C 표준에 따라 다른 걸까?
 
 파일1

````
 #include <stdio.h>
int	main(void)
{
	int arr[5];
	int i = 0;

	while (i < 5)
	{
		arr[i] = i;
		i++;
	}
	return (0); 
}
````

파일2

````
#include <stdio.h>
int	main(void)
{
	int arr[5];
	int i = 0;

	while (i < 5)
	{
		*(arr + i) = i;
		i++;
	}
	return (0);
}
````

 Using malloc to Create a One-Dimensional Array 86
-
 Using the realloc Function to Resize an Array 87
-
 Passing a One-Dimensional Array 90
-
 Using Array Notation 90
-
 Using Pointer Notation 91
-
 Using a One-Dimensional Array of Pointers 92
-
 Pointers and Multidimensional Arrays 94
-
 Passing a Multidimensional Array 96
-
매개변수로 int arr[][5]가 아닌 int *arr일 때의 예시에서, 
````
#include <stdio.h>

void d2DArray(int *arr, int rows, int cols)
{
	for(int i = 0; i < rows; i++)
	{
		for(int j = 0; j < cols; j++)
			printf("%d", *(arr + (i * cols) + j));
		printf("\n");
	}
}

int main(void)
{
	int matrix[2][5] = {
		{1, 2, 3, 4, 5},
		{6, 7, 8, 9, 10}
	};
	d2DArray(&matrix[0][0], 2, 5);
}
````

````
a) printf("%d", *(arr + (i * cols) + j)); 
````
대신에
````
printf("%d", arr[i][j]); 
````
는 잘못되었으며
````
printf("%d", (arr + i)[j]); 
````
를 쓰라고 하는데 사실 이 또한 올바른 답이 나오지 않는다.
arr[i][j]와 같은 논리로 arr은 정수형 포인터 값을 담고 있으므로, arr + i는 sizeof(int) 크기로 작동하기에 옳지 않다.

````
b) printf("%d", (arr + i * cols)[j]);
c) printf("%d", (arr + j)[i * cols]);
````

둘 중 하나로 써야 올바르게 나온다.

 Dynamically Allocating a Two-Dimensional Array 99
-
 Allocating Potentially Noncontiguous Memory 100
-
 Allocating Contiguous Memory 100
-
 Jagged Arrays and Pointers 102
-
 Summary 105
-
