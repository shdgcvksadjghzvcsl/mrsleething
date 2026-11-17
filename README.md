def print_board(board):
    """打印当前棋盘状态"""
    print("\n")
    for i in range(3):
        print(f" {board[i][0]} | {board[i][1]} | {board[i][2]} ")
        if i < 2:
            print("---|---|---")
    print("\n")


def check_winner(board, player):
    """检查当前玩家是否获胜"""
    # 检查行和列
    for i in range(3):
        if all(board[i][j] == player for j in range(3)) or all(
            board[j][i] == player for j in range(3)
        ):
            return True

    # 检查两条对角线
    if (
        board[0][0] == board[1][1] == board[2][2] == player
        or board[0][2] == board[1][1] == board[2][0] == player
    ):
        return True

    return False


def is_full(board):
    """检查棋盘是否已满（平局）"""
    return all(cell != " " for row in board for cell in row)


def play_game():
    # 初始化 3x3 空棋盘
    board = [[" " for _ in range(3)] for _ in range(3)]
    current_player = "X"

    print("=== 欢迎来到井字棋游戏 ===")
    print("输入格式：行号 列号（范围为 1 到 3，用空格隔开，例如：1 2）")

    while True:
        print_board(board)

        try:
            move = input(
                f"玩家 [{current_player}] 请落子 (行 列): "
            ).split()
            if len(move) != 2:
                print("输入格式错误！请输入两个数字，中间用空格隔开。")
                continue

            row, col = int(move[0]) - 1, int(move[1]) - 1

            # 检查坐标是否越界
            if not (0 <= row <= 2 and 0 <= col <= 2):
                print("坐标超出范围！请输入 1 到 3 之间的数字。")
                continue

            # 检查位置是否已被占用
            if board[row][col] != " ":
                print("该位置已有棋子，请重新选择！")
                continue

        except ValueError:
            print("无效输入！请输入整数数字。")
            continue

        # 落子
        board[row][col] = current_player

        # 判断胜负
        if check_winner(board, current_player):
            print_board(board)
            print(f"恭喜！玩家 [{current_player}] 获胜！")
            break

        # 判断平局
        if is_full(board):
            print_board(board)
            print("棋盘已满，双方平局！")
            break

        # 切换玩家
        current_player = "O" if current_player == "X" else "X"


if __name__ == "__main__":
    play_game()
