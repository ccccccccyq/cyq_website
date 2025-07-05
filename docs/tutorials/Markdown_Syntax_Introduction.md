# Markdown简介

Markdown是一种轻量级**标记语言**，创始人为约翰·格鲁伯（英语：John Gruber）。
它允许人们使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML（或者HTML）文档。  
由于Markdown的轻量化、易读易写特性，并且对于图片、图表、数学式都有支持，
许多网站都广泛使用Markdown来撰写帮助文档或是用于论坛上发表消息。

## 1.语法

### 1.1 标题

markdown用 **#** 区分标题级数。 **#** 代表一级标题， **##** 代表二级标题，以此类推。

```markdown
this is a test markdown code.
这是一级标题：  
# Markdown简介  
这是二级标题：  
## 1.语法  
这是三级标题：  
### 1.1 标题  
```

### 1.2 列表

有序列表和无序列表两种形式：

首先是无序列表：（无序列表使用 * 标识）

* item 1
* item 2

其次是有序列表：（有序列表使用1.标识）

1. item 1
2. item 2
3. item 3

### 1.3 代码插入

#### 1.3.1 代码片段

段落上的一个函数或片段的代码可以用反引号(`)把它包起来，例如：

`print()` 函数

```python title="test.py"
# this is a test python code
import numpy as np
my_name = "James"
my_age = 21
print(f"My name is {my_name}, and I'm {my_age} years old.")
```

#### 1.3.2 代码区块

``` python
# this is a test python code
def main():
    pass
```

使用三个`或是三个~都可以定义代码区块，还可以选择语言种类，对代码进行高亮显示。

### 1.4 文本

文本分段，前后至少保留一个空行。

#### 1.4.1 加粗或斜体

* **加粗**（使用ctrl+/）阅览源代码，看到底是如何加粗的
* _斜体_  
* **_粗斜体_**  

#### 1.4.2 线条

* 水平线：三个

---

* 删除线：前后各两个

eg. 原价：100

删除线使用后：

~~原价：100~~

* 下划线：和HTML标签相同
_这里有下划线_

### 1.5 emoji

#### 1.5.1  

:dotted_line_face:

:umbrella:

### 1.6 转义字符

``` markdown

\ 反斜线
` 反引号

```
