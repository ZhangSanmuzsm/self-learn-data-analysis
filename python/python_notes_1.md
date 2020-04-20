# python笔记1——了解python
## python简介
Python是著名的“龟叔”Guido van Rossum在1989年圣诞节期间，为了打发无聊的圣诞节而编写的一个编程语言。
## python的特点
* 优雅
* 明确
* 简单
## python适合的领域与不适合的领域
1. 适合的领域
* Web网站和各种网络服务
* 系统工具和脚本
* 作为“胶水”语言把其他语言开发的模块包装起来方便使用
2. 不适合的领域
* 贴近硬件的代码（首选C）
* 移动开发：iOS/Android有各自的开发语言
* 游戏开发：C/C++
## python和其他语言对比
||类型|运行速度|代码量|
|-|-|-|-|
|C|编译为机器码|非常快|非常多|
|Java|编译为字节码|快|多|
|Python|解释执行|慢|少|

注意：python源码不能加密
## python安装
python是跨平台的，python有2.x版和3.x版，推荐使用3.x,目前最新是3.8版的,建议安装3.8版,直接上官网下载安装系统对应的版本安装即可，记得添加环境变量即可
## python解释器
* CPython:当我们从Python官方网站下载并安装好Python 3.x后，我们就直接获得了一个官方版本的解释器：CPython。这个解释器是用C语言开发的，所以叫CPython。在命令行下运行python就是启动CPython解释器。CPython是使用最广的Python解释器
* IPython:IPython是基于CPython之上的一个交互式解释器，也就是说，IPython只是在交互方式上有所增强，但是执行Python代码的功能和CPython是完全一样的。好比很多国产浏览器虽然外观不同，但内核其实都是调用了IE。CPython用>>>作为提示符，而IPython用In [序号]:作为提示符。
* PyPy:PyPy是另一个Python解释器，它的目标是执行速度。PyPy采用JIT技术，对Python代码进行动态编译（注意不是解释），所以可以显著提高Python代码的执行速度。绝大部分Python代码都可以在PyPy下运行，但是PyPy和CPython有一些是不同的，这就导致相同的Python代码在两种解释器下执行可能会有不同的结果。如果你的代码要放到PyPy下执行，就需要了解PyPy和CPython的不同点。
* Jython:Jython是运行在Java平台上的Python解释器，可以直接把Python代码编译成Java字节码执行。
* IronPython:IronPython和Jython类似，只不过IronPython是运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码。
## 第一个python程序
```python
print("hello,world")
```