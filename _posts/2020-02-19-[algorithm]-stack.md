## Stack

- 중간은 건드리지 않는다. top에서만 넣고 빼고 하는 자료 구조를 구현할 때는 스택!

- 뒤집는 것도 스택을 사용하면 좋다.

- 에디터 문제: 문자열 잡고 미는 방식으로 풀면 시간 초과. 링크드 리스트로도 구현 가능하지만 스택으로도 구현 가능. 커서 왼쪽과 커서 오른쪽 각각을 스택으로 만든다. 위만 넣고 빼기 때문에 스택이 가능한 것.

  ```c++
  #include <iostream>
  #include <stack>
  
  using namespace std;
  
  stack<char> left_stack;
  stack<char> right_stack;
  
  void moveCursor(string &order);
  void printResult(void);
  
  int main(void) {
      freopen("../in.txt", "r", stdin);
  
      ios_base::sync_with_stdio(false);
      cin.tie(nullptr);
  
      string sent;
      cin >> sent;
      for (char ch : sent) {
          left_stack.push(ch);
      }
  
      int m;
      cin >> m;
      cin.ignore();
  
      for (int i = 0; i < m; i++) {
          string order;
          getline(cin, order);
          moveCursor(order);
      }
  
      printResult();
  
      return 0;
  }
  
  void moveCursor(string &order) {
      if (order[0] == 'L') {
          if (left_stack.size() == 0) return;
          right_stack.push(left_stack.top());
          left_stack.pop();
      }
      else if (order[0] == 'D') {
          if (right_stack.size() == 0) return;
          left_stack.push(right_stack.top());
          right_stack.pop();
      }
      else if (order[0] == 'B') {
          if (left_stack.size() == 0) return;
          left_stack.pop();
      }
      else {
          left_stack.push(order[2]);
      }
  }
  
  void printResult(void) {
      int size = left_stack.size();
      for (int i = 0; i < size; i++) {
          right_stack.push(left_stack.top());
          left_stack.pop();
      }
  
      size = right_stack.size();
      for (int i = 0; i < size; i++) {
          cout << right_stack.top();
          right_stack.pop();
      }
  }
  ```

- 오큰수 문제: 오큰수를 찾고 있는 idx를 stack에 저장하는 아이디어.

- 5 1 2 4 6 3이라 했을 때, 5의 오큰수는 6. 5와 6 사이에는 5보다 큰 수 없다.

- 찾았으면 다시 볼 필요 없으니까 pop, idx가 왼쪽부터 순서대로 쌓이기 때문에 모든 수의 오큰수를 찾을 수 있다.

  ```c++
  #include <iostream>
  #include <stack>
  
  using namespace std;
  
  int A[1000010];
  int NGE[1000010];
  
  int main() {
      freopen("../in.txt", "r", stdin);
      ios_base::sync_with_stdio(false);
      cin.tie(nullptr);
  
      int N;
      cin >> N;
  
      stack<int> stk;
  
      for (int i = 0; i < N; i++) {
          cin >> A[i];
          while (!stk.empty() && A[i] > A[stk.top()]) {
              // A[i] is NGE
              NGE[stk.top()] = A[i];
              stk.pop();
          }
          stk.push(i);
      }
  
      for (int i = 0; i < N; i++) {
          cout << (NGE[i] == 0 ? -1 : NGE[i]) << " ";
      }
      cout << '\n';
  
      return 0;
  }
  ```

  
