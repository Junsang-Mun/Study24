# CHAPTER 4. 포인터와 배열 (Pointers and Arrays)
> 모든 self Q, Q에 대한 정답은 접어두도록 하겠다. 질문만 보고 스스로 생각해본 뒤 정답 보기!
> 만약 정답과 비교해봤을 때 부족한 부분이 있었거나, 정답 내용을 모두 떠올리긴 했어도 우다다다 대답하지 못하였다면 그건 아직 모르는 것임!
## 들어가기
<details>
<summary>self Q1. 문자 배열과 문자열를 비교하시오.</summary>



</details>
<details>
<summary>self Q1. 문자열을 선언하는 방법은 어떠한 것들이 있나? 또한 그 방법들의 차이점을 아는가?</summary>

방법1) 포인터 형태 선언  
    
```
    // ex)
    char *a = "hello!";  
``` 
      
방법2) 배열 형태로 선언 (크기 자동 할당)  
    
    ```
    // ex)  
    char a[] = "hello!";
    ```  
    
    - 방법2의 진행 순서  
    
        1. 메모리 어딘가에 "hello!"가 할당된다.  

        2. char *a 변수가 선언된다.  

        3. a는 h의 주소값을 가리킨다. (*string = 'h')
      
 
- 두 방법의 공톰점
    
    - 포맷팅 양식이 %s로 같다.
    
    - 문자열 전체 출력 결과가 같다.
- 두 방법의 차이점
    
   - 할당된 메모리 영역의 크기가 다르다.  
        -> sizeof함수를 적용해봤을 때,방법1의 경우 8(포인터 자료형 크기)이고 방법2의 경우 6(비열 크기)임
    
   - 할당된 메모리 영역의 위치가 다르다.  
    
   - 배열 형태는 내용 수정 가능, 포인터 형태는 내용 수정 불가능 (리터럴 상수)
    
    
  

</details>


## 배열 (array)


## 포인터 표기법과 배열 Pointer Notation and Arrays)


## malloc 함수로 1차원 배열 생성하기 (Using malloc to Create a One-Dimensional Array)


## realloc 함수로 배열 크기 조정하기 (Using the realloc Function to Resize an Array)


## 1차원 배열 전달하기 (Passing a One-Dimensional Array)


## 1차원 포인터 배열 이용하기 (Using a One-Dimensional Array of Pointers)


## 포인터와 다차원 배열 (Pointers and Multidimensional Arrays)


## 다차원 배열 전달하기 (Passing a Multidimensional Array)


## 2차원 배열 동적으로 할당하기 (Dynamically Allocating a Two-Dimensional Array)


## 가변 배열과 포인터 (Jagged Arrays and Pointers)

