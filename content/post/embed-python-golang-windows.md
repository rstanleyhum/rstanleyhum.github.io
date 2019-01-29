---
title: "Embedding Python 2.7 into Golang on Windows 10"
date: 2018-04-22T15:57:18-04:00
tags: []
draft: false
---

So I've been working on a healthcare data science project with Python 3, Pandas, and SciKit-Learn. Unfortunately, the project uses Java and I'm not interested in re-coding all of the data cleaning and the machine learning code in Java. So I've decided to look at creating a microservice. In order to make the microservice a standalone windows executable, I decided to look at writing a Golang program which embeds Python.

The inspiration for this project was https://www.datadoghq.com/blog/engineering/cgo-and-python/.

First, I needed a C compiler for Cgo. I can use Scoop (http://scoop.sh) to install my tools.

```shell

scoop install go
scoop install python27
scoop install gcc
scoop install pkg-config

```

Scoop allows me to install everything in userspace so that I don't need to have administrative privileges.

Second, there are some configurations that I need to make things work.

Scoop installs the apps in C:\Users\<username>\scoop\apps. Thus we need to use those to reference in the compiler.

pkg-config is not standardized across the distributions and definitely not on Windows. We need to set up a directory for the .pc files used by pkg-config.

I've placed them in $GOPATH\apps\pkgconfig\share\. I created the directory then placed a file called python-2.7.pc with the following contents.

```

Name: Python
Description: Python library
Requires:
Version: 2.7
Libs.private: -lpthread -ldl -lutil
Libs: -L"C:/Users/<username>/scoop/apps/python27/current" -lpython27
Cflags: -I"C:/Users/<username>/scoop/apps/python27/current/include" -D MS_WIN64

```

You need the -D MS_WIN64 for the compiler to work.

In the golang src directory, create a main.go as follows:

```golang

package main

// #cgo pkg-config: python-2.7
// #include <Python.h>
import "C"
import (
    "fmt"
)

func main() {
    fmt.Println("Hello World")
    C.Py_Initialize()
    fmt.Println(C.GoString(C.Py_GetVersion()))
    C.Py_Finalize()
}

```

Now in order for this to work then the python27.dll file from the 'current' directory (above) needs to be in the src directory.

Then do a

```shell
go build .
```

Running the executable should give the 

```shell
Hello World
2.7.14 (v2.7.14:84471935ed, Sep 16 2017, 20:25:58) [MSC v.1500 64 bit (AMD64)]
```

Now if I use the nice open source library at https://github.com/sbinet/go-python then we get ...

```python
package main

import (
    "github.com/sbinet/go-python"
)

func main() {
    python.Initialize()
    python.PyRun_SimpleString("print ('hello, world!')")
    python.Finalize()
}

```

And you get ...

```shell
hello, world!
```



