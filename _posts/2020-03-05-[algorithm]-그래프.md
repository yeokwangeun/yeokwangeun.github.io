## 그래프

1. 그래프를 저장하는 방법
   - 인접 행렬, 인접 리스트, 간선 리스트가 있다
   - 한 정점에 연결된 모든 간선을 효율적으로 찾을 수 있는 구조가 좋은 구조
   - 인접 행렬은 모든 정점을 체크, 인접 리스트는 연결된 간선을 체크
   - 인접 행렬은 O(V), 인접 리스트는 O(degree). 인접 리스트가 더 좋다
   - 공간 복잡도도 인접 행렬은 O(V제곱), 인접 리스트는 O(E)
   - 링크드 리스트를 구현해도 좋고, vector를 활용하면 된다.

2. 그래프의 탐색

   - BFS와 DFS가 있다

3. 이분 그래프: A와 B가 서로 연결된 그래프. A끼리는, B끼리는 연결되지 않음. 예를 들면 수강신청. 학생과 과목이 연결. 학생끼리, 혹은 과목끼리는 연결 안됨. BFS/DFS를 이용해서 정점이 이동할 때마다 소속을 바꿔준다. 탐색이 끝나고 나서 연결된 정점끼리 체크하면서 이분 그래프 여부 확인할 수 있다. (BOJ 1707번)

   

## BFS

- 가중치가 1일때 최단 거리를 구하는 알고리즘
- 가중치의 의미와 구하고자 하는 최소 비용의 의미가 일치해야 한다.
- 정점을 나누는 개념. (BOJ 이모티콘 14226). 클립보드에 저장된 것에 따라 같은 이모티콘 수라도 다른 정점으로 봐야 한다.

```c++
int bfs(int s) {
    int visit[1002][1002];
    for (int i = 0; i <= 1000; i++) {
        for (int j = 0; j <= 1000; j++) {
            visit[i][j] = -1;
        }
    }

    struct node { int emo; int clip; };
    std::queue<node> q;
    q.push(node { 1, 0 });
    visit[1][0] = 0;

    while (!q.empty()) {
        node out = q.front();
        if (out.emo == s) return visit[out.emo][out.clip];
        q.pop();
        // - operation
        if (out.emo > 1) {
            node next = out;
            next.emo--;
            if (visit[next.emo][next.clip] == -1) {
                visit[next.emo][next.clip] = visit[out.emo][out.clip] + 1;
                q.push(next);
            }
        }
        // copy operation
        if (out.emo != out.clip) {
            node next { out.emo, out.emo };
            if (visit[next.emo][next.clip] == -1) {
                visit[next.emo][next.clip] = visit[out.emo][out.clip] + 1;
                q.push(next);
            }
        }
        // paste operation
        node next { out.emo + out.clip, out.clip };
        if (next.emo <= 1000 && visit[next.emo][next.clip] == -1) {
            visit[next.emo][next.clip] = visit[out.emo][out.clip] + 1;
            q.push(next);
        }
    }

    return 0;
}
```

- 가중치가 0 또는 1일 때도 활용 가능하다
- 이 때는 큐를 두 개 사용해서 구현한다. q에 가중치가 0인 애들을 먼저 담고, next_q에는 가중치가 1인 애들을 담고. 덱으로도 구현할 수 있다. (BOJ 숨바꼭질3 13549)

```c++
int bfs(int subin, int sister) {
    std::queue<int> q;
    std::queue<int> next_q;

    q.push(subin);
    visit[subin] = 1;

    while (!q.empty()) {
        int out = q.front();
        if (out == sister) return visit[sister] - 1;
        q.pop();
        // *2 move
        if (out >= 1 && out * 2 <= MAX) {
            int next = out * 2;
            if (visit[next] == 0) {
                visit[next] = visit[out];
                q.push(next);
            }
        }
        // +-1 move
        for (int d = -1; d <= 1; d += 2) {
            int next = out + d;
            if (next < 0 || next > MAX) continue;
            if (visit[next] == 0) {
                visit[next] = visit[out] + 1;
                next_q.push(next);
            }
        }
        // q change
        if (q.empty()) {
            q = next_q;
            next_q = std::queue<int>();
        }
    }

    return 0;
}
```

