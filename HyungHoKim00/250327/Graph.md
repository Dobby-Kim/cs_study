## 그래프
정점과 간선의 집합

### 용어
- 방향 그래프: 간선을 통해 한 방향으로만 갈 수 있는 그래프   
- 무방향 그래프: 간선을 통해 양방향으로 갈 수 있는 그래프   
- 가중치 그래프: 간선에 가중치가 주어지는 그래프
- 희소 그래프(Sparse): 간선이 적은 그래프
- 밀집 그래프(Dense): 간선이 많은 그래프
- 완전 그래프(Complete): 모든 정점이 연결된 그래프
- 정점의 차수(Degree): 각 정점에 연결된 간선의 개수   
- 사이클: 특정 정점에서 출발하여 같은 정점으로 도달하는 것   

### 구현
1. 인접 리스트   
각각의 정점에 인접한 정점들을 리스트로 표현한다.   
간선 접근 시간복잡도가 O(V)지만, 공간복잡도가 O(V + E)이다.   
희소 그래프를 표현할 때 유리하다.   
```java
public class Graph {
    LinkedList<Integer>[] graph;
    
    void add(int from, int to) {
        graph[from].add(to);
    }
    
    void remove(int from, int to) {
        graph[from].remove((Integer) to);
    }
    
    boolean containsEdge(int from, int to) {
        return graph[from].contains(to);
    }
}
```

2. 인접 행렬   
각각의 정점에 인접한 정점들을 행렬로 표현한다.   
간선 접근 시간복잡도가 O(1)이지만, 공간복잡도가 O(V^2)이다.   
밀집 그래프를 표현할 때 유리하다.   
```java
public class Graph {
    boolean[][] graph;

    void add(int from, int to) {
        graph[from][to] = true;
    }
    
    void remove(int from, int to) {
        graph[from][to] = false;
    }

    boolean containsEdge(int from, int to) {
        return graph[from][to];
    }
}
```

### 탐색   
1. 깊이 우선 탐색(DFS)   
정점에 연결된 임의의 한 간선의 방향부터 완벽하게 탐색한 후, 다음 간선을 탐색하는 방법.   
stack 혹은 재귀로 구현할 수 있다.   
```java
public class Graph {
    ArrayList<Integer>[] graph;
    boolean[] visited;
    
    public void dfs(int index) {
        visited[index] = true;
        for(int i : graph[index]) {
            if(!visited[i]) {
                dfs(i);
            }
        }
    }
}
```

2. 너비 우선 탐색   
정점에 연결된 모든 간선을 탐색하고, 간선에 연결된 정점을 같은 방식으로 탐색하는 방법.   
queue로 구현할 수 있다.
```java
public class Graph {
    ArrayList<Integer>[] graph;
    boolean[] visited;

    public void bfs(int index) {
        Queue<Integer> queue = new LinkedList<>();
        visited[index] = true;
        queue.offer(index);
        while (!queue.isEmpty()) {
            int node = queue.poll();
            for(int i : graph[node]) {
                if(!visited[i]) {
                    visited[i] = true;
                    queue.offer(i);
                }
            }
        }
    }
}
```

두 알고리즘 모두 O(V + E)의 시간 복잡도를 가진다.   

## 신장 트리
무방향 그래프 중 n-1개의 간선만 선택하여 만든 트리.   
모든 정점들이 연결되어야 하고, 사이클을 포함하지 않는다.   
DFS, BFS를 응용하여 만들 수 있다.

### 최소 신장 트리(Minimum Spanning Tree)
간선들의 가중치가 동이랗지 않을 때, 가중치 비용이 최소가 되는 신장 트리

### 최소 신장 트리 구현
1. 크루스칼 알고리즘
그래프의 간선들을 가중치 오름차순으로 정렬한 후, 사이클을 형성하지 않는 간선을 차례로 선택하는 방법.
시간복잡도: O(ElogE)
```java
public class Graph {
    int n;
    ArrayList<Edge>[] graph;
    ArrayList<Edge>[] mst;
    int[] union;

    public void kruskal() {
        ArrayList<Edge> edges = new ArrayList<>();
        for(ArrayList<Edge> list : graph) {
            for(Edge edge : list) {
                if(edge.from < edge.to) {
                    edges.add(edge);
                }
            }
        }
        edges.sort(Comparator.comparingInt(e -> e.value));
        for(int i = 0; i < n; i++) {
            union[i] = i;
        }
        int count = 0;
        for(Edge edge : edges) {
            int a = find(edge.from);
            int b = find(edge.to);
            if(a == b) {
                continue;
            }
            union(a, b);
            mst[edge.from].add(edge);
            mst[edge.to].add(edge.reverse());
            count++;
            if(count == n - 1) {
                break;
            }
        }
    }
    
    void union(int a, int b) {
        union[b] = a;
    }
    
    int find(int a) {
        if(union[a] == a) {
            return a;
        }
        return union[a] = find(union[a]);
    }
    

    static class Edge {
        int from;
        int to;
        int value;
        
        public Edge(int from, int to, int value) {
            this.from = from;
            this.to = to;
            this.value = value;
        }
        
        Edge reverse() {
            return new Edge(to, from, value);
        }
    }
}
```

2. 프림 알고리즘
임의의 정점에서 시작하여, 최소 가중치 간선을 선택하고 확장하는 방법.
시간복잡도: O(ElogV)
```java
public class Graph {
    int n;
    ArrayList<Edge>[] graph;
    ArrayList<Edge>[] mst;

    public void prim(int v) {
        boolean[] visited = new boolean[n];
        PriorityQueue<Edge> pq = new PriorityQueue<>(Comparator.comparingInt(e -> e.value));
        pq.addAll(graph[v]);
        visited[v] = true;
        int count = 0;
        while (!pq.isEmpty()) {
            Edge edge = pq.poll();
            if(visited[edge.to]) {
                continue;
            }
            visited[edge.to] = true;
            pq.addAll(graph[edge.to]);
            mst[edge.from].add(edge);
            mst[edge.to].add(edge.reverse());
            count++;
            if(count == n - 1) {
                break;
            }
        }
    }

    static class Edge {
        int from;
        int to;
        int value;

        public Edge(int from, int to, int value) {
            this.from = from;
            this.to = to;
            this.value = value;
        }

        Edge reverse() {
            return new Edge(to, from, value);
        }
    }
}
```
