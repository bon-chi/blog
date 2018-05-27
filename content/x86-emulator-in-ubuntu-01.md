+++
title = "自作エミュレータで学ぶx86アーキテクチャコンピュータが動く仕組みを徹底理解！をubuntuで進める"
description = "x86-emulator-in-ubuntu"
date = 2018-05-27
template = "page.html"
tags = ["x86-emulator"]
+++


OS: ubuntu16.04
gcc: (Ubuntu/Linaro 6.3.0-18ubuntu2~16.04) 6.3.0 20170519

## Chapter1
### 1.3
gccコマンドは以下
~~~
gcc -Wl,--entry=func,--oformat=binary -nostdlib -fno-asynchronous-unwind-tables -m32 -o casm-c-sample.bin casm-c-sample.c
~~~

ndisasmはapt-getでnasmをinstallすると一緒に入ってくる
~~~
sudo apt-get install nasm
~~~

## Chapter2
### 2.3
`emu2.3`のMakefileを修正する必要がある
- `TARGET`は`.exe`である必要がないので、`TARGET = px86`
- `CC`でコンパイルに利用するgccを指定している。書籍だとサンプルコード内にあるgccを使うがそれはwindow用なので、ubuntuにinstallされているgccを使う。`CC = gcc`
- `CC`で`Z_TOOLS`を参照しなくなったので、`Z_TOOLS`を削除する

最終的に`Makefile`は以下のようになる
~~~Makefile
TARGET = px86
OBJS = main.o

CC = gcc
CFLAGS += -Wall

.PHONY: all
all :
	make $(TARGET)

%.o : %.c Makefile
	$(CC) $(CFLAGS) -c $<

$(TARGET) : $(OBJS) Makefile
	$(CC) -o $@ $(OBJS)
~~~


## Chapter3
### 3.2, 3.4
`Makefile`をChapter2と同様に修正する


