# Practical Lab 2: Uninformed Search – BFS and DFS on a 2D Maze

**Course:** Artificial Intelligence – BSCS 4th Semester  
**Topic:** Uninformed Search Strategies  
**Environment:** Python 3, Google Colab or VS Code

---

## 1. Objectives

1. Represent a maze as a two-dimensional grid.
2. Understand state-space search.
3. Implement Breadth-First Search (BFS).
4. Implement Depth-First Search (DFS).
5. Compare the paths, frontier sizes, and explored nodes of both algorithms.

## 2. Problem Statement

Create a Python program that moves an agent through a 2D maze from a starting cell to a goal cell. The maze contains open cells and walls. Implement BFS and DFS from scratch, then compare their results.

## 3. Background

The maze is represented by a matrix:

- `0` means an open cell.
- `1` means a wall.
- A state is a cell represented by `(row, column)`.
- The agent can move up, down, left, or right.

**BFS** explores the search space level by level using a FIFO queue. When all moves have equal cost, BFS finds a shortest path if one exists.

**DFS** explores one branch deeply before backtracking. It uses a LIFO stack and does not generally guarantee the shortest path.

## 4. Complete Python Implementation

```python
from collections import deque

maze = [
    [0, 0, 0, 0, 0],
    [0, 1, 1, 1, 0],
    [0, 0, 0, 1, 0],
    [0, 1, 0, 0, 0],
    [0, 1, 1, 1, 0]
]

start = (0, 0)
goal = (4, 4)

# Up, down, left, right
directions = [
    (-1, 0),
    (1, 0),
    (0, -1),
    (0, 1)
]


def valid_move(position):
    row, col = position
    rows = len(maze)
    cols = len(maze[0])

    return (
        0 <= row < rows
        and 0 <= col < cols
        and maze[row][col] == 0
    )


def get_neighbors(position):
    row, col = position
    neighbors = []

    for dr, dc in directions:
        next_position = (row + dr, col + dc)

        if valid_move(next_position):
            neighbors.append(next_position)

    return neighbors


def reconstruct_path(parent, goal):
    path = []
    current = goal

    while current is not None:
        path.append(current)
        current = parent[current]

    return path[::-1]


def bfs(start, goal):
    frontier = deque([start])
    parent = {start: None}
    explored = set()
    max_frontier = 1

    while frontier:
        max_frontier = max(max_frontier, len(frontier))
        current = frontier.popleft()

        if current in explored:
            continue

        explored.add(current)

        if current == goal:
            return reconstruct_path(parent, goal), len(explored), max_frontier

        for neighbor in get_neighbors(current):
            if neighbor not in parent:
                parent[neighbor] = current
                frontier.append(neighbor)

    return None, len(explored), max_frontier


def dfs(start, goal):
    frontier = [start]
    parent = {start: None}
    explored = set()
    max_frontier = 1

    while frontier:
        max_frontier = max(max_frontier, len(frontier))
        current = frontier.pop()

        if current in explored:
            continue

        explored.add(current)

        if current == goal:
            return reconstruct_path(parent, goal), len(explored), max_frontier

        # Reverse insertion preserves the same directional preference.
        for neighbor in reversed(get_neighbors(current)):
            if neighbor not in parent:
                parent[neighbor] = current
                frontier.append(neighbor)

    return None, len(explored), max_frontier


def display_result(name, result):
    path, explored_count, max_frontier = result

    print(f"\n{name}")
    print("-" * 35)

    if path:
        print("Path:", path)
        print("Path length (moves):", len(path) - 1)
    else:
        print("No path found.")

    print("Explored nodes:", explored_count)
    print("Maximum frontier size:", max_frontier)


bfs_result = bfs(start, goal)
dfs_result = dfs(start, goal)

display_result("BREADTH-FIRST SEARCH (BFS)", bfs_result)
display_result("DEPTH-FIRST SEARCH (DFS)", dfs_result)
```

## 5. Code Explanation

- `valid_move()` checks whether a cell is inside the maze and is not a wall.
- `get_neighbors()` generates all legal adjacent cells.
- `parent` records how each state was reached, allowing the path to be reconstructed.
- BFS uses `deque.popleft()` to remove the oldest frontier item.
- DFS uses `list.pop()` to remove the most recently added item.
- `explored` prevents repeatedly processing the same state.
- `max_frontier` records the largest frontier size observed during the search.

**Measurement note:** The reported frontier size is the maximum number of pending entries in the queue or stack. It is not the same as the number of explored nodes.

## 6. Sample Output Format

Exact values depend on the maze and neighbor ordering. The output should contain sections similar to:

```text
BREADTH-FIRST SEARCH (BFS)
-----------------------------------
Path: [(0, 0), ... , (4, 4)]
Path length (moves): ...
Explored nodes: ...
Maximum frontier size: ...

DEPTH-FIRST SEARCH (DFS)
-----------------------------------
Path: [(0, 0), ... , (4, 4)]
Path length (moves): ...
Explored nodes: ...
Maximum frontier size: ...
```

## 7. Comparison

| Feature | BFS | DFS |
|---|---|---|
| Data structure | Queue | Stack |
| Search order | Level by level | Deep branch first |
| Complete | Yes, for a finite graph | Yes, for a finite graph with visited-state handling |
| Shortest path | Yes, for equal step costs | Not guaranteed |
| Main memory concern | Can be high | Often lower, but depends on search space |

## 8. Result and Discussion

Both algorithms search the same maze using different frontier-management strategies. BFS is suitable when a shortest path is required and all moves have equal cost. DFS may reach a goal without exploring the entire shallow search space, but its returned path can be longer.

## 9. Conclusion

BFS and DFS were implemented using a queue and a stack. Their path lengths, explored nodes, and maximum frontier sizes were measured to demonstrate the practical differences between two uninformed search strategies.

## 10. Learning Check

1. What is a state-space graph?
2. Why does BFS use a queue?
3. Why does DFS use a stack?
4. Under what condition does BFS guarantee a shortest path?
5. What is the difference between frontier size and explored-node count?

**Reference:** [Course repository – Lab 2](https://github.com/beyond2013/AI/blob/master/markdown/Lab2.md)
