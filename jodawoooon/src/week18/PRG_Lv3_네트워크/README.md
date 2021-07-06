

## Programmers LV3 네트워크
- 깊이/너비 우선 탐색(DFS/BFS)
- level3



<br>


### 🔍 문제 설명

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

#### 제한사항
컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.  
각 컴퓨터는 0부터 n-1인 정수로 표현합니다.  
i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.  
computer[i][i]는 항상 1입니다.  
<br><br>

###  💡 풀이
dfs를 이용하여 방문할 수 있는 노드까지 방문하는 방식으로 풀었다.

연결되어진 노드 한 묶음이 1개의 네트워크를 뜻한다.
따라서 **dfs로 탐색하여 방문 체크 후, 탐색이 끝나면 네트워크 개수 1개를 증가**시켜주면 된다.
<br>

#### 과정

1. 방문을 체크하는 `boolean`형 `visited` 배열을 이용해
n개의 computers 번호에 대해 방문을 체크한다.

2. `visited`이 false라면 해당 정점부터 dfs 탐색을 시작한다.

3. dfs로 노드 방문 시 `visited[idx]=true`하여 방문 체크 한다.

4. 해당 idx 노드에서부터 `computers`를 탐색 해, computers가 1이면 연결 된 것이므로, 해당 노드로 dfs 탐색을 이어간다.


#### 소스코드
```
	static boolean[] visited;
    public int solution(int n, int[][] computers) {
        int answer = 0;
        visited = new boolean[n];
        
        for(int i = 0 ; i < n ; i++){
            
            if(visited[i]) continue; //방문한 네트워크면 continue
            
            dfs(i,n,computers);
            answer++;
            
        }
        

        return answer;
    }
    
    private static void dfs(int idx, int n, int[][] computers){
        visited[idx] = true; //방문체크
        
        //연결된 노드는 모두 방문
        for(int i=0 ; i<n; i++){
            
            if(i==idx) continue; //자기 자신
            if(visited[i]) continue; //이미 방문한 곳이면 continue
            
            //computers[i][j]=1이면 i번 컴퓨터와 j번 컴퓨터가 연결됨
            if(computers[idx][i]==1) 
                dfs(i,n,computers);
        }
    }
```

<br>