# get next line

Created: Dec 21, 2020 10:56 AM
Status: In Progress

[기록](get%20next%20line%20c86b4d4840294aaab3d142c4539a3e61/%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8%2007c842ce26334a7f92bd0b219b7db54c.csv)

# 개요

## 필수

![get%20next%20line%20c86b4d4840294aaab3d142c4539a3e61/Screenshot_20210221-142207_Adobe_Acrobat.jpg](get%20next%20line%20c86b4d4840294aaab3d142c4539a3e61/Screenshot_20210221-142207_Adobe_Acrobat.jpg)

![get%20next%20line%20c86b4d4840294aaab3d142c4539a3e61/SmartSelect_20210221-143314_Adobe_Acrobat.jpg](get%20next%20line%20c86b4d4840294aaab3d142c4539a3e61/SmartSelect_20210221-143314_Adobe_Acrobat.jpg)

## bonus

The project get_next_line is straightforward and leaves very little room for bonuses, but I am sure that you have a lot of imagination. If you aced perfectly the mandatory part, then by all means complete this bonus part to go further. I repeat, no bonus will be taken into consideration if the mandatory part isn’t perfect.

Turn-in all 3 initial files with _bonus for this part.
• To succeed get_next_line with a single static variable.
• To be able to manage multiple file descriptor with your get_next_line. For example, if the file descriptors 3, 4 and 5 are accessible for reading, then you can call get_next_line once on 3, once on 4, once again on 3 then once on 5 etc. without losing the reading thread on each of the descriptors

# 궁금

Q와 A의 개수 - 꼬리 질문은 Q를 하나 더 쓴다.   ex) Q) read 함수의 리턴타입은? QQ) ssize_t 는 무슨 타입일까?

Q와 A 뒤의 숫자 - 피드백 사이클. 구현하면서 더 많은 것을 알게 되면 더 나은 질문과 답을 덧붙였다. 처음 구현할 때는 ex) Q0 구현 중 질문이 생기면 Q1

구현 하기 전

Q0) 왜 읽어들인 내용을 이중 포인터에 저장하는가?

A0) ft_split의 string 배열이나 ft_lstmap의 리스트 배열을 만들 때 중간에 malloc 에 실패한 경우 처음부터 모두 free 하고 이중 포인터도 free 해야하는 경우가 있었다. 이때 처음 포인터를 가지려면 이중 포인터로 접근할 수 있어야 하기 때문인 것과 관련이 있지 않을까? 읽어들인 내용은 string 하나겠지만 이를 편하게 조작하기 위해선 이중포인터가 필요할 것 같다.

Q0) 뭘 하라는 걸까?

A0) fd 를 통해 '\n' 이 나올 때까지의 메모리 내용을 read 함수로 읽어서 buffer 에 저장한 후 그 내용을 *line 에 다시 저장한 후 읽기 여부를 리턴한다. 맞나?

A1) header 에 stdlib.h, while 문으로 '\n' 까지 읽어서 buffer 에 저장

QQ0) 그러면 gcc -D 옵션으로 지정된 버퍼 크기와 '\n' 까지의 크기를 비교해서 작은 것만큼 읽는 것인가?

QQ0) line 은 매개변수로 들어온 정보이기 때문에 이미 내용이 있는 것 아닌가? fd 에서 내용을 읽어야 하는 함수에 매개볌수로 들어오는 '읽은 내용' 이라는 것은 무엇을 의미 하는가?

QQQ0) 아마도 바로 직전에 읽은 내용인가?

A0)

read 함수를 사용해야 하므로 두번 째 인자에 line 을 사용해야할 것 같다.  

1. read

[https://stackoverflow.com/c/42network/questions/807/812#812](https://stackoverflow.com/c/42network/questions/807/812#812)

이 포스트에 따르면 읽어들인 한 줄의 내용이 line 에 저장되는 것 같다. 

read 함수 두번째 인자에는 따로 선언한 buffer 를 넣어야 할 것 같다. 

QQ0) gnl 을 호출할 때 line 에는 무엇을 집어넣는가?

Q1) 내용이 여러줄일 때 gnl 을 두번째 호출한 경우 두번 째 줄을 읽어야 하는지 어떻게 알까?

A1)몇번 째 줄인지 기억해야 하고 전체 내용을 한 번에 읽을 수는 없으므로 static var 을 이용해야 할 것 같다.

Q1) read의 결과는 어떤 타입으로 저장해야 하는가?

return type 은 ssize_t 이다.

Q0) 어떻게 eof 인 것을 알까?

A0)

[https://stackoverflow.com/questions/1428911/detecting-eof-in-c](https://stackoverflow.com/questions/1428911/detecting-eof-in-c)

A3) textfile  의 경우

[https://stackoverflow.com/questions/38670410/is-eof-hidden-in-txt-file](https://stackoverflow.com/questions/38670410/is-eof-hidden-in-txt-file)

QQ0) read 함수의 결과가 -1 이면 eof 인가?

0이  eof, -1이 에러이다.

첫 구현

Q1) 사용자 시나리오

fd 에 읽은 파일 저장 후 아직 초기화 되지 않은 char *line 의 주소를 매개변수로 gnl 호출 line 에 저장된 내용(한 줄 내용) 출력 → 다음 gnl 호출에서 다음 내용을 출력해야 함 buffer 가 static 이어야 하나?

QQ1) 포인터가 정적 변수인 것은 무엇을 의미하는가?

두번째 구현

Q2) buffer 를 malloc 하고 read 하는 함수를 따로 정의했더니 buffer는 함수 바깥에서 malloc 효과가 없었다.((int)buffer 출력시 0) 함수 호출 후 정적 변수를 다른 함수에 매개변수로 넣을 때 그 함수의 구현은 static 매개변수를 명시해야 하는가?

A2) 잘 모르겠지만 buffer 할당을 get_next_line 함수에서 한 후 read_buffer 함수를 호출했더니 buffer 가 초기화되는 문제는 해결되었다. 왜인지는 모른다.

AA2) → 함수 이름을  read_buffer 에서 read_file 로 바꿔야겠다. file에서 읽어서 buffer 에 저장하는 기능이니까.

Q2) 테스트시 get_next_line 에 넣어줄 파일 디스크립터의 내용은 어떻게 만들어야 하는가?

A2) stdin  에 대한 설명

[https://m.blog.naver.com/PostView.nhn?blogId=tipsware&logNo=220980651156&proxyReferer=https:%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=tipsware&logNo=220980651156&proxyReferer=https:%2F%2Fwww.google.com%2F)

Q2) 임의의 파일에 대해 테스트 하는 법

A3) fcntl.h 가 필요하다.

```c
	char *line;
  int fd;

  fd = open("testfile", O_RDONLY);
  while (get_next_line(fd, &line) > 0)
  {
    printf("\ngnl : %s\n\n", line);
    free(line);
  }

```

Q2) stdin 에서는 '\n'문자를 개행문자로 인식하지 못하는가?

A2) 일반적인 파일 또한 그런듯하다. 개행된 문자열들이 들어간 파일을 따로 만들어서 테스트해야 할 같다.

QQ2) 그러면 printf 에서는 "\n"을 어떻게 개행문자로 처리하는가?

세번째 구현

Q3) 한 줄이 버퍼보다 훨씬 긴 경우 gnl 이 출력해야할 내용의 크기는 버퍼 크기인가 아니면 한 줄 크기인가?

Q3) buf[i] 이런 접근은 어떻게 이뤄지는가?

A3) *(buf + i) 이기 때문에 buf 값을 덮어쓰지 않으면서도 *buf++ 이 방식과 메모리 사용량은 비슷하지 않을까?

Q3) line 의 내용을 덧붙여야 하는 경우 여러번 malloc 할 수 있나?

안되는듯. strnjoin의 결과를 line 으로 업데이트 해야 할 것 같다.

Q3) char * 를 리턴하는 함수의 리턴값이 0인 것과 NULL인 것은 무슨차이인가

Q3)

```c
int main(void)
{
	char    *str;
  size_t  len;

	if (str[0] != 0)
    printf("yes\n");
  len = ft_strlen(str);
  printf("%s\n%zu\n", str, len);
  return 0;
}

output:
yes

1
// 왜 길이가 1인가?
```

Q3)

```c
int main(void) {
  static char *str;
  ssize_t rr;
  
  str = malloc(sizeof(char) * 5);
  rr = read(0, str, 5);
  printf("%s\n",str);
  printf("%d\n",(int)str);
  free(str);
  str = 0;
  printf("%d\n",(int)str);
  return 0;
}

//free(str) 하면 (int)str의 결과가 0이 나오는 줄 알았는데 아니었다.
//
```

Q3) 유효하지 않은 file descriptor 다루기

A3) read 호출시 -1을 출력하기 때문에 따로 다룰 필요 없다고 생각한다.

Q3) free(buffer)한 후  malloc & read 하면 지우기 전 buffer 의 내용이 그대로 덮어씌워져 있다.

A3) free 함수에 대해 좀 더 자세히 알아볼 필요가 있다.

[https://stackoverflow.com/questions/44850286/c-free-is-not-freeing-the-entire-array](https://stackoverflow.com/questions/44850286/c-free-is-not-freeing-the-entire-array) 

그런데 간단한 케이스로 테스트했더니 free 함수는 malloc 된 내용 모두를 지웠다.

Q3) *line이 비어있는 경우는 어떻게 알까?

A3) line_empty  flag 를 따로 선언해서 strnjoin 함수 호출 다음에 line_empty == 0  이 되도록 한다.

Q3) stdin 인 경우 eof 를 어떻게 입력하지?

A3) cntl + d를 입력한다.

[C언어 stdin에 EOF 입력](https://zetawiki.com/wiki/C%EC%96%B8%EC%96%B4_stdin%EC%97%90_EOF_%EC%9E%85%EB%A0%A5)

# 재료

## 1. read 함수

[https://man7.org/linux/man-pages/man2/read.2.html](https://man7.org/linux/man-pages/man2/read.2.html)

### prototype

```jsx
#include <unistd.h>

ssize_t read(int fd, void *buf, size_t count);
```

ssize_t

signed size type. 해당 함수가 실패했을 때 -1 을 반환하기 위한 타입

[https://lacti.github.io/2011/01/08/different-between-size-t-ssize-t/](https://lacti.github.io/2011/01/08/different-between-size-t-ssize-t/)

### description

read() attempts to read up to count bytes from file descriptor fd  into the buffer starting at buf.

On files that support seeking, the read operation commences at the file offset, and the file offset is incremented by the number of bytes read.  If the file offset is at or past the end of file, no bytes are read, and read() returns zero.

If count is zero, read() may detect the errors described below. In the absence of any errors, or if read() does not check for errors, a read() with a count of 0 returns zero and has no other effects.

According to POSIX.1, if count is greater than SSIZE_MAX, the result is implementation-defined; see NOTES for the upper limit on Linux.

### return value

## 2. 정적 변수

varaible life time 이 프로그램의 실행 동안이 됨

[https://en.wikipedia.org/wiki/Static_variable](https://en.wikipedia.org/wiki/Static_variable)

사용 예시와 지역, 전역 정적 변수

[https://dojang.io/mod/page/view.php?id=690](https://dojang.io/mod/page/view.php?id=690)

## 3. free

## 4. option of gcc

-D

## 5. 파일 입출력

[[05] File처리 open, read, write](https://junmung.tistory.com/8)

# 구현

## 0 1 2

buffer 에 read 함수의 결과를 담는다. → 그 전에 buffer  가 비어있지 않고 buffer 를 아직 다 읽지 않았다면 다음 내용을 line 에 담는다.

~~이때 BUFFER__SIZE 와 개행문자 전까지의 문자 개수 중 작은 것만큼의 크기만큼 읽는다. 개행문자 전까지의 문자 개수는 미리 알 수 없으므로 한 글자씩 읽는다.~~ 

→ **tester 에서 read 함수를 BUFFER_SIZE만큼 읽어야 한다는 결과를 얻었다.** 

→ BUFFER_SIZE만큼 read 한 후 개행문자가 중간에 있는 경우 그 전까지 line 에 복사하고 첫 개행문자 다음 포인터를 buffer 에 저장한다.→ 여기서 buffer 가 남은 경우 다음 gnl 호출시  다음 내용을 line 에 넣기 위해서는 buffer 가 static 변수여야 한다는 결론을 얻었다.

buffer  의 내용을 line 에도 복사한다.

## 3

한 줄을 읽기 위해 buffer 를 여러 번 호출해야 하는 경우

while ( 개행문자가 나올 때까지 ) { BUFFER_SIZE만큼 read 후 line 에 붙이기 (strjoin 필요) }

- 한 줄이 BUFFER_SIZE 보다 작을 때도 고려할 수 있는가?

    read 함수를 BUFFER_SIZE로 부르면 파일크기가 그보다 작은 경우 파일크기 만큼만 읽기 때문에 괜찮다.

    while 문에서 나오기 위해 조건에 eof 인 경우를 추가해야 한다.

"개행문자가 나올 때까지"?

- 예를들어 stdin 으로

    ```c
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa //32개
    xyzxyz
    ```

    ```c
    aaaaa aaaaa aaaaa aaaaa aaaaa aaaaa aa\nxy zxyz
    																			//
    ```

    가 들어오고 BUFER_SIZE가 5인 경우 위의 while 문이 끝나기 위해서는 7번째 read 호출 후 line 에 붙여넣을 때 3번째 글자에 개행문자가 나오는 경우를 while 문의 조건으로 저장해야 한다. 

    or strchr 을 이용해 라인피드를 찾는다.

    or strnjoin 을 사용한다

    ```c
    fstrnjoin(char const *s1, char const *s2, size_t n);
    // s2를 앞에서 n 글자만 s1 에 붙인다.
    // *line 이 비어있는 케이스를 고려해서 s2만 n글자만큼 복사한다.
    // -> 할당되지 않은 char* 변수를 매개변수로 사용할 수 있나?
    ```

```c

//1
BUFFER_SIZE = 5

aaaaabbbbbccccc
aaaaabbbbbccccc

aaaaa bbbbb ccccc \naaaa abbbb bcccc c

//2
BUFFER_SIZE = 9

aa
bb
cc
aa
bb
cc

aa\nbb\ncc\n aa\nbb\ncc

//3
BUFFER_SIZE = 5

aaaaabbbbbccccc

```

Q3) line 을 어떻게 할당해야 하나?

```c
gnl(fd, line)
{

	static char *buffer;

	while ( 개행문자가 나올 때까지 )
	{
		buffer 할당 
		BUFFER_SIZE만큼 read해서 buffer 에 저장
		// lf 있는 경우 그 전까지만 붙인다.
		// line이 비어있는 경우도 고려해야 함
		if (buffer 에 lf 없으면)
			line 에 buffer 전체 붙임, buffer 초기화
		else
			line 에 buffer의 lf 전까지 붙이고 while문 나가기 // 다음 gnl 호출 때 lf 다음부터 시작
		
	}
	return (1)
}

// 210310 추가
// strchr 에서 글자 못 찾는 경우 마지막 주소 반환하도록 하면 lf 유무로 분기하지 않아도 될 듯
// buffer 초기화 or 포인터
// buffer 를 여러번 malloc 할 필요가 있나? while 문 밖에서 할당돼있지 않으면 한 번 할당하는 것으로 충분할듯
// buffer 와 line 의 생애주기를 명시적으로 정리할 필요가 있다.
// buffer 의 메모리 공간은 첫 gnl 호출 때 생긴 후 eof 까지 살아있다. 
// 그 내용은 buffer 포인터가 buffer 내용의 끝에 다다르면 free 한 후 read 하면서 갱신된다.
```

수정

```c
gnl(fd, line)
{
	static char *buffer;

	while ( 개행문자가 나올 때까지 )
	{
		buffer 할당 
		BUFFER_SIZE만큼 read해서 buffer 에 저장
		buffer의 개행문자 전까지(없는 경우 전체)의 내용을 line에 이어붙이기
		buffer 옮기기(next_line or free)
	}
	return (1)
}
```

- buffer 시작 포인터 구하기 test  → 구할 필요가 없었다.

```c
size_t  slen(char *s)
{
  size_t len;
  
  len = 0;
  while (s[len])
    len++;
  return (len);
}

int func(char **line)
{
  static char *buf;
  char        **indirect;
  
  indirect = &buf;

  if (!buf)
  {
    buf = malloc(5);
    read(0, buf, 5);
  }
  buf++;
  printf("%zd  , %s\n", (ssize_t)buf, buf);
  if (slen(buf) == 0)
  {
    printf("\n%zd , %s\n", (ssize_t)*indirect, *indirect);
    buf -= 5;
    printf("%zd  , %s\n", (ssize_t)buf, buf);
    free(buf);
    //free(*indirect);
    printf("%zd  , %s\n", (ssize_t)buf, buf);
    buf = 0;

  }
  return (0);

}
int main(void) {
  
  char *line;
  int i;

  i = 5;
  while (i-- > 0)
  { 
    func(&line);
    //free(line);
  }
  return 0;
}

// buffer 의 시작 포인터를 저장하는 방법을 알아내기 위해
// char **indirect 를 정의해서
// indirect = &buf; buf++;
// 한 결과 indirect 는 buf 의 시작 포인터를 저장하지 못했고, buf 가 이동한 만큼 포인터를 뒤로 옮겨 시작 위치로 가야 제대로 free 가 동작 되는 것을 확인했다. 
// 한 칸이라도 잘못 옮기고 free하면 invalid pointer 에러를 냈다.

// buffer 시작 포인터를 알아야 하는 경우
// == gnl 에서 buffer 를 free 해야하는 경우
// 1. strchr 결과 buffer에 lf 가 없어서 buffer 전체를 line 에 복사한 직후
// 2. strchr 결과 buffer에 lf 가 있어서 그 전 내용을 line 에 복사한 후 lf 가 buffer의 마지막 문자임을 확인한 직후
// buffer 를 마지막 글자로 옮긴 후 

```

- 중복 코드 없애기

```c
char            *ft_strchr(const char *s, int c)
{
    size_t  slen;
    size_t  i;

    slen = ft_strlen(s);
    i = 0;
    while (i <= slen) // '\0' 까지 간다.
    {
        if (s[i] == c)
            return ((char *)&s[i]);
        i++;
    }
    return ((char *)&s[i]); // 못 찾은 경우 '\0' 위치 반환, s 가 null terminated 임을 전제 
}

static char     *find_buffer_start(char *buffer) // #2
{
  while (*(--buffer))
    ;
  return (buffer);
}

int             get_next_line(int fd, char **line)
{
    static char     *buffer;
    char            *next_line;
    ssize_t         read_result;
    size_t          lf_found;
		size_t          line_empty; // #3

    if (!line || BUFFER_SIZE < 1 || fd < 0)
        return (-1);
    lf_found = 0;
		line_empty = 1;
    while (!lf_found)
    {
        if (!buffer)
        {
            if ((buffer = malloc(sizeof(char) * (BUFFER_SIZE + 2))) == NULL) // #1 #2
                return (-1);
						buffer[0] = 0; // #2
						buffer++; // #2
            //buffer[BUFFER_SIZE] = 0; // #1
            if ((read_result = read(fd, buffer, BUFFER_SIZE)) == 0
                || read_result == -1)
						{
								if (line_empty) //  #3
								{
										*line = malloc(sizeof(char));
										*line[0] = 0;
								}
                return (read_result); 
						buffer[read_result] = 0; // #1
        }

		next_line = ft_strchr(buffer, '\n');

		// strchr 에서 '\n'을 못 찾아서 마지막 글자 다음 위치를 반환하면 
		// next_line - buffer 는 '\0' 전까지의 글자 개수가 된다.
		if (*next_line == '\n')
				lf_found = 1;
		*line = ft_strnjoin(*line, buffer, next_line - buffer);
		line_empty = 0; // #3
	
		// else 는 buffer 다 읽은 경우
		//if (*next_line) // #1
		if (*next_line && *(next_line + 1)) // #1
				buffer = next_line + 1;
		else 
		{
				// buffer 시작 위치 찾기
			  // buffer = next_line; 굳이 끝으로 옮길 필요가 없어졌다.
				//buffer -= BUFFER_SIZE; // #2
				buffer = find_buffer_starrt(buffer);
				free(buffer);
				buffer = 0;
		}
}

// #1
// buffer 가 '\0' 로 끝나야 strchr 의 결과가 마지막 문자인 경우, 그 다음 문자가 '\0' 인 조건을 사용할 수 있기 때문에 
// buffer 를 한 칸 더 할당한 후 '\0' 를 추가해주었다.
// #2 와 같은 이유로 buffer[read_result]여야 하지 않나?
// buffer 마지막 문자가 '\n' 인 경우 *buffer 가 마지막 '\0'이 된다.

// #2 
// 오류다. 
// 파일 전체크기가 19, BUFFER_SIZE 가 100 인 경우 buffer 시작점으로 가지 못한다.
// buffer 앞에도 '\0' 을 넣으면 해결되지 않을까?
// 이를 위해 buffer 할당시 BUFFER_SIZE + 2 만큼 할당하고 맨 앞에 '\0' 을 넣은 후 buffer를 다음 칸으로 옮겨 read 했다.
// buffer 처음으로 가는 함수를 만들어서 '\0' 가 나올 때까지 buffer 를 앞으로 옮긴 후 free 했다.

// next_line = ft_strchr(buffer, '\n');
// if (*next_line == '\n')
// 
// 두 줄을 
// if (*(next_line  = ft_strchr(buffer, '\n')) == '\n')
// 로 줄일 수 있나?

// #3
// 마지막 줄에 '\n'이 없는 경우에 line 에 내용을 저장하고나서 다시 read가 호출된다.
// 이때 read_result == 0 이 된다. 이때 line을 그대로 출력하는 것이 의도한 바이지만
// 아무 내용도 없는 파일이 들어오는 케이스에서도 read_result == 0이기 때문에 
// 이때 빈 line을 만들어 출력하게 되면 우리가 고려하는 케이스에서는 마지막줄이 빈 문자열로 초기화되어 버린다.
// 두 케이스를 구분하기 위해서 무내용 파일 케이스에서 **line 값이 1임을 알아냈고 이 경우에만 빈 문자열을 만들도록 했다.
// 왜 **line == 1 인지는 모른다. 

// *line 을 malloc 하지 않은 상태의 **line 값은 쓰레기 값이다.
// line_empty flag 를 만들어서 line_empty == 1 인 조건으로 만들었다.
```

- 대안

```c
while (eof 아닐 때까지 BUFFER_SIZE만큼 읽기)
{
  다음 lf 전까지 line 에 넣기
    return 1;
  lf 없으면 line 에 다 넣기
}
// buffer 를 다 읽지 않은 경우 다음 gnl 호출 때 read를 하게 된다.
// while 문 조건으로 lf 검사보다 나은 대안이 있나?
```

012

```c
gnl(fd, **line)
{
	line 에러
	*line 비우기
	BUFFER_SIZE < 1 에러
	
	if (버퍼 처음일 때만)
	{
		버퍼 비우기
		버퍼 할당
		파일 읽어서 버퍼에 저장
	}
	buffer 내용의 개행문자 전까지만 line에 채우기
	다음 버퍼 시작 인덱스 찾기
}
```

## 일반 케이스

- buffer 가 한 줄보다 훨씬 긴 경우 → 0
- 한 줄이 buffer보다 훨씬 긴 경우 → 1
- how does your program behave when you send a newline to the stdout, or ctrl-d

## 고려할 특이 케이스

### 에러

- buffer 나 *line malloc 실패
- read 에러
- free 에러
- line 이 0인 경우
- 임의의 file descriptor

    > get_next_line 수행시 유효하지 않은 임의의 파일 디스크럽터를 전달하세요. (예: 42). 함수는 -1를 리턴해야 합니다.  Pass an arbitrary file descriptor to the get_next_line function on which it is not possible to read, for example 42. The function must return -1.

    [https://wiki.42seoul.work/ko/subjects/get-next-line](https://wiki.42seoul.work/ko/subjects/get-next-line)

    → 어떻게 구분할까? [https://www.notion.so/rhizomized/get-next-line-c86b4d4840294aaab3d142c4539a3e61#2a07a6340f1b4356a41a2714a5d4a9fa](https://www.notion.so/rhizomized/get-next-line-c86b4d4840294aaab3d142c4539a3e61#2a07a6340f1b4356a41a2714a5d4a9fa)

### 크기

- file  크기 < BUFFER_SIZE

    read  의 리턴 값은 file 크기이다. 

- BUFFER_SIZE == 0

### 처음

- 파일이 \n 으로 시작하는 경우

    line_len = 0

- \n 이 연속으로 오는 경우

### 마지막

- 개행문자가 없는 경우
- 마지막 개행 문자 다음의 gnl 호출시 내용을 line 에 넣은 후 리턴 값은 0(eof)인가? 1인가?

    ~~일단 1로 한 뒤 다음 호출에서 0이 나오도록 구현했다.~~  → line 에 넣은 후 0을 리턴해야 한다.(지금 코드는 read 리턴 값이 0이면 바로 0을 리턴하지만 (read 리턴값 < BUFFER_SIZE) 인 경우, 즉 line에 넣을 내용이 남은 동시에 eof 에 도달한 경우에 1을 리턴하도록 되어 있다. 구현을 수정해야 한다.(20210227)

    > tip: 빈 줄은 빈 문자열로 나와야 한다. 예를 들어서 파일이 \n 이라면 get_next_line 을 두 번 실행시킬 수 있어야 하고 두번 다 line 은 빈 문자열로 할당되어야 하고 두번 째 실행때 0(EOF를 만났다는 뜻) 이 리턴되어야 한다.

    [https://wiki.42seoul.work/ko/subjects/get-next-line](https://wiki.42seoul.work/ko/subjects/get-next-line)

buffer : abc\n, abc

file : abc\n

### 메모리

- buffer 를 다 읽어서 다시 read 해야 할 때 기존 메모리를 free 해야 함 → 처음 buffer 를 만드는 경우와 어떻게 구분할 것인가? static variable 은 initialize 되지 않았을 때 값을 가지는가?

```c
static char *b;
char *c;
printf("%lld\n", (long long)b);
printf("%lld\n", (long long)c);

결과
0
140734509372048[아마도 메모리주소값]
```

- gnl 호출시 line이 비어 있지 않은 경우, 할당되어 있는 경우

# 테스트

주소

[https://github.com/harm-smits/gnl-unit-test](https://github.com/harm-smits/gnl-unit-test)

[https://github.com/charMstr/GNL_lover](https://github.com/charMstr/GNL_lover)

[https://github.com/Mazoise/42TESTERS-GNL](https://github.com/Mazoise/42TESTERS-GNL)

## 동작 테스트 in repl.it

### 2 issue

```c
if ((buffer = malloc(sizeof(char) * BUFFER_SIZE)) == NULL)
		return (-1);
read_result = read(0, (void *)buffer, BUFFER_SIZE);

// read 호출 부분에서 멈춘다.
원인: fd == 0 이므로 입력을 받기 위해 기다리는 것이다.
```

```c
static ssize_t	store_line(char **line, char *buffer, size_t start_index)
{
	ssize_t line_len;

	line_len = 0;
	if (buffer[start_index] != '\n') //segfault 1
	{
		while ((buffer[start_index + line_len] != '\n') || (start_index + line_len < BUFFER_SIZE)) // seg fault 2
			line_len++;
  }
	if ((*line = malloc(sizeof(char) * (line_len + 1))) == NULL)
		return (-1);
	ft_strlcpy((void *)*line, (const void *)&buffer[start_index], line_len + 1);
	return (line_len);
}

//buffer[start_index] 에서 segmentation fault 를 일으킨다.
//printf("%d\n", (int)buffer);의 결과로 0이 나온다. 
원인: read_buffer 함수의 malloc 부분을 get_next_line 함수로 옮겼더니 해결되었다. 왜 다른 함수 안에서 malloc 한 buffer 는 그 함수가 끝나면 초기화되는가? 

// whlie 문을 무한히 돈다.
원인: || 가 아니라 && 로 묶여야 한다.
```

```c
int				get_next_line(int fd, char **line)
{
	static char		*buffer;
	static size_t	start_index;
	ssize_t			line_len;
	ssize_t			read_result;

	if (BUFFER_SIZE < 1)
		return (-1);
  
	if (!start_index)
	{
    if ((size_t)buffer)
		free(buffer);
	  if ((buffer = malloc(sizeof(char) * BUFFER_SIZE)) == NULL)
		  return (-1);
		if ((read_result = read_buffer(buffer, fd)) == 0 || read_result == -1)
			return (read_result);
  }
  if ((line_len = store_line(line, buffer, start_index)) == -1)
		return (-1);
>	if ((start_index = set_next_buffer_start(buffer, start_index, line_len)) == 0) // 틀림
		return (0);
	return (1);
}

//> 이 부분에서 0일 때 0을 리턴하면 안된다. set_next_buffer_start의 리턴값이 0이라는 것은 buffer가 끝난거지 file 이 끝난 게 아니다.
//테스트 결과 '\n'이 나올 때는 잘라지지 않았지만 buffer 의 크기만큼 잘라지긴 했다. 
```

### 3 issue

```c
buffer 에 \n 이 없는 경우에 line에 복사 후 free(buffer)했는데 다음 while 문에서
(int)buffer 값이 0이 아님을 알게 되었다. 내용은 사라지지만 메모리 위치는 유지되는 것 같다.

buffer = 0;

추가해야한다.
```

```c
file 의 마지막 줄에서 

free() : invalid pointer 

에러가 난다.
//-------------------------

"testfile"
abcabc
abcabc

buffer : abcab , read_result : 5
no lf
s1 :   ,  s2 : abcab
strjoin : abcab
line : abcab

buffer : c
abc , read_result : 5
lf found
s1 : abcab  ,  s2 : c
abc , n : 1
strnjoin : abcabc
line : abcabc

gnl : abcabc

no lf // 버퍼 내용 출력이 안된다. -> 두번째 gnl 호출에서 buffer 가 0이 아니다.
s1 :   ,  s2 : abc
strjoin : abc
line : abc

free(): invalid pointer
exited, aborted

// free 를 아예 없앴더니 되긴 한다. 그런데 마지막 줄은 출력이 안된다.
// 그냥 main 함수에서 while 문 끝난 후 gnl 호출 한 번 더 했더니 마지막 줄도 나왔다.

// while 문이 끝났다는 건 eof 이기 때문에 gnl 호출하지 않고 line 을 출력했다.

```

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

```c
testfile
//
aaaaabbbbbcccccddddd
eeeeefffffggggghhhhh
iiii
//
BUFFER_SIZE = 32

// iiiidddd 가 출력된다.
// free 가 buffer 의 모든 메모리를 지우지 못하는 것 같다. 
```

 free 문제 [https://www.notion.so/rhizomized/get-next-line-c86b4d4840294aaab3d142c4539a3e61#8c6afcf522094065b8ef18179364cdb8](https://www.notion.so/rhizomized/get-next-line-c86b4d4840294aaab3d142c4539a3e61#8c6afcf522094065b8ef18179364cdb8)

```c
testfile
//
aaaaaaaaaa
12345
//
BUFFER_SIZE = 11

// 마지막 줄이 출력되지 않는다.
// 마지막 줄에 \n을 넣으면 제대로 동작한다.

// 1. 마지막줄을 line에 저장하고 0을 리턴해야 되는지
// 2. 마지막줄을 line에 저장하고 1을 리턴한 후 다음 gnl 호출에서 바로 0을 리턴하고 빈 line을 보내야 하는지 알아야 한다.

// 1. line 저장 후 read_result < BUFFER_SIZE 이면 0을 리턴한다.
// 그런데 모든 줄(\n포함)이 정확하게 BUFFERSIZE인 경우를 고려하지 못한다.

// 2. 현재 구현방식인데 문제점은 마지막줄에 \n이 없어서 while 문을 빠져나오지 않는다는 것이다.
// -> 현재 구현방식은 마지막줄을 복사한 후 다시 read를 호출해서 0을 리턴한다.

// 원인: 마지막줄에서 line 에 복사한 내용이 다음 while 문에서 read_result == 0 이 되면서 빈 문자열로 업데이트 된 후 0을 리턴한다.
// 마지막줄을 읽고 다음 read 전에 0 을 리턴해야 한다.

// 해결책: read_result 가 0이거나 -1 인 경우 line 이 비어있는 경우에만 빈 라인을 다시 만들었다.
// line이 비어있는 경우는 (**line == 1) 이다. -> 1은 의미없는 값이다.

```

## 동작테스트 in zsh

### 3 issue

```c
vim 에서 파일이 무조건 newline으로 끝나서
repl.it 과 달리 read의 결과가 한 글자 추가된다.
```

## GNL_lover

### issue

```c
check if the read function is called with 'BUFFER_SIZE': 🔎\n
1:	read_result = read(fd, buffer, 1);
// BUFFER_SIZE 안 썼다고 에러 남
```

```jsx
==47961==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x00010b541d5d bp 0x7ffee46be8a0 sp 0x7ffee46be830 T0)
==47961==The signal is caused by a READ memory access.
==47961==Hint: address points to the zero page.
    #0 0x10b541d5c in store_line get_next_line.c:37
    #1 0x10b541bb2 in get_next_line get_next_line.c:81
    #2 0x10b542970 in main main.c:44
    #3 0x7fff7b0503d4 in start (libdyld.dylib:x86_64+0x163d4)
```

- 아마도

    > 심지어 에러가 발생하여 -1이 리턴되는 상황이어도 *line에 대한 메모리 누수는 신경 쓸 필요가 없을 수도 있다. (정확한 정보는 수정바람) gnlTester (2019+) 의 테스트 코드를 보면, 0(EOF)이거나 1(읽음)일 때에만 *line 에 대해 free를 해줬다가, 최근에는 무조건 (에러가 발생하더라도) free 해주는 쪽으로 코드가 수정되었다. [2]

    [https://github.com/Tripouille/gnlTester/commit/8ddbaddcee7cde023af9dca4f40d3c4ed29cac4c](https://github.com/Tripouille/gnlTester/commit/8ddbaddcee7cde023af9dca4f40d3c4ed29cac4c)

```c
알록달록한 에러 나옴
```