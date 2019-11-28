## REFERENCE

참조. 한 마디로 별명이다.

(출처: <https://modoocode.com/141>을 보고 공부한 내용입니다)



#### 왜 등장했을까?

참조를 활용하면 포인터로 복잡하게 쓰던 것을 더 간단하게 쓸 수 있다.

```c++
#include <iostream>

void changeValue(int * p) {
    *p = 5;
}

void changeValue2(int &a) {
    a = 7;
}

int main {
    int number = 3;
    
    // changeValue 함수를 이용해서 number를 바꾸려면
    changeValue(&number);
    std::cout << number << std::endl;
    
    // 참조를 이용하면
    changeValue2(number); // int &a = number;와 동일
    std::cout << number << std::endl;
    
    return 0;
}
```



#### 반드시 선언과 동시에 초기화가 필요

어떤 누구의 별명인데, 어떤 누구가 없는 것은 말이 안 된다. int &a; 이렇게는 못 쓴다.

포인터와는 다른 것이, 포인터 자체는 '메모리 값을 보관하는 변수' 자체로 활용될 수 있다.

메모리 값을 보관하기 위해 따로 메모리를 할당한다는 말.

하지만 참조자는 메모리 공간에 할당되고 그 공간에 어떤 값을 보관하는 게 아니라, 컴파일 시에 원래 가리키던 변수의 주소값, 정확히는 \*주소값으로 치환된다. (엄밀히 따지면 아니라고 하지만 일단 이렇게 이해)



#### 한 번 누군가의 별명이면 다른 변수의 별명이 될 수 없다

```c++
int a = 10;
int &ref = a;
int b = 3;

ref = b; // 이러면 이제 b를 별명으로 삼는 게 아니다. 그냥 a = b와 동일.
std::cout << a << std::endl; // 3
```



#### 참조는 누군가의 별명. 참조의 참조도 또 다른 별명일 뿐

```c++
#include <iostream>

int main(void) {
    int x;
    int &y = x;
    int &z = y;
    
    x = 1;
    std::cout << "x : " << x << " y : " << y << " z : " << z << std::endl;
    
    y = 2;
    std::cout << "x : " << x << " y : " << y << " z : " << z << std::endl;
    
    z = 3;
    std::cout << "x : " << x << " y : " << y << " z : " << z << std::endl;
    
    // 셋 다 다 바뀜. 참조는 메모리 할당이 아니다.(라고 이해). y와 z는 모두 x의 별명
    
    return 0;
}
```



#### 상수에 대한 참조

상수도 참조할 수 있다. 단 const 없이 참조하면 리터럴을 바꿀 수 있는 여지를 주는 것이라 에러.

```c++
#include <iostream>

int main(void) {
    int &ref = 4; // error
    const int &ref = 4;
    return 0;
}
```



#### 레퍼런스의 배열

레퍼런스의 배열은 불법!

"There shall be no references to references, no arrays of references, and no pointers to references."

```c++
int a, b;
int& arr[2] = { a, b };
int arr[3] = { 1, 2, 3 };
int(&ref)[3] = arr;
```

왜일까? <https://stackoverflow.com/questions/1164266/why-are-arrays-of-references-illegal>

배열명은 포인터다. arr은 arr[0]을 가리키는 포인터. 근데 참조는 메모리 할당이 따로 없다! 메모리에도 없는 애를 가리키고 있는 포인터가 존재한다는 것이 모순.



#### 배열의 레퍼런스

```c++
int arr[3];
int (&ref)[3] = arr;
// 가능은 하다. 근데 굳이? 배열을 가리키고 싶다면 그냥 포인터 쓰는 게 더 편하다
int *p = arr;
```



#### 레퍼런스를 리턴하는 함수

```cpp
#include <iostream>

int fn1(int &a) { return a; }
int &fn2(int &a) { return a; }

int main(void) {
    int x = 1;
    
    fn1(x)++; 
    // 이건 에러. int를 리턴한다는 것은 임시값을 리턴하는 것
    // 임시값은 메모리를 따로 할당하지 않고, 따라서 ++도 말이 안 된다
    fn2(x)++; 
    // 이건 int &a = x고 a를 리턴한다. 참조를 리턴했고, 그 참조는 x의 별명
    // ++을 하면 x의 값이 증가
}
```

