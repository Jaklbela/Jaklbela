# Жмурина Ксения БПИ218 Домашняя работа 4
### Исходный код на си

```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>

const int buf_size = 4096;

int main(int argc, char** argv) {
	if (argc != 3) {			# Проверяю количество входных данных
		printf("Wrong file names");
		exit(-1);
	}
	int fd[2];				# Создаю файловый дескриптор
	ssize_t size;
	int pipe(int *fd);			# Создаю трубу
	if((fd[0] = open(argv[1], O_RDONLY)) < 0){ 		# Открываю первый файл
		printf("Can't open file\n");
		exit(-1);
	}
	char str_buf[buf_size];			# Инициализирую буфер
	ssize_t read_size = read(fd[0], str_buf, buf_size); # Читаю в буфер
	if(read_size == -1) {
		printf("Can't write this file\n");
		exit(-1);
	}
	if(close(fd[0]) < 0) {			# Закрываю файл
		printf("Can't close file\n");
	}
	if(pipe(fd) < 0) {		# Открываю трубу
		printf("Can't open pipe\n");
		exit(-1);
	}
	size = write(fd[1], str_buf, read_size);	# Пишу в трубу данные
	if(size != read_size) {
		printf("Can't write all string to pipe\n");
		exit(-1);
	}
	size = read(fd[0], str_buf, read_size);		# Читаю данные из трубы
	if(size < 0){
		printf("Can't read string from pipe\n");
		exit(-1);
	}
	if((fd[1] = open(argv[2], O_WRONLY | O_CREAT, 0666)) < 0 {	# Открываю второй файл
		printf("Can't open file\n");
		exit(-1);
	}
	size = write(fd[1], str_buf, read_size);	# Пишу из трубы во второй файл. Я забыла здесь закрыть второй файл, но это по сути то же самое, как и закрыть первый
	if(size != read_size) {
		printf("Can't write all string\n");
		exit(-1);
	}
	if(close(fd[0]) < 0){		# Закрываю обе части трубы
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


### В данной программе требуется создать небольшой файл и ввести его путь и название файла при запуске программы. В таком случае она возьмет данные из первого файла, положит в pipe и оттуда создаст или перезапишет второй файл.
