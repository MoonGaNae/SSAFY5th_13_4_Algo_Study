## Programmers - 주식가격 
- Queue 
- Level2

🔗[주식가격 문제 바로가기](https://programmers.co.kr/learn/courses/30/lessons/42584)

## 풀이

먼저 pirces 배열에 담긴 주식가격의 값을 큐에 담고 큐가 빌때까지 반복문을 실행합니다.
큐에서 차례로 꺼낸 값을 현재 index보다 index+1인 순서부터 반복문을 돌아 
만약 현재 가격보다 낮다면 time을 증가시키고 break, 

~~~java
        		if(price>prices[i]) {
        			time++;
        			break;
        		}
~~~
현재가격보다 크거나 같다면 가격이 내려가지 않았으므로 time만 증가시켜줍니다.

~~~java
        		else {
        			time++;
        		}
~~~

그렇게 나온 time의 값을 answer 배열에 각 index에 맞게 저장하여 값을 출력합니다.


## 소스코드
~~~java
import java.util.*;

public class Programmers_주식가격 {	
	public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];
        Queue<Integer> queue = new LinkedList<>();
        
        // 주식가격을 queue에 넣는다.
        for(int i=0; i<prices.length; i++) {
        	queue.add(prices[i]);
        }
        
        // 주식가격의 순서와 현재 시간을 나타내는 변수 
        int index = 0;
        int price = 0;
        
        while(!queue.isEmpty()) {
        	int time = 0;
        	price = queue.poll();
        	for(int i=index+1; i<prices.length; i++) {
        		// 가격이 떨어지는 경우 
        		if(price>prices[i]) {
        			time++;
        			break;
        		}
        		// 가격이 떨어지지 않는 경우 
        		else {
        			time++;
        		}
        	}
        	answer[index] = time;
        	index++;
        
        }
        return answer;
    }

}
~~~

## 결과 

| 정확성  | 테스트 |
|----|----|
|테스트 1 |	통과 (0.15ms, 52.9MB)|
|테스트 2 |	통과 (0.22ms, 52.1MB)|
|테스트 3 |	통과 (1.21ms, 52.4MB)|
|테스트 4 |	통과 (1.11ms, 53.1MB)|
|테스트 5 |	통과 (1.29ms, 53MB)|
|테스트 6 |	통과 (0.19ms, 52.2MB)|
|테스트 7 |	통과 (0.74ms, 53.3MB)|
|테스트 8 |	통과 (0.97ms, 53.1MB)|
|테스트 9 |	통과 (0.19ms, 51.8MB)|
|테스트 10 |	통과 (1.58ms, 53.3MB)|

-------

|효율성 | 테스트 |
|---|---|
|테스트 1 |	통과 (21.60ms, 75.8MB)|
|테스트 2 |	통과 (24.37ms, 67.2MB)|
|테스트 3 |	통과 (34.37ms, 77.1MB)|
|테스트 4 |	통과 (24.29ms, 73.5MB)|
|테스트 5 |	통과 (22.27ms, 65.1MB)|