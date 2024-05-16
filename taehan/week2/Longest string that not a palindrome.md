## 문제 정의

1. 회문은 모든 문자열이 뒤집었을 시 동일해야 회문의 조건을 충족함
    1. 즉, 회문이 아닌 문장은 단 하나의 문자라도 동일하지 않을 시 이를 충족함 (반복 횟수를 줄일 수 있는 조건이 될 가능성 높음)
2. `전체 문자열이 회문이더라도 단어 하나가 빠지면 회문이 아니게 될 수 있음`
    1. 즉, 단어 하나하나마다 비교를 하는 방법이 가장 직관적이나 이는 당연히 n^n 의 시간 복잡도란 결과를 초래함
    2. 따라서 n^n 을 벗어나는 조건을 찾는다면 문제가 해결됨
    

## 조건 탐색

1. 회문이 아닌 문자임을 확인할 때엔 nlogn * 2 만큼의 시간이 소요될 것으로 보임
2. 회문이 아닌 지의 여부와 그 길이는 가장 긴 회문과 연관성이 높으므로 가장 긴 길이의 회문을 찾고 거기서 부터 줄여나간다면? 
    1. 회문이 매우 짧고 비회문이 긴 경우라면 n^n 과 동일함
        
        <aside>
        💡 AAAAABA 같은 경우도 절대 n^n 을 벗어날 수 없음
        
        </aside>
        
        1. 줄이는 것만이 아니라 늘이는 것도 생각해본다면?
            
            <aside>
            💡 [회문이 매우 짧고 비회문이 긴 경우라면 n^n 과 동일함](https://www.notion.so/n-n-987e00ab319d4ac5b9f9b40ba5eac890?pvs=21) 
            여전히 문제가 발생함
            
            </aside>
            
            1. 하지만 결국 가장 긴 비회문은 가장 긴 회문의 주변에 있을 수밖에 없지 않나?
                
                <aside>
                💡 그러하더라도 n^n 을 벗어날 수 없을 것으로 보임
                
                </aside>
                
                1. 만약 가장 긴 회문이 2개 이상이라면?
                    1. 살려주세요…
                2. 일단 가장 긴 회문을 찾고 거기서 주변을 둘러보게 하자
    
    ## 1차 코드 작성
    
    ```jsx
    import java.io.BufferedReader
    import java.io.InputStreamReader
    
    fun main() {
        var br = BufferedReader(InputStreamReader(System.`in`))
        var bw = System.out.bufferedWriter()
        var input: String = br.readLine()
    
        if (!checkIsPalindrome(input)) {
            bw.write(input.length.toString() + "\n")
        } else {
            bw.write(checkIsNotPalindromeRecursive(input).toString() + "\n")
        }
    
        bw.flush()
    }
    
    fun checkIsPalindrome(input: String): Boolean {
        return input == input.reversed()
    }
    
    fun checkIsNotPalindromeRecursive(input: String): Int {
        if (input.length <= 1) {
            return -1
        }
        if (!checkIsPalindrome(input)) {
            return input.length
        }
    
        if (!checkIsPalindrome(input.substring(0, input.length - 1))) {
            return input.length - 1
        }
    
        if (input[0] == input[input.length - 1]) {
            return checkIsNotPalindromeRecursive(input.substring(1))
        }
    
        return -1
    }
    ```
    
    <aside>
    💡 당연하게도 시간 초과
    
    </aside>
    
    ## 원인 파악
    
    1. 위 코드로 알 수 있는 점은 가장 긴 회문의 주변에 반드시 회문이 아닌 것이 존재한다는 것임
    2. [만약 가장 긴 회문이 2개 이상이라면?](https://www.notion.so/2-057c1eab24024e3d904835f25aa17cd9?pvs=21) 
        1. 위 가정은 어차피 가장 긴 것부터 **하나씩** 찾기 때문에 고려할 필요가 없음
    3. [줄이는 것만이 아니라 늘이는 것도 생각해본다면?](https://www.notion.so/8b8673dbe5424f0ab12b289639556d2b?pvs=21) 
        1. 위 가정 또한 어차피 가장 긴 것부터 **하나씩** 확인하므로 굳이 더 연산을 할 필요가 없음
    4. 4
        
        444444444444444
        
    
    <aside>
    💡 n^n 을 벗어나지 못한 것이 당연히 원인이나,
    생각의 결이 닿지 않음
    
    </aside>
    
    ## 커닝
    
    https://github.com/gro-w-up/algorithm/pull/10/commits/5aa23f07f4898cae26d80bed6ad8e3150dba9e91
    
    에서 한 문자가 달라지면 회문이 아니라는 것이 이해가 되지 않아 테스트
    
    ```jsx
    aaabaaa → 6
    ```
    
    내가 짠 코드에서도 6이 나옴
    
    ```jsx
    abababa → 6
    ```
    
    ```jsx
    abaaba → 5
    ```
    
    마찬가지임
    
    ```jsx
    aaaaba → 6
    ```
    
    애초에 회문이 아님
    
    왜 되지?
