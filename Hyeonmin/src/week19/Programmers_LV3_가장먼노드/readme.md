## Programmers Lv3 가장 먼 노드

- 그래프
- level3

<br>


### 🔍 문제 설명
https://programmers.co.kr/learn/courses/30/lessons/49189

n개의 노드가 있을 때 1부터 n번 까지의 노드에 대한 양방향 간선의 정보를 2차원 배열 edge가 주어진다. 

1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.


#### 제한사항
노드의 개수 n은 2 이상 20,000 이하입니다.
간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
edge 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.
<br><br>

###  💡 풀이


전역 변수
`boolean[][] map` : 두 노드의 연결 여부를 저장하는 2차원 boolean 배열
`boolean[] visited` : bfs 실행 시 방문 여부를 저장하는 boolean 배열 

<br>

BFS로 풀었습니다. 

우선 edge 배열에 담긴 데이터를 통해서 map 배열에 각 노드간의 연결 상태를 저장합니다. edge는 양방향 간선이기 때문에 map에 저장할 때도 양방향으로 체크를 해줍니다.

```
		//edge에 저장된 간선 정보로 map배열에 노드간의 연결 표시
		for (int i = 0; i < edge.length; i++) {
			int from = edge[i][0];
			int to = edge[i][1];
			
			map[from][to] = true;
			map[to][from] = true;
		}
```

1번 노드를 시작으로 1번의 연결로 갈 수 있는 노드들을 `queue`에 저장합니다. 그 다음 또다시 `queue`에 들어있는 노드들에서 1번의 연결로 갈 수 있는 노드들을 추가합니다. `queue`에 노드를 추가 할 때는 자신과 같은 번호의 노드는 추가하지 않고 `map`과 `visited`를 확인하여 연결된 노드중에 아직 방문하지 않은 경우에만 추가합니다.

```
				for (int j = 1; j < map.length; j++) {
					//자기 자신이거나 연결이 안된 노드인 경우는 스킵
					if(nodeNum == j || !map[nodeNum][j])	continue;
					//이미 방문한 노드인 경우는 스킵
					if(visited[j]) continue; 
					
					//방문 체크
					visited[j] = true;
					
					//큐에 저장
					queue.offer(j);
				}
```

이 때 `count`변수를 두어서 큐에 들어있는 데이터의 수만큼씩만 반복하였는데, 이는 bfs를 실행할 때 1노드로 부터 거리가 같은 노드들에 대해서 bfs를 일괄 수행을 하고 그 후에 거리가 1 더 먼 노드들에 대해서 차례로 수행하기 위해서입니다.

```
		while(!queue.isEmpty()) {
			//현재 큐에 들어있는 노드들은 1부터의 거리가 같은 노드들 이므로 이떄의 큐 사이즈가 1부터 가장 멀리있는 노드들의 수이다
			count = queue.size();
			
			for (int i = 0; i < count; i++) {
				//큐에 있는 노드 번호
				int nodeNum = queue.poll();
				
				for (int j = 1; j < map.length; j++) {
					//자기 자신이거나 연결이 안된 노드인 경우는 스킵
					if(nodeNum == j || !map[nodeNum][j])	continue;
					//이미 방문한 노드인 경우는 스킵
					if(visited[j]) continue; 
					
					//방문 체크
					visited[j] = true;
					
					//큐에 저장
					queue.offer(j);
				}
			}
		}
```


큐에 더이상 데이터가 존재하지 않게 되었을 때 마지막으로 저장된 `count`값이 가장 먼 거리에 있는 노드들의 수가 됩니다.


<br><br>

###  💡 소스코드
```
import java.util.LinkedList;
import java.util.Queue;

public class Programmers_LV3_가장먼노드 {
	static boolean[][] map;
	static boolean[] visited;
	
	public static void main(String[] args) {
		int n = 6;
		int[][] edge = {
				{3,6},
				{4,3},
				{3,2},
				{1,3},
				{1,2},
				{2,4},
				{5,2}
		};
		
		int result = solution(n, edge);
		
		System.out.println(result);
	}
	
	static int solution(int n, int[][] edge) {
		//각 노드의 연결 여부를 저장할 2차원 배열
		map = new boolean[n+1][n+1];
		//bfs에서 방문 여부를 저장할 2차원 배열
		visited = new boolean[n+1];
		
		//edge에 저장된 간선 정보로 map배열에 노드간의 연결 표시
		for (int i = 0; i < edge.length; i++) {
			int from = edge[i][0];
			int to = edge[i][1];
			
			map[from][to] = true;
			map[to][from] = true;
		}
		
		return bfs();
	}
	
	//1번 노드에서 가장 멀리 떨어진 노드의 수를 리턴하는 메소드
	static int bfs() {
		//bfs실행을 위해 노드의 번호를 저장할 큐
		Queue<Integer> queue = new LinkedList<Integer>();
		
		//1번 노드를 큐에 저장
		queue.offer(1);
		//1번 노드 방문 체크
		visited[1] = true;
		
		//현재 1번 노드에서 가장 멀리 떨어져 있는 노드의 수
		int count = 0;
		
		while(!queue.isEmpty()) {
			//현재 큐에 들어있는 노드들은 1부터의 거리가 같은 노드들 이므로 이떄의 큐 사이즈가 1부터 가장 멀리있는 노드들의 수이다
			count = queue.size();
			
			for (int i = 0; i < count; i++) {
				//큐에 있는 노드 번호
				int nodeNum = queue.poll();
				
				for (int j = 1; j < map.length; j++) {
					//자기 자신이거나 연결이 안된 노드인 경우는 스킵
					if(nodeNum == j || !map[nodeNum][j])	continue;
					//이미 방문한 노드인 경우는 스킵
					if(visited[j]) continue; 
					
					//방문 체크
					visited[j] = true;
					
					//큐에 저장
					queue.offer(j);
				}
			}
		}
		
		return count;
	}
}

```


<br>