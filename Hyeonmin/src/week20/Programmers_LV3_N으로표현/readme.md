## Programmers Lv3 N으로 표현
- DP
- level3

<br>


### 🔍 문제 설명
https://programmers.co.kr/learn/courses/30/lessons/42895

아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)
12 = 55 / 5 + 5 / 5
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.


#### 제한사항
N은 1 이상 9 이하입니다.
number는 1 이상 32,000 이하입니다.
수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
최솟값이 8보다 크면 -1을 return 합니다.
<br><br>

###  💡 풀이

변수
`List<HashSet<Integer>> list` : N을 i개 써서 만들 수 있는 숫자들을 저장할 HashSet으로 만든 리스트
`Iterator<Integer> dpList1, Iterator<Integer> dpList2` : i개의 숫자 조합으로 만들 수 있는 숫자들을 구하기 위한 dpList
`int temp` : i개의 N을 이어붙인 경우 만들어지는 수

<br>

1개의 N으로 만들 수 있는 숫자를 Set에 저장하고 2개의 N으로 만들기 위한 숫자를 구하기 위해서 1개의 N으로 만든 숫자리스트 2개를 사용해 조합하여 2개의 N으로 만들 수 있는 숫자의 리스트에 값을 넣었다.
이런 식으로 i개의 N을 사용해 만들 수 있는 숫자의 조합을 그 이전에 구한 i개보다 적은 N을 사용해 만든 숫자의 리스트들을 조합하여 8개의 N을 사용해 만들 수 있는 숫자의 리스트까지 채워 나갔다.

목표숫자와 수식에 사용한 숫자가 같은 경우 바로 1을 리턴한다

```java
		//시작 숫자가 목표 숫자인 경우
		if(N == number)
			return 1;
```

리스트에 DP를 위해 사용할 HashSet을 각각 할당한다. 이 떄 i번째 HashSet에 N을 i개 이어붙은 숫자들을 저장해놓는다

```java
		//i개의 N을 사용한 경우를 저장할 Set의 리스트를 만들기
		for (int i = 0; i < 9; i++) {
			//N을 i개 이어붙인 숫자
			temp += N*Math.pow(10, i-1);
			
			//목표랑 일치하면 해당 숫자의 수를 반환
			if(temp == number)
				return i;
			
			//리스트의 각 인덱스에 HashSet 할당
			list.add(new HashSet<Integer>());
			
			//각 HashSet에 N을 i개 이어붙인 숫자 저장
			list.get(i).add(temp);
		}
```

i개의 N으로 만들 수 있는 숫자를 저장할 배열에 값을 채워 넣기 위해 for문을 실행한다.
그 안에서 i보다 적은 N 개수로 만든 숫자의 리스트 `dpList1`을 사용한다.

```java
		//N을 i개 써서 나타낼 수 있는 수식으로 구해지는 숫자를 구할 반복문
		for (int i = 2; i < 9; i++) {
			//N을 i개보다 적게 쓴 경우의 숫자들의 조합으로 i개를 쓴 경우를 구함
			for (int j = 1; j < i; j++) {
				//j개의 N을 사용해 구할 수 있는 수의 리스트
				Iterator<Integer> dpList1 = list.get(j).iterator();
```

총 사용한 N의 개수가 i가 되기 위한 리스트 `dpList2`를 사용하여 `dpList1`의 모든 숫자와의 사칙연산 결과를 인덱스가 i인 HashSet에 저장한다

```java
				//리스트에 숫자가 있는 만큼 반복
				while(dpList1.hasNext()) {
					//N을 j개 써서 만든 숫자
					int num = dpList1.next();
					
					//i-j개를 써서 만든 숫자의 리스트
					Iterator<Integer> dpList2 = list.get(i-j).iterator();
					
					//숫자가 남아있는 만큼 반복
					while(dpList2.hasNext()) {
						int num2 = dpList2.next();
						
						//0이 만들어진 경우는 제외
						if(num2 == 0)
							continue;
						
						//두 숫자의 사칙연산으로 i개의 N로 만들 수 있는 숫자들을 구함
						list.get(i).add(num + num2);
						list.get(i).add(num - num2);
						list.get(i).add(num * num2);
						list.get(i).add(num / num2);
					}
					
				}
```

총 사용한 N의 개수가 i개인 리스트에 가능한 모든 숫자를 넣고나서 `list`의 i번째 HashSet에 `numbers`가 있는지 확인하고 있는 경우 i를 반환한다.

```java
				//i개의 N으로 만들 수 있는 모든 수를 구한 다음 number가 그 목록에 있는 경우 i반환
				if(list.get(i).contains(number))
					return i;
```


<br><br>

###  💡 소스코드
```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.Iterator;
import java.util.List;

public class Programmers_LV3_N으로표현 {
	public static void main(String[] args) {
		int N = 5;
		int number = 12;
		
		System.out.println(solution(N, number));
	}

	static int solution(int N, int number) {
		List<HashSet<Integer>> list = new ArrayList<>();
		int temp = 0;
		
		//시작 숫자가 목표 숫자인 경우
		if(N == number)
			return 1;
		
		//i개의 N을 사용한 경우를 저장할 Set의 리스트를 만들기
		for (int i = 0; i < 9; i++) {
			//N을 i개 이어붙인 숫자
			temp += N*Math.pow(10, i-1);
			
			//목표랑 일치하면 해당 숫자의 수를 반환
			if(temp == number)
				return i;
			
			//리스트의 각 인덱스에 HashSet 할당
			list.add(new HashSet<Integer>());
			
			//각 HashSet에 N을 i개 이어붙인 숫자 저장
			list.get(i).add(temp);
		}
		
		//N을 i개 써서 나타낼 수 있는 수식으로 구해지는 숫자를 구할 반복문
		for (int i = 2; i < 9; i++) {
			//N을 i개보다 적게 쓴 경우의 숫자들의 조합으로 i개를 쓴 경우를 구함
			for (int j = 1; j < i; j++) {
				//j개의 N을 사용해 구할 수 있는 수의 리스트
				Iterator<Integer> dpList1 = list.get(j).iterator();
				
				//리스트에 숫자가 있는 만큼 반복
				while(dpList1.hasNext()) {
					//N을 j개 써서 만든 숫자
					int num = dpList1.next();
					
					//i-j개를 써서 만든 숫자의 리스트
					Iterator<Integer> dpList2 = list.get(i-j).iterator();
					
					//숫자가 남아있는 만큼 반복
					while(dpList2.hasNext()) {
						int num2 = dpList2.next();
						
						//0이 만들어진 경우는 제외
						if(num2 == 0)
							continue;
						
						//두 숫자의 사칙연산으로 i개의 N로 만들 수 있는 숫자들을 구함
						list.get(i).add(num + num2);
						list.get(i).add(num - num2);
						list.get(i).add(num * num2);
						list.get(i).add(num / num2);
					}
					
				}
				
				//i개의 N으로 만들 수 있는 모든 수를 구한 다음 number가 그 목록에 있는 경우 i반환
				if(list.get(i).contains(number))
					return i;
			}
			
		}

		//N의 개수가 8개를 넘어가면  -1 리턴
		return -1;
	}

}



```


<br>


테스트 1 〉	통과 (2.33ms, 52.6MB)
테스트 2 〉	통과 (0.07ms, 52.8MB)
테스트 3 〉	통과 (0.12ms, 52.2MB)
테스트 4 〉	통과 (44.98ms, 56.3MB)
테스트 5 〉	통과 (26.14ms, 53.3MB)
테스트 6 〉	통과 (0.54ms, 52MB)
테스트 7 〉	통과 (0.93ms, 52.4MB)
테스트 8 〉	통과 (22.24ms, 54.4MB)
테스트 9 〉	통과 (0.02ms, 53.2MB)