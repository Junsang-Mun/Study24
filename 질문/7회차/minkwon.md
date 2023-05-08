# Chapter6. Pointers and Structures  

> ## Introduction    
구조체를 선언하는 방법에는 struct 예약어를 사용하는 방법과 typedef 예약어를 사용하는 방법이 있다. 구조체를 선언할 때는 struct 예약어를 항상 사용해야 하는데 typedef 예약어를 함께 사용하여 구조체 이름을 만들어 선언하면 추후 struct를 사용하지 않아도 구조체 이름만으로도 해당 구조체를 사용할 수 있다. 구조체 안에 선언된 변수에 .(dot)을 사용하여 접근할 수 있다. 구조체가 포인터로 선언되었을 경우 ->(points-to operator)를 사용하여 접근할 수 있다. 포인터를 역참조하여 .을 사용하는 방법도 있다.       

> ## How Memory Is Allocated for a Structure 135  
구조체가 선언되면 메모리는 해당 구조체 안에 선언된 변수들 크기의 합만큼 할당된다. 그러나 변수들의 크기가 일정하지 않은 경우 패딩이 일어난다. 예를 들어 구조체에 포인터 변수 3개와 short형 변수 1개가 선언되었을 경우, 해당 구조체의 크기는 16바이트가 된다. short형 변수의 크기는 2바이트이지만 나머지 포인터 변수의 크기 4바이트에 맞춰 2바이트가 추가되었기 때문이다. 

> ## Structure Deallocation Issues 136  
구조체 안에 선언 된 포인터는 자동으로 할당되거나 할당 해제 되는 것이 아니다. 구조체 안에 포인터가 선언되면 초기화 되기 전까지 해당 포인터는 쓰레기 값을 가지게 된다. 따라서 초기화 해주는 작업이 필요한데 별도의 함수를 만들어 초기화할 수 있다. 이 과정에서 해당 함수 호출이 끝날 경우 구조체 포인터 변수는 사용이 끝남과 동시에 메모리에서 사라지나, 해당 포인터를 통해 동적할당 하여 대입한 값은 그대로 힙 공간에 남아있게 된다. 힙에 할당된 값에 대한 주솟값을 가리키는 포인터 변수가 사라짐으로 인해 그것을 해제할 수 없게 된다. 따라서 이것을 해제할 수 있는 별도의 함수가 필요하다. C++과 같은 객제 지향 언어에서는 이러한 작업이 자동으로 이루어진다.  

> ## Avoiding malloc/free Overhead 139  

> ## Using Pointers to Support Data Structures 141  

>> Single-Linked List 142  

>> Using Pointers to Support a Queue 149  
 
>> Using Pointers to Support a Stack 152  
 
>> Using Pointers to Support a Tree 154  
 
