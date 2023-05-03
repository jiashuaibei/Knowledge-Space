### 源代码.py

Python程序运行之后，会在电脑弹出众多文本窗口，可用于表白、惊喜、恶搞……可打包为.exe应用程序。

```python
import tkinter as tk
import random
import threading
import time


def boom():
    window = tk.Tk()
    width = window.winfo_screenwidth()
    height = window.winfo_screenheight()
    a = random.randrange(0, width)
    b = random.randrange(0, height)
    
    window.title('弹窗标题')	 # 括号中填写弹窗的标题。
    window.geometry("400x100" + "+" + str(a) + "+" + str(b))

    # text内容为弹窗的文本内容。
    tk.Label(window, text='这里填写希望弹窗的文本内容', bg='white',
             font=('宋体', 14), width=40, height=8).pack()
    window.mainloop()


threads = []
for i in range(99):	# range取值为弹窗的数量。
    t = threading.Thread(target=boom)
    threads.append(t)
    time.sleep(0.1)
    threads[i].start()
```

### 打包为.exe可执行文件

可以通过pyinstaller将.py文件打包为.exe程序。

**安装pyinstaller**

```
pip install pyinstaller
```

**安装速度慢可更换为国内源：**

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyinstaller #清华源
```

打包.exe需要为程序准备图标，图片格式为.ico，命名为favicon.ico，将favicon.ico图标和源代码name.py放到同一个文件夹里，在文件夹路径打开终端，执行打包命令，等待编译成功即可。

**pyinstaller打包命令：**`pyinstaller -F (-w) -i favicon.ico name.py`

-w 表示编译完成的.exe运行时不会弹出黑色终端窗口，如果运行的程序需要在终端输入内容，则不加-w参数。

-i 表示给.exe添加图标，图标名为favicon.ico