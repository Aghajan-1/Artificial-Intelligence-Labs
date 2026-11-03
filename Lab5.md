# Practical Lab 5: Tic-Tac-Toe Using Minimax and Alpha-Beta Pruning

**Course:** Artificial Intelligence – BSCS 4th Semester  
**Topic:** Adversarial Search and Game Engines  
**Environment:** Python 3, Google Colab or VS Code

> **Course-breakup practical:** This lab is prepared from the Week 5 practical requirement in the supplied course breakup. A separate official Lab 5 file was not linked in the supplied instructions.

---

## 1. Objectives

1. Understand deterministic, two-player, zero-sum games.
2. Represent a game using a board and legal moves.
3. Understand game trees and terminal states.
4. Implement the Minimax algorithm.
5. Optimize Minimax using Alpha-Beta Pruning.
6. Create a playable Tic-Tac-Toe game against an AI agent.

## 2. Problem Statement

Develop a complete Tic-Tac-Toe game in Python. The human player uses `X`, while the computer uses `O`. The computer should select its moves using Minimax with Alpha-Beta Pruning. With correct implementation and full-depth search, the AI should never lose.

## 3. Background

Tic-Tac-Toe is a deterministic, zero-sum, perfect-information game.

- **MAX:** The AI tries to maximize its score.
- **MIN:** The human opponent is assumed to minimize the AI's score.
- **Terminal state:** A win, loss, or draw.
- **Minimax:** Searches possible future moves and chooses the best move assuming the opponent also plays optimally.
- **Alpha-Beta Pruning:** Avoids exploring branches that cannot affect the final decision.

The evaluation scores are:

- AI win: `+1`
- Human win: `-1`
- Draw: `0`

## 4. Complete Python Implementation

```python
HUMAN = "X"
AI = "O"
EMPTY = " "

board = [EMPTY] * 9


def display_board(board):
    print()
    for row in range(3):
        cells = board[row * 3:(row + 1) * 3]
        print(" " + " | ".join(
            str(row * 3 + col + 1) if cell == EMPTY else cell
            for col, cell in enumerate(cells)
        ))
        if row < 2:
            print("---+---+---")
    print()


def check_winner(board):
    winning_lines = [
        (0, 1, 2), (3, 4, 5), (6, 7, 8),
        (0, 3, 6), (1, 4, 7), (2, 5, 8),
        (0, 4, 8), (2, 4, 6)
    ]

    for a, b, c in winning_lines:
        if board[a] == board[b] == board[c] != EMPTY:
            return board[a]

    return None


def is_terminal(board):
    return check_winner(board) is not None or EMPTY not in board


def evaluate(board):
    winner = check_winner(board)

    if winner == AI:
        return 1
    elif winner == HUMAN:
        return -1
    return 0


def minimax(board, is_maximizing, alpha, beta):
    if is_terminal(board):
        return evaluate(board)

    if is_maximizing:
        best_score = float("-inf")

        for index in range(9):
            if board[index] == EMPTY:
                board[index] = AI
                score = minimax(board, False, alpha, beta)
                board[index] = EMPTY

                best_score = max(best_score, score)
                alpha = max(alpha, best_score)

                if beta <= alpha:
                    break

        return best_score

    else:
        best_score = float("inf")

        for index in range(9):
            if board[index] == EMPTY:
                board[index] = HUMAN
                score = minimax(board, True, alpha, beta)
                board[index] = EMPTY

                best_score = min(best_score, score)
                beta = min(beta, best_score)

                if beta <= alpha:
                    break

        return best_score


def best_ai_move(board):
    best_score = float("-inf")
    move = None

    for index in range(9):
        if board[index] == EMPTY:
            board[index] = AI
            score = minimax(
                board,
                False,
                float("-inf"),
                float("inf")
            )
            board[index] = EMPTY

            if score > best_score:
                best_score = score
                move = index

    return move


def play_game():
    print("TIC-TAC-TOE: HUMAN (X) vs AI (O)")
    print("Choose a position from 1 to 9.")
    display_board(board)

    while not is_terminal(board):
        # Human turn
        while True:
            try:
                move = int(input("Enter your move (1-9): ")) - 1

                if 0 <= move < 9 and board[move] == EMPTY:
                    board[move] = HUMAN
                    break

                print("Invalid or occupied position. Try again.")

            except ValueError:
                print("Please enter a number from 1 to 9.")

        display_board(board)

        if is_terminal(board):
            break

        # AI turn
        print("AI is thinking...")
        ai_move = best_ai_move(board)

        if ai_move is not None:
            board[ai_move] = AI

        display_board(board)

    winner = check_winner(board)

    if winner == HUMAN:
        print("Human wins!")
    elif winner == AI:
        print("AI wins!")
    else:
        print("The game is a draw!")


if __name__ == "__main__":
    play_game()
```

## 5. How the Algorithm Works

1. The program checks whether the current board is a win, loss, or draw.
2. If the game is not over, it considers every legal move.
3. The AI (MAX) selects the move with the highest score.
4. The human (MIN) is assumed to select the move with the lowest score.
5. Alpha and beta track the best bounds found so far.
6. If `beta <= alpha`, the remaining branch is pruned because it cannot improve the decision.
7. The selected move is applied to the actual game board.

## 6. Sample Execution

```text
TIC-TAC-TOE: HUMAN (X) vs AI (O)
Choose a position from 1 to 9.

 1 | 2 | 3
---+---+---
 4 | 5 | 6
---+---+---
 7 | 8 | 9

Enter your move (1-9): 1

AI is thinking...
```

The exact game sequence depends on the human's moves and the order in which equally valued AI moves are considered. Against optimal play, the game can end in a draw; the AI should not lose.

## 7. Minimax vs Alpha-Beta Pruning

| Feature | Minimax | Alpha-Beta Pruning |
|---|---|---|
| Purpose | Finds the best move | Finds the same best move more efficiently |
| Search | Explores the game tree | Skips branches that cannot affect the result |
| Result | Optimal under the search assumptions | Same optimal result |
| Efficiency | May explore many nodes | Can explore fewer nodes |

## 8. Result and Discussion

The Tic-Tac-Toe engine allows a human to play against a computer. Minimax evaluates future outcomes, while Alpha-Beta Pruning reduces unnecessary search. The AI's full-depth search makes it unbeatable when implemented correctly.

## 9. Conclusion

A playable Tic-Tac-Toe game was developed using Minimax and Alpha-Beta Pruning. The practical demonstrated adversarial search, game-tree evaluation, optimal decision-making, and search optimization.

## 10. Learning Check

1. What is adversarial search?
2. What is a zero-sum game?
3. What are MAX and MIN in Minimax?
4. What do alpha and beta represent?
5. Why does Alpha-Beta Pruning not change the optimal Minimax result?

**Lecture reference:** [Week 5 – Adversarial Search and Game Engines](https://github.com/beyond2013/AI/blob/master/markdown/Week7.md)
