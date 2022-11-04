---
title: Markdown Sytax Tutorials
date: 2021-11-01 16:48:58 +0800
categories: [Tools, Markdown]
tags: [markdown]
math: true
---
# 这是一级标题
```markdown
# 这是一级标题
```
<br>

## 这是二级标题
```markdown
## 这是二级标题
```
<br>

换行用空格+enter或者`<br>` 
<br>

## 强调
<br>
**hhh** 
```markdown
**hhh**
```
<br>

*hhh* 
```markdown
*hhh*
```
<br>


[[hehehe]]
<br>
```markdown
[[hehehe]] 
```
<br>


### My Great Heading {#custom-id}
```markdown
### My Great Heading {#custom-id}
```
<br>
  
### 公式

```markdown
$$
\lim_{x \to \infty} \frac{sin(t)}{x}=1
$$
```

$$
\lim_{x \to \infty} \frac{sin(t)}{x}=1
$$

在vscode中连续按两下ctrl+m会产生四个\$，来编辑公式。如果要在一段文字中插入公式（行内），用快捷键ctr+m或输入:
```markdown
$\lim_{x \to \infty} \frac{sin(t)}{x}=1$
```
$\lim_{x \to \infty} \frac{sin(t)}{x}=1$ 
<br>





## 表格
```markdown
| 小明 | 大明 | 姚明  |
| :--- | ---: | :---: |
| 1.3  |  1.2 |  1.8  |
``` 

| 小明 | 大明 | 姚明  |
| :--- | ---: | :---: |
| 1.3  |  1.2 |  1.8  |


(按住 *alt+shift+f* 可以将文本中的表格部分格式化)

## 链接
这是一个[链接](https://zh.d2l.ai/)
```markdown
这是一个[链接](https://zh.d2l.ai/)
```

## 代码块
```python
def to_onehot(X, size):  # 本函数已保存在d2lzh包中方便以后使用
    return [nd.one_hot(x, size) for x in X.T]

X = nd.arange(10).reshape((2, 5))
inputs = to_onehot(X, vocab_size)
len(inputs), inputs[0].shape
```

````markdown
```python
def to_onehot(X, size):  # 本函数已保存在d2lzh包中方便以后使用
    return [nd.one_hot(x, size) for x in X.T]

X = nd.arange(10).reshape((2, 5))
inputs = to_onehot(X, vocab_size)
len(inputs), inputs[0].shape
```
````


### 对号
$$
\checkmark
\huge \color{red}{\checkmark}
$$

```markdown
$$
\checkmark
\huge \color{red}{\checkmark}
$$
```



<br>

## vscode常用快捷键
vscode禅模式快捷键：ctrl+k 松开再按z

按两下esc即可退出禅模式


Keybindings
The cmd key for Windows is ctrl.

| Shortcuts            | Functionality              |
| -------------------- | -------------------------- |
| ctrl-k v             | Open preview to the Side   |
| ctrl-shift-v         | Open preview               |
| ctrl-shift-s         | Sync preview / Sync source |
| shift-enter          | Run Code Chunk             |
| ctrl-shift-enter     | Run all Code Chunks        |
| cmd-= or cmd-shift-= | Preview zoom in            |
| cmd-- or cmd-shift-_ | Preview zoom out           |
| cmd-0                | Preview reset zoom         |
| esc                  | Toggle sidebar TOC         |

<br>

## Blockquote
>blockquote

```markdown
>blockquote
```

<br>

## Ordered List
1. First item
2. Second item
3. Third item

```markdown
1. First item
2. Second item
3. Third item
```
<br>

## Unordered List
- First item
- Second item
- Third item

```markdown
- First item
- Second item
- Third item
```
<br>

## Task List
- [x] task1
- [ ] task2

```markdown
- [x] task1
- [ ] task2
```
<br>

## Definition List
term
: definition

```markdown
term
: definition
```
<br>

## Strikethrough
~~The world is flat.~~

```markdown
~~The world is flat.~~
```
<br>

## Image
![]({{ site.url }}/assets/img/2021-11-01-Markdown-Tutorials/CAAQA.png)
![]({{ site.url }}/assets/img/2021-11-01-Markdown-Tutorials/ML.png)

```markdown
![]({{ site.url }}/assets/img/2021-11-01-Markdown-Tutorials/CAAQA.png)
![]({{ site.url }}/assets/img/2021-11-01-Markdown-Tutorials/ML.png)
```
<br>

## Footnote
Here's a sentence with a footnote. [^1]

[^1]: This is the footnote.

```markdown
Here's a sentence with a footnote. [^1]

[^1]: This is the footnote.
```
<br>