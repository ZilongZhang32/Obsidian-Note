ff## Introduction
圖（Graph）是一種非線性資料結構，由頂點（Vertex）和邊（Edge）組成。我們可以將圖 $G$ 抽象地表示為一組頂點 $V$ 和一組邊 $E$ 的集合。以下示例展示了一個包含 5 個頂點和 7 條邊的圖：
$$\displaylines{\begin{aligned} V &= \{1,2,3,4,5\} \\ E &= \{(1,2),(1,3),(1,5),(2,3),(2,4),(2,5),(4,5)\} \\ G &= \{V,E\} \end{aligned}} $$
如果將頂點看作節點，將邊看作連線各個節點的引用（指標），我們就可以將圖看作一種從鏈結串列拓展而來的資料結構。如圖 9-1 所示，**相較於線性關係（鏈結串列）和分治關係（樹），網路關係（圖）的自由度更高**，因而更為複雜。

```md
(1)--(5)--(4)
 |\   |   /
 | \  |  /
 |  \ | /
 |   \|/
(3)--(2)
```

## Types and Terminology
### Types of graphs
圖的種類可以依據分類方式分成以下類型：
- 根據邊是否具有方向
	1. 無向圖（Undirected Graph）
		邊表示兩頂點之間的「雙向」連線關係，例如社交軟體 LINE、Discord 中的「好友」。
	2. 有向圖（Directed Graph）
		邊具有方向性，即 $A→B$ 和 $A←B$ 兩個方向的邊是相互獨立的，例如社交軟體 Instagram 的「關注」與「被關注」關係。

- 根據所有頂點是否連通
	1. 連通圖（Connected Graph）
		從某個頂點出發，可以到達其餘任意頂點。
	2. 非連通圖（Disconneted Graph）
		對於非連通圖，從某個頂點出發，至少有一個頂點無法到達。

```md
Undirected       Directed         Connected        Disconnected
Graph            Graph            Graph            Graph
(1)--(5)  (4)    (1)<-(5)  (4)    (1)--(5)--(4)    (1)--(5)  (4)
 |\   |   /       ^^   ^   ^       |\   |   /       |\   |
 | \  |  /        | \  |  /        | \  |  /        | \  |
 |  \ | /         |  \ | /         |  \ | /         |  \ |
 |   \|/          v   \|/          |   \|/          |   \|
(3)--(2)         (3)->(2)         (3)--(2)         (3)--(2)
```

### Terminology
- 鄰接（Adjacency）
	當兩頂點之間存在邊相連時，稱這兩頂點「鄰接」。在上圖中，頂點 1 的鄰接頂點為頂點 2、3、5。
- 路徑（Path）
	從頂點 $A$ 到頂點 $B$ 經過的邊構成的序列被稱為從 $A$ 到 $B$ 的「路徑」。在上圖中，邊序列 1-5-2-4 是頂點 1 到頂點 4 的一條路徑。
- 度（Degree）
	對於無向圖：一個頂點擁有的邊數。
	對於有向圖：
		入度（In-degree）表示有多少條邊指向該頂點。
		出度（Out-degree）表示有多少條邊從該頂點指出。

## Representation
### Adjacency matrix
設圖的頂點數量為 $n$ ，鄰接矩陣（Adjacency Matrix）使用一個 $n×n$ 大小的矩陣來表示圖，每一行（列）代表一個頂點，矩陣元素代表邊，用 $1$ 或 $0$ 表示兩個頂點之間是否存在邊。

設鄰接矩陣為 $M$、頂點串列為 $V$ ，那麼矩陣元素 $M[i,j]=1$ 表示頂點 $V[i]$ 到頂點 $V[j]$ 之間存在邊，反之 $M[i,j]=0$ 表示兩頂點之間無邊。
為了使排版更加簡潔，以下鄰接矩陣將 $0$ 省略不寫。

| 頂點串列 | (1) | (3) | (2) | (5) | (4) |
| :--: | :-: | :-: | :-: | :-: | :-: |
|  索引  | `0` | `1` | `2` | `3` | `4` |
| `0`  |     | $1$ | $1$ | $1$ |     |
| `1`  | $1$ |     | $1$ |     |     |
| `2`  | $1$ | $1$ |     | $1$ | $1$ |
| `3`  | $1$ |     | $1$ |     | $1$ |
| `4`  |     |     | $1$ | $1$ |     |

鄰接矩陣具有以下特性：
- 在簡單圖中，頂點不能與自身相連，此時鄰接矩陣主對角線元素沒有意義。
- 對於無向圖，兩個方向的邊等價，此時鄰接矩陣關於主對角線對稱。
- 將鄰接矩陣的元素從 $1$ 和 $0$ 替換為權重，則可表示有權圖（Weighted Graph）。

使用鄰接矩陣表示圖時，我們可以直接訪問矩陣元素以獲取邊，因此增刪查改操作的效率很高，時間複雜度均為 $O(1)$ 。然而，矩陣的空間複雜度為 $O(n^2)$ ，記憶體佔用較多。

### Adjacency list
鄰接表（Adjacency List）使用 $n$ 個鏈結串列來表示圖，鏈結串列節點表示頂點。第 $i$ 個鏈結串列對應頂點 $i$ ，其中儲存了該頂點的所有鄰接頂點（與該頂點相連的頂點）。

```md
(V)|->Its adjacent vertices
----------------------------
(1)|->(2)->(3)->(5)
(3)|->(1)->(2)
(2)|->(1)->(3)->(5)->(4)
(5)|->(1)->(2)->(4)
(4)|->(2)->(5)
```

接表僅儲存實際存在的邊，而邊的總數通常遠小於 $n^2$，因此它更加節省空間。然而，在鄰接表中需要透過走訪鏈結串列來查詢邊，因此其時間效率不如鄰接矩陣。

觀察上圖，**鄰接表結構與雜湊表中的「鏈式位址」非常相似，因此我們也可以採用類似的方法來最佳化效率**。比如當鏈結串列較長時，可以將鏈結串列轉化為 [[AVL Tree|AVL 樹]]或[[Red-Black Tree|紅黑樹]]，從而將時間效率從 $O(n)$ 最佳化至 $O(\log ⁡n)$ ；還可以把鏈結串列轉換為雜湊表，從而將時間複雜度降至 $O(1)$ 。

## Typical Applications
許多現實系統可以用圖來建模，相應的問題也可以約化為圖計算問題。以下表格舉出了一些範例：

|      | 頂點  | 邊          | 圖計算問題  |
| ---- | --- | ---------- | ------ |
| 社交網路 | 使用者 | 好友關係       | 好友推薦   |
| 地鐵網路 | 站點  | 站點間的連通性    | 最短路徑計算 |
| 太陽系  | 星體  | 星體間的萬有引力作用 | 行星軌道計算 |

## Common Operations
圖的基礎操作可分為對「邊」的操作和對「頂點」的操作。在「鄰接矩陣」和「鄰接表」兩種表示方法下，實現方式也有所不同。
### Adjacency-matrix based
給定一個頂點數量為 $n$ 的無向圖，則：
- **新增或刪除邊**
	直接在鄰接矩陣中修改指定的邊即可，使用 $O(1)$ 時間。而由於是無向圖，因此需要同時更新兩個方向的邊。
- **新增頂點**
	在鄰接矩陣的尾部新增一行一列，並全部填 $0$ 即可，使用 $O(n)$ 時間。
- **刪除頂點**
	在鄰接矩陣中刪除一行一列。當刪除首行首列時達到最差情況，需要將 $(n−1)^2$ 個元素「向左上移動」，從而使用 $O(n^2)$ 時間。
- **初始化**：傳入 $n$ 個頂點，初始化長度為 $n$ 的頂點串列 `vertices` ，使用 $O(n)$ 時間；初始化 $n×n$ 大小的鄰接矩陣 `adjMat` ，使用 $O(n^2)$ 時間。

### Adjacency-list based
實作時，可以考慮使用雜湊表，或是串列，來替代鏈結串列。
設無向圖的頂點總數為 $n$、邊總數為 $m$，則：
- **新增邊**
	在頂點對應鏈結串列的末尾新增邊即可，使用 $O(1)$ 時間。因為是無向圖，所以需要同時新增兩個方向的邊。
- **刪除邊**
	在頂點對應鏈結串列中查詢並刪除指定邊，使用 $O(m)$ 時間。在無向圖中，需要同時刪除兩個方向的邊。
- **新增頂點**
	在鄰接表中新增一個鏈結串列，並將新增頂點作為鏈結串列頭節點，使用 $O(1)$ 時間。
- **刪除頂點**
	需走訪整個鄰接表，刪除包含指定頂點的所有邊，使用 $O(n+m)$ 時間。
- **初始化**
	在鄰接表中建立 $n$ 個頂點和 $2m$ 條邊，使用 $O(n+m)$ 時間。

### Comparison
似乎鄰接表（雜湊表）的時間效率與空間效率最優。但實際上，在鄰接矩陣中操作邊的效率更高，只需一次陣列訪問或賦值操作即可。綜合來看，鄰接矩陣體現了「以空間換時間」的原則，而鄰接表體現了「以時間換空間」的原則。要使用哪種表示法，

## Traversal
樹代表的是「一對多」的關係，而圖則具有更高的自由度，可以表示任意的「多對多」關係。因此，我們可以把樹看作圖的一種特例。顯然，**樹的走訪操作也是圖的走訪操作的一種特例**。

圖和樹都需要應用搜索演算法來實現走訪操作。圖的走訪方式也可分為兩種：廣度優先走訪和深度優先走訪。

### Breadth First Search
**廣度優先走訪（Breadth First Search, BFS）是一種由近及遠的走訪方式**，從某個節點出發，始終優先訪問距離最近的頂點，並一層層向外擴張。
#### Implementation
BFS 通常藉助佇列來實現，程式碼如下所示。[[Queue|佇列]]具有「先入先出」的性質，這與 BFS 的「由近及遠」的思想異曲同工。
1. 將走訪起始頂點 `startVet` 加入列列，並開啟迴圈。
2. 在迴圈的每輪迭代中，彈出佇列首頂點並記錄訪問，然後將該頂點的所有鄰接頂點加入到佇列尾部。
3. 迴圈步驟 `2.` ，直到所有頂點被訪問完畢後結束。

為了防止重複走訪頂點，我們需要藉助一個雜湊集合 `visited` 來記錄哪些節點已被訪問。

> [!NOTE] 
> 雜湊集合可以看作一個只儲存 `key` 而不儲存 `value` 的雜湊表，它可以在 $O(1)$ 時間複雜度下進行 `key` 的增刪查改操作。根據 `key` 的唯一性，雜湊集合通常用於資料去重等場景。

~~~tabs
tab: C
```c
/* 節點佇列結構體 */
typedef struct {
    Vertex *vertices[MAX_SIZE];
    int front, rear, size;
} Queue;

/* 建構子 */
Queue *newQueue() {
    Queue *q = (Queue *)malloc(sizeof(Queue));
    q->front = q->rear = q->size = 0;
    return q;
}

/* 判斷佇列是否為空 */
int isEmpty(Queue *q) {
    return q->size == 0;
}

/* 入列操作 */
void enqueue(Queue *q, Vertex *vet) {
    q->vertices[q->rear] = vet;
    q->rear = (q->rear + 1) % MAX_SIZE;
    q->size++;
}

/* 出列操作 */
Vertex *dequeue(Queue *q) {
    Vertex *vet = q->vertices[q->front];
    q->front = (q->front + 1) % MAX_SIZE;
    q->size--;
    return vet;
}

/* 檢查頂點是否已被訪問 */
int isVisited(Vertex **visited, int size, Vertex *vet) {
    // 走訪查詢節點，使用 O(n) 時間
    for (int i = 0; i < size; i++) {
        if (visited[i] == vet)
            return 1;
    }
    return 0;
}

/* 廣度優先走訪 */
// 使用鄰接表來表示圖，以便獲取指定頂點的所有鄰接頂點
void graphBFS(GraphAdjList *graph, Vertex *startVet, Vertex **res, int *resSize, Vertex **visited, int *visitedSize) {
    // 佇列用於實現 BFS
    Queue *queue = newQueue();
    enqueue(queue, startVet);
    visited[(*visitedSize)++] = startVet;
    // 以頂點 vet 為起點，迴圈直至訪問完所有頂點
    while (!isEmpty(queue)) {
        Vertex *vet = dequeue(queue); // 佇列首頂點出隊
        res[(*resSize)++] = vet;      // 記錄訪問頂點
        // 走訪該頂點的所有鄰接頂點
        AdjListNode *node = findNode(graph, vet);
        while (node != NULL) {
            // 跳過已被訪問的頂點
            if (!isVisited(visited, *visitedSize, node->vertex)) {
                enqueue(queue, node->vertex);             // 只入列未訪問的頂點
                visited[(*visitedSize)++] = node->vertex; // 標記該頂點已被訪問
            }
            node = node->next;
        }
    }
    // 釋放記憶體
    free(queue);
}
```

tab: C++
```cpp
/* 廣度優先走訪 */
// 使用鄰接表來表示圖，以便獲取指定頂點的所有鄰接頂點
vector<Vertex *> graphBFS(GraphAdjList &graph, Vertex *startVet) {
    // 頂點走訪序列
    vector<Vertex *> res;
    // 雜湊集合，用於記錄已被訪問過的頂點
    unordered_set<Vertex *> visited = {startVet};
    // 佇列用於實現 BFS
    queue<Vertex *> que;
    que.push(startVet);
    // 以頂點 vet 為起點，迴圈直至訪問完所有頂點
    while (!que.empty()) {
        Vertex *vet = que.front();
        que.pop();          // 佇列首頂點出隊
        res.push_back(vet); // 記錄訪問頂點
        // 走訪該頂點的所有鄰接頂點
        for (auto adjVet : graph.adjList[vet]) {
            if (visited.count(adjVet))
                continue;            // 跳過已被訪問的頂點
            que.push(adjVet);        // 只入列未訪問的頂點
            visited.emplace(adjVet); // 標記該頂點已被訪問
        }
    }
    // 返回頂點走訪序列
    return res;
}
```

tab: Python
```python
def graph_bfs(graph: GraphAdjList, start_vet: Vertex) -> list[Vertex]:
    """廣度優先走訪"""
    # 使用鄰接表來表示圖，以便獲取指定頂點的所有鄰接頂點
    # 頂點走訪序列
    res = []
    # 雜湊集合，用於記錄已被訪問過的頂點
    visited = set[Vertex]([start_vet])
    # 佇列用於實現 BFS
    que = deque[Vertex]([start_vet])
    # 以頂點 vet 為起點，迴圈直至訪問完所有頂點
    while len(que) > 0:
        vet = que.popleft()  # 佇列首頂點出隊
        res.append(vet)  # 記錄訪問頂點
        # 走訪該頂點的所有鄰接頂點
        for adj_vet in graph.adj_list[vet]:
            if adj_vet in visited:
                continue  # 跳過已被訪問的頂點
            que.append(adj_vet)  # 只入列未訪問的頂點
            visited.add(adj_vet)  # 標記該頂點已被訪問
    # 返回頂點走訪序列
    return res
```
~~~

> [!QUESTION]- 廣度優先走訪的序列是否唯一？
> 不唯一。廣度優先走訪只要求按「由近及遠」的順序走訪，**而多個相同距離的頂點的走訪順序允許被任意打亂**。

#### Complexity analysis
##### Time complexity
所有頂點都會入列並出隊一次，使用 $O(|V|)$ 時間；在走訪鄰接頂點的過程中，由於是無向圖，因此所有邊都會被訪問 $2$ 次，使用 $O(2|E|)$ 時間；總體使用 $O(|V|+|E|)$ 時間。
##### Space complexity
串列 `res` ，雜湊集合 `visited` ，佇列 `que` 中的頂點數量最多為 $|V|$ ，使用 $O(|V|)$ 空間。

### Depth First Search
深度優先走訪是一種**優先走到底、無路可走再回頭的走訪方式**。
#### Implementation
這種「走到盡頭再返回」的演算法範式通常基於遞迴來實現。與廣度優先走訪類似，在深度優先走訪中，我們也需要藉助一個雜湊集合 `visited` 來記錄已被訪問的頂點，以避免重複訪問頂點。

~~~tabs
tab: C
```c
/* 檢查頂點是否已被訪問 */
int isVisited(Vertex **res, int size, Vertex *vet) {
    // 走訪查詢節點，使用 O(n) 時間
    for (int i = 0; i < size; i++) {
        if (res[i] == vet) {
            return 1;
        }
    }
    return 0;
}

/* 深度優先走訪輔助函式 */
void dfs(GraphAdjList *graph, Vertex **res, int *resSize, Vertex *vet) {
    // 記錄訪問頂點
    res[(*resSize)++] = vet;
    // 走訪該頂點的所有鄰接頂點
    AdjListNode *node = findNode(graph, vet);
    while (node != NULL) {
        // 跳過已被訪問的頂點
        if (!isVisited(res, *resSize, node->vertex)) {
            // 遞迴訪問鄰接頂點
            dfs(graph, res, resSize, node->vertex);
        }
        node = node->next;
    }
}

/* 深度優先走訪 */
// 使用鄰接表來表示圖，以便獲取指定頂點的所有鄰接頂點
void graphDFS(GraphAdjList *graph, Vertex *startVet, Vertex **res, int *resSize) {
    dfs(graph, res, resSize, startVet);
}
```

tab: C++
```cpp
/* 深度優先走訪輔助函式 */
void dfs(GraphAdjList &graph, unordered_set<Vertex *> &visited, vector<Vertex *> &res, Vertex *vet) {
    res.push_back(vet);   // 記錄訪問頂點
    visited.emplace(vet); // 標記該頂點已被訪問
    // 走訪該頂點的所有鄰接頂點
    for (Vertex *adjVet : graph.adjList[vet]) {
        if (visited.count(adjVet))
            continue; // 跳過已被訪問的頂點
        // 遞迴訪問鄰接頂點
        dfs(graph, visited, res, adjVet);
    }
}

/* 深度優先走訪 */
// 使用鄰接表來表示圖，以便獲取指定頂點的所有鄰接頂點
vector<Vertex *> graphDFS(GraphAdjList &graph, Vertex *startVet) {
    // 頂點走訪序列
    vector<Vertex *> res;
    // 雜湊集合，用於記錄已被訪問過的頂點
    unordered_set<Vertex *> visited;
    dfs(graph, visited, res, startVet);
    return res;
}
```

tab: Python
```python
def dfs(graph: GraphAdjList, visited: set[Vertex], res: list[Vertex], vet: Vertex):
    """深度優先走訪輔助函式"""
    res.append(vet)  # 記錄訪問頂點
    visited.add(vet)  # 標記該頂點已被訪問
    # 走訪該頂點的所有鄰接頂點
    for adjVet in graph.adj_list[vet]:
        if adjVet in visited:
            continue  # 跳過已被訪問的頂點
        # 遞迴訪問鄰接頂點
        dfs(graph, visited, res, adjVet)

def graph_dfs(graph: GraphAdjList, start_vet: Vertex) -> list[Vertex]:
    """深度優先走訪"""
    # 使用鄰接表來表示圖，以便獲取指定頂點的所有鄰接頂點
    # 頂點走訪序列
    res = []
    # 雜湊集合，用於記錄已被訪問過的頂點
    visited = set[Vertex]()
    dfs(graph, visited, res, start_vet)
    return res
```
~~~

#### Complexity analysis
##### Time complexity
所有頂點都會被訪問 $1$ 次，使用 $O(|V|)$ 時間；所有邊都會被訪問 $2$ 次，使用 $O(2|E|)$ 時間；總體使用 $O(|V|+|E|)$ 時間。
##### Space complexity
串列 `res` ，雜湊集合 `visited` 頂點數量最多為 $|V|$ ，遞迴深度最大為 $|V|$ ，因此使用 $O(|V|)$ 空間。

## Q & A
> [!QUESTION]- 路徑的定義是頂點序列還是邊序列？
> 維基百科上不同語言版本的定義不一致：英文版是「路徑是一個邊序列」，而中文版是「路徑是一個頂點序列」。以下是英文版原文：
> > In graph theory, a path in a graph is a finite or infinite sequence of edges which joins a sequence of vertices.
> 
> 在一般的數學或計算機科學文獻中，路徑通常指**頂點序列**；在某些正式的定義或特殊情境（如權重圖或流網絡）中，可能會用**邊序列**來描述路徑。無論哪種定義，頂點與邊的關係是相互依存的，因此它們在不同的情境下都可以用來描述路徑。

> [!QUESTION]- 非連通圖中是否會有無法走訪到的點？
> 在非連通圖中，從某個頂點出發，至少有一個頂點無法到達。走訪非連通圖需要設定多個起點，以走訪到圖的所有連通分量。

> [!QUESTION]- 在鄰接表中，「與該頂點相連的所有頂點」的頂點順序是否有要求？
> 可以是任意順序。但在實際應用中，可能需要按照指定規則來排序，比如按照頂點新增的次序，或者按照頂點值大小的順序等，這樣有助於快速查詢「帶有某種極值」的頂點。
