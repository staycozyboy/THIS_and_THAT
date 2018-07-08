# THIS & THAT
## 이것저것 정리 #1

새벽에 겸사겸사 이것저것 짜잘한 팁같은 거 정리하려고 하는데 혹시 읽으시는 분 있으면 <br> 틀린 부분같은거 알려주시면 매우매우 감사함다!!
### STACK/ QUEUE / DEQUE
#### STACK과 QUEUE
STACK과 QUEUE는 매우 기본적인 자료구조입니다. 가장 직관적인 예시로 STACK은 택배 상하차, QUEUE는 은행창구를 떠올리시면 됩니다. 아래의 사진이 두 가지 개념을 모두 설명해주고 있는것 같습니다...


![image](https://user-images.githubusercontent.com/37537248/42273837-5fce3d9c-7fc5-11e8-81e6-cbdddb644161.png)


STACK은 LIFO 구조로 구현되어 있으며, push로 맨 위에 쌓고 pop으로 맨 위의 자료를 꺼내 쓸 수 있습니다.


![image](https://user-images.githubusercontent.com/37537248/42273875-7d453826-7fc5-11e8-88ab-9c195efca4b6.png)


QUEUE는 STACK과 반대되는 개념입니다. FIFO 구조로 구현되어 있으며, Push를 통해 쌓은 자료에서 가장 먼저 넣은 자료를 pop을 통해 이용할 수 있습니다.

##### *원형 queue
선형 큐의 문제점(배열로 큐를 선언할시 큐의 삭제와 생성이 계속 일어났을때, 마지막 배열에 도달후 실제로는 데이터공간이 남아있지만 오버플로가 발생)을 보완한 것이 환형 큐입니다. front가 큐의 끝에 닿으면 큐의 맨 앞으로 자료를 보내어 원형으로 연결 하는 방식입니다. 


#### DEQUE (Double-Ended Queue)
DEQUE는 QUEUE와 STACK이 혼합된 개념으로 이해하시면 됩니다. 아래의 그림에서 보시다시피 앞과 뒤 양쪽에서 Pop과 Push연산이 사용가능합니다.


![image](https://user-images.githubusercontent.com/37537248/42273892-90a00b76-7fc5-11e8-94f1-584510b1e321.png)



### PYTHON과 STACK/ QUEUE / DEQUE
#### 직접 구현 (백준 10845번)
```python
class Queue:
    def __init__(self):
        self.items = []
    def empty(self):
        return self.items == []
    def push(self,item):
        return self.items.append(item)
    def pop(self):
        try: return self.items.pop(0)
        except IndexError as e: return -1
    def front(self):
        try: return self.items[0]
        except IndexError as e: return -1
    def back(self):
        try: return self.items[-1]
        except IndexError as e: return -1
    def size(self): return len(self.items)

q = Queue() #큐!
```
설명은 생략하겠습니다.

#### `from queue import Queue`를 이용한 구현
```python
from queue import Queue

q = Queue(maxsize=) #큐!
```
(설명 생략)

#### 모듈내의 deque 이용
```python
from queue import deque

q1 = deque(maxlen=)
```
또한 이것도 됩니다. 둘이 무슨 차이가 있는지는 잘 모르겠습니다..
```python
from collections import deque

q2 = deque(maxlen=)
```
### 백준...

앞서 살펴보았던 세 가지 자료구조의 경우 개념은 매우 간단합니다. 중요한 건 얘네들을 잘 이용해서 써먹는 거겠죠..? 그냥 효율적인 코드를 짜는 것을 넘어 기본적인 자료구조를 바탕으로 구현된 알고리즘도 찾아보면 되게 많더라구요. 저는 그런데 초보자라 배우는 중이고.. [여기](https://blog.naver.com/ndb796/221226794899)에 설명 짱짱 잘 되있으니 참고하시면 될 것 같습니다. (코드는 C로 되어있습니다) 저는 걍 사소해서 아무도 정리안해놓는데 백준풀 때 꽤 쓸만한 팁들이 많더라고요.. 그런거 조금 정리했어용

#### List와 deque
deque의 경우, 앞서 개념을 설명할 때 앞뒤에서 자료를 인출하거나 삽입할 수 있는 형태를 가지고 있다고 적어놨는데요. 그거는 검색해보니까 다 그렇게 나와서 그렇게 적어둔거고,, 사실 deque를 써보면 list랑 별 다를 바가 없습니다. list에서 할 수 있는 거의 모든(모든인가..?) 연산을 다 할 수 있어요. 그런데 같은 연산이라도 효율이 다릅니다. 1874문제를 살펴보겠습니다.. 주석 부분만 읽으시면 됩니다..
```python
import sys
n = int(sys.stdin.readline())
nums = list(range(1,n+1))
stack = [] #리스트
pnm = []  #리스트
numss = []
yesorno = "YES"
for i in range(n):
    numss.append(int(sys.stdin.readline()))

for num in numss:
    if num in stack:  #O(N)
        if num == stack[-1]:
            stack.pop()
            pnm.append('-')
        else:
            yesorno = "NO"
            break
    else:
        while not num in stack:
            stack.append(nums.pop(0)) #O(N)
            pnm.append('+')
        stack.pop()
        pnm.append('-')

if yesorno == 'YES':
    for pm in pnm:
        print(pm)
else: print("NO")

# 상단의 코드는 시간초과 났었던 코드  => O(N^2)
# 하단의 코드는 296MS로 통과했던 코드

import sys
from collections import deque
n = int(sys.stdin.readline())
push_num = 0
stack = deque(maxlen=n) #deque
pnm = deque(maxlen=n*2) #deque
yesorno = True
for i in range(n):
    num = int(sys.stdin.readline())
    if num <= push_num: #요거를 바꿨음
        if num == stack[-1]:
            stack.pop()
            pnm.append('-')
        else: yesorno = False; break
    else:
        for i in range(push_num+1,num+1): #이것도 바꿨구나..
            stack.append(i)
            pnm.append('+')
        stack.pop()
        pnm.append('-')
        push_num = num
if yesorno:
    for pm in pnm:
        print(pm)
else: print("NO")
```

음.. 결론은 이 문제의 경우 deque를 쓰니까 개빨라졌다는겁니다. 그래서 이거 보고 와 deque가 개꿀이구나..! 했는데 아니더라고요. 케바케여서 서로 더 빠른 경우가 다 다릅니다.


list는 정수를 주면 그에 해당하는 위치에 있는 원소를 O(1)만에 가져올 수 있습니다. 그러려면 각 원소가 알맞은 위치에 있어야 하므로, pop(0)를 하면 모든 원소가 한 칸씩 왼쪽으로 이동하기 때문에 O(N)이 걸립니다. 그냥 pop()은 옮길 원소가 없으므로 O(1)만에 됩니다. 한편, deque는 특정 위치를 바로 가져올 수 없지만 (즉 최악의 경우 O(N)) 시작과 끝을 O(1)만에 추가, 삭제할 수 있습니다. array-based list와 linked list의 원리를 찾아보시면 도움이 될 것입니다. 

상단 코드의 문제점은 "num in stack"와 "nums.pop(0)"입니다. 둘 다 O(N)이 걸리므로 전체 코드는 O(N^2)입니다.

좀 짜잘하게 아는 거 많이 정리하고 싶었는데 생각보다 시간이 너무 오래 걸려요....;;;;; ㅠㅠ 백준풀이에 대해서 더 많은 꿀팁을 얻으시고 싶으시면 [이분](https://www.acmicpc.net/blog/view/55)의 글을 참고하시길 바랍니다! ㄹㅇ 글 친절하게 잘 쓰셔유..

이미지 출처 : http://www.leafcats.com/125

