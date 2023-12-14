# 记录js调用C的唤醒引擎-wasm集成

> 背景为AIUI那边没有浏览器端可以直接通过唤醒词进行语音交互的sdk，因为调用了研究院的唤醒引擎，是C开发的。于是调研使用js集成引擎的可能性。

## 1. 安装环境

### 搭建emscripten环境

由于WebAssembly使用了emscripten工具来编译C/C++的代码，需要先搭建环境。

#### 下载emsdk源码

>  参考https://emscripten.org/docs/getting_started/downloads.html

```bash
# Get the emsdk repo
git clone https://github.com/emscripten-core/emsdk.git
# 我是直接到github地址下载zip包

# Enter that directory
cd emsdk
```

#### 下载最新的工具

下载sdk工具，这一步比较慢，需要保证网络通畅(梯子)，并安装python3.6以上环境。

```bash
# Download and install the latest SDK tools.
./emsdk install latest
```

![image-20231205154806617](https://oss-beijing-m8b.openstorage.cn/luwangpublic2/image-20231205154806617.png)

#### 写入 .emscripten 文件

```bash
# Make the "latest" SDK "active" for the current user. (writes .emscripten file)
./emsdk activate latest
```

![image-20231205155137512](https://oss-beijing-m8b.openstorage.cn/luwangpublic2/image-20231205155137512.png)

#### 激活路径

```bash
# Activate PATH and other environment variables in the current terminal
source ./emsdk_env.sh
```

![image-20231205160013687](https://oss-beijing-m8b.openstorage.cn/luwangpublic2/image-20231205160013687.png)

## 2. 编译一个例子

环境设置好后，我们来看看如何使用它来编译一个 C 示例到 Wasm。使用 Emscripten 进行编译时，有许多选项可用，但我们将介绍的主要两种场景是：  

* 编译为 Wasm 并创建 HTML 来运行我们的代码，以及在 Web 环境中运行 Wasm 所需的所有 JavaScript“胶水”代码；
* 编译为 Wasm ，仅创建 JavaScript。

### hello world

在工程下创建examples目录。

```bash
cd [your_project_path]/src
mkdir -vp /examples/simple-hello-world
```

```cpp
#include <stdio.h>

int main() {
    printf("Hello World\n");
    return 0;
}
```

编译文件

```bash
emcc hello.c -o hello.html
```

![image-20231205164249904](https://oss-beijing-m8b.openstorage.cn/luwangpublic2/image-20231205164249904.png)

即可发现打包好了一些html和js文件

![image-20231205164334309](https://oss-beijing-m8b.openstorage.cn/luwangpublic2/image-20231205164334309.png)

访问html就会发现已经输出了Hello World

![image-20231205164500078](https://oss-beijing-m8b.openstorage.cn/luwangpublic2/image-20231205164500078.png)