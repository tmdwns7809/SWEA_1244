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
			
			// ******************** 1. 완전 탐색을 시작하기 전 의미없는 교환 횟수를 제거한다.  ********************

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

<br/><br/>

## 풀이

입력받은 카드의 **개수를 N**
교환해야하는 **횟수를 C**
라고 한다면

단순히 완전탐색을 수행하며 최대 상금을 찾을 경우 N = 6, C = 10 일 경우
5!^10 이라는 매우 큰 경우의 수를 탐색해야 되는 경우가 생길 수 있다.

따라서 그리디적인 방법을 사용하여 교환 횟수와 탐색 횟수를 줄여줘야 한다.

### ***1. 완전 탐색을 시작하기 전 의미없는 교환 횟수를 제거한다.***

	C 가 N-1 보다 클 경우 C 횟수를 교환하여 만들 수 있는 최대 상금은 교환 횟수가 N-1을 넘기 이전에 이미 만들어 질 수 있다.

증명

C 횟수를 교환하여 만들 수 있는 최대 상금의 배열을 A B C ... 라고 한다면
처음 받은 숫자의 배열이 어떤 수이든지 간에
최대 상금에서의 A의 위치가 처음 배열에서의 A의 위치와 다르다면 첫 번째 교환에서
최대 상금 A의 위치로 바로 옮길 수 있다.
마찬가지로 그 다음 최대 상금에서의 B의 위치가 처음 배열에서의 B의 위치와 다르다면 두 번째 교환에서
최대 상금 B의 위치로 바로 옮길 수 있다.
동일하게 계속 진행하게 될 경우
최대 N-1 번째 교환에서 최대 상금은 무조건 만들어 질 수밖에 없다.

	C-2 가 N-1 보다 클 경우 C-2 횟수를 교환하여 만들 수 있는 최대 상금은 C 횟수를 교환하여 만들 수 있는 최대 상금보다 클 수 없다.

증명

C-2 횟수를 교환했을때 C 횟수를 교환하여 만들 수 있는 최대 상금보다 큰 상금을 만들 수 있게 된다면
그 C-2 횟수를 사용하여 만든 더 큰 상금의 배치에서 어떤 수든지 한 번 서로 바꾸고 그 다음 똑같이 그 수들을 다시 서로 바꾼다면
C 횟수만큼 사용하여 C-2횟수만큼 사용했을때 만들 수 있는 최대상금을 만들 수 있게 된다.
따라서 이는 모순이 되므로 성립하지 않는다.

	따라서 C-2 가 N-1보다 클 때 C-2 교환 횟수로 만들 수 있는 최대 상금은 C 교환 횟수로 만들 수 있는 최대 상금과 같다.
증명

첫 번째 증명에서 C 횟수로 만들 수 있는 최대 상금은 N-1의 교환 횟수를 넘기기 전에 만들어 질 수 있고
두 번째 증명에서 C-2 횟수로 만들 수 있는 최대 상금은 C 교환 횟수로 만들 수 있는 최대 상금보다 클 수가 없으므로
C-2 가 N-1보다 크다면 C 교환 횟수로 만들 수 있는 최대 상금이 N-1의 교환 횟수 이내에 만들어질 수 있으므로
C-2 의 최대상금과 C의 최대상금은 같다.

### 위를 통해 C의 최대상금은 (N-1보다 클 경우)C-2의 최대상금과 같고
### C-2의 최대상금은 (N-1보다 클 경우)C-4의 최대상금과 같으므로
### 교환 횟수 C로 만들 수 있는 최대 상금은 N-1보다 큰 C-2n 로 만들 수 있는 최대상금과 같다.<br/>
### ***따라서 교환 횟수 C는 C-2n(n은 자연수이고 C-2n은 N-1보다 크다)으로 줄어들 수 있다.***
  
