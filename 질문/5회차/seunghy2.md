
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
 Differences Between Arrays and Pointers 85
-
 -vector[i]와 *(vector + i)의 머신코드가 다르다고 나와있다. 그런데 이를 확인하기 위해 두 파일을 gcc -S를 통해 어셈블리어로 컴파일링 하고, 이를 diff를 활용하여 비교해보았는데, 어떠한 차이도 발견하지 못 하였다. 혹시 C 표준에 따라 다른 걸까?
 
 파일1


 #include <stdio.h>
int main(void) {
	int arr[5];
	int i = 0;

	while (i < 5)
	{
		arr[i] = i;
		i++;
	}
	return (0); }


파일2


#include <stdio.h>
int main(void) {
	int arr[5];
	int i = 0;

	while (i < 5)
	{
		*(arr + i) = i;
		i++;
	}
	return (0); }


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
