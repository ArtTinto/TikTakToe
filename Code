import java.util.Random;
import java.util.Scanner;

public class TikTakToe {

    public static char playerSymbol;
    public static char cpuSymbol;

    public static void print_velha(char mat[][]) {
        for (int i = 0; i < mat.length; i++) {
            System.out.println();
            for (int j = 0; j < mat[i].length - 1; j++) {
                print_char(mat[i][j]);
                print_char('|');
            }
            print_char(mat[i][2]);

            System.out.println();
            for (int k = 0; k < 11; k++) {
                print_char('-');
            }
        }
        System.out.println();

    }

    public static void print_char(char character) {

        // Color if x
        if (character == 'x') {
            System.out.print("\u001B[34m " + character + "\u001B[0m");
        }
        // Color if o
        else if (character == 'o') {
            System.out.print("\u001B[31m " + character + "\u001B[0m");
        }
        // Color if |
        else if (character == '|') {
            System.out.print("\u001B[33m " + character + "\u001B[0m");
        }
        // Color if -
        else if (character == '-') {
            System.out.print("\u001B[33m" + character + "\u001B[0m");
        }
        // Color if letters a to i
        else {
            System.out.print("\u001B[32m " + character + "\u001B[0m");
        }

    }

    public static char getWinner(char mat[][]) {
        for (int i = 0; i < mat.length; i++) {
            for (int j = 0; j < mat[i].length; j++) {

                if (j + 2 < mat[i].length) {
                    // Check all rows
                    if (mat[i][j] == mat[i][j + 1] && mat[i][j] == mat[i][j + 2]) {
                        return mat[i][j];
                    }
                }

                if (i + 2 < mat[i].length) {
                    // Check all columns
                    if (mat[i][j] == mat[i + 1][j] && mat[i][j] == mat[i + 2][j]) {
                        return mat[i][j];
                    }

                    // Check Main Diagonal
                    if (mat[i][i] == mat[i + 1][i + 1] && mat[i][i] == mat[i + 2][i + 2]) {
                        return mat[i][i];
                    }

                    // Check Secundary Diagonal
                    if (mat[i][mat[i].length - 1] == mat[i + 1][mat[i].length - 2]
                            && mat[i][mat[i].length - 1] == mat[i + 2][mat[i].length - 3]) {
                        return mat[i][mat[i].length - 1];
                    }
                }
            }
        }
        // Return blank in case no one won
        return ' ';
    }

    // Detects either an attack or a threat depending on the given symbol
    public static int[] recognizePattern(char[][] mat, char simbol) {
        // Check Rows
        for (int i = 0; i < 3; i++) {
            int count = 0;
            int blank = -1;
            for (int j = 0; j < 3; j++) {
                if (mat[i][j] == simbol)
                    count++;
                else if (mat[i][j] != playerSymbol && mat[i][j] != cpuSymbol)
                    blank = j;
            }
            if (count == 2 && blank != -1)
                return new int[] { i, blank };
        }

        // Check Columns
        for (int j = 0; j < 3; j++) {
            int count = 0;
            int blank = -1;
            for (int i = 0; i < 3; i++) {
                if (mat[i][j] == simbol)
                    count++;
                else if (mat[i][j] != playerSymbol && mat[i][j] != cpuSymbol)
                    blank = i;
            }
            if (count == 2 && blank != -1)
                return new int[] { blank, j };
        }

        // Check Main Diagonal
        int countDiag = 0, blankDiag = -1;
        for (int i = 0; i < 3; i++) {
            if (mat[i][i] == simbol)
                countDiag++;
            else if (mat[i][i] != playerSymbol && mat[i][i] != cpuSymbol)
                blankDiag = i;
        }
        if (countDiag == 2 && blankDiag != -1)
            return new int[] { blankDiag, blankDiag };

        // Check Secondary Diagonal
        countDiag = 0;
        blankDiag = -1;
        for (int i = 0; i < 3; i++) {
            if (mat[i][2 - i] == simbol)
                countDiag++;
            else if (mat[i][2 - i] != playerSymbol && mat[i][2 - i] != cpuSymbol)
                blankDiag = i;
        }
        if (countDiag == 2 && blankDiag != -1)
            return new int[] { blankDiag, 2 - blankDiag };

        // If there is no pattern, return null
        return null;
    }

    // Return pos [row, column] where CPU has to block
    public static int[] findThreat(char[][] mat) {
        // Check all rows and columns
        // Return [i, j] of the position that can be won
        int[] pos = new int[2];

        // Check if CPU can win first
        pos = recognizePattern(mat, cpuSymbol); // ← trocado 'o' por cpuSymbol
        if (pos != null)
            return pos;

        // If CPU can't win, try to block the player
        pos = recognizePattern(mat, playerSymbol); // ← trocado 'x' por playerSymbol
        if (pos != null)
            return pos;

        // No winning or blocking move found
        return null;
    }

    public static boolean cpuPlayEasy(char[][] position, int qtd_plays, Random random) {
        int playCPU = random.nextInt(9 - qtd_plays);

        for (int i = 0; i < position.length; i++) {
            for (int j = 0; j < position[i].length; j++) {
                // Skip occupied cells
                if (position[i][j] == 'x' || position[i][j] == 'o')
                    continue;

                // When the random counter reaches zero, play here
                if (playCPU == 0) {
                    position[i][j] = cpuSymbol; // ← trocado 'o' por cpuSymbol
                    return true;
                }

                playCPU--;
            }
        }

        return false; // CPU couldn't play (should never happen)
    }

    // CPU hard mode: tries to win or block before playing randomly
    public static boolean cpuPlayHard(char[][] position) {
        // First, try to find a winning move for the CPU
        int[] attack = recognizePattern(position, cpuSymbol);
        if (attack != null) {
            position[attack[0]][attack[1]] = cpuSymbol;
            return true;
        }

        // If no winning move, try to block the player's threat
        int[] threat = recognizePattern(position, playerSymbol);
        if (threat != null) {
            position[threat[0]][threat[1]] = cpuSymbol;
            return true;
        }

        // Otherwise, play randomly like the easy mode
        Random random = new Random();
        return cpuPlayEasy(position, countPlays(position), random);
    }

    // Counts total number of moves made on the board
    public static int countPlays(char[][] mat) {
        int count = 0;
        for (int i = 0; i < mat.length; i++) {
            for (int j = 0; j < mat[i].length; j++) {
                if (mat[i][j] == playerSymbol || mat[i][j] == cpuSymbol)
                    count++;
            }
        }
        return count;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();
        char play;
        int qtd_plays = 0;
        int dificuldade;

        // Dificulty selector 1 for easy 2 for hard
        do {
            System.out.print("Escolha a dificuldade:\n1 - Fácil\n2 - Difícil\n");
            while (!scanner.hasNextInt()) {
                System.out.print("Digite apenas 1 ou 2: ");
                scanner.next();
            }
            dificuldade = scanner.nextInt();
            scanner.nextLine();
        } while (dificuldade != 1 && dificuldade != 2);

        // Select Simbol
        do {
            System.out.print("Escolha seu símbolo:\nX - Jogar com X\nO - Jogar com O\n");
            String input = scanner.next().toLowerCase();
            if (input.equals("x")) {
                playerSymbol = 'x';
                cpuSymbol = 'o';
            } else if (input.equals("o")) {
                playerSymbol = 'o';
                cpuSymbol = 'x';
            } else {
                System.out.println("Digite apenas X ou O!\n");
                continue;
            }
            break;
        } while (true);

        // Creates an array with the position in game
        char position[][] = { { 'a', 'b', 'c' }, { 'd', 'e', 'f' }, { 'g', 'h', 'i' } };

        // If player chose 'O', CPU starts first
        if (playerSymbol == 'o') {
            System.out.println("\nCPU começa o jogo!\n");
            boolean hasPlayed;
            if (dificuldade == 2) {
                hasPlayed = cpuPlayHard(position);
            } else {
                hasPlayed = cpuPlayEasy(position, qtd_plays, random);
            }
            if (hasPlayed)
                qtd_plays++;
        }

        for (;;) {
            print_velha(position);
            System.out.println();

            boolean valid;
            boolean isOcupidy;
            do {
                // Get player play
                System.out.print("\u001B[31mSUA VEZ\u001B[0m\n\u001B[35mDigite a letra da posição desejada: \u001B[0m");
                play = scanner.next().toLowerCase().charAt(0);
                System.out.println();

                // Looks if the player char is valid in the matriz position
                // is like doing a if char between the ASCII
                valid = (play >= 'a' && play <= 'i');
                isOcupidy = false;

                // Show a message if the input is invalid
                if (!valid) {
                    System.out.println("\u001B[33mPosição inválida! Digite uma letra de 'a' a 'i'.\u001B[0m\n");
                } else {
                    // Check if the position is ocupidy
                    boolean find = false;
                    for (int i = 0; i < position.length; i++) {
                        for (int j = 0; j < position[i].length; j++) {
                            if (position[i][j] == play) {
                                find = true;
                            }
                        }
                    }

                    if (!find) {
                        isOcupidy = true;
                        System.out.println("\u001B[33mEssa posição já está ocupada! Escolha outra.\u001B[0m\n");
                    }
                }

            } while (!valid || isOcupidy);

            // Check the player play to switch it to player choice
            for (int i = 0; i < position.length; i++) {
                for (int j = 0; j < position[i].length; j++) {
                    if (position[i][j] == play) {
                        position[i][j] = playerSymbol;
                        qtd_plays++;
                    }
                }
            }

            // Check if game has been a draw
            if (qtd_plays < 9) {
                boolean hasPlayed = false;

                // Choose CPU difficulty mode
                if (dificuldade == 2) {
                    hasPlayed = cpuPlayHard(position);
                } else {
                    hasPlayed = cpuPlayEasy(position, qtd_plays, random);
                }

                if (hasPlayed)
                    qtd_plays++;
            }

            // Check if someone has winned
            char winner = getWinner(position);

            // Exhib a message if the player has winned
            if (winner == playerSymbol) {
                // Print the last array
                print_velha(position);
                for (int i = 40; i <= 47; i++) {

                    // Fun print
                    System.out.print("\u001B[" + i + "mCongrats, You Won!!\u001B[0m\n");
                    try {
                        Thread.sleep(500);
                    } catch (Exception e) {
                    }
                }
                break;
            }
            // Exhib a message if the CPU has winned
            else if (winner == cpuSymbol) {
                // Print the last array
                print_velha(position);
                for (int i = 40; i <= 47; i++) {

                    // Fun print
                    System.out.print("\u001B[" + i + "mDude, You Lost To a CPU!!\u001B[0m\n");
                    try {
                        Thread.sleep(1000);
                    } catch (Exception e) {
                    }
                }
                break;
            }
            // If hasn't winners then ends
            if (qtd_plays == 9) {
                System.out.println("Deu Velha");
                break;
            }
        }

        scanner.close();
    }
}
