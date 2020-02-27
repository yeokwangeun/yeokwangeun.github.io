## Dynamic Programming

1. Dynamic Programming이란

- 큰 문제를 작은 문제로 나눠서 푸는 알고리즘

- overlapping subproblem(작은 문제끼리 겹친다. 점화식의 형태로 표현 가능하다), optimal substructure(작은 문제의 정답을 구해가다 보면 큰 문제의 정답을 구할 수 있다)가 DP의 특징

- optimal substructure를 만족한다는 것은 작은 문제는 한 번 풀고 나면 더 이상 풀지 않아도 된다는 말. 그 문제는 구할 때마다 정답이 같다.

- 피보나치를 예로 들면, F(N) = F(N - 1) + F(N - 2). F(2)는 한 번 구하고 나면 더 이상 안 풀어도 된다. 이걸 배열에 메모해둔다! (memoization)

- 완전 탐색을 하면 작은 문제들을 계속해서 다시 풀지만, memoization을 하면 안 그래도 된다. 시간 복잡도가 2의 N제곱에서 O(N)으로 감소하는것!!

- 재귀를 활용한 Top Down 방식과 반복문을 활용한 Bottom Up 방식이 있다

  ```c++
  // top down
  int memo[100];
  int fibo(int n) {
    if (n <= 1) return n;
    if (memo[n] > 0) return memo[n]; // 다시 계산하지 않는다
    memo[n] = fibo(n - 1) + fibo(n - 2);
    return memo[n];
  }
  
  // bottom up
  int memo[100];
  int fibo(int n) {
    memo[0] = memo[1] = 1;
    for (int i = 2; i <= n; i++) {
      memo[i] = memo[i - 1] + memo[i - 2];
    }
    return memo[n];
  }
  ```

2. 어떻게 풀 것인가

- 결국 점화식을 구하는 것이 핵심
- 문제를 더 이상 나눌 수 없는 한 단계와 나머지로 나누는 것이 문제풀이의 핵심
- 그걸 하고 나면 점화식을 구할 수 있다

- 예를 들어, 1 2 3 더하기 문제

- 더 이상 나눌 수 없는 한 단계는 +1을 하거나 +2를 하거나 +3을 하거나 하는 한 단계

- D[N]을 1, 2, 3의 합으로 N을 만드는 경우의 수라고 한다면

- N-1까지에다가 +1, N-2까지에다가 +2, N-3까지에다가 +3을 하면 D[N]이 된다

- D[N] = D[N - 1] + D[N - 2] + D[N - 3]이 되는 것!

  ```c++
  memo[1] = 1;
  memo[2] = 2;
  memo[3] = 4;
  for (int i = 4; i <= 11; i++) {
    memo[i] = memo[i - 1] + memo[i - 2] + memo[i - 3];
  }
  ```
