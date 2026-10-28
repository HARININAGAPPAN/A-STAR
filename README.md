<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name:  Harini N     </h3>
<h3>Register Number:  212223040057         </h3>
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````
// A* Search Algorithm
1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor
        i) if successor is the goal, stop search
        
        ii) else, compute both g and h for successor
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
          successor.f = successor.g + successor.h

        iii) if a node with the same position as 
            successor is in the OPEN list which has a 
           lower f than successor, skip this successor

        iV) if a node with the same position as 
            successor  is in the CLOSED list which has
            a lower f than successor, skip this successor
            otherwise, add  the node to the open list
     end (for loop)
  
    e) push q on the closed list
    end (while loop)

``````
# code:

```

from collections import defaultdict
import networkx as nx
import matplotlib.pyplot as plt


def a_star(graph, heuristic, start, goal):
    open_set = {start}
    closed_set = set()
    g = {start: 0}
    parent = {start: None}

    while open_set:
        current = min(
            open_set,
            key=lambda node: g[node] + heuristic[node]
        )

        if current == goal:
            path = []
            while current is not None:
                path.append(current)
                current = parent[current]

            path.reverse()
            print("\nShortest Path:", " -> ".join(path))
            print("Total Cost:", g[goal])
            return path

        open_set.remove(current)
        closed_set.add(current)

        for neighbour, cost in graph[current]:
            if neighbour in closed_set:
                continue

            new_cost = g[current] + cost

            if new_cost >= g.get(neighbour, float('inf')):
                continue

            g[neighbour] = new_cost
            parent[neighbour] = current
            open_set.add(neighbour)

    print("Path does not exist!")
    return None


graph = defaultdict(list)
G = nx.Graph()

n, e = map(int, input("Enter number of nodes and edges: ").split())

print("\nEnter the edges (u v cost):")

for i in range(e):
    u, v, cost = input(f"Edge {i + 1}: ").split()
    cost = int(cost)

    graph[u].append((v, cost))
    graph[v].append((u, cost))
    G.add_edge(u, v, weight=cost)

print("\nAdjacency List")
for node in graph:
    print(node, "->", graph[node])

heuristic = {}
print("\nEnter Heuristic Values")

for i in range(n):
    node, h = input(f"{i + 1}. Node Heuristic: ").split()
    heuristic[node] = int(h)

print("\nHeuristic Values")
print(heuristic)

plt.figure(figsize=(8, 6))

pos = nx.spring_layout(G, seed=20)

nx.draw_networkx_nodes(G, pos, node_color="skyblue", node_size=1800)
nx.draw_networkx_labels(G, pos, font_size=12, font_weight="bold")
nx.draw_networkx_edges(G, pos, width=2)

edge_labels = nx.get_edge_attributes(G, 'weight')
nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)

plt.title("Original Graph")
plt.axis("off")
plt.show()

start = input("\nEnter Start Node: ")
goal = input("Enter Goal Node: ")

if start not in graph:
    print("Invalid Start Node!")
elif goal not in graph:
    print("Invalid Goal Node!")
elif start not in heuristic or goal not in heuristic:
    print("Heuristic value missing for start or goal node!")
else:
    shortest_path = a_star(graph, heuristic, start, goal)

    if shortest_path:
        path_edges = []

        for i in range(len(shortest_path) - 1):
            path_edges.append(
                (shortest_path[i], shortest_path[i + 1])
            )

        plt.figure(figsize=(8, 6))

        nx.draw_networkx_nodes(
            G, pos, node_color="skyblue", node_size=1800
        )
        nx.draw_networkx_labels(
            G, pos, font_size=12, font_weight="bold"
        )

        # Draw all edges
        nx.draw_networkx_edges(
            G, pos, edge_color="gray", width=2
        )

        # Highlight shortest path
        nx.draw_networkx_edges(
            G, pos, edgelist=path_edges,
            edge_color="red", width=4
        )

        nx.draw_networkx_edge_labels(
            G, pos, edge_labels=edge_labels
        )

        plt.title("Shortest Path Highlighted (Red)")
        plt.axis("off")
        plt.show()

```

## Input:
Enter number of nodes and edges: 5 4

Enter the edges (u v cost):
Edge 1: a b 1
Edge 2: b c 2
Edge 3: c d 3
Edge 4: d e 4

Adjacency List
a -> [('b', 1)]
b -> [('a', 1), ('c', 2)]
c -> [('b', 2), ('d', 3)]
d -> [('c', 3), ('e', 4)]
e -> [('d', 4)]

Enter Heuristic Values
1. Node Heuristic:  a 1
2. Node Heuristic: b 2
3. Node Heuristic: c 5
4. Node Heuristic: d 4
5. Node Heuristic: e 3

Heuristic Values
{'a': 1, 'b': 2, 'c': 5, 'd': 4, 'e': 3}
## Output:
<img width="640" height="504" alt="image" src="https://github.com/user-attachments/assets/5c1328fe-feb0-48da-b627-29e1cb62a129" />

## start path and end path:
Enter Start Node: b
Enter Goal Node: e

Shortest Path: b -> c -> d -> e
Total Cost: 9
## output:
<img width="640" height="504" alt="image" src="https://github.com/user-attachments/assets/5b3be2e4-57d8-4ebf-ba68-8bac8f4ea02e" />
## Result:
Thus, the Breadth-First Search (BFS) algorithm was implemented successfully in Python. The graph was traversed starting from node A, and the BFS traversal obtained was A, B, C, D.


