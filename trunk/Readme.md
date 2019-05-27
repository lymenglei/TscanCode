
## 基于原repo的修改：

我本地只有安装vs2013，而原repo是基于vs2015搞的，所以用vs打开sln，分别对cli和tscancode的 属性->配置属性->常规->平台工具集
由v140改为v120，这样就不影响编译了。

然后呢，还会报错，说snprintf找不到标识符。百度了一下，发现这里的宏定义有点问题。

```c++
#if defined(_MSC_VER) && (_MSC_VER > 1600 ) && (!defined WINCE)
#define  SNPRINTF snprintf
#elif defined _MSC_VER
#define SNPRINTF	_snprintf
#else
#define SNPRINTF	snprintf
#endif
```

其中，我本机_MSC_VER定义为1800，故在前面加一个“!”

```c++
#if defined(_MSC_VER) && !(_MSC_VER > 1600 ) && (!defined WINCE)
#define  SNPRINTF snprintf
#elif defined _MSC_VER
#define SNPRINTF	_snprintf
#else
#define SNPRINTF	snprintf
#endif
```

这样就可以顺利编译了。

其中tscancode工程会生成一个tscancode-core库，而cli工程则会生成对应的exe程序，生成的程序是在命令行下运行的。

生成的目录在TscanCode/trunk/bin/debug/