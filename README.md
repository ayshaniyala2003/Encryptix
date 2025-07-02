# Encryptix
 Implement an AI agent that plays the classic game of Tic-Tac-Toe  against a human player. You can use algorithms like Minimax with or  without Alpha-Beta Pruning to make the AI player unbeatable. This  project will help you understand game theory and basic search  algorithms
import math

# Initialize empty board
board = [' ' for _ in range(9)]

# Print the board
def print_board():
    print()
    for row in [board[i*3:(i+1)*3] for i in range(3)]:
        print('| ' + ' | '.join(row) + ' |')
    print()

# Check for winner
def check_winner(player):
    win_cond = [(0,1,2), (3,4,5), (6,7,8),  # Rows
                (0,3,6), (1,4,7), (2,5,8),  # Columns
                (0,4,8), (2,4,6)]           # Diagonals
    return any(board[a] == board[b] == board[c] == player for a,b,c in win_cond)

# Check for draw
def is_draw():
    return ' ' not in board

# Minimax algorithm
def minimax(depth, is_maximizing):
    if check_winner('O'): return 1
    if check_winner('X'): return -1
    if is_draw(): return 0

    if is_maximizing:
        best_score = -math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'O'
                score = minimax(depth + 1, False)
                board[i] = ' '
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'X'
                score = minimax(depth + 1, True)
                board[i] = ' '
                best_score = min(score, best_score)
        return best_score

# AI move
def ai_move():
    best_score = -math.inf
    move = 0
    for i in range(9):
        if board[i] == ' ':
            board[i] = 'O'
            score = minimax(0, False)
            board[i] = ' '
            if score > best_score:
                best_score = score
                move = i
    board[move] = 'O'

# Main game loop
def play_game():
    print("Welcome to Tic-Tac-Toe! You are 'X', AI is 'O'.")
    print_board()

    while True:
        # Human move
        try:
            move = int(input("Enter your move (1-9): ")) - 1
            if board[move] != ' ' or move not in range(9):
                print("Invalid move. Try again.")
                continue
            board[move] = 'X'
        except:
            print("Please enter a valid number.")
            continue

        print_board()

        if check_winner('X'):
            print("You win!")
            break
        if is_draw():
            print("It's a draw!")
            break

        # AI move
        ai_move()
        print("AI has moved:")
        print_board()

        if check_winner('O'):
            print("AI wins!")
            break
        if is_draw():
            print("It's a draw!")
            break

# Run the game
if __name__ == "__main__":
    play_game()
