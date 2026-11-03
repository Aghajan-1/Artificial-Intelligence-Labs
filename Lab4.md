# Practical Lab 4: Hill Climbing Search for the N-Queens Problem

**Course:** Artificial Intelligence – BSCS 4th Semester  
**Topic:** Local Search and Optimization  
**Environment:** Python 3, Google Colab or VS Code  
**Library:** Matplotlib

---

## 1. Objectives

1. Understand local search and the Hill Climbing algorithm.
2. Represent the N-Queens problem as a state.
3. Create an evaluation function that counts attacking queen pairs.
4. Implement basic Hill Climbing.
5. Implement Random-Restart Hill Climbing.
6. Plot the evaluation score over iterations.

## 2. Problem Statement

Place eight queens on an 8×8 chessboard so that no two queens attack each other. Queens attack along rows, columns, and diagonals.

Use Hill Climbing to improve a board configuration. Plot the number of attacking pairs over the iterations and use random restarts to improve the chance of finding a solution.

## 3. Background

In local search, the main concern is the quality of the current state rather than the path taken to reach it.

For this implementation:

- There is exactly one queen in each column.
- A state is a list of eight row positions.
- The index represents the column.
- The value at that index represents the queen's row.
- The evaluation function counts pairs of queens attacking each other.
- A solution has a score of `0`.

Hill Climbing moves to a neighboring state with a lower conflict score. It can become stuck at a local minimum or plateau, so random restarts are useful.

## 4. Python Implementation

```python
import random
import matplotlib.pyplot as plt

N = 8


def random_state():
    """Return a random board with one queen in each column."""
    return [random.randrange(N) for _ in range(N)]


def count_conflicts(state):
    """Count pairs of queens attacking each other."""
    conflicts = 0

    for col1 in range(N):
        for col2 in range(col1 + 1, N):
            same_row = state[col1] == state[col2]

            same_diagonal = (
                abs(state[col1] - state[col2])
                == abs(col1 - col2)
            )

            if same_row or same_diagonal:
                conflicts += 1

    return conflicts


def get_best_neighbor(state):
    """Find a neighbor with the lowest conflict score."""
    best_state = state[:]
    best_score = count_conflicts(state)

    for col in range(N):
        for row in range(N):
            if row == state[col]:
                continue

            neighbor = state[:]
            neighbor[col] = row
            score = count_conflicts(neighbor)

            if score < best_score:
                best_state = neighbor
                best_score = score

    return best_state, best_score


def hill_climbing(initial_state):
    current = initial_state[:]
    scores = [count_conflicts(current)]

    while True:
        neighbor, neighbor_score = get_best_neighbor(current)
        current_score = count_conflicts(current)

        if neighbor_score >= current_score:
            break

        current = neighbor
        scores.append(neighbor_score)

        if neighbor_score == 0:
            break

    return current, scores


def random_restart_hill_climbing(max_restarts=100):
    best_state = None
    best_score = float("inf")
    all_runs = []

    for restart in range(max_restarts + 1):
        initial = random_state()
        solution, scores = hill_climbing(initial)
        all_runs.append(scores)

        final_score = count_conflicts(solution)

        if final_score < best_score:
            best_state = solution
            best_score = final_score

        if best_score == 0:
            break

    return best_state, best_score, all_runs


def plot_scores(runs):
    """Plot the conflict score over iterations for each restart."""
    plt.figure(figsize=(10, 5))

    for index, scores in enumerate(runs):
        plt.plot(
            range(len(scores)),
            scores,
            marker="o",
            label=f"Restart {index + 1}"
        )

    plt.title("Hill Climbing: N-Queens Conflict Score")
    plt.xlabel("Iteration")
    plt.ylabel("Number of Attacking Pairs")
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()


if __name__ == "__main__":
    solution, score, runs = random_restart_hill_climbing()

    print("Best board:", solution)
    print("Final conflict score:", score)
    print("Number of runs:", len(runs))

    if score == 0:
        print("Solution found: No queens attack each other.")
    else:
        print("No complete solution found within the restart limit.")

    plot_scores(runs)
```

## 5. Code Explanation

- `random_state()` generates a random board.
- `count_conflicts()` calculates the number of attacking queen pairs.
- `get_best_neighbor()` tests alternative row positions for each column.
- `hill_climbing()` repeatedly moves to a better neighbor.
- `random_restart_hill_climbing()` starts from new random boards when necessary.
- `plot_scores()` visualizes the conflict score over the iterations.

A lower conflict score is better. A score of zero represents a valid solution.

## 6. Expected Output

The output will vary because the initial board is random.

```text
Best board: [ ... ]
Final conflict score: 0
Number of runs: ...
Solution found: No queens attack each other.
```

The graph should show the conflict score decreasing or staying constant until the search stops. A run that gets stuck may finish with a score greater than zero.

## 7. Result and Observation

Record your actual results:

| Observation | Your result |
|---|---|
| Best conflict score | Fill after running |
| Number of restarts | Fill after running |
| Was a solution found? | Yes / No |
| Did the score decrease? | Yes / No |

## 8. Limitations

Basic Hill Climbing can stop at a local minimum or plateau. Random restarts help by trying new initial states, but a fixed restart limit does not guarantee that every run will find a solution.

## 9. Conclusion

Hill Climbing was implemented to solve the N-Queens problem. The evaluation function measured attacking queen pairs, while Random-Restart Hill Climbing improved the search by exploring multiple initial configurations. Matplotlib was used to visualize the optimization process.

## 10. Learning Check

1. What is local search?
2. What is a local minimum?
3. Why is a conflict score of zero important?
4. How does Random-Restart Hill Climbing help?
5. What does the evaluation graph tell us?

**Reference:** [Course repository – Lab 4](https://github.com/beyond2013/AI/blob/master/markdown/Lab4.md)
