``` java
package org.example;

import java.io.*;
import java.util.Objects;

import static java.lang.Boolean.FALSE;
import static java.lang.Boolean.TRUE;

/**
 * 문제 링크 : https://www.acmicpc.net/problem/9663
 * 문제 해석 :
 * - N * N인 체스판에서 N개의 퀸이 서로 공격할 수 없는 개수를 구해라.
 */
public class NQueen {

    /// 필드
    private Integer n;
    private Integer[] queens;
    private static Integer solutionCount = 0;

    /// 생성자
    private NQueen(Integer n) {
        this.n = n;
        this.queens = new Integer[n];
        solution(0);
    }

    /// 메소드
    private void solution(Integer row) {
        if (Objects.equals(n, row)) {
            solutionCount++;
            debug();
            return;
        }
        // 체스판 세로 길이 만큼 움직인다.
        for (int col = 0; col < n; col++) {
            // 놓을 수 있는 자리인지 확인
            if (isSafe(row, col)) {
                queens[row] = col; // 현재 행에다가 열의 위치를 저장
                solution(row + 1); // 가로 증가 시키며 재귀 호출
            }
        }
    }

    /**
     * 퀸을 놓을 수 있는 자리인지 판단하는 메소드
     * 퀸이 움직일 수 있는 범위: 가로, 세로, 대각선 무제한
     * @param row 체스판 가로
     * @param col 체스판 세로
     * @return Boolean
     */
    private Boolean isSafe(Integer row, Integer col) {
        // 체스판 가로 길이 만큼 움직인다.
        for (int idx = 0; idx < row; idx++) {
            // 같은 열에 퀸이 있으면 공격 가능하기 때문에 FALSE 반환
            if (Objects.equals(col, queens[idx])) {
                return FALSE;
            }
            // 대각선에 퀸이 있으면 공격 가능하기 때문에 FALSE 반환
            if (Math.abs(queens[idx] - col) == Math.abs(idx - row)) {
                return FALSE;
            }
        }
        return TRUE;
    }

    private void debug() {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (queens[i] == j) {
                    System.out.print("ㅋ ");
                } else {
                    System.out.print("ㅁ ");
                }
            }
            System.out.println();
        }
        System.out.println();
    }

    public static void main(String[] args) throws IOException {
        // System.out.println("입력:");
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        System.out.print("입력:");
        Integer n = Integer.parseInt(br.readLine());
        new NQueen(n);

        System.out.print("출력:");
        bw.write(solutionCount + System.lineSeparator());

        br.close();
        bw.close();
    }
}
/**
 * n = 1
 * ㅋ 하나 놓으면 끝, 답 1
 *
 * n = 2
 * ㅋㅁ
 * ㅁㅁ 2개를 놓을 수 없네? 그럼 답 0
 *
 * n = 3
 * ㅋㅁㅁ
 * ㅁㅁㅁ 이것도 3개를 놓을 수 없는데 답 0
 * ㅁㅋㅁ
 *
 * n = 4
 * ㅁㅋㅁㅁ
 * ㅁㅁㅁㅋ
 * ㅋㅁㅁㅁ
 * ㅁㅁㅋㅁ
 *
 * ㅁㅁㅋㅁ
 * ㅋㅁㅁㅁ
 * ㅁㅁㅁㅋ
 * ㅁㅋㅁㅁ 이 두 조합 밖에 안되네 답 2
 */

```
