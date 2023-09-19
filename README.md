# SW Expert Academy 1244. 최대 상금
> [링크][link]

[link]: https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15Khn6AN0CFAYD&categoryId=AV15Khn6AN0CFAYD&categoryType=CODE&problemTitle=1244&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1 "link"

<br/><br/><br/>

## 문제
![문제1](/images/문제1.PNG)
![문제2](/images/문제2.PNG)

<br/><br/><br/>

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

				// ******************** 2. 이미 나왔던 배치의 재탐색을 제외한다.  ********************

				dfs(cnt-1, i, numbers);

				// *****************************************************************************

				numbers[j] = numbers[i];
				numbers[i] = temp;
				
			}
		
	}
	
}
```

<br/><br/><br/>

## 풀이

입력받은 카드의 **개수를 N**
교환해야하는 **횟수를 C**
라고 한다면

단순히 완전탐색을 수행하며 최대 상금을 찾을 경우 N = 6, C = 10 일 경우
5!^10 이라는 매우 큰 경우의 수를 탐색해야 되는 경우가 생길 수 있다.

따라서 그리디적인 방법을 사용하여 교환 횟수와 탐색 횟수를 줄여줘야 한다.

<br/><br/>

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
C-2 가 N-1보다 크다면 C 교환 횟수로 만들 수 있는 최대 상금이 N-1의 교환 횟수 이내에 만들어질 수 있다.
따라서 C-2 의 최대상금과 C의 최대상금은 같다.

### 위를 통해 C의 최대상금은 (N-1보다 클 경우)C-2의 최대상금과 같고
### C-2의 최대상금은 (N-1보다 클 경우)C-4의 최대상금과 같으므로
### 교환 횟수 C로 만들 수 있는 최대 상금은 N-1보다 큰 C-2n 로 만들 수 있는 최대상금과 같다.<br/>
### ***따라서 교환 횟수 C는 C-2n(n은 자연수이고 C-2n은 N-1보다 크다)으로 줄어들 수 있다.***

<br/><br/>

### ***2. 이미 나왔던 배치의 재탐색을 제외한다.***

	현재까지의 교환으로 바뀐 숫자의 배치들은 가장 왼쪽카드 부터의 교환으로 무조건 만들어질 수 있다.
증명

현재까지 바뀐 카드의 수를 n, 현재까지 교환한 횟수를 c라고 하면 c는 n-1보다 클 수밖에 없고 그렇다면 바뀐 뒤의 n의 배치가 처음에 어떤 배치였던지 간에 위 1의 첫번째 증명에서 보았던 것과 같이 교환 위치를 왼쪽부터 오른쪽으로 증가하는 방향으로만 만들 수 있다.

### ***따라서 다음 교환의 첫번째 카드의 위치를 이전 교환까지의 첫번째 카드의 위치 이전 부터 교환하는 것은 그 이전에 이미 나온 배치일 것이므로 다음교환의 첫번째 카드의 시작 위치는 현재교환의 첫번째 카드의 위치부터만 고려하면 된다.***

<br/><br/><br/>

## 추가

``` Java
	if(numbers.length<cnt)
		cnt = numbers.length;
```

인터넷에는 탐색을 시작하기 전 교환횟수를 줄여주는 로직으로 위와 같은 조건문이 많이 돌아다닌다.
하지만 이는 정답 테스트 케이스에는 포함되지 않는 반례가 존재한다.
예를들어 입력으로 주어진 카드가 49 이고 교환 횟수가 3으로 주어진다면 최대 상금은 94이지만 위 로직을 사용하면 49라고 출력된다.
이는 처음 코드를 작성한 사람과 그 이후에 참고한 사람들이 왜 일정교환 횟수 이상이 의미가 없는 것인지에 대한 명확한 이해가 없이 로직을 작성했고 그 로직의 반례를 우연치 않게 정답 테스트 케이스가 포함되지 못했기 때문인 것으로 보인다.
위 로직이 잘못된 이유는 
