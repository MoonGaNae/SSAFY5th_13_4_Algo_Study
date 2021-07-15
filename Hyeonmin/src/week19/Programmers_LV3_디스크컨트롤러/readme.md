## Programmers Lv3 디스크 컨트롤러
- 힙
- level3

<br>


### 🔍 문제 설명
https://programmers.co.kr/learn/courses/30/lessons/42627

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)


#### 제한사항
jobs의 길이는 1 이상 500 이하입니다.
jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.
<br><br>

###  💡 풀이

클래스
`
static class Node implements Comparable<Node>{
		int time;
		int start;
		
		public Node(int time, int start) {
			super();
			this.time = time;
			this.start = start;
		}

		@Override
		public int compareTo(Node o) {
			return this.time-o.time;
		}
	}
`

변수
`PriorityQueue<Node> pq` : 요청이 된 작업을 소요시간의 오름차순으로 저장할 우선순위 큐
`boolean[] check` : 각 작업이 요청이 완료되었는지 여부를 저장할 배열
`int count` : 요청이 완료된 작업의 수
`int sum` : 각 작업의 요청시간 부터 완료까지 걸린 시간의 합
`int time` : 현재 시간

<br>

우선순위 큐를 사용해서 풀었습니다. 

우선 `jobs` 배열에 담긴 데이터중 요청 시간이 0인 데이터를 오름차순 정렬을 하는 우선순위 큐 `pq`에 저장을 합니다

```
		for (int i = 0; i < jobs.length; i++) {
			if(jobs[i][0] == 0) {
				pq.offer(new Node(jobs[i][1], jobs[i][0]));
				check[i] = true;
				count++;
			}
		}
```

while문을 실행해 `pq`에 있는 현재 요청된 작업중 소요시간이 가장 작은 것을 꺼내 `node`에 저장합니다.
`node.time`값을 `time`에 더해서 해당 작업을 수행한 것으로 하고 `sum`에 그 작업의 요청시간 부터 완료될 때 까지 걸린 시간을 더합니다.
그 후 for문을 통해서 각 작업의 요청시간 `jobs[i][0]`의 값이 현재시간`time`이하인 경우를 모두 `pq`에 저장합니다.

`first`의 값이 `K`이상인 경우는 남은 스코빌 지수들도 모두 `K`이상입니다.
따라서 이 과정을 `pq`에 더이상 값이 없거나 `first`의 값이 `K`와 같거나 클 때까지 반복합니다.

만약 `pq`가 비어있다면 요청된 작업의 수 `count`가 전체 작업의 수와 같은 경우 while문을 중단하고 그렇지 않은 경우 `time`을 1증가시켜서 요청 가능한 작업들이 있는지 확인합니다.

```
		while(true) {
			//우선순위 큐에 값이 더 없는 경우
			if(pq.isEmpty()) {
				//모든 작업 처리가 끝났으면 중단
				if(count == jobs.length)
					break;
				
				//작업이 남았으면 다음 시간으로 이동
				time++;
			}
			//우선수위 큐에 작업이 있는 경우
			else {
				Node node = pq.poll();
				
				//현재 작업을 처리하고 난 후의 시간
				time += node.time;
				
				//현재 작업이 처리될 떄 까지 걸린 시간을 더함
				sum += time - node.start;
			}
			
			//현재 시간 기준 아직 처리하지 않은 작업중 요청이 된 작업을 우선순위 큐에 저장
			for (int i = 0; i < jobs.length; i++) {
				if(!check[i] && jobs[i][0] <= time) {
					pq.offer(new Node(jobs[i][1], jobs[i][0]));
					check[i] = true;
					count++;
				}
			}
			
		}
```

while문의 반복이 끝이나게 되면 `average`에 현재까지 구한 각 작업의 처리까지 걸린 시간 `sum`을 전체 작업의 수로 나누어 저장하고 리턴합니다.

```
		int average = sum/jobs.length;
		
		return average;
```


<br><br>

###  💡 소스코드
```
import java.util.PriorityQueue;

public class Programmers_LV3_디스크컨트롤러 {
	
	//소요 시간으로 정렬되는 클래스
	static class Node implements Comparable<Node>{
		//소요 시간
		int time;
		//요청 시점
		int start;
		
		public Node(int time, int start) {
			super();
			this.time = time;
			this.start = start;
		}

		@Override
		public int compareTo(Node o) {
			return this.time-o.time;
		}
	}
	
	public static void main(String[] args) {
		int[][] jobs = {
				{0,4},
				{0,1},
				{0,2},
				{0,3}
		};
		
		int result = solution(jobs);
		
		System.out.println(result);
	}
	
	static int solution(int[][] jobs) {
		//현재 시점에 요청이 된 작업을 소요 시간으로 정렬할 우선순위 큐
		PriorityQueue<Node> pq = new PriorityQueue<Node>();
		
		//큐에 넣었는지 여부를 저장
		boolean[] check = new boolean[jobs.length];
		
		//큐에 넣은 작업의 수
		int count = 0;
		
		//요청 시간이 0인 작업을 큐에 저장
		for (int i = 0; i < jobs.length; i++) {
			if(jobs[i][0] == 0) {
				pq.offer(new Node(jobs[i][1], jobs[i][0]));
				check[i] = true;
				count++;
			}
		}
		
		//각 작업의 요청 시간에서부터 완료까지의 걸린 시간들의 합
		int sum = 0;
		//현재 시간
		int time = 0;
		
		while(true) {
			//우선순위 큐에 값이 더 없는 경우
			if(pq.isEmpty()) {
				//모든 작업 처리가 끝났으면 중단
				if(count == jobs.length)
					break;
				
				//작업이 남았으면 다음 시간으로 이동
				time++;
			}
			//우선수위 큐에 작업이 있는 경우
			else {
				Node node = pq.poll();
				
				//현재 작업을 처리하고 난 후의 시간
				time += node.time;
				
				//현재 작업이 처리될 떄 까지 걸린 시간을 더함
				sum += time - node.start;
			}
			
			//현재 시간 기준 아직 처리하지 않은 작업중 요청이 된 작업을 우선순위 큐에 저장
			for (int i = 0; i < jobs.length; i++) {
				if(!check[i] && jobs[i][0] <= time) {
					pq.offer(new Node(jobs[i][1], jobs[i][0]));
					check[i] = true;
					count++;
				}
			}
			
		}
		
		//요청부터 처리까지 걸린 시간의 평균
		int average = sum/jobs.length;
		
		return average;
	}
}


```


<br>

