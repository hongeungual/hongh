from collections import deque

n, l, r = map(int, input().split())


graph = []
for _ in range(n):
    graph.append(list(map(int, input().split())))

dx = [-1, 0, 1, 0]
dy = [0, -1, 0, 1]


def process(x, y, index):
    
    united = []
    united.append((x, y))

    q = deque()
    q.append((x, y))
    union[x][y] = index
    summary = graph[x][y] 
    count = 1 # 현재 연합의 국가 수
   
    while q:
        x, y = q.popleft()
      
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
           
            if 0 <= nx < n and 0 <= ny < n and union[nx][ny] == -1:
                # 옆에 있는 나라와 인구 차이가 L명 이상, R명 이하라면
                if l <= abs(graph[nx][ny] - graph[x][y]) <= r:
                    q.append((nx, ny))
                    # 연합에 추가하기
                    union[nx][ny] = index
                    summary += graph[nx][ny]
                    count += 1
                    united.append((nx, ny))
    # 연합 국가끼리 인구를 분배
    for i, j in united:
        graph[i][j] = summary // count


total_count = 0
while True: # 더 이상 인구 이동을 할 수 없을 때까지 반복
    union = [[-1] * n for _ in range(n)]
    index = 0
    for i in range(n):
        for j in range(n):
            if union[i][j] == -1: # 해당 나라가 아직 처리되지 않았다면
                process(i, j, index)
                index += 1

    # 모든 인구 이동이 끝난 경우
    if index == n * n: (인덱스가 n*n라면 연합이 하나도 없는 경우라서 바로 break)
        break

    else: 
        total_count += 1 (인구이동이 한번이라도 이뤄졌다면)
        -> while문이 돌아간 횟수가 인구이동을 한 횟수

# 인구 이동 횟수 출력
print(total_count)
