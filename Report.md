# ������� ������ ���218 �������� ������ 4
### �������� ��� �� ��

```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>

const int buf_size = 4096;

int main(int argc, char** argv) {
	if (argc != 3) {			# �������� ���������� ������� ������
		printf("Wrong file names");
		exit(-1);
	}
	int fd[2];				# ������ �������� ����������
	ssize_t size;
	int pipe(int *fd);		# ������ �����
	if((fd[0] = open(argv[1], O_RDONLY)) < 0){ 	# �������� ������ ����
		printf("Can't open file\n");
		exit(-1);
	}
	char str_buf[buf_size];	# ������������� �����
	ssize_t read_size = read(fd[0], str_buf, buf_size); # ����� � �����
	if(read_size == -1) {
		printf("Can't write this file\n");
		exit(-1);
	}
	if(close(fd[0]) < 0) {	# �������� ����
		printf("Can't close file\n");
	}
	if(pipe(fd) < 0) {		# �������� �����
		printf("Can't open pipe\n");
		exit(-1);
	}
	size = write(fd[1], str_buf, read_size);	# ���� �� � ����� ������
	if(size != read_size) {
		printf("Can't write all string to pipe\n");
		exit(-1);
	}
	size = read(fd[0], str_buf, read_size);	# ����� ������ �� �����
	if(size < 0){
		printf("Can't read string from pipe\n");
		exit(-1);
	}
	if((fd[1] = open(argv[2], O_WRONLY | O_CREAT, 0666)) < 0 { # �������� ������ ����
		printf("Can't open file\n");
		exit(-1);
	}
	size = write(fd[1], str_buf, read_size);	# ���� �� ����� �� ������ ����. � ������ ����� ������� ������ ����, �� ��� �� ���� �� �� �����, ��� � ������� ������
	if(size != read_size) {
		printf("Can't write all string\n");
		exit(-1);
	}
	if(close(fd[0]) < 0){		# �������� ��� ����� �����
		printf("Can't close reading side of pipe\n"); 
		exit(-1);
	}
	if(close(fd[1]) < 0){
		printf("Can't close writing side of pipe\n");
		exit(-1);
	}
	return 0;
}
```


### � ������ ��������� ��������� ������� ��������� ���� � ������ ��� ���� � �������� ����� ��� ������� ���������. � ����� ������ ��� ������� ������ �� ������� �����, ������� � pipe � ������ ������� ��� ����������� ������ ����.