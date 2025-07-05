# Data Structure & Algorithm in Python

!!! example
    If G is a simple undirected graph with n vertices and $c(1\leq c \leq n)$ connected components, what is the largest number of edges G might have? Explain how you derive the largest number of edges.

**Answer:**

Suppose the c connected components of G have $k_1, k_2, ..., k_c$ vertices,
respectively. The maximum number of edges is achieved only all components are
fully connected, i.e., each vertice in a component is connected to every other
vertice in the same component (or a component has only 1 single vertice, in which
case there's no edge).

If c = 1, the maximum number of edges is $n(n - 1) / 2$.

If c = n, the maximum number of edges is 0.

Let's consider 1 < c < n. If only 1 component (say component i) has multiple
vertices, whereas all the other (c-1) components have single vertex,
the maximum number of edges is $k_i(k_i - 1) / 2$, where $k_i = n - (c-1) = n-c+1$.

If there are **multiple components with multiple vertices**, suppose component i has
the most number of vertices. Let's move one vertex from another component
(say component j) also with multiple vertices to component i. The number of edges
in component j will reduce by $(k_j-1)$, while the number of edges in component i
will increase by $k_i$. Since by assumption, $k_i > k_j - 1$, **the total number**
**of edges in G will be increased by such a move**. Hence the maximum number of edges
in G will achieved by having only 1 component with multiple vertices, and all
other (c-1) components with only a single vertex. Thus the maximum number of
edges of G is:

$$ k_i(k_i - 1) / 2 = (n-c+1)(n-c) / 2 $$

For example, if n = 7 and c = 4, we can calculate that $k_i = n-c+1 = 4$, and
the maximum number of edges is $e_{max} = 6$.

!!! example
    Given an unsorted array A and a target value B. The task is to find out
    whether or not b is in A. In terms of worst-case time complexity, is it better
    to do a **linear search** to A or first sort A using **insertion sort**
    and then do a **binary search** on the sorted array? Explain your answer.

**Answer:**

Suppose the length of A is n, so linear search takes O(n) operations.

**Insertion sort**: In the worst-case scenario, the array to be sorted is in
reverse order, which means that for each element, you have to compare it with all
the previous elements. So it takes up to $\sum_{i=1}^{n} i = n(n+1) / 2$ operations.
Hence, the insertion sort takes $O(n^2)$ operations.

**Binary search**: Binary search takes $O(log_2 n)$ operations. Hence if we first
sort A using insertion sort and then do a binary search on the sorted array, the
total time complexity is $O(n^2 + log_2 n) = O(n^2)$ (1), which is greater than $O(n)$.
{ .annotate }

1. We can ignore the lower order term $O(log_2 n)$ in this case.

Therefore, **linear search** is better than **insertion sort** +
**binary search** in terms of worst-case time complexity.

!!! info "More Thinking"
    Answer previous question again if you need to perform the search
    for n different b values, where n is the length of array A. What about
    searching for $n^2$ different b values?

**Answer:**

(For convenience, we call the **linear search** method A, and **insertion sort** +
**binary search** method B.)  

**For searching n different b values,**
linear search takes $O(n)$ operations for each b value. We need to do this n
times, so the total number of operations is $O(n^2)$.

Insertion sort takes $O(n^2)$ operations. Binary search takes
$O(log_2 n)$ operations. If we need to do binary search for n times,
the total number of operations is $O(n \cdot log_2 n)$.

Hence if we use method A, the total number of operations is $O(n^2)$;
If we use method B, the total number of operations is
$O(n^2 + n \cdot log_2 n) = O(n^2)$.

Hence method A and method B has the same worst-time complexity in this case.

**For searching $n^2$ different b values,**
Linear search takes $O(n)$ operations for each b value. We need to do this $n^2$
times, so the total number of operations is $O(n^3)$.

Insertion sort takes $O(n^2)$ operations. Binary search takes
$O(log_2 n)$ operations. If we need to do binary search for $n^2$ times,
the total number of operations is $O(n^2 \cdot log_2 n)$.

Hence if we use method A, the total number of operations is $O(n^3)$;
If we use method B, the total number of operations is
$O(n^2 + n^2 \cdot log_2 n) = O(n^2 log_2 n)$.

Hence, in terms of worst-time complexity, method B is better than method A.

**We can conclude from this question that in terms of the time complexity, the**
**performance differs according to different algorithms applied. Intuitively,**
**if more elements need to be searched, we'd better get the array sorted before**
**searching.**

!!! example
    Determine the time complexity and space complexity of the following code snippet:
    ``` Python
    def power(x, n):
        if n == 0:
            return 1
        else:
            temp = power(x, n // 2)
            if n % 2 == 0:    # if n is even:
                return temp * temp
            else:             # if n is odd:
                return temp * temp * x
    ```

Answer:

From the recursive algorithm, we can get the following systems of equation:

$$
  power(x, n) =
  \begin{cases}
  1 & \text{if } n = 0 \\
  x \cdot power(x, \lfloor \frac{n}{2} \rfloor)^{2} & \text{if } n > 0 \: and \: n \: is \: odd \\
  power(x, \lfloor \frac{n}{2} \rfloor)^{2} & \text{if } n > 0 \: and \: n \: is \: even
  \end{cases}
$$

Therefore, the time complexity of the algorithm is $O(log_2 n)$;
the space complexity is also $O(log_2 n)$.

!!! note "知识点"
    **Breath-First Search (BFS) Algorithm**

    ``` python

    def bfs(graph, start):
        visited = [False] * (len(graph) + 1)  # initialize the visited array
        queue = []  # initialize a list as a queue for BFS
        traversal_order = []  # store the order of visited vertices

        # Start BFS from the given vertex
        queue.append(start)
        visited[start] = True

        while queue:
            vertex = queue.pop(0)  # Dequeue the front element
            traversal_order.append(vertex)

            # traverse all the vertices adjacent to vertice
            for neighbour in graph[vertex]:
                if not visited[neighbour]:
                    queue.append(neighbour)
                    visited[neighbour] = True

    return traversal_order
    ```

!!! note "知识点"
    **We can also use BFS algorithm to find the shortest path in a graph:**

    ``` python

    def bfs_shortest_path(graph, start):
        distances = {}  # to store shortest path lengths
        visited = [False]*(len(graph) + 1)
        queue = []

        # Start BFS from the given vertex
        queue.append(start)
        visited[start] = True
        distances[start] = 0  # Distance from start to itself is 0

        while queue:
            vertex = queue.pop(0)  # Dequeue the front element

            for neighbour in graph[vertex]:
                if not visited[neighbour]:
                    queue.append(neighbour)
                    visited[neighbour] = True
                    distances[neighbour] = distances[vertex] + 1  # Increment

    return distances
    ```

!!! note "知识点"
    **Depth-First Search (DFS) Algorithm for binary tree:**

    ``` python

    class TreeNode:
      def __init__(self, key):
          self.left = None
          self.right = None
          self.val = key

    def dfs_traversal(root):
        if root is None:
            return []

        stack = []  # Stack for DFS traversal
        traversal_order = []
        stack.append(root)

        while stack:
            node = stack.pop()
            traversal_order.append(node.val)

            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)

    return traversal_order
    ```

!!! note "知识点"
    **Depth-First Search (DFS) Algorithm for graphs:**

    ``` python

    def dfs(graph, start):
        visited = [False] * (len(graph) + 1)
        stack = []
        traversal_order = []

        # Start DFS from the given vertex:
        stack.append(start)

        while stack:
            vertex = stack.pop()
            if not visited[vertex]:
                traversal_order.append(vertex)
                visited[vertex] = True

            # Push all unvisited adjacent vertice to stack
            for neighbour in reversed(graph[vertex]):
                if not visited[neighbour]:
                    stack.append(neighbour)

    return traversal_order
    ```

**The essence of BFS and DFS is actually the performance of the queue and stack data structures.**

Time complexity is of BFS and DFS are both $O(N + M)$,
where $N$ is the number of vertices and $M$ is the number of edges in the graph.

!!! example
    How to prove the statement that a tree with n vertices has n-1 edges?

**Proof:**
By using **induction** method. A tree is an acyclic, connected graph.

Base case: For a tree with one vertex has no edges, the number of edges
is $0 = 1-1$. Thus, the base case holds.

Inductive hypothesis: Assume that for a tree with $k$ vertices, the number of edges is
$k-1$.That is, we assume that **any tree** with $k$ vertices has $k-1$ edges.

Inductive step: Now, consider a tree with $k+1$ vertices. A tree is acyclic
and connected, so there must exist at least one leaf node
(a vertex connected to only one other vertex).

* Remove a leaf node and its associated edge from the tree.
  The resulting graph is still a tree, but it now has $k$ vertices.
* By the inductive hypothesis, a tree with $k$ vertices has $k-1$ edges.

When we add back the removed vertex and edge, the number of edges becomes:

$$ k-1+1=k $$

Thus, a tree with $k+1$ vertices has $k$ edges.

Conclusion: By induction, any tree with $n$ vertices has $n-1$ edges.

!!! note "知识点"
    **Spanning tree:**
    A spanning tree of a graph G is a subgraph of G **that is a tree** and that
    **includes all the vertices of G**.

!!! note "知识点"
    **Minimum spanning tree:**
    A minimum spanning tree (MST) of a graph G is a spanning tree of G that
    has the **minimum possible total edge weight**.

!!! note "知识点"
    **Shortest path:**
    A shortest path between two vertices in a graph is a path that has the
    minimum possible total edge weight.

!!! note "知识点"
    **Dijkstra's algorithm:**
    Dijkstra's algorithm is a popular algorithm for finding the shortest path
    between two vertices in a graph. It uses a priority queue to keep track of

!!! info "Small question"
    Why we can use recursive algorithm on DFS, but not on BFS?

**Answer:**

The reason is that the recursive algorithm for DFS uses a **stack data structure**
to keep track of the vertices that have been visited, while the iterative
algorithm for BFS uses a **queue data structure**. The stack data structure has a
LIFO (Last In, First Out) behavior, while the queue data structure has a FIFO
(First In, First Out) behavior. The traversal order of DFS naturally suits a recursive 
call stack, and it can also be implemented using an explicit stack data structure 
to simulate the recursion process. 

**TBC...**
