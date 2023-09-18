# SW Expert Academy 1244. 최대 상금
> [링크][link]

[link]: https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15Khn6AN0CFAYD&categoryId=AV15Khn6AN0CFAYD&categoryType=CODE&problemTitle=1244&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1 "link"

<br/><br/>

## 문제
![문제1](/images/문제1.PNG)
![문제2](/images/문제2.PNG)

<br/><br/>

## 코드
``` Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

class Solution
{
	public static int result;
    
	public static void main(String args[]) throws Exception
	{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int T = Integer.parseInt(br.readLine());

		for(int test_case = 1; test_case <= T; test_case++) {
			
			StringTokenizer st = new StringTokenizer(br.readLine());
		
			// 처음 받은 카드 배열로 만들어진 숫자
			int number = Integer.parseInt(st.nextToken());
			
			// 교환해야하는 횟수
			int cnt = Integer.parseInt(st.nextToken());
			
			// 받은 숫자를 우선 교환하기 편하게 만들기 위해 문자의 배열로 변환
			char[] numbers = Integer.toString(number).toCharArray();
			
			// 정답을 저장할 변수의 초기화
			result = 0;
			
			// ******************** 1. 일정 교환 횟수 이상을 넘어가면 의미가 없으므로  ********************
			// ********************	   교환 횟수를 가능한 많이 줄여 놓는다.           ******************

			if (numbers.length - 1 < cnt) 
				cnt = numbers.length - 1 + (cnt - numbers.length + 1) % 2;

			// *****************************************************************************
		
			// 완전탐색을 수행하며 만들 수 있는 최대 숫자를 찾는다
			dfs(cnt,0, numbers);
            
			// 정답을 출력한다
			System.out.println("#" + test_case + " " + result);
            
		}
		
	}
	
	// 완전 탐색
	public static void dfs(int cnt, int start, char[] numbers) {
		
		// 교환 횟수를 전부 수행했다면 종료
		if(cnt==0) {
			
			// 현재 카드의 배치를 숫자로 변환 한다 
			int current = Integer.parseInt(new String(numbers));
			
			// 만들어진 숫자가 기존의 정답보다 크다면 갱신한다
			if(current>result)
				result = current;
			
			
			return;
			
		}

		// 교환 횟수만큼 가능한 모든 교환을 진행한다
		for(int i=start; i<numbers.length-1; ++i) 
			for(int j=i+1; j<numbers.length; ++j) {
				
				// 교환 횟수 1을 소모 하여 i, j 위치의 카드들을 교환한뒤 다음 교환을 진행한다
                char temp = numbers[i];
				numbers[i] = numbers[j];
				numbers[j] = temp;

				// ******************** 2. 다음교환 시작 위치가 0이 아닌 i인 이유는           *****************
				// ********************    0부터 i까지의 교환은 그 이전에 이미 나왔기 때문이다 ***************

				dfs(cnt-1, i, numbers);

				// ****************************************************************************

				numbers[j] = numbers[i];
				numbers[i] = temp;
				
			}
		
	}
	
}
```
