---
title: test
date: 2023-12-13
---

# test 测试

一个优秀的`md`工具应该能够完全支持`Typora`里的功能，`Typora`在随时写作方面具有绝对的优势

- 启动快
- 能够在当前目录新建（Windows)
- 能够上传图片转换为链接

## 检查

- [ ] 公式
- [ ] 代码块
- [x] 目录

## 其它

### latex

- 漂亮，公式支持

- `md`转换比较麻烦，而且`pdf`的形式也限制了只能在PC上面观看

### Notion

对行内公式比如$\alpha$空格影响大，对中文有些不友好，需要快捷键而不是直接`md`语法

和行内代码块`code`支持有点小麻烦

段内代码有太长的行距

```cpp
#include <iostream>
using namespace std;
int main(){
    
    cout << "Hello world" << endl;
    
    return 0;
}
```

## 公式支持

行内公式$a^2 + b^2 = c^2$支持，尤其在同一段内$\alpha + \beta = 0$可能会出现问题

对于段内公式
$$
a^2 + b^2 = c^2 \\
\alpha + \beta = 0 \\
\frac{x^2}{a^2} + \frac{y^2}{b^2} = R^2
$$
可能有些并不支持`tex`格式的：
$$
\mathrm{R}=\mathrm{E}(\mathrm{v}^\mathrm{T})=
\begin{pmatrix}
\sigma_1^2 & \cdots & 0 \\\\ \vdots & \ddots & \vdots \\\\ 0 & \cdots & \sigma_\mathrm{k}^2
\end{pmatrix}
$$

## Shortcode

其实尽可能不用，至少主要的功能比如公式不应该由shortcode实现

> In every ending, there is a new beginning.

## 对比

### Hugo-tranquilpeak

公式支持

段距偏大

### Hugo-LoveIt

不是很支持`Tex`格式的

能支持一些基础的`tex`语法，但高级的应该就不行了
$$
\begin{aligned}
a &= x_0 + x_1^2 \\\\
b &= c^2
\end{aligned}
$$
换行得是`\\\\\`

