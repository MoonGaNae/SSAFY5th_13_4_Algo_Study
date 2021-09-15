## Programmers - 경주로 건설 
- bfs
- Level 3 
- 2020 카카오 인턴십 
🔗[경주로 건설 문제 바로가기](https://programmers.co.kr/learn/courses/30/lessons/67259)

## 풀이

이 문제는 최소 비용을 구하는 문제이기 때문에 dfs가 아닌 bfs로 풀었습니다.
먼저 Node 클래스에 행, 열, 방향, 비용 정보를 담아주었습니다.

~~~java
    static class Node{
        int r;
        int c;
        int dir;
        int cost;
        public Node(int r,int c,int dir,int cost){
            this.r = r;
            this.c = c;
            this.dir = dir;
            this.cost = cost;
        }
    }
~~~

그리고 처음에 큐에 시작점 (0,0) 과 비용 0, 방향 -1 을 넣어주었습니다. 그리고 방문 체크 여부를 표시하는 배열의 값을 true로 설정해주었습니다.

그 다음 사방탐색을 해주었습니다. 방향이 -1이거나 이전 방향과 같은 경우에는 현재의 cost에 100원을, 아니라면 600원 (코너와 직선도로를 한번에 만들기 때문에 100+500)을 더해주었습니다.

그리고 방문을 한적이 없거나 board에 저장된 비용보다 적다면 방문 여부를 true로 해주고 더 작은 값으로 갱신해 주었습니다.

## 막힌점
전에 풀었던 문제라서 다시 풀었는데 테스트케이스가 추가 된건지 25번에서 틀렸다고 나옵니다..,, 
해결방법을 못찾아서 일단 25번 테케 빼고 다 정답으로 나오는 코드를 올리겠습니다!



## 소스코드
~~~java
import java.util.*;

class Solution {
    static int[] dr = {-1,1,0,0};
    static int[] dc = {0,0,-1,1};
    static int N;
    static boolean[][] visited;
    static int min = Integer.MAX_VALUE;
    static class Node{
        int r;
        int c;
        int dir;
        int cost;
        public Node(int r,int c,int dir,int cost){
            this.r = r;
            this.c = c;
            this.dir = dir;
            this.cost = cost;
        }
    }
    public int solution(int[][] board) {
        int answer = 0;
        N = board.length;
        visited = new boolean[N][N];
        bfs(board);
        answer = min;
        return answer;
    }
    
    public void bfs(int[][] board){
        Queue<Node> queue = new LinkedList<>();
        queue.add(new Node(0,0,-1,0));
        visited[0][0] = true;
        
        while(!queue.isEmpty()){
            Node node = queue.poll();
            int curDir = node.dir;
            
            // 도착점에 도달했을 때 
            if(node.r==N-1 && node.c==N-1){
                min = Math.min(min,node.cost);
            }
            
            for(int d=0; d<4; d++){
                int nr = node.r + dr[d];
                int nc = node.c + dc[d];
                int cost = node.cost;
                
                // 칸이 벽으로 채워져있지 않거나 위치가 범위내에 있을 때 
                if(nr>=0 && nc>=0 && nr<N && nc<N && board[nr][nc]!=1){
                    int newCost = 0;
                    // 직선도로를 만드는 경우 
                    if(curDir == -1 || curDir == d){
                        newCost = node.cost +  100;
                    }
                    // 코너를 만드는 경우 
                    else{
                        newCost = node.cost + 600;
                    }
                    
                    // 현재 가격과 비교하여 낮은 가격으로 갱신 
                    if(!visited[nr][nc] || board[nr][nc] >= newCost){
                        visited[nr][nc] = true;
                        board[nr][nc] = newCost;
                        queue.add(new Node(nr,nc,d,newCost));
                    }
                }
            }
        }
    }
}

~~~

## 결과 

| 정확성  | 테스트 |
|----|----|
|테스트 1 |	통과 (0.27ms, 72.4MB)|
|테스트 2 |	통과 (0.28ms, 70.4MB)|
|테스트 3 |	통과 (0.27ms, 70MB)|
|테스트 4 |	통과 (0.28ms, 70.8MB)|
|테스트 5 |	통과 (0.28ms, 71.1MB)|
|테스트 6 |	통과 (0.37ms, 70.1MB)|
|테스트 7 |	통과 (0.41ms, 72.1MB)|
|테스트 8 |	통과 (0.37ms, 58.7MB)|
|테스트 9 |	통과 (1.02ms, 59.7MB)|
|테스트 10 |	통과 (1.14ms, 70.8MB)|
|테스트 11 |	통과 (13.83ms, 73.8MB)|
|테스트 12 |	통과 (1.46ms, 59.4MB)|
|테스트 13 |	통과 (0.39ms, 60.4MB)|
|테스트 14 |	통과 (0.45ms, 58.6MB)|
|테스트 15 |	통과 (1.22ms, 60.6MB)|
|테스트 16 |	통과 (1.25ms, 57.8MB)|
|테스트 17 |	통과 (3.31ms, 59.9MB)|
|테스트 18 |	통과 (2.21ms, 71.9MB)|
|테스트 19 |	통과 (9.86ms, 60.3MB)|
|테스트 20 |	통과 (1.04ms, 73.2MB)|
|테스트 21 |	통과 (0.85ms, 71.8MB)|
|테스트 22 |	통과 (0.33ms, 59.7MB)|
|테스트 23 |	통과 (0.31ms, 60.3MB)|
|테스트 24 |	통과 (0.47ms, 58.9MB)|
|테스트 25 |	실패 (0.29ms, 69.1MB)|