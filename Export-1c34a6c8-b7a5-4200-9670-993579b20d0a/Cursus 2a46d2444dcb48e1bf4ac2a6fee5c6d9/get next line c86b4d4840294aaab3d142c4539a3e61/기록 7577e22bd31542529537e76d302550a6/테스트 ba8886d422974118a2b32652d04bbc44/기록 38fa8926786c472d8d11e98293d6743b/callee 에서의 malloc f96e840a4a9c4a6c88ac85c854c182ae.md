# callee 에서의 malloc

Status: need research
구현버전: 2
문제유형: semantic error
발견법: replit

# 상세

```c
static ssize_t	store_line(char **line, char *buffer, size_t start_index)
{
	ssize_t line_len;

	line_len = 0;
	if (buffer[start_index] != '\n') //segfault 1
	{
		while ((buffer[start_index + line_len] != '\n')
		|| (start_index + line_len < BUFFER_SIZE)) // seg fault 2
			line_len++;
  }
	if ((*line = malloc(sizeof(char) * (line_len + 1))) == NULL)
		return (-1);
	ft_strlcpy((void *)*line, (const void *)&buffer[start_index], line_len + 1);
	return (line_len);
}

```

buffer[start_index] 에서 segmentation fault 를 일으킨다. printf("%d\n", (int)buffer);의 결과로 0이 나온다. → gnl 함수에서 store_line 함수를 호출하여 malloc 및 내용 저장하면 gnl 함수에서는 반영되지 않는다. 

# 원인

모른다.

# 해결

read_buffer 함수의 malloc 부분을 get_next_line 함수로 옮겼더니 해결되었다. 왜 다른 함수 안에서 malloc 한 buffer 는 그 함수가 끝나면 초기화되는가?