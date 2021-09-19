## BOJ 17779 G4 개리멘더링2
- 구현
- gold4

<br>


### 🔍 문제 설명
https://www.acmicpc.net/problem/17779

재현시의 시장 구재현은 지난 몇 년간 게리맨더링을 통해서 자신의 당에게 유리하게 선거구를 획정했다. 견제할 권력이 없어진 구재현은 권력을 매우 부당하게 행사했고, 심지어는 시의 이름도 재현시로 변경했다. 이번 선거에서는 최대한 공평하게 선거구를 획정하려고 한다.

재현시는 크기가 N×N인 격자로 나타낼 수 있다. 격자의 각 칸은 구역을 의미하고, r행 c열에 있는 구역은 (r, c)로 나타낼 수 있다. 구역을 다섯 개의 선거구로 나눠야 하고, 각 구역은 다섯 선거구 중 하나에 포함되어야 한다. 선거구는 구역을 적어도 하나 포함해야 하고, 한 선거구에 포함되어 있는 구역은 모두 연결되어 있어야 한다. 구역 A에서 인접한 구역을 통해서 구역 B로 갈 수 있을 때, 두 구역은 연결되어 있다고 한다. 중간에 통하는 인접한 구역은 0개 이상이어야 하고, 모두 같은 선거구에 포함된 구역이어야 한다.

선거구를 나누는 방법은 다음과 같다.

기준점 (x, y)와 경계의 길이 d1, d2를 정한다. (d1, d2 ≥ 1, 1 ≤ x < x+d1+d2 ≤ N, 1 ≤ y-d1 < y < y+d2 ≤ N)
다음 칸은 경계선이다.
(x, y), (x+1, y-1), ..., (x+d1, y-d1)
(x, y), (x+1, y+1), ..., (x+d2, y+d2)
(x+d1, y-d1), (x+d1+1, y-d1+1), ... (x+d1+d2, y-d1+d2)
(x+d2, y+d2), (x+d2+1, y+d2-1), ..., (x+d2+d1, y+d2-d1)
경계선과 경계선의 안에 포함되어있는 곳은 5번 선거구이다.
5번 선거구에 포함되지 않은 구역 (r, c)의 선거구 번호는 다음 기준을 따른다.
1번 선거구: 1 ≤ r < x+d1, 1 ≤ c ≤ y
2번 선거구: 1 ≤ r ≤ x+d2, y < c ≤ N
3번 선거구: x+d1 ≤ r ≤ N, 1 ≤ c < y-d1+d2
4번 선거구: x+d2 < r ≤ N, y-d1+d2 ≤ c ≤ N


#### 입력
첫째 줄에 재현시의 크기 N이 주어진다.

둘째 줄부터 N개의 줄에 N개의 정수가 주어진다. r행 c열의 정수는 A[r][c]를 의미한다.

#### 출력
첫째 줄에 인구가 가장 많은 선거구와 가장 적은 선거구의 인구 차이의 최솟값을 출력한다.

###  💡 풀이

각 위치의 구역 번호를 2차원 배열`area`에 저장을 한 후에 순회를 하며 구역별 인구를 구해서 풀었다.

5번구역의 외곽에 해당하는 위치를 먼저 구해 `area`에 저장한 후 내부를 5번구역으로 바꾼다

그 후에 1~4번 구역 정보를 `area`에 저장한 후에 배열 전체를 순회해서 최소와 최대 인구를 구해 비교하여 `result`에 저장하였다




<br><br>

###  💡 소스코드
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class BOJ_17779_개리맨더링2 {
	static int N, result = Integer.MAX_VALUE;
	static int[][] map;
	static int[] population;

	public static void main(String[] args) throws Exception{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		N = Integer.parseInt(br.readLine());

		map = new int[N][N];
		
		for (int i = 0; i < N; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			for (int j = 0; j < N; j++) {
				map[i][j] = Integer.parseInt(st.nextToken());
			}
		}
		
		//시작 좌표와 길이에 따른 반복문 실행
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				for (int d1 = 1; d1 <= (N-1)/2; d1++) {
					for (int d2 = 1; d2 <= (N-1)/2; d2++) {
						//범위를 벗어나는 경우들
						if(i+d2 >= N || i-d1 < 0)	continue;
						if(j+d1+d2 >= N)	continue;
						
						countPopulation(i,j,d1,d2);
					}
				}
				
			}
		}
		
		System.out.println(result);
	}
	
	static void countPopulation(int y, int x, int d1, int d2) {
		population = new int[5];
		//각 위치의 구역
		int[][] area = new int[N][N];
		
		//5번 구역 경계선
		for (int i = 0; i <= d1; i++) {
			area[y-i][x+i] = 5;
		}
		
		for (int i = 0; i <= d2; i++) {
			area[y+i][x+i] = 5;
		}
		
		for (int i = 0; i <= d2; i++) {
			area[y-d1+i][x+d1+i] = 5;
		}
		
		for (int i = 0; i <= d1; i++) {
			area[y+d2-i][x+d2+i] = 5;
		}
		
		boolean check = false;
		//5번구역 내부
		for (int i =y-d1+1 ; i <= y+d2-1; i++) {
			for (int j = x; j <= x+d1+d2; j++) {
				if(check) {
					if(area[i][j] == 5) {
						check = false;
						break;
					}
					
					area[i][j] = 5;
				}
				
				if(area[i][j] == 5)
					check = true;
			}
		}
		
		//1번 구역
		for (int i = 0; i < y; i++) {
			for (int j = 0; j <= x+d1; j++) {
				if(area[i][j] == 5)	break;
				
				area[i][j] = 1;
			}
		}

		//2번 구역
		for (int i = 0; i <= y-d1+d2; i++) {
			for (int j = N-1; j > x+d1; j--) {
				if(area[i][j] == 5)	break;
				
				area[i][j] = 2;
			}
		}
		
		//3번 구역
		for (int i = y; i < N; i++) {
			for (int j = 0; j < x+d2; j++) {
				if(area[i][j] == 5)	break;
				
				area[i][j] = 3;
			}
		}
		
		//4번 구역
		for (int i = y-d1+d2+1; i < N; i++) {
			for (int j = N-1; j >= x+d2; j--) {
				if(area[i][j] == 5)	break;
				
				area[i][j] = 4;
			}
		}
		
		int min = Integer.MAX_VALUE;
		int max = 0;
		
		population = new int[5];
		
		//각 구역별 인구 더하기
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				population[area[i][j]-1]+=map[i][j];
			}
		}
		
		//최소, 최대 인구
		for (int i = 0; i < 5; i++) {
			min = Math.min(population[i], min);
			max = Math.max(population[i], max);
		}
		
		result = Math.min(result, max-min);
	}

}





```


<br>



메모리|시간
--|--
36556 KB|340 ms