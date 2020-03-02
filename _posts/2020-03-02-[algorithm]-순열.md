## 순열

1. 다음순열, 이전순열 구하기

   - 1~7까지의 수를 이용해 만들 수 있는 모든 순열에 대해서
   - 1 2 3 4 5 6 7은 처음 순열. 오름차순이다
   - 7 6 5 4 3 2 1은 마지막 순열. 내림차순이다
   - 7 4 3 6 5 3 1은 7 4 3으로 시작하는 마지막 순열이다. 그 뒤가 내림차순이기 때문
   - 7 5 2 6 1 3 4는 7 5 2 6으로 시작하는 첫 순열이다. 그 뒤가 오름차순이기 때문
   - 다음순열을 구하기 위해서는, 먼저 그 수열이 무엇으로 시작하는 마지막 순열인지 구하고, 그 다음으로 시작하는 첫 순열을 구하면 된다

   ```c++
   // 다음 순열 구하기
   // step1. 뭐로 시작하는 마지막 순열이니?
   // 끝에서부터 시작해서 내림차순이 끝나는 순간을 본다
   // step2. 743으로 시작한다면 3을 뭐로 바꾸면 될까?
   // 뒤의 수 중 3보다 큰 수 중 가장 작은 수
   // step3. 그 후에는?
   // 뒤는 여전히 내림차순일 것. 그러면 이제 마지막->첫이 돼야 하니까 그대로 순서를 바꿔준다
   
   #include <stdio.h>
   
   void swap(int &a, int &b) {
       int tmp = a;
       a = b;
       b = tmp;
   }
   
   bool next_p(int n, int *arr) {
       // step 1: arr은 arr[i - 1]로 시작하는 마지막 순열
       int i;
       for (i = n - 1; i > 0; i--) {
           if (arr[i - 1] <= arr[i]) break;
       }
       if (i == 0) return false;
   
       // step 2: arr[i - 1]을 뒤에 있는 수 중 하나랑 바꾸
       int j;
       for (j = n - 1; i <= j; j--) {
           if (arr[j] > arr[i - 1]) break;
       }
       swap(arr[i - 1], arr[j]);
   
       // step 3: arr[i]부터 끝까지 뒤집
       j = n - 1;
       while (i < j) {
           swap(arr[i], arr[j]);
           i++;
           j--;
       }
   
       return true;
   }
   ```

   ```c++
   // 이전 순열 구하기
   #include <stdio.h>
   
   void swap(int &a, int &b) { int tmp = a; a = b; b = tmp; }
   bool prev_p(int n, int *arr) {
       // step1. 무엇으로 시작하는 첫 순열?
       int i;
       for (i = n - 1; i > 0; i--) {
           if (arr[i - 1] >= arr[i]) break;
       }
       if (i == 0) return false;
   
       // step2. arr[i - 1]을 무엇과 바꿀까?
       int j;
       for (j = n - 1; i <= j; j--) {
           if (arr[i - 1] > arr[j]) break;
       }
       swap(arr[i - 1], arr[j]);
   
       // step3. arr[i - 1]로 시작하는 마지막 순열 만들기
       j = n - 1;
       while (i < j) {
           swap(arr[i], arr[j]);
           i++;
           j--;
       }
   
       return true;
   }
   ```

2. 모든 순열 구하기: do while을 활용한다

   ```c++
   int main() {
       int N;
       scanf("%d", &N);
       int arr[10];
       for (int i = 0; i < N; i++)
           arr[i] = i + 1;
   
       do {
           for (int i = 0; i < N; i++)
               printf("%d ", arr[i]);
           printf("\n");
       } while (next_p(N, arr) == true);
   
       return 0;
   }
   ```

   
