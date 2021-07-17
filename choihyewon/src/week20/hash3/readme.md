## Programmers - 위장 
- Hash, Map 
- Level2

🔗[위장 문제 바로가기](https://programmers.co.kr/learn/courses/30/lessons/42578)

## 풀이
저는 hashMap을 이용하여 의상의 종류별로 key값은 의상의 종류, value값은 각 의상 종류의 가지 수를 담아주었습니다. 

~~~java
	for(int i=0; i<clothes.length; i++) {
        	String kind = clothes[i][1];
        	// 의상의 종류별로 각 몇개인지 map에 put한다.
        	if(map.containsKey(kind)) {
        		map.put(kind, map.get(kind)+1);
        	}else {
        		map.put(kind, 1);
        	}
       }
~~~

그리고 key가 있는 만큼 for문을 돌려 각 key의 value + 1을 한 값을 answer에 곱해줍니다.
+1을 하는 이유는 의상을 고를지 안고를지의 가지수를 나타냅니다.


~~~java
	for (String key : map.keySet()) {
        	// 각 옷별로 선택할지 안할지 여부를 곱해준다. 
			answer *= (map.get(key) + 1);
	}
~~~


그리고 아무 의상도 선택하지 않을 경우의 수를 빼주어 답을 구합니다.

~~~java
	answer = answer - 1;
~~~

## 막힌점 
경우의 수를 구하는 부분에서 마지막 모두 입지 않는 경우를 빼주는 이유를 몰라 막혔었습니다. 

## 소스코드
~~~java
import java.util.*;

public class Programmers_위장 {
	public int solution(String[][] clothes) {
        int answer = 1;
        HashMap<String,Integer> map = new HashMap<String,Integer>();
        
        for(int i=0; i<clothes.length; i++) {
        	String kind = clothes[i][1];
        	// 의상의 종류별로 각 몇개인지 map에 put한다.
        	if(map.containsKey(kind)) {
        		map.put(kind, map.get(kind)+1);
        	}else {
        		map.put(kind, 1);
        	}
        }
        
        for (String key : map.keySet()) {
        	// 각 옷별로 선택할지 안할지 여부를 곱해준다. 
			answer *= (map.get(key) + 1);
		}
        
        // 아무도 선택하지 않을 경우를 빼준다. 
        answer = answer - 1;
        
        return answer;
    }

}
~~~

## 결과 

| 정확성  | 테스트 |
|----|----|
|테스트 1 |	통과 (0.06ms, 52.8MB)|
|테스트 2 |	통과 (0.06ms, 53.3MB)|
|테스트 3 |	통과 (0.05ms, 53.1MB)|
|테스트 4 |	통과 (0.06ms, 52.6MB)|
|테스트 5 |	통과 (0.10ms, 52.9MB)|
|테스트 6 |	통과 (0.06ms, 53.4MB)|
|테스트 7 |	통과 (0.07ms, 52.5MB)|
|테스트 8 |	통과 (0.06ms, 53.7MB)|
|테스트 9 |	통과 (0.03ms, 52.6MB)|
|테스트 10 |	통과 (0.05ms, 52MB)|
|테스트 11 |	통과 (0.08ms, 53.3MB)|
|테스트 12 |	통과 (0.10ms, 52.8MB)|
|테스트 13 |	통과 (0.07ms, 52.3MB)|
|테스트 14 |	통과 (0.05ms, 52.1MB)|
|테스트 15 |	통과 (0.07ms, 51.3MB)|
|테스트 16 |	통과 (0.04ms, 52.6MB)|
|테스트 17 |	통과 (0.08ms, 53.4MB)|
|테스트 18 |	통과 (0.06ms, 52.8MB)|
|테스트 19 |	통과 (0.05ms, 52.9MB)|
|테스트 20 |	통과 (0.05ms, 52.1MB)|
|테스트 21 |	통과 (0.05ms, 52.3MB)|
|테스트 22 |	통과 (0.06ms, 52.6MB)|
|테스트 23 |	통과 (0.05ms, 52.3MB)|
|테스트 24 |	통과 (0.06ms, 52.5MB)|
|테스트 25 |	통과 (0.06ms, 52MB)|
|테스트 26 |	통과 (0.06ms, 53.3MB)|
|테스트 27 |	통과 (0.06ms, 52MB)|
|테스트 28 |	통과 (0.07ms, 52.8MB)|