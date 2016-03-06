title:  Makefile的高级用法例子
date: 2016-13-06 13:51:51
tags: [Makefile，自动编译]
categories: [Linux]
---
##关于make
自动编译工具有很多，在小的工程中使用make，cmake，gmake这些自动编译工具不仅简单明了，也可以对项目的整体结构，代码的编译链接过程更加清晰，make的基本用法比较简单，但是我遇到的很多的项目的makefile文件都不是简单的表达方式，这里对一个高级的makefile例子做个解释。
源makefile是这样的：
<pre><code>
CC=g++
CFLAGS=-c -Wall
LDFLAGS=
SOURCES=main.cpp hello.cpp factorial.cpp
OBJECTS=$(SOURCES:.cpp=.o)
EXECUTABLE=hello

all: $(SOURCES) $(EXECUTABLE)
    
$(EXECUTABLE): $(OBJECTS) 
	$(CC) $(LDFLAGS) $(OBJECTS) -o $@

    .cpp.o:
    	$(CC) $(CFLAGS) $< -o $@

.PHONY: clean
clean:
	-rm *.o $(EXECUTABLE)
</code></pre>

    `OBJECTS=$(SOURCES:.cpp=.o)`
这里的变量OBJECTS做了模式匹配，意思是名字和SOURCES.cpp相同，只是.cpp变成.o了，具体就是main.o hello.o factorial.o

    `all: $(SOURCES) $(EXECUTABLE)`
默认的target是all，所以编译的时候会make或者make all,　这里是例子的第一个真正的规则，它告诉make要想build all的话必须先build它的依赖，也就是main.cpp hello.cpp factorial.cpp,以及hello，然后什么都不做，因为这一句后面没有具体的怎么做的规则。

    $(EXECUTABLE): $(OBJECTS) 
	$(CC) $(LDFLAGS) $(OBJECTS) -o $@
如果想生成hello，必须先生成3个.o文件，这里的$@也是一个匹配模式，它表示所有的依赖$(OBJECTS) ，也就是这里的所有.o

    .cpp.o:
    $(CC) $(CFLAGS) $< -o $@
这里的.cpp.o是一种后缀匹配，为了build结尾为.o的文件，必须先build相应的.cpp,这里的$<指的是第一个依赖，默认为同文文件名的.cpp文件。

##编译过程
当你输入make的时候，make工具试着生成默认的all，它没有存在，make工具就寻找它的依赖，依次为main.cpp hello.cpp factorial.cpp 和　hello。因为前三个都存在，make寻找它们的依赖，发现没有，就什么都不做了。如果这时候没有main.cpp，那么就会有  "no rule to make target 'main.cpp'" 的提示。
对于hello,它依赖与main.o hello.o factorial.o，如果不存在，就根据模式匹配中的构建规则中生成它们，例如g++ -c -Wall -o main.o main.cpp。
当然当依赖文件的时间比目标本身的时间新的话，也会自动的重新rebuild。

