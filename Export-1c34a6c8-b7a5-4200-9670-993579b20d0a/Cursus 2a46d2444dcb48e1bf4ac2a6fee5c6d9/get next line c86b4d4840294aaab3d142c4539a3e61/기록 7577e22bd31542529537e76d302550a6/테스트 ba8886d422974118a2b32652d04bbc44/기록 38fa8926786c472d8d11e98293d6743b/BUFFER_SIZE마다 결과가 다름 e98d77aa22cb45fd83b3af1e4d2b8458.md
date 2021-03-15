# BUFFER_SIZE마다 결과가 다름

Status: in feedback
구현버전: 3
문제유형: semantic error
발견법: replit

```c
testfile
//
aaa
aaaa

 a
//
결과
BUFFER_SIZE >= 6 부터 제대로 나왔다.
1 부터 5까지 결과가 다 다르다.

```