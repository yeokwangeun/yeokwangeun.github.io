## Tree

#### 트리의 정의

- 트리도 그래프다
- 사이클이 없는 연결 그래프를 트리라고 한다 (정의)
- 정점이 V개 있으면, 간선은 V-1개
- 이 정의를 잘 생각해보면, 임의의 두 정점 사이의 경로는 하나만 존재한다

#### 용어

- 루트: 더 이상 부모가 없는 정점. 이건 정하기 나름임.
- 이진 트리: 각 정점에서, 자식이 최대 2개인 트리
- 포화 이진 트리: 리프 노드(터미널 노드)를 제외하고 모두 자식이 2개. 리프 노드의 레벨이 다 같은 트리
- 완전 이진 트리: 포화이진트리의 일종. 터미널 노드에서, 오른쪽부터 몇 개가 없을 수 있는 트리.

#### 트리 구현

- 그냥 그래프 하듯이 vector로 해도 되고
- 이진트리라면 left, right를 멤버로 가지는 구조체로 구현해도 되고
- 트리의 부모만 배열에 저장할 수도 있다 (union find 알고리즘에서 활용). 
- 완전이진트리라면 부모를 x, left를 2x, right를 2x+1로 배열에 저장할 수도 있다 (heap, segment tree에서 활용)

#### 트리 탐색

- 트리도 그래프다
- bfs, dfs를 쓰면 된다
- 임의의 두 점 사이에 존재하는 경로는 단 하나기 때문에, dfs로 탐색해도 최단 거리를 구할 수 있다

#### 트리 순회

- preorder, inorder, postorder가 있다
- "나"를 언제 방문할 것이냐를 기준으로 나눈 것
- preorder는 나부터 그리고 왼오. postorder는 왼오 그리고 나. inorder는 거의 안 쓰이는데 왼 나 오
- 일반적으로 우리가 했던 dfs가 preorder. postorder는 자식의 처리 결과를 나에 반영할 때 사용되기 때문에 중요.

```c++
void preorder(int curr) {
    if (curr == -1) return;
    printf("%c", curr + 'A');
    preorder(node[curr].left);
    preorder(node[curr].right);
}

void inorder(int curr) {
    if (curr == -1) return;
    inorder(node[curr].left);
    printf("%c", curr + 'A');
    inorder(node[curr].right);
}

void postorder(int curr) {
    if (curr == -1) return;
    postorder(node[curr].left);
    postorder(node[curr].right);
    printf("%c", curr + 'A');
}
```

#### 트리의 지름

- 트리에 존재하는 모든 경로 중 가장 긴 경로의 길이
- bfs/dfs를 두 번 하면 구할 수 있다
- 임의의 점에서 시작, 가장 먼 점을 x. 그 x에서 시작 가장 먼 점을 y. x와 y 사이의 거리가 지름이다
- 포스트 오더를 활용해서도 구할 수 있다
