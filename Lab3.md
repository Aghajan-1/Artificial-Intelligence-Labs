# Practical Lab 3: A* Search for the 8-Puzzle

**Course:** Artificial Intelligence – BSCS 4th Semester  
**Topic:** Informed Search and Heuristics  
**Environment:** Python 3, Google Colab or VS Code

---

## 1. Objectives

1. Represent the 8-Puzzle as a search problem.
2. Understand the evaluation function `f(n) = g(n) + h(n)`.
3. Implement A* using a priority queue.
4. Implement Misplaced Tiles and Manhattan Distance heuristics.
5. Compare the number of expanded nodes for both heuristics.

## 2. Problem Statement

The 8-Puzzle consists of a 3×3 board containing tiles numbered 1–8 and one blank space represented by `0`. The objective is to move the blank space until the board matches the goal configuration.

Implement A* search and compare two heuristics: Misplaced Tiles and Manhattan Distance.

## 3. Background

A* evaluates each state using:

`f(n) = g(n) + h(n)`

- `g(n)`: Cost from the initial state to the current state.
- `h(n)`: Estimated cost from the current state to the goal.
- `f(n)`: Estimated total solution cost through the current state.

**Misplaced Tiles:** Counts the numbered tiles that are not in their goal positions.

**Manhattan Distance:** Adds the horizontal and vertical distances of every numbered tile from its goal position.

Both heuristics are admissible for the standard 8-Puzzle when each move costs one.

## 4. Complete Python Implementation

```python
import heapq
from itertools import count

START = (
    1, 2, 3,
    4, 0, 6,
    7, 5, 8
)

GOAL = (
    1, 2, 3,
    4, 5, 6,
    7, 8, 0
)

SIZE = 3


def misplaced_tiles(state):
    return sum(
        1 for index, tile in enumerate(state)
        if tile != 0 and tile != GOAL[index]
    )


def manhattan_distance(state):
    distance = 0

    for index, tile in enumerate(state):
        if tile == 0:
            continue

        current_row, current_col = divmod(index, SIZE)
        goal_index = GOAL.index(tile)
        goal_row, goal_col = divmod(goal_index, SIZE)

        distance += (
            abs(current_row - goal_row)
            + abs(current_col - goal_col)
        )

    return distance


def get_neighbors(state):
    blank = state.index(0)
    row, col = divmod(blank, SIZE)

    possible_moves = [
        (-1, 0),  # Up
        (1, 0),   # Down
        (0, -1),  # Left
        (0, 1)    # Right
    ]

    neighbors = []

    for dr, dc in possible_moves:
        new_row = row + dr
        new_col = col + dc

        if 0 <= new_row < SIZE and 0 <= new_col < SIZE:
            new_index = new_row * SIZE + new_col
            new_state = list(state)

            new_state[blank], new_state[new_index] = (
                new_state[new_index],
                new_state[blank]
            )

            neighbors.append(tuple(new_state))

    return neighbors


def reconstruct_path(parent, state):
    path = []

    while state is not None:
        path.append(state)
        state = parent[state]

    return path[::-1]


def a_star(start, heuristic):
    priority_queue = []
    tie_breaker = count()

    g_cost = {start: 0}
    parent = {start: None}
    closed = set()
    expanded = 0

    heapq.heappush(
        priority_queue,
        (heuristic(start), next(tie_breaker), start)
    )

    while priority_queue:
        _, _, current = heapq.heappop(priority_queue)

        if current in closed:
            continue

        if current == GOAL:
            return reconstruct_path(parent, current), expanded

        closed.add(current)
        expanded += 1

        for neighbor in get_neighbors(current):
            tentative_g = g_cost[current] + 1

            if tentative_g < g_cost.get(neighbor, float("inf")):
                g_cost[neighbor] = tentative_g
                parent[neighbor] = current

                f_cost = tentative_g + heuristic(neighbor)

                heapq.heappush(
                    priority_queue,
                    (f_cost, next(tie_breaker), neighbor)
                )

    return None, expanded


def print_board(state):
    for row in range(SIZE):
        values = state[row * SIZE:(row + 1) * SIZE]
        print(" ".join("_" if x == 0 else str(x) for x in values))
    print()


def run_comparison():
    heuristics = [
        ("Misplaced Tiles", misplaced_tiles),
        ("Manhattan Distance", manhattan_distance)
    ]

    for name, heuristic in heuristics:
        path, expanded = a_star(START, heuristic)

        print("=" * 45)
        print(name)
        print("=" * 45)

        if path:
            print("Solution found!")
            print("Number of moves:", len(path) - 1)
            print("Expanded nodes:", expanded)
            print("\nSolution path:")

            for step, state in enumerate(path):
                print(f"Step {step}:")
                print_board(state)
        else:
            print("No solution found.")


if __name__ == "__main__":
    run_comparison()
```

## 5. Understanding the Code

1. The board is stored as a tuple so that a state can be used as a dictionary key.
2. `get_neighbors()` generates legal moves by swapping the blank tile with an adjacent tile.
3. `heapq` maintains a priority queue ordered by the lowest estimated total cost.
4. `g_cost` stores the best known path cost to each state.
5. `parent` stores the previous state for reconstructing the solution.
6. `expanded` counts states removed from the priority queue and processed.
7. The same starting puzzle is solved with both heuristics for a fair comparison.

## 6. Expected Result

For the provided starting state, the goal is two moves away:

```text
Initial:
1 2 3
4 _ 6
7 5 8

Goal:
1 2 3
4 5 6
7 8 _
```

The solution path should contain two moves. Both heuristics should find an optimal solution for this puzzle. The expanded-node count is printed by the program and should be recorded from your own run.

## 7. Comparison and Observation

| Heuristic | Meaning | Expected behavior |
|---|---|---|
| Misplaced Tiles | Counts incorrectly positioned tiles | Simple estimate |
| Manhattan Distance | Adds each tile's grid distance from its goal | Usually more informative |

Record the actual expanded-node counts from your execution:

| Heuristic | Expanded nodes observed | Solution moves |
|---|---:|---:|
| Misplaced Tiles | Fill after running | 2 |
| Manhattan Distance | Fill after running | 2 |

Do not assume that a stronger heuristic must expand fewer nodes on every individual tie-breaking implementation; record the measured values.

## 8. Result and Discussion

A* combines the cost already incurred with a heuristic estimate of the remaining cost. The two heuristics provide different estimates of the distance to the goal. The experiment demonstrates how domain knowledge can guide search.

## 9. Conclusion

A* search was implemented for the 8-Puzzle using a priority queue. Misplaced Tiles and Manhattan Distance were used to estimate the remaining cost, and their search performance was compared through expanded-node counts.

## 10. Learning Check

1. What is the purpose of `g(n)` in A*?
2. What is a heuristic function?
3. What makes a heuristic admissible?
4. How does Manhattan Distance differ from Misplaced Tiles?
5. Why does A* use a priority queue?

**Reference:** [Course repository – Lab 3](https://github.com/beyond2013/AI/blob/master/markdown/Lab3.md)
