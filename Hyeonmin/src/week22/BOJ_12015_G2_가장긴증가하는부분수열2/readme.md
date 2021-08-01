## BOJ 12015 gold2 가장 긴 증가하는 부분 수열 2
- 이분탐색
- gold 2

<br>


### 🔍 문제 설명
https://www.acmicpc.net/problem/12015

수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 구하는 프로그램을 작성하시오.

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {10, 20, 30, 50} 이고, 길이는 4이다.


#### 제한사항
첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000,000)이 주어진다.
둘째 줄에는 수열 A를 이루고 있는 Ai가 주어진다. (1 ≤ Ai ≤ 1,000,000)

#### 출력
첫째 줄에 수열 A의 가장 긴 증가하는 부분 수열의 길이를 출력한다.

<br><br>

###  💡 풀이

변수
`int[] numbers` : 부분 수열을 저장할 배열
`int max` : 부분 수열의 
`int N` : 수열 A의 크기


<br>

이 문제에서는 부분 수열 자체를 구하는게 아니라 그 길이만 구하면 되므로 새로 저장될 숫자가  현재 부분 수열에 저장된 모든 숫자보다 크지 않으면 그 숫자보다 큰 수중에 가장 작은 수가 있는 위치에 저장시키면 된다


입력한 숫자들에 대해서 부분수열의 기존 위치에 있는 숫자 대신 저장을 할 지 새 위치에 저장을 할지 정해서 처리한다
새로 읽은 숫자가 부분 수열에 있는 모든 수보다 크면 부분 수열의 맨 뒤에 새로 저장을 한다 

```java
			int num = Integer.parseInt(st.nextToken());
			
			//이분 탐색을 할 왼쪽 끝 범위
			int left = 0;
			//이분 탐색을 할 오른쪽 끝 범위
			int right = max;
			
			//새로 들어올 숫자가 부분 수열에 있는 모든 숫자보다 크면 뒤에 새로 저장한다
			if(num > numbers[max-1]) {
				numbers[max++] = num;
				continue;
			}
```

새로 읽은 숫자가 부분 수열의 범위에 해당하는 경우는 적절한 위치를 찾는다
부분 수열에 같은 숫자가 있는 경우에는 별도의 처리를 하지 않고 그렇지 않은 경우에는 읽은 숫자보다 큰 수중에 가장 작은 수가 저장된 위치에 새로 읽은 숫자를 저장한다

```java
			int mid = (left+right)/2;
			
			boolean isSame = false;
			
			//이분탐색
			//부분 수열에서 num 숫자보다 큰 수중에 가장 작은 숫자가 있는 위치를 num으로 변경한다
			while(left <= right) {
				mid = (left+right)/2;
				
				//부분 수열에 같은 숫자가 있는 경우
				if(numbers[mid] == num) {
					isSame = true;
					break;
				}
				else if(numbers[mid] > num) {
					right = mid - 1;
				}
				else {
					left = mid + 1;
				}
			}
			
			if(isSame)
				continue;
			
			if(numbers[mid] < num)
				idx = mid+1;
			else
				idx = mid;
			
			numbers[idx++] = num;
		}
```


<br><br>

###  💡 소스코드
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class BOJ_12015_G2_가장긴증가하는부분수열2 {
	static int N;
	static int[] numbers;

	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

		N = Integer.parseInt(br.readLine());

		StringTokenizer st = new StringTokenizer(br.readLine());

		//부분 수열을 저장할 배열
		numbers = new int[N+1];
		
		//부분 수열에서 교체될 숫자가 들어갈 인덱스
		int idx = 1;
		//부분 수열에서 새로 추가될 숫자가 들어갈 인덱스
		int max = 1;

		//배열의 앞 부분부터 숫자를 적절한 위치에 저장
		for (int i = 0; i < N; i++) {
			int num = Integer.parseInt(st.nextToken());
			
			//이분 탐색을 할 왼쪽 끝 범위
			int left = 0;
			//이분 탐색을 할 오른쪽 끝 범위
			int right = max;
			
			//새로 들어올 숫자가 부분 수열에 있는 모든 숫자보다 크면 뒤에 새로 저장한다
			if(num > numbers[max-1]) {
				numbers[max++] = num;
				continue;
			}
			
			int mid = (left+right)/2;
			
			boolean isSame = false;
			
			//이분탐색
			//부분 수열에서 num 숫자보다 큰 수중에 가장 작은 숫자가 있는 위치를 num으로 변경한다
			while(left <= right) {
				mid = (left+right)/2;
				
				//부분 수열에 같은 숫자가 있는 경우
				if(numbers[mid] == num) {
					isSame = true;
					break;
				}
				else if(numbers[mid] > num) {
					right = mid - 1;
				}
				else {
					left = mid + 1;
				}
			}
			
			if(isSame)
				continue;
			
			if(numbers[mid] < num)
				idx = mid+1;
			else
				idx = mid;
			
			numbers[idx++] = num;
		}
		
		System.out.println(max-1);
	}
}



```


<br>



메모리|시간
--|--
121384 KB|544 ms