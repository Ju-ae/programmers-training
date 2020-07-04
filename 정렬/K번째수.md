# [코딩테스트 연습] K번째수
>https://programmers.co.kr/learn/courses/30/lessons/42748?language=java

## 문제

### 문제설명
배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
2에서 나온 배열의 3번째 숫자는 5입니다.
배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.


### 제한사항
array의 길이는 1 이상 100 이하입니다.
array의 각 원소는 1 이상 100 이하입니다.
commands의 길이는 1 이상 50 이하입니다.
commands의 각 원소는 길이가 3입니다.


### 입출력 예
```
array
[1, 5, 2, 6, 3, 7, 4]                

commands
[[2, 5, 3], [4, 4, 1], [1, 7, 3]]

return		
[5, 6, 3]

```

## 코드
```
class Solution {
    public int[] split_array(int[] array, int i, int j){
        int len = j - i + 1;
        int[] ret = new int[len];
        int cnt = 0;
        for(int a = i; a<= j; a++){
            ret[cnt] = array[a];
            cnt += 1;
        }

        for(int b = 0; b < len; b++){
            int min = ret[b];
            int m_index = b;
            for(int c = b; c < len; c++){
                if(min > ret[c]){
                    m_index = c;
                    min = ret[c];
                }
            }            
            if(m_index != b){
                ret[m_index] = ret[b];
                ret[b] = min;
            }
        }

        return ret;
    }

    public int[] solution(int[] array, int[][] commands) {
        int[] answer  = new int[commands.length];
        for(int i = 0; i<commands.length; i++){
            int _i = commands[i][0] - 1;
            int _j = commands[i][1] - 1;
            int _k = commands[i][2] - 1;
            int ret[] = split_array(array, _i, _j);
            answer[i] = ret[_k];
        }

        return answer;
    }
}
```
## 다른사람의 풀이


### 함수사용 & 간결한 코드
```
import java.util.Arrays;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];

        for(int i=0; i<commands.length; i++){
            int[] temp = Arrays.copyOfRange(array, commands[i][0]-1, commands[i][1]);
            Arrays.sort(temp);
            answer[i] = temp[commands[i][2]-1];
        }

        return answer;
    }
}
```
- `Arrays.copyOfRange()`
    - Arrays.copyOf(원본배열, 복사할 길이);
    - Arrays.copyOfRange(원본 배열, 시작인덱스, 끝인덱스())

- `Arrays.sort()`
    - Arays.sort(정렬할 배열) : 오름차순으로 정렬됨

### 퀵정렬 구현

```
class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int n = 0;
        int[] ret = new int[commands.length];

        while(n < commands.length){
            int m = commands[n][1] - commands[n][0] + 1;

            if(m == 1){
                ret[n] = array[commands[n++][0]-1];
                continue;
            }

            int[] a = new int[m];
            int j = 0;
            for(int i = commands[n][0]-1; i < commands[n][1]; i++)
                a[j++] = array[i];

            sort(a, 0, m-1);

            ret[n] = a[commands[n++][2]-1];
        }

        return ret;
    }

    void sort(int[] a, int left, int right){
        int pl = left;
        int pr = right;
        int x = a[(pl+pr)/2];

        do{
            while(a[pl] < x) pl++;
            while(a[pr] > x) pr--;
            if(pl <= pr){
                int temp = a[pl];
                a[pl] = a[pr];
                a[pr] = temp;
                pl++;
                pr--;
            }
        }while(pl <= pr);

        if(left < pr) sort(a, left, pr);
        if(right > pl) sort(a, pl, right);
    }
}
```
