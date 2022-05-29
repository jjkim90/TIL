# 자바 오류 모음



### 향상된 for문에 Scanner 사용

```
for (int i : )
```



### 키보드 입력 받아 최소값 구할 때 출력값 계속 0.

- 키보드로 값을 받기 전에 미리 min에 userNumber[0] 값 넣으면 자동 초기화된 값 0이 들어가서 이후 받는 유저값과 상관없이 최소값이 0으로 나옴.
- 키보드로 값을 받은 이후에 최소값 변수에 0번째 userNumber 값을 넣어야 원활하게 비교가 진행됨.

```java
// 잘못된 코딩. 2번이 1번보다 위쪽에 위치해야 함.
int min = userNumber[0]; // 1번
int count = 0
Scanner sc = new Scanner(System.in);

System.out.println("학생수를 입력하세요.");
count = sc.nextInt();
int[] userNumber = new int[count]; // 2번

System.out.println("점수를 입력하세요.");
for (int i = 0; i < userNumber.length; i++){
	userNumber[i] = scan.nextInt();
}

for(int i = 0; i < userNumber.length; i++){
	if(min <= userNumber[i]){
	min = userNumber[i];
	}
}
System.out.println("최소값 : " + min);
```

