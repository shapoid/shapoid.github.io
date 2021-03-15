# 같은위치malloc

Status: need research
구현버전: 3
문제유형: ignorance
발견법: replit

```c
testfile
//
aaaa
bbbb
//
결과
BUFFER_SIZE == 5 에서 
aaaa
bbbbbbbb 가 나온다.

원인
buffer malloc 하면 *line과 같은 주소로 할당된다. 이유를 모르겠다.
```

```c
buffer = 0;
한 후에 
buffer = malloc(...);
해도 buffer 위치는 동일하다. 왜일까?
```