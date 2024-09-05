```java
public boolean isValidSudoku(char[][] board) {
    Set<String> seen = new HashSet<>();
    for (int i = 0; i < 9; ++i) {
        for (int j = 0; j < 9; ++j) {
            char number = board[i][j];
            if (number != '.') {
                // add 는 element가 이미 존재하는 경우 false를 반환한다
                if (!seen.add(number + " in row " + i) ||  // 이미 존재하면 valid sudoku가 아니다 
                        !seen.add(number + " in column " + j) ||
                        !seen.add(number + " in block " + i / 3 + "-" + j / 3)) {
                    return false;
                }
            }
        }
    }
    return true;
}
```
