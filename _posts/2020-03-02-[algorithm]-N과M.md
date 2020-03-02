## 브루트포스(N과 M)

1. 순서/순열 문제

   - N개 중 M개를 고르는 문제. 중복을 제거해야 하면 check 배열 사용!
   - 재귀 함수를 만들 때는 깊이가 최대 얼마인지, 그리고 각 단계에서 수행해야 하는 일은 무엇인지를 정해야 한다. 단계마다 기억해서 가져가야 할 것들은 인수로 넣어줘야 한다.

   ```c++
   #include <stdio.h>
   
   int N, M;
   int list[10];
   bool check[10];
   
   void dfs(int L) {
       if (L > M) {
           for (int i = 1; i <= M; i++)
               printf("%d ", list[i]);
           printf("\n");
           return;
       }
   
       for (int i = 1; i <= N; i++) {
           if (check[i]) continue; // 중복 제거
           list[L] = i;
           check[i] = true;
           dfs(L + 1);
           check[i] = false;
       }
   }
   
   int main() {
       scanf("%d %d", &N, &M);
       dfs(1);
   
       return 0;
   }
   ```

2. 조합, 선택 문제

   - O(N!)으로 풀 수도 있고, O(2의N제곱)으로 풀 수도 있다
   - N!로 푸는 것은 뒤를 안 돌아보고 하나씩 선택하는 개념. start_idx를 활용해야 한다
   - 2의N제곱으로 푸는 것은 각 단계마다 선택/비선택하는 개념

   ```c++
   #include <stdio.h>
   
   int N, M;
   int list[10];
   
   void dfs(int L, int selected) {
       if (selected > M) {
           for (int i = 1; i < selected; i++)
               printf("%d ", list[i]);
           printf("\n");
           return;
       }
       if (L > N) return;
   
       list[selected] = L;
       dfs(L + 1, selected + 1);
       list[selected] = 0;
       dfs(L + 1, selected);
   }
   
   int main() {
       scanf("%d %d", &N, &M);
       dfs(1, 1);
   
       return 0;
   }
   ```

   

