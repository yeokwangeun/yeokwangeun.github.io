## 비트마스크

- 비트 연산을 이용해서 부분집합을 표현할 수 있다
- 2의N제곱의 경우의 수를 만들 때, 재귀를 사용해도 되지만 비트마스크를 활용하면 좋다.
- 비트마스크의 장점은 정수라는 것. 공간도 절약하고, 배열의 인덱스로도 사용 가능하다!
- 0부터 N-1까지의 수를 담을 수 있다. int면 31까지 가능한 것
- 비트 연산을 활용해서 수를 추가, 검사, 제거, 토글할 수 있다

```c++
#include <iostream>
#include <string>

using namespace std;

int S = 0x0;

void operate(const string &order, const int &num) {
    if (order == "add") {
        S |= (1 << num);
    } else if (order == "remove") {
        S &= ~(1 << num);
    } else if (order == "check") {
        bool chk = ((S & (1 << num)) != 0);
        cout << (chk == true ? 1 : 0) << '\n';
    } else if (order == "toggle") {
        S ^= (1 << num);
    } else if (order == "all") {
        S = 0x1FFFFF;
    } else if (order == "empty") {
        S = 0x0;
    }
}
```

