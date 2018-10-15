## stat函数  

​	获得文件的属性信息

​	比如查看文件的大小

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/stat.h>

int main(){
	struct stat sbuf;
	if(stat("./test.txt", &sbuf) == -1)
		perror("stat"),exit(1);
	printf("size : %d\n", sbuf.st_size);
}	
```

