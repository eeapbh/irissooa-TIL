# IM대비

## 2309_일곱난쟁이

- 조합으로 풀기

```python
'''
조합으로 하면 되지 않을깡
9개중에 7개를 뽑아서 그 합이 100인것 뽑기
'''
def combination(idx,sidx):
    if sidx == 7:
        if sum(R) == 100:
            for i in sorted(R):
                print(i)
            exit()
            return
        # print('못찾았나')
        return
    if idx == N:
        return
    R[sidx] = height[idx]
    combination(idx+1,sidx+1)
    combination(idx+1,sidx)

height = []
for t in range(9):
    height.append(int(input()))
# print(height)
N = 9
R = [0]*7
combination(0,0)
```

- itertools로 풀기

```python
import sys
from itertools import combinations
input = sys.stdin.readline

def solve(case):
    if sum(case) == 100:
        case = list(case)
        case.sort()
        for old in case:
            print(int(old))
        return True
    return False

if  __name__='__main__':
    child = set()
    for i in range(9):
        nai = int(input().strip())
        if nai <= 100:
            child.add(nai)
    for case in combinations(child,7):
        if solve(case):
            break
```

- 2중 for문으로 풀기(완전탐색)

```python
a = []
for i in range(9):
    a.append(int(input()))
res = sum(a)
a.sort()
for i in range(9):
    for j in range(i+1,9):
        if res-a[i]-a[j] == 100:
            for k in range(9):
                if k==i or k==j:
                    continue
                else:
                    print(a[k])
           	exit()
```

- 스윗...

```python
def combination(idx, sidx):
    global ans
    if ans:#이미 구했다면 수행 x
        return
    if sidx == 7:
        if sum(sel) == 100:
            ans = sel[:]
        return
    if idx == 9:
        return
    sel[sidx] = height[idx]
    combination(idx + 1, sidx + 1)#뽑고가고
    combination(idx + 1, sidx)#안뽑고가고


height = [int(input()) for _ in range(9)]

ans = []
sel = [0] * 7
height.sort()
combination(0, 0)
print("\n".join(map(str, ans)))#출력
```



## 2605_줄세우기

> 나중에 다시 풀기...생각안남...ㅠ

```python
'''
첫번째 ->무조건 0번
2번째-> 0또는 1(0: 그자리그대로, 1: 바로 앞의 학생 앞)
3->0,1,2(뽑은 번호만큼 앞자리로 가서 줄섬)
각자 뽑은 번호는 자기 번호보다 1 작은 범위 내
뽑은 번호를 순서대로 하나씩 보면서 뒤에들어온 수만큼 앞으로 보냄, idx를 순서대로 기록!...
'''

import sys
sys.stdin = open('input.txt','r')

N = int(input())
num = list(map(int,input().split()))
order=[1] #1번은 그자리 그대로니까 미리 넣어두고 시작
for i in range(2,len(num)+1):
    if num[i-1] > 0:
        pass
```



- idx를 다시 정하는 건 알겠는데, 어떻게 리스트에 넣는줄 몰라서 많이 헤맸다.......

- `insert`사용!
- insert : `리스트.insert(입력할index, 값)`

```python
a = [1, 2, 3]
a.insert(1, 5)
a
[1, 5, 2, 3]
```

```python
N = int(input())
choice = list(map(int, input().split()))
ans = []
num = 1
for i in range(N):
    idx = len(ans) - choice[i]
    ans.insert(idx, num)
    num += 1
print(*ans)
```





## 2578_빙고

> ㅎ..이거도 못품 큰일났다아ㅏ아ㅏ....

- 정말....ㅎ 어떻게 풀지..노가다...큰이ㅣㄹ.....ㅠ

```python
'''
25개 칸 빙고
사회자가 부르는 수 차례로 지워감
만약 가로,세로, 대각선 줄이 다 지워진 것이 3개이상 있다면 빙고!
사회자가 몇번쨰수를 부른 후 빙고를 외치는가
사회자가 부른 수를 0으로 바꿈!
for문을 돌면서 i가 같은데 다 0 이거나, j가 같은데 전부 0이거나, i==j인데 전부 0이거나, i==4-j 인데 전부0인것이
3개이상 있다면 빙고!
'''
import sys
sys.stdin = open('input.txt','r')
from pprint import pprint

bingo = [list(map(int,input().split())) for _ in range(5)]
num = [list(map(int,input().split())) for _ in range(5)]
# print(bingo)
# print(num)
#빙고조건에 맞다면 cnt += 1
cnt = 0
for r in range(5):
    for c in range(5):
        dae_l = 0 #대각선 수 셈
        dae_r = 0
        for i in range(5):
            zero_r=0
            zero_c=0
            for j in range(5):
                #num에서 차례대로 나오는 수와 빙고와 일치하는게 있다면 그거 0으로 바꿈
                if num[r][c] == bingo[i][j]:
                    bingo[i][j] = 0

                #for문을 돌면서 i가 같은데 다 0 이거나,
                if bingo[i][j] == 0:
                    # print(zero_r)
                    zero_r+=1

                # j(열)가 같은데 전부 0이거나,
                if bingo[j][i] == 0:
                    zero_c += 1

                # i==j인데 전부 0이거나,
                if i == j and bingo[i][j] == 0:
                    dae_l += 1

                # i==4-j 인데 전부0인것이
                if i == 4-j and bingo[i][j] == 0:
                    dae_r += 1

                if zero_r == 5:
                    print(i,'행 빙고')
                    pprint(bingo)
                    cnt += 1
                    zero_r=0

                if zero_c == 5:
                    print(i,'열 빙고')
                    pprint(bingo)
                    cnt += 1
                    zero_c=0

                if dae_l == 5:
                    print('왼대각선빙고')
                    pprint(bingo)
                    cnt += 1
                    dae_l=0

                if dae_r == 5:
                    print('오대각선빙고')
                    pprint(bingo)
                    cnt += 1
                    dae_r = 0

                #종료조건
                if cnt == 3:
                    print('빙고')
                    pprint(bingo)
                    print(zero_r,zero_c,dae_l,dae_r)
                    print(r*5+c,r,c)
                    exit()

```



- 

```python
#선생님 풀이
def check():
    cnt = 0
    # 가로세로 확인
    for i in range(N):
        cnt_row = 0
        cnt_col = 0
        for j in range(N):
            if bingo[i][j] == 0: cnt_col += 1
            if bingo[j][i] == 0: cnt_row += 1

        if cnt_row == N: cnt += 1
        if cnt_col == N: cnt += 1

    # 대각선, 역대각선 확인
    cnt_l = 0
    cnt_r = 0
    for i in range(N):
        if bingo[i][i] == 0: cnt_l += 1
        if bingo[i][N - 1 - i] == 0: cnt_r += 1

    if cnt_l == N: cnt += 1
    if cnt_r == N: cnt += 1

    if cnt >= 3: return True
    return False


N = 5
bingo = [list(map(int, input().split())) for _ in range(N)]

pos = [0] * 26
#해당 번호 좌표 담기
for i in range(N):
    for j in range(N):
        pos[bingo[i][j]] = (i, j)

call = []

#사회자가 호출하는 번호 한줄로 만들기
for i in range(N):
    call += list(map(int, input().split()))

ans = 0

while not check():
    r, c = pos[call[ans]]
    bingo[r][c] = 0
    ans += 1
print(ans)

```



- 주아언니

```python
def countbingo(v):
    global countList
    a = b = 0
    flag = True
    for i in range(5):
        for j in range(5):
            if arr[i][j] == v:
                a = i
                b = j
                flag = False
                break
        if flag == False: break

    countList[a] += 1
    countList[5+b] += 1
    if a == 2 and b == 2:
        countList[10] += 1
        countList[11] += 1
    if a == b and a!=2 and b!=2:
        countList[10] += 1
    if a + b == 4 and a!=2 and b!=2:
        countList[11] += 1
    return

arr = [list(map(int, input().split())) for _ in range(5)]


countList = [0]*12

given = [list(map(int, input().split())) for _ in range(5)]


count = 0
flag2 = True
for i in range(5):
    for j in range(5):
        count += 1
        a = given[i][j]
        countbingo(a)

        if countList.count(5) >= 3:
            print(count)
            flag2 = False
            break
    if flag2 == False: break
```





## 2563_색종이

```python
'''
가로, 세로 100 정사각형
크기가 각 10
검은 영역 넓이를 구하는 프로그램
'''
N = int(input())
arr = [[0 for j in range(100)] for i in range(100)]
for i in range(N):
    #x는 열, y는 행
    y,x = map(int,input().split())
    for i in range(10):
        for j in range(10):
            arr[y+i][x+j] = 1
    
cnt = 0
for i in range(100):
    for j in range(100):
        if arr[i][j] == 1:
            cnt += 1


print(cnt)
```

- 스윗..

```python
N = int(input())

paper = [[0] * 102 for _ in range(102)]

for i in range(N):
    r, c = map(int, input().split())
    for j in range(r, r + 10):
        for k in range(c, c + 10):
            paper[j][k] = 1

ans = 0
for i in range(102):
    ans += paper[i].count(1)
print(ans)
```





## 2491_수열

```python
'''
수열중에서 다음 수가 연속해서 커지는 것 세고
연속해서 작아지는 것 세고
더 긴것 출력
'''
N = int(input())
# N = 6
arr = list(map(int,input().split()))
# arr = [3, 2, 1, 2, 3, 4]
MAX = 1
MIN = 1
len_list = []
for i in range(1,N):
    #다음수가 크거나 같다면
    if arr[i-1] <= arr[i]:
        # print(arr[i])
        MAX += 1
    #작은수가 나오면 0
    else:
        len_list.append(MAX)
        # print(len_list)
        MAX = 1
len_list.append(MAX)
for i in range(1,N):
    #다음수가 더 작은수가 나온다면
    if arr[i-1] >= arr[i]:
        MIN += 1
    else:
        len_list.append(MIN)
        # print(len_list)
        MIN = 1
len_list.append(MIN)
print(max(len_list))

'''
6
3 2 1 2 3 4
'''
```

- 스윗..

```python
def check(nums):
    global ans
    cnt = 1
    for i in range(1, N):
        if nums[i-1] <= nums[i]: cnt+=1
        else: cnt = 1

        if ans < cnt : ans = cnt


N = int(input())
arr = list(map(int, input().split()))

ans = 1

check(arr)
check(arr[::-1])
print(ans)
```





## 2669_직사각형 네개의 합집합 구하기

```python
'''
좌표가 해당 2차원배열의 idx번호
'''
arr = [[0 for j in range(100)] for i in range(100)]
for _ in range(4):
    x1,y1,x2,y2 = map(int,input().split())
    for i in range(y1,y2):
        for j in range(x1,x2):
            arr[i][j] = 1
cnt = 0
for i in range(100):
    for j in range(100):
        if arr[i][j] == 1:
            cnt += 1
print(cnt)
```

- 스윗..

```python
arr = [[0] * 102 for _ in range(102)]

for i in range(4):
    x1, y1, x2, y2 = map(int, input().split())

    for i in range(x1, x2):
        for j in range(y1, y2):
            arr[i][j] = 1
ans = 0
for i in range(102):
    ans += sum(arr[i])

print(ans)
```





## 13300_방배정

```python
'''
성별이 다르고, 학년이 같아야되고, K명을 넘을 수 없다
방의 최소 개수구하기
'''
N,K = map(int,input().split())
#학생의 성별S(0:여,1:남), 학년 Y
#학년별 idx
G_arr = [0 for _ in range(7)]
B_arr = [0 for _ in range(7)]
for _ in range(N):
    S,Y = map(int,input().split())
    #남자
    if S:
        B_arr[Y] += 1
    #여자
    else:
        G_arr[Y] += 1
# print(B_arr)
# print(G_arr)
#학년이 idx
room = 0
# cnt = 0
for i in range(1,7):
    if B_arr[i] > K:
        if B_arr[i]%K:
            room += B_arr[i]//K + 1
        else:
            room += B_arr[i]//K
    elif B_arr[i]:
        room += 1
    if G_arr[i] > K:
        if G_arr[i] % K:
            room += G_arr[i] // K + 1
        else:
            room += G_arr[i] // K
    elif G_arr[i]:
        room += 1
print(room)

'''
5 2
0 1
0 1
0 1
0 1
0 1
'''
```

- 스윗..

```python
N, K = map(int, input().split())

student = [[0] * 7 for _ in range(2)]

for i in range(N):
    gender, grade = map(int, input().split())

    student[gender][grade] += 1

room = 0

for i in range(2):
    for j in range(1, 7):
        room += student[i][j] // K
        if student[i][j] % K:
            room += 1

print(room)
```



## 2564_경비원

- 이거...ㅎ 못품...ㅎ 어떻게 풀지감도 안옴...

```python
'''
블록의 가로, 세로 길이가 주어짐
상점의 개수와 위치, 동근이의 위치도 주어짐(이동할 때 가로지를수 없음)
동근의 위치와 각 상점사이의 최단 거리 합을 구함
'''
import sys
sys.stdin=open('input.txt','r')
from pprint import pprint

#가로와 세로
c,r = map(int,input().split())
arr = [[0 for j in range(c)] for i in range(r)]
#테두리 1로 둘러주기
for i in range(r):
    for j in range(c):
        if i == 0 or i == r-1:
            arr[i][j] = 1
        else:
            if j == 0 or j == c-1:
                arr[i][j] = 1
# pprint(arr)
#상점의 개수
N = int(input())
# 상점의 위치 1은 블록의 북, 2는 블록의 남, 3은 블록의 서, 4는 블록의 동
#마지막 줄 동근이 위치
for n in range(N):
    x, dist = map(int,input().split())
    # print(x,dist)
    #상점의 위치 표시(N으로)
    if x == 1:
        arr[0][dist-1] = N
    if x == 2:
        arr[r-1][dist-1] = N
    if x == 3:
        arr[dist-1][0] = N
    if x == 4:
        arr[dist-1][c-1] = N
dong_x,dong_dist = map(int,input().split())

# pprint(arr)
#동근이의 위치와 각 상점사이의 최단거리 합...구하기
```

- 다시 선생님 코드 보고 품..후

```python
'''
블록의 가로, 세로 길이가 주어짐
상점의 개수와 위치, 동근이의 위치도 주어짐(이동할 때 가로지를수 없음)
동근의 위치와 각 상점사이의 최단 거리 합을 구함
'''
import sys
sys.stdin=open('input.txt','r')
from pprint import pprint

def clockwise(x,pos):
    if x == 1:#북
        return pos
    elif x == 2:#남
        return c+r+c-pos
    elif x == 3: #서
        return c+r+c+r-pos
    else: #동
        return c+pos

#가로와 세로
c,r = map(int,input().split())
N = int(input())
dist = []
for i in range(N+1):
    x,pos = map(int,input().split())
    dist.append(clockwise(x,pos))
my_dist = dist[-1]
cir = (r+c)*2

result = 0
for i in range(N):
    ans =abs(my_dist-dist[i])
    result += min(ans,cir-ans)
print(result)
```



- 

```python
#선생님은 이렇게 풀으셨더라...ㅠ
def dist_calc(idx, pos):
    if idx == 1:  # 북
        return pos
    elif idx == 2:  # 남
        return C + R + C - pos
    elif idx == 3:  # 서
        return C + R + C + R - pos
    else:  # 동
        return C + pos


C, R = map(int, input().split())
circumference= (C+R)*2

N = int(input())

dist = []
for i in range(N+1):
    idx, pos = map(int,input().split())
    dist.append(dist_calc(idx,pos))

my_dist = dist[-1]

ans = 0

for i in range(N):
    clockwise = abs(my_dist-dist[i])
    ans += min(clockwise,circumference-clockwise)
print(ans)

```





## IM대비

```python
import sys
sys.stdin = open('input.txt','r')
from pprint import pprint
'''
5개 항목 중 정답인 항목 고름
오지선다 형식 객관식 총 M개 문제 주어짐
맞힌 문제 하나당 1점, 연속으로 맞출 경우 1점 가산
가장 높은 점수 받은 학생과 가장 낮은 점수를 받은 학생의 점수차를 출력
정답 list를 받음
N명의 학생들이 제출한 답지를 훑어보면서 답이 맞다면 +1 
그다음 것도 맞다면 연속으로 맞은 개수만큼 곱한값을 더해주다가
틀리면  점수0, cnt도 0
'''

T = int(input())
for tc in range(1,T+1):
    N,M = map(int,input().split())
    score = list(map(int,input().split()))
    # print(score)
    students=[]
    for j in range(N):
        students.append(list(map(int,input().split())))
    # pprint(students)
    students_score=[]
    for student in students:
        S = 0#점수
        cnt = 0
        for i in range(M):
            #정답이라면
            if score[i] == student[i]:
                cnt += 1
                S += 1 *cnt
                # print(S,'정딥',end=' ')
            #오답이라면
            else:
                cnt=0
                # S = 0
                # print(S,'오답',end=' ')
        students_score.append(S)
        # print()
    # print(students_score)
    print('#{} {}'.format(tc,max(students_score)-min(students_score)))
```





## 2628_종이자르기

### 1. DFS로 풀어보기

> 이거 DFS로 풀었는데,,,,테케는 맞았지만 틀렷따!
>
> 아마 idx+1에서 문제가 생긴 것 같은데 IM끝나고 다시 해봐야갰다!
>
> 이거 날림.....너무 복잡하다...런타임에러뜸......ㅠ

```python
import sys
sys.stdin=open('input.txt','r')
from pprint import pprint

di = [0,0,1,-1]#우좌하상
dj = [1,-1,0,0]
def DFS(i,j):
    global cnt
    visited[i][j] = True

    for d in range(4):
        ni = i + di[d]
        nj = j + dj[d]
        if arr[ni][nj] == 0:
            continue
        if visited[ni][nj]:
            continue
        cnt += 1
        DFS(ni,nj)

C,R=map(int,input().split())
N=int(input())
#전체 색종이 배열을 만듦
arr=[[1]*C for _ in range(R)]
#0으로 띠를 둘러줌
arr.insert(0,[0]*C)
arr.append([0]*C)

for x in arr:
    x.insert(0,0)
    x.append(0)
# pprint(arr)

#색종이를 자르는 곳에 0으로 넣어주기
for _ in range(N):
    #가로,세로 & 점선번호
    dir, idx = map(int,input().split())
    #가로라면
    if dir==0:
        #해당 idx에 추가를 해주는데 가로에 추가된만큼 idx가 변동될수있음
        #만약 앞에 0이 들어온게 있다면 idx에 그 수만큼 더해줘야됨..->이걸 어떻게 처리할까?
        #idx앞에 0 이 있으면(제일앞 0 제외) 그 개수만큼 더해줌
        plus = 1
        for i in range(1,idx):
            if arr[1][i] == 0:
                plus += 1
        arr.insert(idx+plus,[0]*(len(arr[0])))
        print('가로추가',plus)
        pprint(arr)
    #세로라면
    else:
        #세로도 마찬가지 idx를 어떻게 지정해줄까...
        for x in arr:
            # 해당 idx에 추가를 해주는데 세로에 추가된만큼 idx가 변동될수있음
            # 만약 앞에 0이 들어온게 있다면 idx에 그 수만큼 더해줘야됨..->이걸 어떻게 처리할까?
            # idx앞에 0 이 있으면(제일앞 0 제외) 그 개수만큼 더해줌
            plus = 1
            for i in range(1, idx):
                if arr[i][1] == 0:
                    plus += 1
            x.insert(idx+plus,0)
        print('세로추가',plus)
        pprint(arr)
print()
pprint(arr)
#dfs돌리기
visited = [[False for j in range(len(arr[0]))] for i in range(len(arr))]
ans = []#넓이 값을 받을 리스트
#세로값이 변동됐으니 len으로 그 값을 받음
for i in range(len(arr)):
    #가로값도 바꼈으니 arr의 첫번째 리스트의
    for j in range(len(arr[0])):
        if arr[i][j] == 1 and visited[i][j] == False:
            cnt = 1
            DFS(i,j)
            ans.append(cnt)
print(max(ans))
```

- 띠를 둘러주는 법

```python
#1. 방법
#0이면 멈춰야되니까 전체 띠를 두름
# 제일 위와 제일 밑에 띠를 두른것처럼 0으로 만들어줌
arr = [0] * (N+2)
arr[0] = arr[N+1] = [0] * (N+2)
for i in range(N):
    #왼쪽 오른쪽에도 띠를 만들어줌
    arr[i+1] = [0]+list(map(int,input().split()))+[0]
    
    
    
#2. insert,append사용
arr = [[1,1,1],[1,1,1],[1,1,1]]
#위, 아래 배열에 0으로 채워줌
arr.insert(0,[0]*len(arr[0])) #3을곱한 배열을 0idx에 삽입해서 앞에 0으로 채움
arr.append([0]*len(arr[0])) #뒤에도 마찬가지로 그만큼 삽입
#오른쪽, 왼쪽 idx에도 0으로 채워줌
#for문을 돌면서 각 배열에 앞 뒤로 0삽입
for x in arr:
    #x배열 0idx에 0채우기
    x.insert(0,0)
    #x배열 제일 뒤에 0채우기
    x.append(0)
print(arr)
'''
[[0, 0, 0, 0, 0],
 [0, 1, 1, 1, 0],
 [0, 1, 1, 1, 0],
 [0, 1, 1, 1, 0],
 [0, 0, 0, 0, 0]]
'''
```



### 2. 현우's idea

> 현우's idea로 다시 풀어봄... 아니 근데 이거 왜 자꾸 런타임에러??????????화난다......후ㅠㅠㅠㅠㅠㅠㅠㅠㅠ

```python
#가로, 세로
c,r = map(int,input().split())
arr = [[1 for j in range(c)] for i in range(r)]
# pprint(arr)
#자를 점선의 개수
N = int(input())
C = [0,r] #가로 idx
R = [0,c] #세로 idx
for _ in range(N):
    #가로인지 세로인지, 점선idx
    dir, idx = map(int,input().split())
    #가로
    if dir == 0:
        C.append(idx)
    else: #세로
        R.append(idx)
R.sort()
C.sort()
# print(R,C)
MAX = 0
#넓이를 어떻게 구하자ㅣ이이이이이이이이
#가로는 리스트 안의 연속된 두개의 차, 세로도 마찬가지
for rr in range(len(R)-1):
    garo = R[rr+1]-R[rr]
    print('가로',garo)
    for cc in range(len(C)-1):
        sero = C[cc+1]-C[cc]
        print('세로',sero)
        area = garo*sero
        print('넓이',area)
        if area > MAX:
            MAX = area
print(MAX)
```



### 3. 자른 가로, 세로 길이를 리스트에 넣고 MAX값만 곱함

> 2번이랑 다를게 없어보이는데....2번은 런타임에러 뜨고 이건 정답...

```python
'''
또..다른 아이디어...
가로, 세로 자른 길이들을 담고 가장 큰 값들끼리 곱하기
'''

#가로, 세로
c,r = map(int,input().split())
arr = [[1 for j in range(c)] for i in range(r)]
# pprint(arr)
#자를 점선의 개수
N = int(input())
C = [0,r] #가로 idx
R = [0,c] #세로 idx

for _ in range(N):
    #가로인지 세로인지, 점선idx
    dir, idx = map(int,input().split())
    #가로
    if dir == 0:
        C.append(idx)
    else: #세로
        R.append(idx)
R.sort()
C.sort()
# print(R,C)
garo,sero = [],[]
#
#넓이를 어떻게 구하자ㅣ이이이이이이이이
#가로는 리스트 안의 연속된 두개의 차, 세로도 마찬가지
for rr in range(len(R)-1):
    garo.append(R[rr+1]-R[rr])
    # print('가로',garo)
    for cc in range(len(C)-1):
        sero.append(C[cc+1]-C[cc])
        # print('세로',sero)
print(max(garo)*max(sero))
```



### 2번 가로,세로 순서를 바꿨더니 됐다....! 왜지????????

> 선생님이 그냥 그대로 제출하니 성공했다고 한다...ㅠ 몰라...

```python
'''
이거도 망함....다시푼다..
가로,세로 자를 배열의 idx를 각각 리스트에 저장
제일 작은 idx부터 잘라진 사각형의 넓이를 구함
넓이를 비교하며 가장 큰 값을 구함
'''
#가로, 세로
c,r = map(int,input().split())
arr = [[1 for j in range(c)] for i in range(r)]
# pprint(arr)
#자를 점선의 개수
N = int(input())
C = [0,r] #가로 idx
R = [0,c] #세로 idx
for _ in range(N):
    #가로인지 세로인지, 점선idx
    dir, idx = map(int,input().split())
    #가로
    if dir == 0:
        C.append(idx)
    else: #세로
        R.append(idx)
R.sort()
C.sort()
# print(R,C)
MAX = 0

#2번에 가로세로 순서만 바꿈!
for cc in range(len(C)-1):
    sero = C[cc+1]-C[cc]
    # print('세로',sero)
    for rr in range(len(R)-1):
        garo = R[rr+1]-R[rr]
        # print('가로',garo)
        area = garo*sero
        # print('넓이',area)
        if area > MAX:
            MAX = area
print(MAX)
```





## 14696_딱지놀이

```python
'''
두 딱지의 별 개수가 다르면 별이 많은 쪽의 딱지가 이김
별의 개수가 같고 동그라미의 개수가 다르면 동그라미가 많은 쪽의 딱지가 이김
이런식으로
별(4) > 동그라미(3) > 네모(2) > 세모(1)의 순서!
A와 B의 딱지문양수를 리스트에 넣고 idx가 제일 큰게 더 클면 이김
'''
import sys
sys.stdin = open('input.txt','r')
#총 라운드 수
N = int(input())
arr = []
for _ in range(2*N):
    temp = list(map(int,input().split()))
    bin = [0]*4
    for i in range(1,len(temp)):
        bin[temp[i]-1] += 1
    arr.append(bin)
# print(arr)
for i in range(0,2*N,2):
    A = arr[i]
    B = arr[i+1]
    for j in range(3,-1,-1):
        if A[j] == B[j]:
            continue
        elif A[j] > B[j]:
            print('A')
            break
        else:
            print('B')
            break
    else:
        print('D')

```



## 2116_주사위쌓기

>이거 선생님 풀이보고 힌트...ㅠ 문제 더 많이 풀어봐야겠따.....
>
>선생님 풀이보고 힌트
>각 idx의 반대면 idx를 만들고
>옆면들 중 최대값만 뽑음!

```python
'''
주사위를 쌓는데 겹친 부분의 수가 같으면 됨
그렇게 쭉 쌓았을 때 옆면의 숫자의 합이 최대인 값을 구해라
겹친숫자가 1부터~6일때 모두 구함
그리고 각 경우의 수에서 옆면의 합중 최대값을 구함
옆면은 아래 위만 빼고 하나씩 뽑아서 다 더한 값의 최대값을 구하면 됨
A(0)-F(5) // B(1)-D(3) // C(2)-E(4)
아래 위가 정해지면 옆면들을 list에 담음...그리고 각 하나씩 값을 더해보면 되려나?
주사위 밑면은 1~6으로 6가지 경우의수를 가짐
예) 밑면이 1일때 해당 idx에 마주보는 면의 idx를 제외한 옆면들을 옆면list에 담는다
다음 주사위는 아래 주사위의 윗면과 같은 수의 idx와 해당 idx와 마주보는 idx를 제외하고 옆면 list에 담는다
이거반복....

'''
import sys
sys.stdin = open('input.txt','r')
from pprint import pprint

#주사위 밑면 idx와 주사위번호를 넣었을 때 그 주사위 옆면의 최고값 반환
def choice(idx,dice_num):
    max_num = 0
    oidx = o_index[idx]
    # print('idx',idx,'oidx',oidx)
    for i in range(6):
        if i == idx or i == oidx:
            continue
        if dice[dice_num][i] > max_num:
            max_num = dice[dice_num][i]
    # print('옆면 max',max_num)
    return max_num

#주사위개수
N = int(input())
dice = []
for _ in range(N):
    arr = list(map(int,input().split()))
    dice.append(arr)
# pprint(dice)
o_index = [5,3,4,1,2,0]

ans = 0
#경우의 수
for num in range(1,7):
    next = num
    temp = 0
    #주사위번호
    for n in range(N):
        #0~5까지의 idx
        for i in range(6):
            if dice[n][i] == next:
                temp += choice(i,n)
                # print(temp)
                next = dice[n][o_index[i]]
                break
    if temp > ans:
        ans = temp
print(ans)
```





## 10163_색종이

```python
'''
평면에 색이 서로 다른 직사각형 색종이 N장
비스듬 X
서로 평행, 수직 둘중 하나
순서대로 색종이가 쌓이면서 이전 것을 가림
N장의 색종이가 주어진 위치에 차례로 놓인 후 각 색종이가 보이는 부분의 면적을 구하는 프로그램 작성
N이 놓일 수 있는 곳의 배열은 100x100
첫번째 색종이부터 숫자를 입력(1)
두번째 -> 2
이렇게 해서 1인거 개수, 2인거 개수,,,N인거 개수 세서 출력!
'''
import sys
sys.stdin = open('input.txt','r')
# from pprint import pprint

#평면 배열
arr = [[0 for j in range(101)] for i in range(101)]
#색종이 장수
N = int(input())
for n in range(1,N+1):
    #색종이 x좌표(열), y좌표(행), 너비, 높이
    x, y, w, h =map(int,input().split())
    #좌표에 순서(n)를 입력
    #행 좌표(y)에서 높이(h)만큼
    for i in range(y,y+h):
        #열 좌표(x)에서 너비(w)만큼
        for j in range(x,x+w):
            #해당 순서를 배열에 넣어줌
            arr[i][j] = n
# pprint(arr)
#순서대로 숫자 개수 세어서 list에 담아주기
area = [0] *(N+1)
for n in range(1,N+1):
    for i in range(101):
        for j in range(101):
            if arr[i][j] == n:
                area[n] += 1
#순서대로 넓이 출력
for a in range(1,N+1):
    print(area[a])
```

- 선생님 풀이

```python
N = int(input())

paper = [[0] * 101 for _ in range(101)]

num = 1

for _ in range(N):
    x, y, w, h = map(int, input().split())
    for i in range(x, x+w):
        for j in range(y, y+h):
            paper[i][j] = num

    num += 1

for i in range(1, N + 1):
    cnt = 0
    for r in range(101):
        #한 행씩 i번째 색종이의 수를 셈(넓이)
        cnt += paper[r].count(i)
    #i번째 색종이 수를 다 세면 출력하고 cnt 다시 1로 리셋
    print(cnt)
```





## 1244_스위치 켜고 끄기

> 오래걸린 이유
>
> 1. 한 줄 에 20까지 출력하는 법
> 2. 남학생 스위치 번호에서 배수는 2배가 아니라 곱!

```python
'''
1은 스위치 켜져있음
0은 스위치 꺼져있음
학생들 1이상, 스위치 개수 이하인 자연수 나눠줌
학생들은 자신의 성별과 받은 수에 따라 스위치 조작
남-스위치번호가 자기가 받은 수의 배수이면 그 스위치 상태를 바꿈
여-자기가 받은 수와 같은 번호가 붙은 스위치를 중심으로 좌우 대칭이면서
가장 많은 스위치를 포함하는 구간을 찾아서 그 구간에 속산 스위치 상태 바꿈
스위치들 처음 상태 주어짐, 각 학생의 성별과 받은 수 주어짐 -> 스위치 마지막 상태 출력
'''
import sys
sys.stdin = open('input.txt','r')

#스위치 상태 바꾸는 함수
def change(num):
    #1, 켜져있으면 0으로
    if switch[num]:
        switch[num] = 0
    #0꺼져있다면
    else:
        switch[num] = 1
    return

#남학생 스위치 바꾸는 함수
def boy(num):
    #num의 배수들을 바꿈
    gop = num
    while num <= N:
        change(num)
        # print(num,'boy',gop)
        #이건 두배지 배수가 아니잖아....
        # num *= 2
        num += gop
    return


#여학생 스위치 바꾸는 함수
def girl(num):
    #방문한곳 방문 표시 후, 전부 바꿔줌
    visited = [False for _ in range(N+1)]
    #받은 num 방문표시
    visited[num] = True
    #그다음 idx에 지정해줄 값
    next = 1
    while True:
        #종료조건
        #범위를 벗어나거나 (num+next,num-next)까지 볼거니까
        if num-next <= 0 or num+next > N:
            break
        #좌우대칭일때까지 계속 상태를 바꿈
        # print(num-next,num+next)
        if switch[num-next] == switch[num+next]:
            visited[num-next] = True
            visited[num+next] = True
            #다음 idx로 넘어감
            next += 1
        #좌우대칭이 아니면 바로끝냄
        else:
            break
    #방문한 곳을 change함
    for v in range(len(visited)):
        #방문했다면
        if visited[v]:
            change(v)
    return

#스위치 개수
N = int(input())

#스위치 상태, index와 숫자 맞춤
switch = [0] + list(map(int,input().split()))

#학생 수
students = int(input())

for student in range(students):
    #학생 상별, 받은 숫자
    sex, num = map(int,input().split())

    #남학생(1) -> 받은숫자 배수 스위치 상태 바꿈
    if sex == 1:
        boy(num)
    #여학생(2) -> 좌우대칭, 가장 스위치 많이 포함 시킨 뒤-> 스위치 상태 바꿈
    else:
        girl(num)

    # print(switch)
# print()
# print(switch)
#20개까지 출력해야됨
s = 1
while s <= N:
    print(switch[s],end=' ')
    if s % 20 == 0:
        print()
    s += 1
```

- 승범's code

```python
import sys
sys.stdin = open("input.txt", "r")

#남학생 스위치 변환(처음 받은 수의 배수 : 1배, 2배, 3배..)
def men(x):
    help_x = x
    while x <= switch_num:
        #스위치 상태 바꾸기 1이라면 0으로, 0이라면 1로
        switchs[x] = (switchs[x] + 1) % 2
        #다음 x(스위치 번호)는 곱!이니까 현재 x에서 원래 스위치번호였던 help_x 더함
        x += help_x
        
#여학생 스위치 변환(처음 받은 수에서 좌우대칭으로 바꿈)
def women(x):
    #a는 좌우대칭으로 더하거나 뺄 idx값
    a = 1
    #스위치 상태 바꾸기
    switchs[x] = (switchs[x] + 1) % 2
    #다음 스위치 번호가 범위 내이고 좌우대칭이라면 while문 돌기
    while (x-a) > 0 and (x+a) <= switch_num and switchs[x-a] == switchs[x+a]:
        #스위치 상태 바꾸기
        switchs[x-a] = (switchs[x-a] + 1) % 2
        switchs[x+a] = (switchs[x+a] + 1) % 2
        a += 1
        
#스위치 개수
switch_num = int(input())
#스위치 처음 상태
switchs = [False] + list(map(int, input().split()))
#학생수
students = int(input())

for _ in range(students):
    #학생정보
    student = list(map(int, input().split()))
    #학생성별
    if student[0] == 1:
        #학생이 처음 받은 스위치 번호 = student[1]
        men(student[1])
    else:
        women(student[1])
        
#한 줄에 20개씩 출력
a = 0
for i in range(1, switch_num+1):
    a += 1
    print(switchs[i], end=' ')
    if a == 20:
        a = 0
        print()
```

- 선생님 코드

```python
#남학생일때 -> 원래 받은 수의 배수번호 스위치 변환
def man(num):
    #스위치 번호만큼 돈다
    for i in range(1, N + 1):
        #num의 배수라면 스위치 변환
        if i % num == 0:
            #1이면 0으로 0이라면 1로
            if onoff[i]:
                onoff[i] = 0
            else:
                onoff[i] = 1

#여학생일때 -> 원래 받은 수의 좌우대칭으로 스위치 변환
def woman(num):
    #오른쪽, 왼쪽 idx를 변수로 따로 지정
    left = num
    right = num
	
    while True:
        #좌우대칭으로 계속 나아감
        left -= 1
        right += 1
        #left, right가 범위 밖이거나 좌우대칭이 아니라면 left와 right는 이전상태로 돌려놓고 while문 종료
        if left <= 0 or right > N or onoff[left] != onoff[right]:
            left += 1
            right -= 1
            break
	#해당 left에서 right까지 스위치 변환
    for i in range(left, right + 1):
        if onoff[i]:
            onoff[i] = 0
        else:
            onoff[i] = 1

#스위치번호
N = int(input())
#스위치 처음 상태
onoff = [0] + list(map(int, input().split()))
#학생수
H = int(input())
for i in range(H):
    #학생성별, 처음 받은 수
    gender, num = map(int, input().split())
    #남학생
    if gender == 1:
        man(num)
    #여학생
    else:
        woman(num)
        
#한 줄에 20개씩 출력
for i in range(1, N + 1):
    print(onoff[i], end=" ")
    if i % 20 == 0:
        print()
```

