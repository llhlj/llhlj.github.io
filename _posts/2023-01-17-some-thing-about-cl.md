---
layout: post
title: 一些关于编译器的讨论
date: 2023-01-17 21:29
category: 
author: 
tags: []
summary: 
---


https://www.v2ex.com/t/909407#r_12587735
```
目前跨平台项目编译问题我还是推荐使用 conan ，对于三方库的管理和编译工具链没有重度依赖。  
我们现在工程 C++ 标准锁定在 14 ，Windows > MSVC15 （如果没有用一些较新的 API ，使用 7.0 SDK XP 都可以支持）、macOS 、iOS apple-clang > 12.4 （ macOS ARM64 支持） 以上、Android NDK21 clang9 以上、Linux GCC5 以上。  
GCC5 已经支持大部分 C++14 标准，如果是为了兼容一些 GCC5 没有支持的特性或者旧内核系统，  
可以考虑 GCC7+ glibc2.23 的 docker 镜像，这样 Ubuntu 16.04 都可以跑。
主流的 libevent 、openssl 、sqlite3 、libcurl 、zlib 、qt 等都验证编译通过。
```

https://www.v2ex.com/t/909407#r_12588945
```
glibc 的兼容性，和 C++ 动态库的兼容性是两件事，而且这两点几乎都和 clang 没什么关系。你提到的这些问题，大部分锅是 glibc 的。  
glibc 几乎很少有人需要最新版的特性，所以只要你去链最老的 glibc 就可以了，一般会推荐让你到所有目标平台中最老的那个去编译。glibc 就是唯一特殊的那个崽，不能静态链接。  
c++ 你想用新的编译器、新的库（包括 std ）是完全可以的，既可以静态链接（如果你能搞到静态库），也可以自己打包动态库（比如 /usr/lib/myapp/libxxx.so ）然后设置 rpath 到 $ORIGIN 之类的，后者其实就是 Windows 那种一个 exe 带一堆 dll ( vcruntime140.dll, etc) 的风格。  
这两点综合起来就是，比如你需要给 Ubuntu 18.04, 20.04, ... 这些平台提供支持，那你首先准备一台 Ubuntu18.04 的环境，然后通过 toolchain ppa 之类的东西安装或者编译一套新版的 g++/clang ，多新的都可以，只要你能在 18.04 上跑。最后用这套工具链去编译你的 app ，然后把所有依赖 (除了 glibc) 通过静态或者动态的东西打包带走。  
这个你找台环境试一下就知道怎么回事了，比如尝试一下给一台默认 gcc 5.x 的环境编一个使用了 C++17 Filesystem 的应用。  

你要是觉得麻烦就直接 docker 得了。
```
