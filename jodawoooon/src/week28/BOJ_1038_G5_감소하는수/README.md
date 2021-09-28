## BOJ 1038 G5 감소하는 수
- 백트래킹
- gold5



<br><br>


### 🔍 문제 설명
https://www.acmicpc.net/problem/1038

음이 아닌 정수 X의 자릿수가 가장 큰 자릿수부터 작은 자릿수까지 감소한다면, 그 수를 감소하는 수라고 한다. 예를 들어, 321과 950은 감소하는 수지만, 322와 958은 아니다. N번째 감소하는 수를 출력하는 프로그램을 작성하시오. 0은 0번째 감소하는 수이고, 1은 1번째 감소하는 수이다. 만약 N번째 감소하는 수가 없다면 -1을 출력한다.




<br>

#### ✔ 입력
첫째 줄에 N이 주어진다. N은 1,000,000보다 작거나 같은 자연수 또는 0이다.
<br>

#### ✔ 출력
첫째 줄에 N번째 감소하는 수를 출력한다.
<br>


<br>

###  💡 풀이

감소하는 수는 다음과 같다
```
1 ... 9
10 20 21 30 31 32 ...
...
```
그리고 가장 큰 감소하는 수는 9876543210의 10자리 숫자이다.
따라서 길이별로 구분하여 for문을 돌려 감소하는 수 `n`을 구하는 재귀함수를 구현했다.

재귀함수 `getDecNum`은 다음과 같다.

<br>

```java
	private static void getDecNum(int n, int idx, int len) {
		if(idx==len) {
			cnt++; 									//감소하는 수 갯수 증가
			if(cnt==N) ans = n;						//N번째 감소하는 수
			return;
		}

		
		for(int i = 0 ; i< n%10 ; i++) {			//현재 감소하는 수의 맨 끝 자릿수보다 작은 수
			getDecNum(n*10+i, idx+1, len);				//감소하는 수 n의 자리수 높이기 (*10), 감소하는수 + i하여 그 다음 감소하는 수 구하기
		}
	}
```

재귀함수의 `n`, `idx`, `len`은 차례대로 `현재 수`, `현재 자릿수`, `만들 감소하는 수의 길이`를 가리킨다.

따라서 재귀함수의 탈출조건은 `idx==len`으로 만들 감소하는 수의 길이와 현재 자릿수가 일치할 때이다.

<br>
재귀함수 내부에서는 현재 감소하는 수 `n`의 다음 감소하는 수를 구해야하므로  
맨 끝자릿수 보다 작은 수 i에 대하여 붙여가며 재귀를 돌렸다.


<br><br>

###  💬 소스코드

```java
package week28.BOJ_1038_G5_감소하는수;

import java.io.*;
import java.util.*;

/**
 * 
 * 
 * ✨ Algorithm ✨
 * @Problem : BOJ 1038 G5 감소하는수
 * @Author : Daun JO
 * @Date : 2021. 9. 26. 
 *
 */
public class Main_BOJ_1038_G5_감소하는수 {
	static int N,ans,cnt;
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		N = Integer.parseInt(br.readLine());
		
		cnt = -1;
		
		/*
		 * 감소하는 수
		 * 0~10
		 * 10 .. 20... 21.... 90...
		 */
		
		if(N < 10) { 								//1의 자리
			System.out.println(N);
			System.exit(0);
		}
		if(N == 1022) {
			System.out.println("9876543210");
			System.exit(0);
		}
		if(N > 1022) {
			System.out.println(-1);
			System.exit(0);
		}
		
		for(int l =0 ; l <= 10 ; l++) {				// ㅣ = 감소하는 수의 길이. 10자리를 넘어갈 수 없다
			for(int i =0 ; i< 10 ; i++) {			// i = 감소하는 수의 첫번째 자리수
				getDecNum(i,1,l);					// (감소하는 수 , 수의 자릿수, 만들려는 자리수)
				if(cnt>=N) {
					System.out.println(ans);
					System.exit(0);
				}
			}
		}
		
		
		
	}
	private static void getDecNum(int n, int idx, int len) {
		if(idx==len) {
			cnt++; 									//감소하는 수 갯수 증가
			if(cnt==N) ans = n;						//N번째 감소하는 수
			return;
		}

		
		for(int i = 0 ; i< n%10 ; i++) {			//현재 감소하는 수의 맨 끝 자릿수보다 작은 수
			getDecNum(n*10+i, idx+1, len);				//감소하는 수 n의 자리수 높이기 (*10), 감소하는수 + i하여 그 다음 감소하는 수 구하기
		}
	}
	
	
}

```
<br><br>


###  💯 채점 결과
	12956	92