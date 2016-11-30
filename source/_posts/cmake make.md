[鸟哥的Linux私房菜](http://linux.vbird.org/linux_basic/)

[CMake实践]()

## gcc

### 单一程序

* hello.c文件

```c
#include <stdio.h>

int main(){
    printf("Hello World gcc!\n");
    return 0;
}
```

* 编译

```bash
gcc hello.c //生成可执行文件a.out
```

```bash
gcc -c hello.c //生成目标文件hello.o
gcc -o hello hello.o //生成可执行文件hello
```

### 主程序、子程序链接

* thanks.c

```c
#include <stdio.h>

int main(){
    printf("Hello World!\n");
    thanks2();
    return 0;
}
```

* thanks2.c

```c
#include <stdio.h>

void thanks2(){
    printf("Thank you!\n");
}
```

* 编译链接

```bash
gcc -c thanks.c thanks2.c
gcc -o thanks thanks.o thanks2.o
```

### 调用外部函数库

* sin.c

```c
#include <stdio.h>

int main(){
    float value;
    value = sin(3.14 / 2);
    printf("%f\n", value);
    return 0;
}
```

* 编译

```bash
gcc sin.c -lm -L/usr/lib -I/usr/include
```

## make

```bash
wget -c http://linux.vbird.org/linux_basic/0520source/main.tgz
```

```
.
├── cos_value.c
├── haha.c
├── main.c
└── sin_value.c
```

* gcc版

```bash
gcc -c main.c haha.c sin_value.c cos_value.c
gcc -o main main.o haha.o sin_value.o cos_value.o -lm -L/usr/lib -L/lib
```

* make版1

* makefile文件

```makefile
main: main.o haha.o sin_value.o cos_value.o
	gcc -o main main.o haha.o sin_value.o cos_value.o -lm
```

```bash
make
```

* make版2

* makefile文件

```makefile
main: main.o haha.o sin_value.o cos_value.o
	gcc -o main main.o haha.o sin_value.o cos_value.o -lm
clean:
    rm -f main main.o haha.o sin_value.o cos_value.o
```

```bash
make
make clean
```

* make版3

* makefile文件

```makefile
LIBS = -lm
OBJS = main.o haha.o sin_value.o cos_value.o
main: ${OBJS}
	gcc -o main ${OBJS} ${LIBS}
clean:
    rm -f main ${OBJS}
```

```bash
CFLAGS="-Wall" make clean main
```

* make版4

* makefile文件

```makefile
LIBS = -lm
OBJS = main.o haha.o sin_value.o cos_value.o
CFLAGS = -Wall
main: ${OBJS}
	gcc -o $@ ${OBJS} ${LIBS}
clean:
    rm -f main ${OBJS}
```

```bash
make clean main
```

## 初试make-CMake的Hello World

### 内部构建

* main.c文件

```c
#include <stdio.h>

int main(){
    printf("Hello World cmake!\n");
    return 0;
}
```

* CMakeLists.txt文件

```cmake
PROJECT(HELLO)
SET(SRC_LIST main.c)
MESSAGE(STATUS "This is BINARY dir" ${PROJECT_BINARY_DIR})
MESSAGE(STATUS "This is SOURCE dir" ${PROJECT_SOURCE_DIR})
ADD_EXECUTABLE(hello ${SRC_LIST})
```

```bash
cmake .
make
./hello
make clean
```

### 外部构建

```bash
mkdir build
cd build
cmake ..
make
make clean
rm -rf *
```

## 更好一点的Hello World

* 在`cmake/t2`目录下新建如下结构

```bash
.
├── build
├── CMakeLists.txt
├── COPYRIGHT
├── doc
│   └── hello.txt
├── README
├── runhello.sh
└── src
    ├── CMakeLists.txt
    └── main.c
```

* t2/CMakeLists.txt

```cmake
PROJECT(HELLO)
ADD_SUBDIRECTORY(src bin)
INSTALL(FILES COPYRIGHT README DESTINATION share/doc/cmake/t2)
INSTALL(PROGRAMS runhello.sh DESTINATION bin)
INSTALL(DIRECTORY doc/ DESTINATION share/doc/cmake/t2)
```

* runhello.sh

```shell
hello
```

* t2/src/CMakeLists.txt

```cmake
ADD_EXECUTABLE(hello main.c)
INSTALL(TARGETS hello RUNTIME DESTINATION bin)
```

```bash
cd build
cmake -DCMAKE_INSTALL_PREFIX=/tmp/t2/usr ..
make
make install
```
