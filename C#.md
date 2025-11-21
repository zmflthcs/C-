This is C# Details
========================================



Task
-------------------------------
# Task
### 1. 비동기 메서드/작업의 결과, 완료 상태, 예외 등을 나타내는 객체
### 2.  결과가 없는 경우 Task, 값을 반환하면 Task<T>
### 3. Async 사용시 반환값은 Task, 이벤트의 경우에만 void 쓴다고 볼 수도....

## 예시

1. 기본 실행
// 별도 쓰레드에서 비동기 작업 실행
Task task = Task.Run(() => {
});
task.Wait(); // 동기 대기(강제)
// 또는 await task; (비동기)

2. 결과 반환
Task<int> calcTask = Task.Run(() => {
});

// 결과 받기
int result = calcTask.Result;        // 동기
// 또는 int result = await calcTask; // 비동기(메서드 내부 async 필요)

3. 연속작업
Task<int> baseTask = Task.Run(() => 10);
Task<int> nextTask = baseTask.ContinueWith(t => {
    // baseTask 결과 활용
});

4. 병렬실행
var task1 = Task.Run(() => {
});
var task2 = Task.Run(() => {
});
// 모두 끝날 때까지 대기
await Task.WhenAll(task1, task2);  

## Task 대신 사용되는 아이들
1. ValueTask
-Task와 달리 객체의 생성 오버헤드가 없음. (메모리/성능 최적화) - 캐시 hit, DB 풀
-따라서 빠른 비동기 결과 반환을 위한 경량 async 반환 
  
2. Channel / Pipelines
- 동시성 프로그래밍, 고성능 Producer/Consumer 패턴 구현, 데이터를 스트림 방식으로 비동기 처리
- 네트워크/소켓/파일 입출력, 메시지 서버, 이벤트 스트림
- 여러 Task들이 동시에 데이터를 주고받아야 할 때
- 병목 없는 대용량, 동시 데이터 처리
