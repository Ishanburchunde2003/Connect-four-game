import java.util.Scanner;
import java.util.Random;

public class ConnectFour {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        
        System.out.print("Player 1, enter your name: ");
        String player1Name = scanner.nextLine();

        
        int rows = 6;
        int columns = 7;
        char[][] board = new char[rows][columns];

        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < columns; col++) {
                board[row][col] = ' ';
            }
        }

        boolean gameOver = false;

        while (!gameOver) {
            printBoard(board);

            
            System.out.print(player1Name + " (X), choose a column (1-" + columns + "): ");
            int player1Move = scanner.nextInt() - 1;

            if (isValidMove(board, player1Move)) {
                dropPiece(board, player1Move, 'X');

                if (isWinner(board, 'X')) {
                    printBoard(board);
                    System.out.println(player1Name + " wins!");
                    gameOver = true;
                    break;
                } else if (isBoardFull(board)) {
                    printBoard(board);
                    System.out.println("It's a draw!");
                    gameOver = true;
                    break;
                }
            } else {
                System.out.println("Invalid move. Please try again.");
                continue;
            }

            
            System.out.println("Player 2 (O)'s turn:");

            int player2Move;
            do {
                player2Move = new Random().nextInt(columns);
            } while (!isValidMove(board, player2Move));

            dropPiece(board, player2Move, 'O');

            if (isWinner(board, 'O')) {
                printBoard(board);
                System.out.println("Player 2 wins!");
                gameOver = true;
            } else if (isBoardFull(board)) {
                printBoard(board);
                System.out.println("It's a draw!");
                gameOver = true;
            }
        }

        scanner.close();
    }

    public static void printBoard(char[][] board) {
        System.out.println(" 1 2 3 4 5 6 7");
        System.out.println("---------------");
        for (int row = 0; row < board.length; row++) {
            System.out.print("|");
            for (int col = 0; col < board[row].length; col++) {
                System.out.print(board[row][col] + "|");
            }
            System.out.println();
        }
        System.out.println("---------------");
    }

    public static boolean isValidMove(char[][] board, int column) {
        return column >= 0 && column < board[0].length && board[0][column] == ' ';
    }

    public static void dropPiece(char[][] board, int column, char player) {
        for (int row = board.length - 1; row >= 0; row--) {
            if (board[row][column] == ' ') {
                board[row][column] = player;
                break;
            }
        }
    }

    public static boolean isWinner(char[][] board, char player) {
        // Check for a horizontal win
        for (int row = 0; row < board.length; row++) {
            for (int col = 0; col <= board[row].length - 4; col++) {
                if (board[row][col] == player && board[row][col + 1] == player &&
                    board[row][col + 2] == player && board[row][col + 3] == player) {
                    return true;
                }
            }
        }

        
        for (int row = 0; row <= board.length - 4; row++) {
            for (int col = 0; col < board[row].length; col++) {
                if (board[row][col] == player && board[row + 1][col] == player &&
                    board[row + 2][col] == player && board[row + 3][col] == player) {
                    return true;
                }
            }
        }

        
        for (int row = 0; row <= board.length - 4; row++) {
            for (int col = 0; col <= board[row].length - 4; col++) {
                if (board[row][col] == player && board[row + 1][col + 1] == player &&
                    board[row + 2][col + 2] == player && board[row + 3][col + 3] == player) {
                    return true;
                }
            }
        }

        
        for (int row = 0; row <= board.length - 4; row++) {
            for (int col = 3; col < board[row].length; col++) {
                if (board[row][col] == player && board[row + 1][col - 1] == player &&
                    board[row + 2][col - 2] == player && board[row + 3][col - 3] == player) {
                    return true;
                }
            }
        }

        return false;
    }

    public static boolean isBoardFull(char[][] board) {
        for (int col = 0; col < board[0].length; col++) {
            if (board[0][col] == ' ') {
                return false;
            }
        }
        return true;
    }
}
