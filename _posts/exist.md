class Solution {
    private int[][] dir = {{1,0}, {-1,0}, {0,1}, {0,-1}};
    public boolean dfs(char[][] board, int i, int j, int[][] visited, char[] cs, int loc) {
        if (loc == cs.length) {
            return true;
        }
        if (board[i][j] != cs[loc]) {
            return false;
        }
        visited[i][j] = 1;
        boolean ans = false;

        visited[i][j] = 0;
    }
    public boolean exist(char[][] board, String word) {
        int[][] visited = new int[board.length][board[0].length];
        char[] cs = word.toChaArray();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board.length; j++) {
                if (dfs(board, i, j, visited, cs, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
}
